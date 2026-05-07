# risk_register.md

Dựa trên:
- PRD Day 17 của app AI luyện speaking realtime
- Unit Economics + Runway Day 18

Giả định burn rate hiện tại: **~$10K/tháng**. Vì vậy mọi impact bên dưới được quy đổi về **tháng runway**, không chỉ nhìn theo tiền hoặc % user.

---

# Risk 1 - Vendor Risk (Replika-style)

## Type
Vendor

## If
OpenAI tăng giá Realtime API, thay đổi ToS voice/conversation, hoặc giới hạn usage khiến GPT-4o realtime latency >3s trong giờ cao điểm,

## Then
AI companion phản hồi chậm, voice conversation bị delay, user cảm thấy app "không realtime", và startup phải migrate model gấp hoặc giảm quality speaking experience,

## Leading to
mất khoảng **3 tháng runway** vì:
- API cost tăng hoặc phải mua tier cao hơn
- paid users churn vì realtime UX tệ
- engineer phải rebuild voice pipeline thay vì ship roadmap MVP

## Likelihood
4/5 - sản phẩm phụ thuộc gần như trực tiếp vào OpenAI realtime voice.

## Impact
4/5 - 3 tháng runway nằm trong nhóm high impact.

## Score
16 -> **KILL ZONE**

## Mitigation trong 1 tuần
- Thêm latency/cost alert trong OpenAI dashboard + Helicone.
- Chuẩn bị fallback text-mode khi voice latency >3s.
- Test 1 provider/model backup cho non-realtime feedback, không cần migrate toàn bộ app ngay.

---

# Risk 2 - Customer-facing AI Risk (Air Canada-style)

## Type
Customer-facing

## If
AI speaking feedback hallucinate grammar correction hoặc pronunciation evaluation sai trong production, nhưng trả lời với giọng quá chắc chắn,

## Then
user học sai tiếng Anh, screenshot feedback sai lên TikTok/Facebook group, uninstall app, và warning bạn bè không dùng sản phẩm,

## Leading to
mất khoảng **2-3 tháng runway** vì:
- mất trust vào core value của app
- churn paid users
- founder phải xử lý support/compensation thay vì sales
- phải rollback prompt và rebuild retention flow

## Likelihood
4/5 - feedback speaking là core feature và dễ sai ở edge cases.

## Impact
4/5 - 2-3 tháng runway, có thể thành KILL ZONE nếu viral.

## Score
16 -> **KILL ZONE**

## Mitigation trong 1 tuần
- Chạy eval set 50 speaking cases trước mỗi lần deploy prompt.
- Thêm fallback khi confidence thấp: không sửa chắc chắn, gợi ý user thử nói lại hoặc chuyển text mode.
- Review 20 logs/tuần trong Friday Speaking Audit.

---

# Risk 3 - Founder-bandwidth Risk

## Type
Founder-bandwidth

## If
founder hoặc core engineer bị overload/ốm vài ngày trong khi realtime voice system gặp critical bug, OpenAI outage, hoặc complaint viral,

## Then
không ai verify log, rollback prompt, hoặc trả lời customer nhanh trong 30 phút đầu; incident kéo dài thành mất trust công khai,

## Leading to
mất khoảng **1-2 tháng runway** do:
- user churn trong giai đoạn MVP
- app rating giảm
- mất credibility khi demo với mentor/investor
- roadmap speaking MVP bị chậm

## Likelihood
3/5 - team nhỏ nên single point of failure là thật.

## Impact
3/5 - 1-2 tháng runway, chưa phải KILL ZONE nhưng cần mitigate.

## Score
9 -> **MITIGATE**

## Mitigation trong 1 tuần
- Viết incident checklist 1 trang để founder mệt lúc 3h sáng vẫn làm theo.
- Trao quyền cho 1 engineer rollback prompt/config khi founder không online.
- Tạo Slack/phone escalation rule: nếu complaint viral hoặc feedback sai >3 cases/ngày thì gọi ngay.

---

# 2x2 Risk Matrix

| Risk | Likelihood | Impact | Score | Zone |
|---|---:|---:|---:|---|
| Vendor API / ToS / latency change | 4 | 4 | 16 | KILL ZONE |
| AI feedback sửa sai | 4 | 4 | 16 | KILL ZONE |
| Founder overload / outage | 3 | 3 | 9 | MITIGATE |

---

# KILL ZONE Priorities tuần này

## Priority 1: AI feedback sửa sai

Lý do: core trust của app = feedback đúng, rõ mức độ chắc chắn, và không làm user học sai.

Action tuần này: eval set 50 cases + fallback confidence thấp + Friday Speaking Audit.

## Priority 2: Vendor API / ToS / latency change

Lý do: toàn bộ realtime speaking UX phụ thuộc OpenAI Realtime API.

Action tuần này: dashboard alert + text-mode fallback + test model backup cho non-realtime feedback.

---

# Insight quan trọng

App AI speaking này có 3 survival risks lớn nhất: vendor dependency, trust vào feedback, và founder single point of failure. Nếu founder kiểm soát được 3 điểm này bằng rails rẻ + ritual hằng tuần, startup có phanh đủ tốt để tiếp tục scale.
