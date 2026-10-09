# Worksheet — UniPilot

**Họ tên:** Đoàn Quang Thanh · **MSSV:** 2A202602841 · **Ngày:** 09/10/2026 (ICT)  
**Trạng thái:** Bản nháp có AI hỗ trợ; sinh viên phải tự xác minh, viết lại và sở hữu quyết định cuối. Không có benchmark ngoài nào được dùng để đặt các ngưỡng dưới đây.

## Ma trận nguồn sự thật → bằng chứng

| Câu hỏi | Nguồn chính | Dùng tại | Giới hạn |
|---|---|---|---|
| Ai dùng/trả tiền và North Star? | `UniPilot_Day23_reference_pack/SOURCE_MAP.md`; Day20 Metrics Pack HTML, sections North Star, Activation, Retention, Tracking | CP1–2, dashboard | Product semantics/contract, không có event warehouse hay số usage |
| Sự kiện, điều kiện hợp lệ, dedupe | Day20 HTML, Tracking nhanh / Acceptance criteria AC1–AC4 | Metric cards 1–4 | Tên và schema được định nghĩa; không chứng minh tracking đang chạy |
| Giá, cost, GM, CAC | Day22 workbook `1_Cost_Job!B9:B11,B15:B22,B41,B46:B56,B62:B67`; `2_Pricing!B19:B21,B29:B35,B53`; `4_Channel_Fit!B5:B9` | CP3, MH | Mô hình/ước tính; không phải hóa đơn hay giao dịch thật |
| Bằng chứng thử nghiệm | Day22 `evidence/pilot_status.md`, `eval_results.md`; `reports/math_checks.md` | Honesty, gates | Chỉ synthetic deterministic baseline; chưa có LLM/user pilot |
| Quy định bài | `RUBRIC.md`, `SUBMISSION.md`, `RULES.md`, `CHECKPOINTS.md`, `HANDBOOK.md` §2 và §3.1 | Tất cả CP | Các quyết định cuối phải do sinh viên tự viết lại |

Workbook có đủ bảy sheet: `0_README`, `1_Cost_Job`, `2_Pricing`, `3_Value_Metric`, `4_Channel_Fit`, `5_90Day_Plan`, `6_Benchmarks`. Packet nằm tại `UniPilot_Day23_reference_pack/` thay vì `day23_references/`; đã dùng đường dẫn thực tế. Tập tham chiếu được cung cấp không được đưa vào các lệnh stage/commit.

## Trạm 1 — Loại mô hình và measurability

**Câu chốt:** UniPilot hiện là mô hình B2C tự phục vụ/PLG được đề xuất vì sinh viên dự kiến là cả người dùng lẫn người trả tiền thuê bao; chưa có bằng chứng trường học/đối tác mua hoặc phân phối, nên không phải B2B/B2B2C đang vận hành.

Dấu ✅/🔧/❌ dưới đây mô tả khả năng đo theo tình trạng dữ liệu hiện có: ✅ có nguồn dữ liệu quan sát; 🔧 có thể đo trong 2 tuần sau khi triển khai instrumentation/thu thập; ❌ chưa có cách hoặc dữ liệu khả thi hiện thời. Một định nghĩa event không đồng nghĩa có bản ghi thực tế.

