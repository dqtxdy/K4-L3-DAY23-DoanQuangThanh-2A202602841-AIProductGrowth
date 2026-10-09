# UniPilot — Operating Dashboard

**Mô hình:** B2C tự phục vụ đề xuất · **Cập nhật:** 09/10/2026 · **Đoàn Quang Thanh · 2A202602841**

**NORTH STAR — Weekly On-Time Important Task Completions:** task học tập do sinh viên capture và đánh dấu important, rồi xác nhận hoàn tất trước hoặc đúng hạn trong academic week; chỉ tính transition chưa hoàn tất → hoàn tất đã persist, đúng user/task, loại duplicate, save failure và AI plan delivery. **Hiện tại: CHƯA ĐO.** Mục tiêu sau baseline: ≥ median của 4 tuần học đủ điều kiện gần nhất.

Ngưỡng North Star [TB]: sau 4 tuần baseline dương, cohort cố định với ≥10 sinh viên có actionable work mỗi tuần và QA ≥95%; xanh ≥ median, vàng ≥80% và <100% median, đỏ <80%. Median 0 hoặc eligibility mix đổi thì CHƯA ĐO và lập lại baseline.

### Đèn báo sớm (Leading)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Eligible-week W4 retention | CHƯA ĐO | ≥30% / 15%–<30% / <15% | [TB] cohort mature, n≥10, W4 đủ quan sát | Paid retention/LTV; W2→W4 chỉ chẩn đoán |
| 7-day first-value activation | CHƯA ĐO | ≥40% / 25%–<40% / <25% | [TB] first-task cohort, n≥10, đủ 7 ngày | W1 eligible retention |
| Upcoming-task readiness | CHƯA ĐO | ≥80% / 60%–<80% / <60% | [TB] ≥30 task/tuần gồm deadline thiếu, 2 tuần | Activation/NSM; giả thuyết về task information |

### Đèn vận hành (Operating)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Priority-plan adoption-to-value | CHƯA ĐO | ≥50% / 25%–<50% / <25% | [TB] ≥20 task/tuần đã theo dõi đến due date, 2 tuần | NSM; không phải AI quality |
| All-in direct cost / paid-seat request | CHƯA ĐO | ≤$0.3992 / >$0.3992–$0.4990 / >$0.4990 | [MH]+[TB] ≥30 attempts, cost reconciliation; failures/restored credits included, retries are cost | Paid-seat GM; $0.499×4 protects 60% floor only at $4.99 net revenue and ≤4 requests/seat |

### Đèn kết quả (Lagging)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| CAC / net-new paid student account | CHƯA ĐO | ≤$9.349056 / >$9.349056–$11.68632 / >$11.68632 | [MH]+[TB]; ≥10 accounts, 30-day refund window and attribution complete |
| Paid-seat gross margin | CHƯA ĐO | ≥70% / 60%–<70% / <60% | [MH]+[TB]; 60% model floor; ≥10 complete paid seat-months |

Missing denominator, immature cohort, or incomplete attribution/cost allocation = CHƯA ĐO, never zero or red. Cost/request alone does not establish each seat’s GM when requests exceed four or seat usage, costs, or net revenue vary; allocate seat-month COGS and verify aggregate COGS ≤40% of aggregate net recognized revenue.

### 5 luật quyết định (⏹ = luật dừng)

