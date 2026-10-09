# Worksheet — UniPilot

**Họ tên:** Đoàn Quang Thanh · **MSSV:** 2A202602841 · **Ngày:** 09/10/2026 (ICT)

## Trạm 1 — Loại mô hình

**Câu chốt:** UniPilot được định vị B2C tự phục vụ vì sinh viên là người dùng và là nhóm khách hàng dự kiến trả phí thuê bao. Chưa có bằng chứng trường học hoặc tổ chức là bên mua hay kênh phân phối.

Bảng dưới đây ghi khả năng đo theo dữ liệu hiện có: ✅ có số liệu quan sát; 🔧 có thể đo sau khi bổ sung instrumentation; ❌ hiện chưa có nguồn dữ liệu khả thi. Event schema không đồng nghĩa với event đã được ghi nhận.

| Đèn B2C | Trạng thái | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| Retention curve D1/D7/D30/D60 theo tuần học đủ điều kiện | 🔧 | Day20 Metrics Pack định nghĩa academic_task_completed, user_id và eligibility; cần event store, academic calendar và cohort query. Chưa có cohort quan sát. |
| Activation: task hợp lệ đầu tiên đến completion đầu trong 7 ngày | 🔧 | Cần lưu academic_task_created, academic_task_completed, identity và timestamp; hiện chưa có event stream/baseline. |
| p95 AI cost per user/month ÷ ARPU | ❌ | Day22 chỉ có cost estimate cho paid seat; cần phân phối usage/cost theo user, invoices và ARPU quan sát. |
| Trial-to-paid | 🔧 | Day22 funnel là scenario; cần checkout, charge, refund và qualified-visit events. Chưa có giao dịch. |
| Retention M12 | ❌ | Cần paid cohort, renewal history và 12 tháng follow-up; hiện chưa có cohort đủ tuổi. |
| Free-tier cost ÷ total COGS | ❌ | Chưa có free-tier policy, traffic hoặc cost log; cần tag plan và meter trước launch. |
| Refund rate | 🔧 | Cần payment/refund ledger với transaction IDs; chưa có giao dịch/hoàn tiền. |
| LTV/CAC, CAC payback và GM | 🔧 | Day22 có mô hình, chưa có revenue, attributed spend, invoices hay cohort survival thực tế; cần billing, acquisition và cost ledgers. |

## Trạm 2 — North Star và thẻ đèn

**North Star:** Weekly On-Time Important Task Completions. **Hiện tại: CHƯA ĐO** (không phải 0); mục tiêu vận hành là duy trì ít nhất median của bốn tuần học đủ điều kiện gần nhất sau khi lập được baseline dương. Đếm task học tập do sinh viên capture, đánh dấu important và xác nhận hoàn tất trước hoặc đúng hạn trong tuần học. Một task chỉ tính khi trạng thái chuyển từ chưa hoàn tất sang hoàn tất, được persist đúng user/task và không trùng. Công thức: đếm distinct user/task có transition hợp lệ và hoàn tất không muộn hơn deadline trong tuần. Không tính login, mở app, click, gợi ý hoặc AI plan delivery.
Cả bảy metric hỗ trợ hiện cũng **CHƯA ĐO**; các band/target khởi đầu được ghi ở Trạm 3.