| Đèn B2C §3.1 | Trạng thái | Nguồn/số hiện có | Dữ liệu thực tế | Owner đề xuất · hành động tiếp |
|---|---|---|---|---|
| Retention curve có phẳng không (D1/D7/D30/D60; adapted sang tuần học đủ điều kiện) | 🔧 | Day20 `academic_task_completed`, `user_id`, timestamp + trạng thái tuần/task; chưa có cohort logger | Chưa có user/cohort/retention | Product + Engineering: thêm event store, eligibility và cohort query; bắt đầu sau privacy review, mục tiêu 15/11/2026 |
| Activation rate (task tạo hợp lệ → completion đầu trong 7 ngày) | 🔧 | Day20 `academic_task_created`, `academic_task_completed`; 7 ngày là hypothesis | Chưa có event stream hay activation baseline | Product Analytics: persist hai event, timezone và identity; tính trong 2 tuần kể từ bật logging |
| p95 cost/user/month ÷ ARPU | ❌ | Workbook có 4-credit cost estimate/seat; không có phân phối chi phí theo user, free tier hoặc ARPU quan sát | Chưa có provider, invoice, paid seats, user-level spend | Engineering/Finance: trước paid pilot instrument user-level cost, invoice, free/paid split; không hiển thị p95 giả |
| Trial → paid | 🔧 | Workbook `4_Channel_Fit` funnel scenarios là giả định; payment/refund events chưa có | 0 verified paying customers; không có thử nghiệm checkout | GTM/Finance: sau approved checkout, instrument qualified visit→paid/refund and cohort denominator |
| Retention M12 | ❌ | Day20 cho retention theo academic week; không có 12 tháng follow-up, billing/renewal log | Chưa có paid cohort; chưa có follow-up | Finance/Product: không thể có số M12 trong 2 tuần; cần instrument subscription renewal và chỉ báo cáo sau 12 tháng eligible follow-up |
| Chi phí free tier ÷ tổng COGS | ❌ | Không có free-tier usage allowance hoặc cost logs; Day22 giả định 4 credits của paid seat | Không có free-tier traffic/cost hoặc tổng COGS thật | Product + Finance: quyết định free-tier trước launch, tag plan, meter cost; nếu vẫn chưa có free tier thì metric N/A, không tự đặt bằng 0 |
| Refund rate | 🔧 | Không có giao dịch; cần payment/refund ledger | Chưa có giao dịch/hoàn tiền | GTM/Finance: thêm transaction/refund IDs, gross paid denominator; chỉ đọc sau checkout hợp lệ |
| LTV/CAC · CAC payback · GM (bundle Lagging trong handbook) | 🔧 | Workbook chỉ có modeled GM/payback/CAC budget; thiếu revenue, spend, cost invoices/cohort survival | Chưa có actuals; không có runway xác minh | Finance: gắn spend + net paid IDs + billing, infra/API invoices; không gọi model output là actual |

## Trạm 2 — North Star và thẻ metric

**North Star:** **Weekly On-Time Important Task Completions**. Hiện tại: **CHƯA ĐO** (không phải 0); mục tiêu: chưa phê duyệt. Đơn vị là task học tập có ý nghĩa được student capture và xác nhận complete trước/đúng deadline trong academic week. “Important” phải do student đánh dấu high priority hoặc rule được định nghĩa. Completion chỉ hợp lệ khi task tồn tại, chuyển non-completed→completed, đúng `user_id`/`task_id`, persistence thành công và event không trùng. Không tính login/open, create, suggestion click, AI plan delivery, save failure, duplicate hay task đã completed từ trước.

Bảy metric cards dưới đây bổ sung cho North Star, tổng cộng 8 metrics trên dashboard. “Proposed instrumentation” không có nghĩa đã triển khai. Nếu denominator bằng 0/thiếu hoặc cohort chưa đủ tuổi, kết quả là **NOT MEASURABLE**, không chia 0 và không gán đỏ.

