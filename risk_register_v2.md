# risk_register_v2.md

## Context

Product: SpeakMate AI, app luyện speaking realtime với AI companion cho sinh viên Việt Nam 18-24 tuổi.

Core MVP:
- Realtime voice conversation với GPT-4o/OpenAI Realtime API
- Feedback đơn giản sau session: grammar, pronunciation, 1 bước cải thiện
- Short session memory 3-5 thông tin trong buổi nói

Unit economics base case:
- ARPU: 129,000 VND/user/tháng
- COGS: 50,000 VND/user/tháng
- Gross margin: ~61%
- CAC: 250,000 VND
- Fixed cost: **160M VND/tháng**
- Cash: **3B VND**
- Current runway: **>= 24 tháng**

Impact bên dưới đo bằng **tháng runway**, với quy đổi thực tế: 1 tháng runway khoảng **160M VND fixed burn**, chưa tính opportunity cost từ churn và roadmap delay.

---

## AI augmentation evidence

Tôi bắt đầu từ 3 risks manual:
1. OpenAI Realtime API vendor dependency
2. AI speaking feedback sửa sai
3. Founder/core engineer overload

Sau khi dùng AI như CRO reviewer, tôi bổ sung các blindspots sau:

| AI-found risk | Vì sao tôi miss |
|---|---|
| Speech-to-text accent bias với sinh viên Việt Nam | Tôi nghĩ risk nằm ở LLM feedback, nhưng input voice sai từ đầu cũng làm feedback sai. |
| CAC spike do TikTok/Facebook group backlash | Tôi chỉ nghĩ user churn, chưa nghĩ incident có thể làm kênh acquisition rẻ bị cháy. |
| Session memory privacy/GDPR/VN data risk | Tôi xem memory là personalization feature, nhưng nó cũng là sensitive data risk. |
| Prompt injection để AI nói toxic/nhạy cảm | Tôi nghĩ app học tiếng Anh ít bị abuse, nhưng realtime companion vẫn có bề mặt tấn công. |

---

# Risk Register v2

| # | Risk | Type | L | I | Score | Zone |
|---:|---|---|---:|---:|---:|---|
| 1 | OpenAI Realtime dependency | Vendor | 4 | 4 | 16 | KILL ZONE |
| 2 | AI feedback sửa sai | Customer-facing | 4 | 4 | 16 | KILL ZONE |
| 3 | Voice latency >3s | Vendor | 4 | 3 | 12 | Mitigate |
| 4 | Speech-to-text accent bias | Customer-facing | 4 | 4 | 16 | KILL ZONE |
| 5 | Founder unavailable during incident | Founder-bandwidth | 3 | 3 | 9 | Mitigate |
| 6 | Prompt injection viral response | Reputational | 3 | 4 | 12 | Mitigate |
| 7 | Session memory privacy leak | Regulatory | 3 | 4 | 12 | Mitigate |
| 8 | EU AI Act / chatbot transparency miss | Regulatory | 2 | 3 | 6 | Watch |
| 9 | API cost per user doubles | Vendor | 4 | 4 | 16 | KILL ZONE |
| 10 | Feedback makes user feel judged | Customer-facing | 4 | 3 | 12 | Mitigate |
| 11 | CAC channel backlash | Reputational | 3 | 4 | 12 | Mitigate |
| 12 | No rollback owner | Founder-bandwidth | 3 | 4 | 12 | Mitigate |
| 13 | Payment/refund policy confusion | Customer-facing | 3 | 3 | 9 | Mitigate |
| 14 | Data retention unclear | Regulatory | 3 | 3 | 9 | Mitigate |

---

## Risk 1: OpenAI Realtime dependency