| # | Tầng | Đèn | Định nghĩa: đếm và loại trừ | Công thức | Nhịp · người lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | Leading ⭐ | Eligible-week W4 retention | W4 là tuần học đủ điều kiện thứ tư sau completion hợp lệ đầu tiên. Tử số: user cohort có ≥1 completion hợp lệ trong W4. Mẫu số: user có actionable work và W4 được quan sát đầy đủ. Loại trừ tuần nghỉ/không có task actionable; báo riêng độ phủ eligibility để thấy selection. | Users có completion W4 ÷ users có actionable work và W4 quan sát đủ ×100%. | Hàng tuần theo cohort; Product Analytics; event store + academic calendar. | Paid retention/LTV; cần billing cohort để xác nhận. W1/W2/W4 trajectory là chẩn đoán, không thay màu W4. |
| 2 | Leading | 7-day first-value activation | Một lần cho mỗi user: task học hợp lệ đầu tiên được tạo rồi có completion đã persist trong ≤7×24 giờ. Không tính app open, suggestion, completion trước task, demo/internal, duplicate hoặc save failure. | Users có completion hợp lệ trong 7 ngày ÷ users có task hợp lệ đầu tiên và đủ 7 ngày follow-up ×100%. | Hàng tuần theo first-task cohort; Product; Day20 events, identity và timestamps. | W1 eligible retention; kiểm tra onboarding có dẫn tới giá trị đầu tiên không. |
| 3 | Leading | Upcoming-task readiness | Tại thời điểm capture, mẫu số là task học active có due date hợp lệ trong 7 ngày hoặc deadline thiếu/không phân tích được/không hợp lệ/đã quá hạn. Loại task có deadline hợp lệ >7 ngày. Tử số cần deadline hợp lệ trong 7 ngày và actionable context; task thiếu deadline ở mẫu số nhưng không đạt tử số. | Task đủ deadline ≤7 ngày và actionable context ÷ task đủ điều kiện trong mẫu số ×100%. | Hàng tuần; task capture cohort 7 ngày; Product; due date, context, completion state. | Activation/NSM; giả thuyết phát hiện thiếu thông tin trước khi lập kế hoạch. |
| 4 | Operating | Priority-plan adoption-to-value | Task priority đã được trình bày, có due date trong academic week và sau đó được xác nhận hoàn tất đúng hạn. Không tính click/view, suggestion chưa trình bày, hoàn tất trễ hoặc duplicate. | Task trình bày rồi hoàn tất đúng hạn ÷ task đủ điều kiện đã trình bày và theo dõi đến due date ×100%. | Hàng tuần sau khi due date qua; Product; task_priority_presented và academic_task_completed. | NSM; đo việc dùng kế hoạch, không đo AI quality. |
| 5 | Operating | All-in direct cost / paid-seat request | Tử số gồm provider-token, infrastructure, retry-token và direct QA cost gắn với mọi user-initiated paid-seat attempt, kể cả lỗi và retries. Mẫu số là số lần user khởi tạo request, gồm failed/restored-credit attempts; provider retry nội bộ là cost, không phải request mới. Tách free/promo; không gồm overhead phân bổ nếu không báo riêng. | Tổng direct cost quy thuộc paid requests ÷ số user-initiated paid requests. | Theo dõi tuần, đối soát tháng; Engineering + Finance; traces, invoices, credit ledger, QA time. | Cảnh báo cost trước GM. Mức $0.499 chỉ bảo vệ portfolio floor trong điều kiện nêu tại Trạm 3; dùng seat-month GM khi usage/revenue khác nhau. |
| 6 | Lagging | CAC / net-new paid student account | Tử số: paid media, phí channel/affiliate, creative/agency và acquisition labor phân bổ nhất quán. Mẫu số: account mới có charge subscription thành công và còn net consideration dương sau cửa sổ refund/chargeback 30 ngày; dedupe account, loại free/trial/internal/test và fully refunded; báo reactivation riêng. Attribution [TB]: first tagged qualified touch trong 30 ngày trước charge; cohort theo tháng charge đầu tiên. | Chi phí acquisition quy thuộc cohort ÷ account net-new đủ điều kiện. | Hàng tháng sau cửa sổ refund; GTM + Finance; attribution và billing/refund ledger. | Scoreboard; so với trần payback 3 tháng. Usage credits là COGS, không trừ vào CAC spend. |
| 7 | Lagging | Paid-seat gross margin | Net recognized subscription revenue sau discounts/refunds/chargebacks trừ direct COGS thực tế quy thuộc paid seat (provider, infrastructure, retry, QA, delivered promo credits); không trừ overhead. | (Net recognized revenue − attributable direct COGS) ÷ net recognized revenue ×100%. | Đóng sổ tháng; Finance; billing/refunds và invoices. | Xác nhận viability sau usage; không phải net profit. |

## Trạm 3 — Ngưỡng