1. **⏹ STOP — NẾU** W4 retention <15% **TRONG** hai cohort mature liên tiếp **VÀ** mỗi cohort có ≥10 sinh viên đủ điều kiện với W4 quan sát đầy đủ **THÌ** trong tuần kế tiếp đóng băng paid-acquisition launch và chạy 5 workflow sessions có điều phối, ghi lại blocker từ tạo task đến completion **KHÔNG THÌ** không mua ads để bù churn hoặc tính tuần nghỉ/không có task là churn.
2. **⏹ STOP — NẾU** 7-day activation <25% **TRONG** hai first-task cohort liên tiếp **VÀ** mỗi cohort có ≥10 user, đủ follow-up 7 ngày và event QA ≥95% **THÌ** tạm dừng paywall work, sửa onboarding step có failure cao nhất trong logs và chạy 10 task-flow sessions tuần sau **KHÔNG THÌ** không giảm giá để che việc chưa đạt first value.
3. **⏹ STOP — NẾU** all-in direct cost >$0.499/request **HOẶC** có seat >4 requests/tháng **HOẶC** net recognized revenue ≠$4.99 **TRONG** tháng đã đóng sổ **VÀ** có ≥30 attempts, đối soát cost và seat-month counts đầy đủ **THÌ** đóng băng mở rộng allowance; nếu cost vượt trần thì cap test ở 4 requests/seat-month; nếu usage hoặc revenue khác giả định thì phân bổ COGS và tính seat GM cùng aggregate COGS/net revenue **KHÔNG THÌ** không coi $0.499/request tự nó chứng minh 60% GM hoặc báo cost mô hình là actual.
4. **NẾU** upcoming-task readiness <60% **TRONG** 2 tuần học **VÀ** mỗi tuần có ≥30 task đủ điều kiện gồm cả missing/invalid deadline và QA ≥95% **THÌ** sprint kế tiếp thêm validation bắt buộc cho due date/actionable context và đo lại cohort sau **KHÔNG THÌ** không loại task thiếu deadline khỏi mẫu số hoặc tự suy deadline/context bằng AI rồi gọi task là sẵn sàng.
5. **⏹ STOP — NẾU** CAC/net-new account vượt mức thấp hơn của $11.68632 và 3× gross contribution tháng/account quan sát **TRONG** một acquisition cohort đã đóng **VÀ** có ≥10 account mới sau cửa sổ refund/chargeback 30 ngày, spend/attribution/refund records đầy đủ **THÌ** đóng băng paid launch/expansion và chạy một tagged organic/referral cohort tối đa 10 người tuần sau; chưa có giao dịch thì giữ NO-GO **KHÔNG THÌ** không scale từ stretch scenario hoặc giá đối thủ.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 08/11/2026 | Pain-validation interview yield | ≥8/12 người được phỏng vấn độc lập nêu vấn đề deadline chồng lấn hằng tuần và cho thấy workaround hiện tại | 12 ghi chú phỏng vấn có consent, đã khử định danh; coding rubric và timestamp. Đây là cổng học, không phải doanh thu. | <8/12: sửa giả thuyết problem/ICP; không tuyên bố demand đã xác thực. |
| 60 · 08/12/2026 | 7-day first-value activation | ≥40%; n≥10, đủ follow-up 7 ngày, QA ≥95% | Event export có timestamp, phiên bản định nghĩa, cohort query và QA log. | Đạt: bắt đầu retention cohort giới hạn; trượt/thiếu mẫu: FIX workflow/instrumentation, không paid scale. |
| 90 · 07/01/2027 | Eligible academic W4 retention | ≥30% trong cohort first-completion đã mature; n≥10 sinh viên đủ điều kiện với actionable work và W4 đủ quan sát | Cohort table đã khử định danh, eligibility audit, event QA và follow-up dates. | Đạt: sau pricing approval, mở một tagged checkout test tối đa 50 qualified visits. W4<30% khi cohort mature: một FIX trước 14/01/2027 và tuyển cohort khác trước 21/01/2027; thiếu mẫu/follow-up: CHƯA ĐO, sửa đo lường, không KILL. Paid acquisition vẫn NO-GO. |

**KILL CRITERIA — D135 (21/02/2027), không phải D90:** D90 W4 <30% dẫn tới một FIX trước 14/01/2027 và cohort riêng sau sửa trước 21/01/2027. Ngừng định vị B2C hiện tại chỉ khi post-fix W4 <10% trong cohort có ≥10 sinh viên đủ điều kiện, bốn tuần học đủ quan sát và event QA ≥95%. W4 từ 10% đến <30%: PIVOT; W4 ≥30%: cho phép tagged checkout test tối đa 50 qualified visits sau pricing approval. Nếu cohort chưa mature vào D135, kết quả CHƯA ĐO và hoãn KILL.

**CHƯA ĐO ĐƯỢC:** Chưa có verified users, interviews, paid customers, CAC, billing/refund, retention, task outcomes, production LLM/provider/hosting invoices, free-tier cost, p95, WTP hay runway. Day22 100/100 là kết quả validator offline trên dữ liệu tổng hợp, không phải model/user success. Cần event, billing, attribution, provider-cost và invoice records; baseline product đầu tiên có thể bắt đầu sau review dự kiến 15/11/2026, còn CAC/retention cần checkout/cohort đủ trưởng thành. Missing is not zero.