- Type: Vendor
- If: OpenAI changes Realtime API pricing, ToS, model availability, or rate limits for voice/conversation.
- Then: SpeakMate AI cannot deliver stable realtime conversation, must degrade UX or migrate model under pressure.
- Leading to: **3 tháng runway** lost from higher API bill, rebuild voice pipeline, user churn, and roadmap delay.
- Likelihood: 4/5
- Impact: 4/5
- Score: 16, KILL ZONE
- Mitigation, <$500/month:
  1. Add OpenAI cost/latency alerts and daily Helicone dashboard review. Cost: $0.
  2. Build text-mode fallback when realtime voice fails or latency >3s. Cost: $0.
  3. Test 1 backup provider/model for non-realtime post-session feedback. Cost: <$50/month.

---

## Risk 2: AI feedback sửa sai

- Type: Customer-facing
- If: AI gives wrong grammar/pronunciation correction with high confidence after a speaking session.
- Then: user learns wrong English, loses trust, screenshots the error, and churns before D7 retention.
- Leading to: **2-3 tháng runway** lost from paid user churn, compensation, support, and retention rebuild.
- Likelihood: 4/5
- Impact: 4/5
- Score: 16, KILL ZONE
- Mitigation, <$500/month:
  1. Run eval set of 50 Vietnamese-student speaking cases before every prompt release. Cost: $0.
  2. Add confidence fallback: if uncertain, AI says it is not sure and asks user to repeat or switch text mode. Cost: $0.
  3. Friday Speaking Audit: founder reviews 20 Helicone logs weekly. Cost: $0.

---

## Risk 3: Voice latency >3s

- Type: Vendor
- If: GPT-4o Realtime API or network latency rises above 3 seconds during user practice.
- Then: conversation flow breaks, shy students stop speaking, and the core MVP assumption cannot be validated.
- Leading to: **1-2 tháng runway** lost because activation and D7 retention data become unreliable.
- Likelihood: 4/5
- Impact: 3/5
- Score: 12, Mitigate
- Mitigation, <$500/month:
  1. Track p50/p95 latency in Helicone/OpenAI dashboard. Cost: $0.
  2. Auto-switch to short text prompt after 2 failed voice attempts. Cost: $0.
  3. Reduce prompt/context length for realtime path. Cost: $0.

---

## Risk 4: Speech-to-text accent bias

- Type: Customer-facing
- If: ASR misunderstands Vietnamese-accented English or noisy dorm/classroom audio.
- Then: AI feedback corrects a sentence the user did not actually say, making feedback feel unfair and embarrassing.
- Leading to: **2-3 tháng runway** lost from trust damage, low activation, and wrong product decisions from noisy data.
- Likelihood: 4/5
- Impact: 4/5
- Score: 16, KILL ZONE
- AI-found: Yes. I missed this because I focused on LLM output, not voice input quality.
- Mitigation, <$500/month:
  1. Save transcript preview and let user confirm "AI heard me right" before feedback on low-confidence audio. Cost: $0.
  2. Add 30 Vietnamese-accent test clips to eval set. Cost: $0 using founder-recorded samples.
  3. Trigger "please repeat" when ASR confidence is low. Cost: $0.

---

## Risk 5: Founder unavailable during incident

- Type: Founder-bandwidth
- If: founder or core engineer is sick, traveling, or overloaded when AI feedback incident goes viral.
- Then: no one verifies logs, soft-kills feedback, or posts founder response in the first 30 minutes.
- Leading to: **1-2 tháng runway** lost from prolonged incident, churn, and credibility loss.
- Likelihood: 3/5
- Impact: 3/5
- Score: 9, Mitigate
- Mitigation, <$500/month:
  1. Keep 1-page incident playbook pinned in Slack/Notion. Cost: $0.
  2. Give one engineer permission to rollback prompt/config without founder approval in crisis. Cost: $0.
  3. Set phone escalation rule for complaint viral or >3 wrong-feedback cases/day. Cost: $0.

---

## Risk 6: Prompt injection viral response

