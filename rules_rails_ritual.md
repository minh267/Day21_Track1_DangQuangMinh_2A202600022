# rules_rails_ritual.md

## Risk lớn nhất

AI speaking feedback sửa sai grammar/pronunciation -> user học sai -> mất trust, churn paid users, và làm app mất credibility với investor.

Burn rate giả định: ~$10K/tháng. Một incident mất trust 2-3 tháng = ~$20K-$30K runway.

---

## R1 - RULES

### KHÔNG được làm

- KHÔNG deploy prompt speaking feedback mới nếu chưa có 2 người review: founder + 1 engineer.
- KHÔNG cho AI tự generate grammar rules ngoài CEFR guideline hoặc rubric speaking đã lưu trong Notion.
- KHÔNG dùng ChatGPT public/Claude public để paste transcript user, audio transcript, API key, hoặc production prompt.
- KHÔNG trả feedback chắc chắn khi confidence thấp. Nếu model không chắc, phải dùng fallback: "AI chưa chắc lỗi này, bạn hãy thử nói lại hoặc chuyển sang text mode."

### Được làm

- Dùng GPT-4o/OpenAI Realtime API cho speaking evaluation production. Cost theo usage, theo dõi hằng ngày trong OpenAI dashboard.
- Dùng Helicone để log prompt/response speaking feedback. Cost: $0 ở giai đoạn MVP.
- Dùng LangSmith hoặc PromptLayer để version prompt và rollback. Cost: ~$25/tháng.
- Dùng bộ eval 50 speaking cases trước mỗi lần đổi prompt. Cost: $0, lưu trong Notion/GitHub.

### Hậu quả vi phạm

- Lần 1: founder review 1-1 trong ngày, ghi lại case và sửa process.
- Lần 2: mất quyền deploy prompt/config production cho tới khi qua lại checklist.

### Ai update rules?

Founder update file này mỗi thứ Sáu sau Friday Speaking Audit, hoặc ngay trong ngày nếu có incident AI feedback sai >3 cases/ngày.

---

## R2 - RAILS

| Rail | Mục đích | Cost |
|---|---|---:|
| Helicone | Log mọi prompt/response speaking feedback, filter theo user_id/session_id khi có complaint | $0 |
| LangSmith/PromptLayer | Version prompt, compare output, rollback prompt trong incident | ~$25/tháng |
| GitHub branch protection | Prompt/config production phải có 1 reviewer trước merge | $0 |
| Eval set 50 speaking cases | Chạy regression test trước mỗi prompt release | $0 |
| OpenAI dashboard budget alert | Cảnh báo API cost/latency tăng bất thường | $0 |

Tổng cost: **~$25/tháng**, dưới mức $500/tháng. Setup được trong 1 tuần.

---

## R3 - RITUAL

### Friday Speaking Audit - 30 phút/tuần

- Founder + engineer review 20 speaking logs/tuần trong Helicone.
- Check 3 lỗi: grammar correction sai, pronunciation feedback quá tự tin, feedback không theo CEFR.
- Nếu thấy >3 feedback sai/ngày: rollback prompt trong ngày và mở incident mini-review.

Question founder hỏi user mỗi tuần:

> "Có lần nào AI sửa tiếng Anh của bạn nhưng bạn thấy không đúng hoặc không tự nhiên không? Nếu có, bạn còn tin feedback của app sau lần đó không?"

Expected output: 1 note ngắn trong Notion gồm top 3 lỗi, prompt version liên quan, và 1 action founder làm trong tuần sau.