Không dùng [BM]: chưa có benchmark trực tiếp phù hợp. [TB] là policy hypothesis, không phải baseline hay kết quả empirically validated. Thiếu denominator, sample guard hoặc follow-up thì CHƯA ĐO. Các khoảng dùng biên đóng/mở đúng như biểu thức dưới đây; không làm tròn ranh giới CAC trước khi phân loại.

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn · kỳ · sample guard | Lý do |
|---|---|---|---|---|---|---|
| 0 | North Star: Weekly On-Time Important Task Completions | ≥ median của 4 tuần học đủ điều kiện trước | ≥80% và <100% median | <80% median | [TB]; cohort cố định, ≥10 sinh viên có actionable work/tuần; 4 tuần baseline dương; event QA ≥95%. Median 0 hoặc eligibility mix đổi = CHƯA ĐO/reset. | So với chính cohort để tránh đặt absolute task target khi workload chưa biết. |
| 1 | Eligible-week W4 retention | ≥30% | ≥15% và <30% | <15% | [TB]; cohort first-completion đã mature; n≥10 có actionable work và W4 đủ quan sát. | 30% là proposed gate; 15% là provisional action boundary, không phải benchmark. |
| 2 | 7-day activation | ≥40% | ≥25% và <40% | <25% | [TB]; first-task cohort, đủ follow-up 7 ngày; n≥10. Một cohort chỉ là tín hiệu thăm dò; so ≥2 cohort trước đổi sản phẩm. | Tìm onboarding friction; boundary cần hiệu chỉnh bằng dữ liệu thực. D60 dùng cùng n=10 như feasibility guard. |
| 3 | Upcoming-task readiness | ≥80% | ≥60% và <80% | <60% | [TB]; ≥30 task eligible/tuần gồm thiếu deadline, trong 2 tuần; 0 eligible = CHƯA ĐO. | Bao gồm deadline thiếu để giữ tín hiệu về thông tin không đầy đủ. |
| 4 | Priority-plan adoption-to-value | ≥50% | ≥25% và <50% | <25% | [TB]; ≥20 task trình bày/tuần, đã theo dõi đến due date; 2 tuần hoạt động. | Phân biệt suggestion được xem với value đã xác nhận; mức là calibration hypothesis. |
| 5 | All-in direct cost / paid-seat request | ≤$0.3992 | >$0.3992 và ≤$0.4990 | >$0.4990 | [MH]+[TB]; USD/user-initiated request; ≥30 attempts/tháng và cost allocation đầy đủ. Gồm failures/restored credits; retry nội bộ tính vào cost, không tính mẫu số. | $0.3992 là buffer 20%. $0.499×4=$1.996 giữ 60% GM tại $4.99 net revenue và ≤4 requests mỗi seat; lệch usage/cost/revenue cần seat-month GM và aggregate COGS check. |
| 6 | CAC / net-new paid student account | ≤$9.349056 (hiển thị $9.3491) | >$9.349056 và ≤$11.68632 | >$11.68632 | [MH]+[TB]; cohort tháng first paid; ≥10 account mới; cửa sổ refund/chargeback 30 ngày đóng; attribution đầy đủ. CAC thực tế CHƯA ĐO. | $11.68632 là trần payback 3 tháng theo $4.99 và GM mô hình; tính lại từ gross contribution quan sát nếu khác. Green có buffer 20%. |
| 7 | Paid-seat gross margin | ≥70% | ≥60% và <70% | <60% | [MH]+[TB]; đóng sổ tháng; ≥10 paid seat-month đầy đủ invoice và policy refund. | 60% là floor mô hình; 70% là buffer vận hành 10 điểm %, không phải benchmark. |

**[MH] 1 — Trần CAC theo payback 3 tháng**

ARPU mô hình = $4.99/paid seat-month; direct cost = 4 × $0.27364 = $1.09456/seat-month.
GM = ($4.99 − $1.09456)/$4.99 = 0.7806492986 = 78.06492986%.
Gross contribution tháng = $4.99 − $1.09456 = $3.89544.
Trần CAC = $3.89544 × 3 = $11.68632/account. Green boundary = $11.68632 × 80% = $9.349056.
[MH] trace: Day22 workbook 2_Pricing B19:B21; 4_Channel_Fit B5:B9. CAC chưa quan sát; dùng net revenue và direct COGS thực khi có cohort.