- Type: Reputational
- If: a user pushes the companion to insult the product, say toxic content, or produce a memeable response.
- Then: screenshot spreads on Facebook/TikTok and SpeakMate is framed as unsafe or unserious.
- Leading to: **2 tháng runway** lost from brand damage, support load, and lower conversion from student communities.
- Likelihood: 3/5
- Impact: 4/5
- Score: 12, Mitigate
- Mitigation, <$500/month:
  1. Add system prompt refusal for self-harm, hate, sexual, and product-insult bait. Cost: $0.
  2. Log and review unusual prompt patterns in Helicone weekly. Cost: $0.
  3. Add "report response" button for user screenshots before they go public. Cost: $0-$20/month.

---

## Risk 7: Session memory privacy leak

- Type: Regulatory
- If: short session memory stores personal goals, school, anxiety, or private conversation without clear notice/deletion.
- Then: user complains about privacy, regulator/mentor flags poor data governance, and app must pause memory feature.
- Leading to: **2 tháng runway** lost from refactor, legal review, and trust repair.
- Likelihood: 3/5
- Impact: 4/5
- Score: 12, Mitigate
- AI-found: Yes. I missed this because I treated memory as engagement, not sensitive data.
- Mitigation, <$500/month:
  1. Add clear in-app note: what memory stores, why, and how to delete. Cost: $0.
  2. Store only 3-5 non-sensitive facts per session, no raw audio by default. Cost: $0.
  3. Add delete-memory button in settings or support flow. Cost: $0.

---

## Risk 8: EU AI Act / chatbot transparency miss

- Type: Regulatory
- If: SpeakMate expands to EU users or exchange students but does not disclose that users are interacting with AI.
- Then: transparency obligations are missed and trust/compliance risk rises before fundraising or partnership.
- Leading to: **1 tháng runway** lost from policy rewrite, UX changes, and deal delay.
- Likelihood: 2/5
- Impact: 3/5
- Score: 6, Watch
- Mitigation, <$500/month:
  1. Add "You are practicing with AI" label in onboarding and chat screen. Cost: $0.
  2. Keep privacy/AI disclosure page in Notion or website footer. Cost: $0.
  3. Review regulatory scope quarterly before international launch. Cost: $0.

---

## Risk 9: API cost per user doubles

- Type: Vendor
- If: average voice minutes per user grows faster than ARPU, or Realtime API price/usage doubles.
- Then: COGS rises from 50,000 VND to 100,000+ VND per user/month, destroying the 61% gross margin.
- Leading to: **3-4 tháng runway** lost from negative unit economics and forced pricing/usage caps.
- Likelihood: 4/5
- Impact: 4/5
- Score: 16, KILL ZONE
- Mitigation, <$500/month:
  1. Add per-user monthly API budget cap and alert. Cost: $0.
  2. Limit free users to a daily voice-minute quota. Cost: $0.
  3. Move post-session feedback to cheaper non-realtime model when possible. Cost: <$100/month test budget.

---

## Risk 10: Feedback makes user feel judged

- Type: Customer-facing
- If: feedback is technically correct but too harsh, too many corrections, or uses teacher-like wording.
- Then: shy students feel judged, stop speaking, and the riskiest MVP assumption fails.
- Leading to: **1-2 tháng runway** lost because activation and D7 retention stay below target.
- Likelihood: 4/5
- Impact: 3/5
- Score: 12, Mitigate
- Mitigation, <$500/month:
  1. Limit feedback to 1 positive point + 1 correction + 1 next step. Cost: $0.
  2. A/B test feedback vs no feedback for second-session return. Cost: $0.
  3. Ask weekly Customer Friday question about whether feedback feels supportive or judgmental. Cost: $0.

---

## Risk 11: CAC channel backlash

- Type: Reputational
- If: a viral negative post spreads in the same Facebook/TikTok student channels used for low-CAC acquisition.
- Then: organic conversion drops, CAC rises from 250,000 VND toward the pessimistic 800,000 VND case.
- Leading to: **2-4 tháng runway** lost because payback period worsens and growth experiments become expensive.
- Likelihood: 3/5
- Impact: 4/5
- Score: 12, Mitigate
- AI-found: Yes. I missed this because I separated incident response from acquisition economics.
- Mitigation, <$500/month:
  1. Founder replies personally in the same channel within 30 minutes when incident is real. Cost: $0.
  2. Keep a public changelog of fixes from user feedback. Cost: $0.
  3. Recruit 5 beta ambassadors who can give honest feedback before public launch. Cost: <$100/month in perks.

