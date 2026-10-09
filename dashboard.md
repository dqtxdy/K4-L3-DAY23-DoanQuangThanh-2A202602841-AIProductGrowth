# UniPilot — Operating Dashboard

**Mô hình:** B2C tự phục vụ đề xuất · **Cập nhật:** 09/10/2026 · **Đoàn Quang Thanh · 2A202602841**

**NORTH STAR — Weekly On-Time Important Task Completions:** distinct student tasks marked important and confirmed complete by deadline in an academic week; require persisted incomplete→complete transition, deduplicate user/task, exclude save failures and AI plan delivery. **Hiện tại: CHƯA ĐO.** Mục tiêu sau baseline: ≥ median của 4 tuần học đủ điều kiện gần nhất.

Ngưỡng North Star [TB]: sau 4 tuần baseline dương, cohort cố định với ≥10 sinh viên có actionable work mỗi tuần và QA ≥95%; xanh ≥ median, vàng ≥80% và <100% median, đỏ <80%. Median 0 hoặc eligibility mix đổi thì CHƯA ĐO và lập lại baseline.

### Đèn báo sớm (Leading)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| Eligible-week W4 retention | CHƯA ĐO | ≥30% / 15%–<30% / <15% | [TB] mature cohort; n≥10; complete W4 | NSM; hypothesis: W4 activity sustains on-time completions; test by cohort. Paid retention/LTV needs billing data. |
| 7-day first-value activation | CHƯA ĐO | ≥40% / 25%–<40% / <25% | [TB] first-task cohort; n≥10; full 7 days | W4 retention; hypothesis: first value supports return in eligible weeks; test by cohort. |
| Upcoming-task readiness | CHƯA ĐO | ≥80% / 60%–<80% / <60% | [TB] ≥30 tasks/week incl. missing deadlines; 2 weeks | Activation; hypothesis: complete deadline/context lowers first-value friction; test by cohort. |

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

The AI-cost light is metric 5: All-in direct cost / paid-seat request.

Missing denominator, immature cohort, or incomplete attribution/cost allocation = CHƯA ĐO, never zero or red. Cost/request alone does not establish each seat’s GM when requests exceed four or seat usage, costs, or net revenue vary; allocate seat-month COGS and verify aggregate COGS ≤40% of aggregate net recognized revenue.

### 5 luật quyết định (⏹ = luật dừng)

1. **⏹ STOP — NẾU** W4 <15% **TRONG** 2 cohort mature liên tiếp **VÀ** mỗi cohort có ≥10 sinh viên đủ điều kiện, W4 quan sát đủ **THÌ** tuần sau đóng băng paid acquisition; chạy 5 workflow sessions có điều phối và ghi blocker từ tạo task đến completion **KHÔNG THÌ** không mua ads bù churn; tuần nghỉ/không actionable work không phải churn.
2. **⏹ STOP — NẾU** activation <25% **TRONG** 2 first-task cohort liên tiếp **VÀ** mỗi cohort có ≥10 user, đủ follow-up 7 ngày, QA ≥95% **THÌ** dừng paywall; sửa onboarding step lỗi nhiều nhất, chạy 10 task-flow sessions tuần sau **KHÔNG THÌ** không giảm giá để che thiếu first value.
3. **⏹ STOP — NẾU** all-in direct cost/request >$0.499 **HOẶC** bất kỳ seat nào >4 requests/tháng **HOẶC** net revenue ≠$4.99 **TRONG** tháng đã đóng sổ **VÀ** ≥30 attempts, cost đối soát và seat counts đủ **THÌ** đóng băng tăng allowance; vượt cost trần thì cap test 4 requests/seat-month; lệch usage/revenue thì tính seat GM và aggregate COGS/net revenue **KHÔNG THÌ** không suy GM 60% từ riêng cost/request hoặc báo modeled cost là actual.
4. **NẾU** readiness <60% **TRONG** 2 tuần học **VÀ** mỗi tuần ≥30 task (gồm thiếu/invalid deadline), QA ≥95% **THÌ** sprint sau bắt buộc validation due date/context và đo lại cohort **KHÔNG THÌ** không bỏ task thiếu deadline khỏi mẫu số hoặc để AI đoán rồi gọi task là sẵn sàng.
5. **⏹ STOP — NẾU** CAC/account vượt mức thấp hơn giữa $11.68632 và 3× gross contribution tháng/account quan sát **TRONG** 1 acquisition cohort đã đóng **VÀ** ≥10 account mới sau refund/chargeback 30 ngày, đủ spend/attribution/refund records **THÌ** đóng băng paid launch/expansion; tuần sau chạy tagged organic/referral cohort tối đa 10 người; chưa có giao dịch thì NO-GO **KHÔNG THÌ** không scale từ stretch scenario hay giá đối thủ.

### Cổng gác 90 ngày

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 08/11/2026 | Pain-validation interview yield | ≥8/12 independent interviewees report weekly overlapping-deadline pain + current workaround; learning gate, no revenue claim | 12 consented, de-identified notes; coding rubric; timestamps. | <8/12: FIX problem/ICP; do not claim validated demand. |
| 60 · 08/12/2026 | 7-day first-value activation | ≥40%; n≥10; full 7-day follow-up; QA ≥95% (GO: limited retention validation) | Timestamped event export; metric version; cohort query; QA log. | Below threshold or insufficient sample: FIX workflow/instrumentation; no paid scaling. |
| 90 · 07/01/2027 | Eligible academic W4 retention | ≥30%; mature cohort; n≥10 eligible with actionable work and complete W4 (GO after pricing approval: tagged checkout, max 50 qualified visits) | De-identified cohort table; eligibility audit; event QA; follow-up dates. | Mature W4 <30%: FIX by 14/01; distinct post-fix cohort by 21/01. Insufficient sample/follow-up: CHƯA ĐO; fix measurement, no KILL. |

**KILL CRITERIA — D135 (21/02/2027), not D90:** D90 W4 <30% → FIX by 14/01; distinct post-fix cohort by 21/01. Stop B2C only if post-fix W4 <10%, n≥10 eligible, 4 observed academic weeks, QA ≥95%. W4 10%–<30%: PIVOT; ≥30%: tagged checkout, max 50 visits, after pricing approval. Immature: CHƯA ĐO; defer KILL.

**CHƯA ĐO ĐƯỢC:** No verified users/interviews, paid customers, CAC, billing/refunds, retention, task outcomes, production invoices, free-tier cost, p95, WTP or runway. Day22 100/100 is an offline synthetic-data validator result, not product success. Need event, billing, attribution and invoice records; baseline may start after planned 15/11/2026 review; CAC/retention need checkout and mature cohorts. Missing is not zero.