| # · Tier | Metric; định nghĩa, numerator/denominator, unit | Events include / exclude | Cohort; window; cadence; owner; source | Báo trước/ý nghĩa; rule |
|---|---|---|---|---|
| 1 · Leading ⭐ | **Eligible-week value retention / trajectory.** Numerator: users trong cohort entry có ≥1 valid `academic_task_completed` ở eligible return academic week. Denominator: cohort users đủ điều kiện quan sát tuần đó. Unit: %. Đọc W1/W2/W4 và đường cong nhiều cohort; không gộp tuần nghỉ. | Include completion hợp lệ; exclude internal/demo, semester break và tuần không có task actionable; taskless user-week không phải churn. | Cohort user-level theo tuần first valid completion; tuần học theo calendar; weekly; Product Analytics; event store + academic calendar/actionable status. | Bật đèn B2C sớm, dự báo paid retention/LTV (cần billing cohort để xác nhận). Rule 1. Curve chưa đủ W4 → NOT MEASURABLE. |
| 2 · Leading | **7-day first-value activation.** Numerator: users tạo task hợp lệ đầu tiên rồi có valid completion trong ≤7×24h. Denominator: users có first valid `academic_task_created` đủ 7 ngày follow-up. Unit: %. | Include first creation và persisted completion; exclude app opens, recommendation, completion trước task creation, demo/internal, duplicate và save fail. | Signup không cần làm cohort; cohort là first task create; 7 ngày; weekly; Product; Day20 events + timestamps/identity. | Dự báo W1 eligible retention; kiểm tra onboarding/workflow có đưa tới value không. Rule 2. 7 ngày là hypothesis Day20. |
| 3 · Leading | **Upcoming-task readiness rate.** Numerator: captured tasks due trong 7 ngày tới có valid due date + actionable context. Denominator: captured tasks đến hạn trong cùng cửa sổ. Unit: %. | Include `academic_task_created` tasks và context/deadline hợp lệ; exclude ngoài UniPilot, inferred tasks, invalid dates; không gọi readiness là completion. | Task-level, active teaching week; rolling 7-day; weekly; Product; event + task state. | Giả thuyết báo trước NSM/activation do task actionable rõ hơn; tương quan chưa chứng minh causality. Rule 3. |
| 4 · Operating | **Priority-plan adoption-to-value.** Numerator: distinct presented priority tasks later validly completed on time trong academic week. Denominator: distinct eligible tasks được `task_priority_presented` trong tuần và có due date trong window. Unit: %. | Include presented event, matching task completion + due date; exclude click/view alone, generated but not presented suggestion, late/duplicate completion. | Student-task presented cohort; academic week; weekly; Product; `task_priority_presented`, `academic_task_completed`, task history. | Kiểm tra plan có được dùng để hoàn tất việc đúng hạn; báo trước NSM, không phải AI quality. Rule 4. |
| 5 · Operating | **All-in direct cost / generation credit.** Numerator: attributable provider token + infra + retry-token + QA direct costs in period. Denominator: customer generation credits requested in period (also show cost per credit attempted; don't divide by successful plan). Unit: USD/credit. | Include all observed paid/free attribution separately, retry and failed requests; exclude allocated overhead unless separately labeled; no use modeled $0.27364 as observation. | By plan type and month; monthly close/weekly early alert; Engineering + Finance; provider traces, invoices, credit ledger, QA time. | Early warning for seat GM <60%; four-credit payer ceiling [MH]. Rule 5. p95/user requires enough distribution, not current data. |
| 6 · Lagging | **Net CAC / net new paid seat.** Numerator: attributable acquisition spend in acquisition cohort, net of credits? (include channel spend; separately disclose discounts/refunds), denominator: net new paid seats after refunds in same attributed cohort. Unit: USD/seat. | Exclude organic as paid spend only if tagged and report channel; exclude hypothetical funnel cases; remove refunded/duplicate seats per attribution policy. | First paid cohort by acquisition month; monthly, mature after refund window; GTM + Finance; tagged source and billing/refund ledger. | Scoreboard, not current leading signal; compare to 3-month modeled payback cap. Rule 5. If denominator 0, NOT MEASURABLE. |
| 7 · Lagging | **Paid-seat gross margin.** Numerator: recognized subscription revenue − attributable direct COGS (tokens, infra, retries, QA, refunds/credits under approved policy); denominator: recognized subscription revenue. Unit: %. | Separate free users, overhead, seat subscription from plan delivery; no modeled GM as actual; no counting task completion as AI revenue. | Paid seat-month cohort; monthly close; Finance; billing, refunds, provider/infrastructure invoices. | Confirms cost model and 60% floor after usage occurs; not net profitability (overhead excluded). Rule 5. |

### Ranh giới value và AI

Day20 `academic_task_completed` là kết quả học tập được student xác nhận; Day22 generation credit là request tạo weekly priority plan. Plan được schema-validate/persist/present không chứng minh usefulness hoặc task completion. Day22 offline deterministic synthetic validator 100/100 không phải LLM success, retention hay customer outcome. Tỷ lệ R=85% chỉ là estimate và không được dùng làm current metric.

## Trạm 3 — Ngưỡng đề xuất

Không dùng benchmark [BM]; không có benchmark public trực tiếp phù hợp cho các metric product này. Ngưỡng [TB] là **policy hypothesis để khởi động đo**, không phải historical baseline. Cửa sổ tối thiểu và mẫu hợp lệ là điều kiện trước khi tô màu; chưa đủ dữ liệu = **CHƯA ĐO**.

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn · kỳ/biên/sufficiency | Lý do |
|---|---|---:|---:|---:|---|---|
| 1 | Eligible-week retention W4 + curve | W4 ≥30% và W2→W4 giảm ≤5 pp qua ≥2 mature cohorts | W4 15%–<30%, hoặc W2→W4 giảm >5 pp | W4 <15% qua ≥2 cohorts | [TB], W4 eligible academic weeks, cohort n≥10 each, follow-up complete; otherwise CHƯA ĐO. 30% W4 is Day22 proposed scale gate, not benchmark; 15% lower action boundary is policy proposal. | Tách target scale của model khỏi cảnh báo dưới mức chấp nhận; trend phải quan sát đủ thời gian và eligible weeks. |
| 2 | 7-day activation | ≥40% | ≥25% và <40% | <25% | [TB], first-task cohorts, 7-day window; n≥10 users with full window. Initial calibration 09/10–08/11; rebase only after ≥2 cohorts. | Band tạm để tìm onboarding friction, không là market benchmark. |
| 3 | Upcoming-task readiness | ≥80% | ≥60% và <80% | <60% | [TB], weekly task-level denominator ≥30 eligible captured tasks; two academic weeks; no tasks = CHƯA ĐO. | Policy hypothesis về đủ thông tin để hành động; phải kiểm tra theo deadline/context quality. |
| 4 | Priority-plan adoption-to-value | ≥50% | ≥25% và <50% | <25% | [TB], weekly denominator ≥20 eligible presented tasks, followed through due date; two active weeks. | Distinguishes delivered/viewed suggestions from student-confirmed value; threshold is calibration hypothesis. |
| 5 | All-in direct cost / credit | ≤$0.3992 | >$0.3992 to ≤$0.4990 | >$0.4990 | [MH], USD per credit, monthly invoice/credit reconciliation; n≥30 credits and complete cost allocation. $0.3992 is 20% headroom beneath 60%-GM ceiling; policy buffer, not required by math. | Green requires cost headroom; red crosses model-derived 60% GM ceiling at 4 credits. |
| 6 | Net CAC / net new paid seat | ≤$9.3491 | >$9.3491 to ≤$11.68632 | >$11.68632 | [MH], acquisition cohort/month; ≥10 net paid seats and complete spend/refund attribution. Actual CAC currently CHƯA ĐO. Green = 20% buffer below modeled limit. | Red violates 3-month payback model; yellow uses buffer to prompt caution before breach. |
| 7 | Paid-seat gross margin | ≥70% | ≥60% and <70% | <60% | [MH] floor 60%; 70% upper band is policy alert boundary, not benchmark. Monthly; ≥10 complete paid seat-months with invoices/refund policy; otherwise CHƯA ĐO. | Green has 10pp buffer over chosen minimum; yellow remains viable under the model but little room for cost error. |

**Quy ước biên:** tất cả giá trị đo hợp lệ nằm trong đúng một band; ví dụ cost = $0.4990 còn yellow, >$0.4990 red; CAC = $11.68632 còn yellow, > limit red; GM = 60% yellow, <60% red. Tỷ lệ có biên tương tự như trong bảng. Missing/zero denominator, sample guard fail hoặc observation chưa mature không được tô màu. Dữ liệu tuần phải dùng timezone/academic calendar thống nhất.

### Phụ lục [MH] — phép tính

**[MH] 1 — CAC tối đa (theo Day22 workbook)**

```text
ARPU proposed = $4.99 / paid seat-month
Direct cost estimate = 4 credits × $0.27364 = $1.09456 / seat-month
Modeled seat GM = ($4.99 − $1.09456) / $4.99 = 0.7806492986 = 78.06492986%
Allowed payback = 3 months (policy hypothesis)
Monthly gross contribution = $4.99 × 0.7806492986 = $3.89544
Maximum CAC = $3.89544 × 3 = $11.68632 / net new paid seat
Green ≤ $9.34906 (20% below cap) · Yellow >$9.34906 to $11.68632 · Red >$11.68632
```

CAC payback metric is CHƯA ĐO; neither $28.57 base funnel CAC nor $7.50 stretch is actual. Workbook trace: `2_Pricing!B19:B21`; `4_Channel_Fit!B5:B9`.

**[MH] 2 — Direct cost ceiling per generation credit**

```text
Subscription = $4.99 / month; allowance = 4 credits (policy hypothesis)
Chosen minimum GM = 60%; maximum direct cost/seat-month = $4.99 × (1−0.60) = $1.996
Maximum ALL-IN direct cost/credit = $1.996 / 4 = $0.499
Non-infrastructure = API $0.008 + expected retry tokens $0.00064 + QA $0.015 = $0.02364
Maximum INFRASTRUCTURE-only cost/credit = $0.499 − $0.02364 = $0.47536
Modeled current total cost = $0.27364/credit; modeled infra = $0.25, not invoice telemetry
Green total cost ≤$0.3992 (20% buffer) · Yellow >$0.3992 to $0.499 · Red >$0.499
Infrastructure separate hard ceiling = $0.47536; never substitute it for all-in $0.499
```

Workbook trace: `1_Cost_Job!B15:B22,B41,B46:B56,B62:B67`; `2_Pricing!B29:B35,B53`. The $0.3992 green/yellow split is discretionary 20% buffer. It is not mathematically necessary; the $0.499 all-in red line is derived from 60% GM.

**[MH] 3 — paid-seat GM bands**

```text
GM = (recognized subscription revenue − attributable direct cost) / recognized revenue
Minimum GM floor = 60% (Day22 policy assumption)
Green ≥70% is 10 pp operating buffer; Yellow [60%,70%); Red <60% violates floor
```

Do not confuse gross margin with net profit; workbook excludes operating overhead. Proposed $4.99 pricing/four-credit allowance and failure-credit treatment require separate approval.

## Trạm 4 — 5 luật quyết định (AI-assisted drafts; rewrite required)

Sample minima below are operational guard proposals; if unmet, pause classification and collect data. Conflicting cohorts are reviewed separately; a bad week in one segment is not averaged away. Technical/instrumentation failure is diagnosed before product failure. Each `THÌ` can be completed within the next week.

1. **NẾU** eligible-week retention W4 **<15%** **TRONG** hai cohort liên tiếp đã đủ W4 **VÀ** mỗi cohort có ≥10 eligible users, teaching weeks only **THÌ** trong 7 ngày đóng băng mọi kế hoạch paid-acquisition launch và chạy 5 moderated workflow sessions tập trung first-task→completion; ghi lỗi workflow và bản sửa vào issue log **KHÔNG THÌ** không chi tiền acquisition để bù churn hoặc gộp tuần taskless vào churn. **⏹ STOP** (no live campaign assumed; freeze launch).
2. **NẾU** activation <25% **TRONG** hai weekly cohorts **VÀ** mỗi cohort có ≥10 user đủ 7 ngày follow-up và event QA pass ≥95% trên sampled records **THÌ** tạm dừng phát triển paywall, sửa một onboarding blocker cụ thể và chạy lại 10 consented task-flow sessions tuần sau **KHÔNG THÌ** không giảm giá $4.99 để che việc chưa tới first value. **⏹ STOP** paywall work.
3. **NẾU** all-in direct cost >$0.499/credit **TRONG** một tháng invoice-close **VÀ** có ≥30 credit requests được map đủ provider/infrastructure/QA cost **THÌ** khóa mọi tăng allowance, đặt internal cap tối đa 4 credits trong test build và loại chi phí chưa phân bổ trước lần test kế tiếp **KHÔNG THÌ** không báo cáo modeled $0.27364 là cost đã đo hoặc tăng quota theo cảm tính. **⏹ STOP** allowance expansion.
4. **NẾU** upcoming-task readiness <60% **TRONG** hai academic weeks **VÀ** có ≥30 due tasks mỗi tuần, ≥95% completeness event QA **THÌ** trong sprint tới thêm validation bắt buộc cho due date/actionable context và đo lại readiness trên cohort kế tiếp **KHÔNG THÌ** không tự suy deadline/context bị thiếu bằng AI rồi gọi đó là task sẵn sàng.
5. **NẾU** net CAC >$11.68632/paid seat **TRONG** một acquisition cohort đóng sổ **VÀ** có ≥10 net paid seats cùng spend/refund attribution đầy đủ **THÌ** đóng băng paid acquisition launch/expansion và chạy một tagged organic/referral-only cohort tuần kế tiếp; nếu chưa có giao dịch thì giữ NO-GO **KHÔNG THÌ** không scale paid acquisition chỉ dựa vào $7.50 stretch scenario hay competitor price. **⏹ STOP** paid scaling.

**Red-team:** Mẫu nhỏ được bảo vệ bằng ngưỡng n và hai cohort cho metric biến động; cohort chưa mature/taskless tuần bị loại. Các metric chi phí/CAC có thể bị false positive do phân bổ thiếu hoặc refund; rule bắt reconciliation/QA trước quyết định. False negative vẫn có thể xảy ra ở cohort rất nhỏ: quyết định chỉ là freeze/diagnose, không tự động kill. Nếu event QA <95%, xử lý lỗi tracking trong 7 ngày và metric chuyển CHƯA ĐO thay vì kết luận sản phẩm kém. Paid channel chưa chạy; các rule “freeze” là cấm launch/mở rộng. Khi leading signals tốt nhưng paid GM xấu, chặn scale theo cost/CAC rule; không lấy task completions làm bằng chứng doanh thu.

## 30/60/90-day gates — đúng một metric mỗi cổng

Ngày tính từ 09/10/2026; chủ vai trò là đề xuất, chưa nhận assignment. Các hoạt động Day22 khác là supporting evidence, không phải gating metric.

| Ngày / gate | Một gating metric & pass threshold | Ngày kiểm | Bằng chứng vật lý | Qua / trượt |
|---|---|---|---|---|
| **30 · Learning** | **Pain-validation interview yield:** ≥8/12 interviewed students independently report recurring weekly overlapping-deadline pain and show their current workaround (one binary-coded response/person). Chỉ metric này là gate; không phải revenue. | 08/11/2026 | 12 consented, de-identified interview notes + coding rubric + source timestamps; recruit target is Day22 proposal, not completed. | ≥8: GO to prototype learning; <8: FIX ICP/problem statement and do not claim validated demand. |
| **60 · Operating validation** | **7-day first-value activation ≥40%** of first-task cohort with full follow-up; minimum n=10, ≥95% sampled event QA. | 08/12/2026 | Timestamped event export, definition/version, QA log, cohort query. | Pass: GO to bounded retention cohort; fail or insufficient n: FIX workflow/instrumentation, no paid scale. |
| **90 · Business viability** | **Eligible academic W4 retention ≥30%** across ≥10 fully mature students with actionable work in the return window; report W1/W2/W4 trajectory separately. | 07/01/2027 | De-identified cohort table, academic-week eligibility audit, event QA, follow-up dates. | Pass: consider next experiment only after economics risk controls; fail but ≥10%: FIX/PIVOT once; <10% after one documented FIX meets KILL criterion below. |

**CAC non-gating risk control at D90:** paid acquisition remains NO-GO unless separate observed net CAC ≤$11.68632 and the approved checkout/refund attribution is complete. This is not a second D90 gate metric; it is a constraint in Rule 5.

**KILL CRITERION — draft:** At **07/01/2027**, discontinue the current B2C academic-task positioning if eligible W4 retention is **<10%** for ≥10 fully mature students after one documented workflow FIX and event QA ≥95%. If sample or eligible follow-up is insufficient, outcome is NOT MEASURABLE, not KILL. The student must independently approve or rewrite the threshold/date/consequence.

## CHƯA ĐO ĐƯỢC

Không có verified real users/interviews, paid customers, checkout/refunds, retention cohort, real task outcomes, production model/API, provider token traces, hosting invoices, free-tier usage, live CAC, p95 cost/user, WTP, or established runway. Day22’s `100/100` is an offline deterministic validator result on 100 synthetic cases, not model or user performance; `R=85%`, $0.25 infra, 8% retry, $0.015 QA, $4.99 price, 4 credits, 78.0649% GM and funnel cases are estimates/proposals. `academic_task_completed` is separate from AI plan delivery. Instrument the five Day20 events with identity, dedupe, timestamps and persisted-state acceptance checks; add proposed billing/refund, acquisition attribution, provider request/token/retry/latency and invoice allocation records only after privacy/security and policy review. First product baselines could reasonably start after 15/11/2026 review; paid CAC/retention require an approved checkout and fully observed cohorts, likely no earlier than the 90-day gate. Missing is not zero.

## Yêu cầu sinh viên tự sign-off

Trước khi nộp, sinh viên cần tự viết lại bằng lời mình: (1) provisional TB boundaries và sample minima; (2) [MH] arithmetic/display and 20% headroom choices; (3) each of the five rules, especially all stop/freeze actions; (4) exactly-one-metric gate wording, dates and <10% kill condition; (5) whether pricing/credit assumptions are acceptable to show. AI-prepared draft is not student-authored approval. The present estimates are not a claimed rubric score or submission readiness.