---

## Risk 12: No rollback owner

- Type: Founder-bandwidth
- If: prompt/config deploy causes wrong feedback but only founder knows how to rollback.
- Then: incident lasts hours longer than needed and more users receive wrong feedback.
- Leading to: **2 tháng runway** lost from preventable blast radius and support/compensation.
- Likelihood: 3/5
- Impact: 4/5
- Score: 12, Mitigate
- Mitigation, <$500/month:
  1. Document rollback steps with exact LangSmith/PromptLayer links. Cost: $0.
  2. Require prompt versions to have stable names and changelog. Cost: $0.
  3. Run one 30-minute rollback drill monthly. Cost: $0.

---

## Risk 13: Payment/refund policy confusion

- Type: Customer-facing
- If: AI companion or support text gives unclear subscription/refund promises, like "free month" or "unlimited use" without policy.
- Then: user expects compensation that product cannot honor, causing Air Canada-style trust/legal precedent.
- Leading to: **1-2 tháng runway** lost from refunds, support, and policy cleanup.
- Likelihood: 3/5
- Impact: 3/5
- Score: 9, Mitigate
- Mitigation, <$500/month:
  1. Keep a 1-page refund/compensation policy that AI is not allowed to improvise. Cost: $0.
  2. Put payment/refund questions into safe canned responses. Cost: $0.
  3. Founder approves any public compensation template. Cost: $0.

---

## Risk 14: Data retention unclear

- Type: Regulatory
- If: raw audio, transcript, or feedback logs are stored indefinitely without retention policy.
- Then: privacy risk accumulates and deletion requests become hard to satisfy.
- Leading to: **1-2 tháng runway** lost from cleanup, data deletion work, and trust repair.
- Likelihood: 3/5
- Impact: 3/5
- Score: 9, Mitigate
- Mitigation, <$500/month:
  1. Set MVP retention: raw audio not stored by default; transcript/log retained 30 days for QA. Cost: $0.
  2. Document data retention in privacy page. Cost: $0.
  3. Add monthly cleanup job or manual export/delete checklist. Cost: $0.

---

# Top 5 KILL ZONE / highest-priority mitigations

| Priority | Risk | This-week founder action | Cost |
|---:|---|---|---:|
| 1 | AI feedback sửa sai | Build 50-case eval set + confidence fallback + Friday log audit | $0 |
| 2 | OpenAI Realtime dependency | Add cost/latency alerts + text-mode fallback + backup non-realtime test | <$50/month |
| 3 | Speech-to-text accent bias | Add transcript confirmation for low confidence + 30 Vietnamese-accent clips | $0 |
| 4 | API cost per user doubles | Add per-user quota/alert + move post-session feedback to cheaper model | <$100/month |
| 5 | CAC channel backlash | Founder public response template + changelog + 5 beta ambassadors | <$100/month |

Total mitigation cost this month: **<$250/month**, below the $500/month startup-governance constraint.

---

# Final checklist

- [x] >= 10 risks: 14 risks total
- [x] Cover all 5 types: Vendor, Customer-facing, Founder-bandwidth, Regulatory, Reputational
- [x] Every risk uses If / Then / Leading to with months of runway
- [x] >= 2 AI-found risks: 4 marked in AI augmentation evidence
- [x] Top 5 priority risks have founder-implementable mitigation in 1 week
- [x] Mitigation cost < $500/month
- [x] At least 1 regulatory risk: session memory privacy, EU AI Act transparency, data retention

Founder conclusion: SpeakMate AI can move fast, but only if I treat voice accuracy, feedback trust, vendor dependency, and privacy as weekly operating work, not as a policy document I write once and forget.