**[MH] 2 — Trần direct cost/request theo floor GM 60%**

Net subscription revenue mô hình = $4.99; COGS tối đa = $4.99 × (1 − 60%) = $1.996/seat-month.
Với 4 requests/seat-month, all-in ceiling = $1.996 ÷ 4 = $0.499/request.
Non-infrastructure = API $0.008 + retry tokens $0.00064 + QA $0.015 = $0.02364; infrastructure-only ceiling = $0.499 − $0.02364 = $0.47536.
Modeled cost = $0.27364/request = $0.25 infra + $0.02364 other direct cost (estimate, not invoice). Green buffer = $0.499 × 80% = $0.3992.
$0.499/request protects 60% GM only at $4.99 net revenue and ≤4 user-initiated requests for every seat. Failed/restored-credit attempts remain in request count; internal retries add cost. Above four or with unequal usage/cost/revenue, allocate actual COGS and calculate seat-month GM; aggregate COGS must also be ≤40% of aggregate net recognized revenue.
[MH] trace: Day22 workbook 1_Cost_Job B15:B22, B41, B46:B56, B62:B67; 2_Pricing B29:B35, B53.

## Trạm 4 — 5 luật quyết định

1. **⏹ STOP — NẾU** W4 retention <15% **TRONG** hai cohort mature liên tiếp **VÀ** mỗi cohort có ≥10 sinh viên đủ điều kiện với W4 quan sát đầy đủ **THÌ** trong tuần kế tiếp đóng băng paid-acquisition launch và chạy 5 workflow sessions có điều phối, ghi lại blocker từ tạo task đến completion **KHÔNG THÌ** không mua ads để bù churn hoặc tính tuần nghỉ/không có task là churn.
2. **⏹ STOP — NẾU** 7-day activation <25% **TRONG** hai first-task cohort liên tiếp **VÀ** mỗi cohort có ≥10 user, đủ follow-up 7 ngày và event QA ≥95% **THÌ** tạm dừng paywall work, sửa onboarding step có failure cao nhất trong logs và chạy 10 task-flow sessions tuần sau **KHÔNG THÌ** không giảm giá để che việc chưa đạt first value.
3. **⏹ STOP — NẾU** all-in direct cost >$0.499/request **HOẶC** có seat >4 requests/tháng **HOẶC** net recognized revenue ≠$4.99 **TRONG** tháng đã đóng sổ **VÀ** có ≥30 attempts, đối soát cost và seat-month counts đầy đủ **THÌ** đóng băng mở rộng allowance; nếu cost vượt trần thì cap test ở 4 requests/seat-month; nếu usage hoặc revenue khác giả định thì phân bổ COGS và tính seat GM cùng aggregate COGS/net revenue **KHÔNG THÌ** không coi $0.499/request tự nó chứng minh 60% GM hoặc báo cost mô hình là actual.
4. **NẾU** upcoming-task readiness <60% **TRONG** 2 tuần học **VÀ** mỗi tuần có ≥30 task đủ điều kiện gồm cả missing/invalid deadline và QA ≥95% **THÌ** sprint kế tiếp thêm validation bắt buộc cho due date/actionable context và đo lại cohort sau **KHÔNG THÌ** không loại task thiếu deadline khỏi mẫu số hoặc tự suy deadline/context bằng AI rồi gọi task là sẵn sàng.
5. **⏹ STOP — NẾU** CAC/net-new account vượt mức thấp hơn của $11.68632 và 3× gross contribution tháng/account quan sát **TRONG** một acquisition cohort đã đóng **VÀ** có ≥10 account mới sau cửa sổ refund/chargeback 30 ngày, spend/attribution/refund records đầy đủ **THÌ** đóng băng paid launch/expansion và chạy một tagged organic/referral cohort tối đa 10 người tuần sau; chưa có giao dịch thì giữ NO-GO **KHÔNG THÌ** không scale từ stretch scenario hoặc giá đối thủ.
