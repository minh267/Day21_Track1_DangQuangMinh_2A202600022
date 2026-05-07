# Day 17 Submission

**Student:** Đặng Quang Minh - 2A202600022  
**Date:** 25/04/2026  
**Product idea:** App luyện speaking realtime với AI companion giúp sinh viên vượt qua nỗi sợ nói tiếng Anh  

---

## 1. MVP Boundary Sheet

**Riskiest Assumption:**
> Sinh viên thực sự sẵn sàng nói tiếng Anh với AI nếu trải nghiệm đủ “an toàn” và không bị đánh giá.

### In-Scope (tối đa 3)

- [ ] Real-time speaking conversation với AI companion (voice)  
  → test: user có thực sự nói không (behavior)

- [ ] AI feedback đơn giản sau mỗi đoạn nói (grammar + suggestion)  
  → test: user thấy value và hiểu lỗi

- [ ] Context memory ngắn (ghi nhớ 3–5 thông tin user trong session)  
  → test: personalization có tăng engagement không

---

### Out-of-Scope

- Gamification (points, leaderboard) → không cần để test core value  
- Multi-topic learning path → chưa cần  
- Voice emotion analysis → phức tạp, không cần  
- Long-term progress tracking → không cần cho MVP  

---

### Non-Goals

- Không thay thế giáo viên  
- Không dạy ngữ pháp đầy đủ  
- Không build full course learning system  

---

## 2. PRD Skeleton

### Problem Statement
> Sinh viên Việt Nam có thể hiểu tiếng Anh nhưng không dám nói do sợ sai và thiếu môi trường luyện tập, dẫn đến mất cơ hội giao tiếp và phát triển kỹ năng.

---

### Target User
> Sinh viên (18–24 tuổi), có nền tảng tiếng Anh cơ bản, nhưng:
- Ngại nói trước người khác  
- Không có môi trường luyện speaking  
- Muốn cải thiện giao tiếp thực tế  

---

### User Stories

**Story 1:**
> As a shy university student,  
I want to talk to an AI companion in English via voice,  
so that I can practice speaking without fear of being judged.

**Story 2:**
> As a student learning English,  
I want to receive simple corrections after speaking,  
so that I can improve without feeling overwhelmed.

---

### AI-Specific

#### Model Selection

- Model: GPT-4o (voice + text)

**Lý do chọn:**
- Tốt cho conversational + voice realtime  
- Hiểu context tốt → phù hợp companion UX  

**Trade-offs chấp nhận:**
- Cost cao hơn  
- Latency vừa phải (~1–2s acceptable)

**Trade-offs không chấp nhận:**
- Delay > 3s (phá flow nói)  
- Response quá robotic (mất cảm giác companion)

---

#### Data Requirements

- Nguồn:
  - User input (voice + text)
  - Session memory (conversation history)

- Owner:
  - User (privacy-first)

- Update frequency:
  - Real-time (conversation)

- Risk:
  - Sai grammar correction
  - Misunderstanding speech

---

#### Fallback UX

- Chiến lược: Human-in-the-loop  

**Trigger:**
- Confidence thấp (speech unclear / ASR fail)  
- Output có ambiguity  

**Hành động:**
> “I didn’t catch that clearly, can you repeat?”

**User options:**
- Repeat  
- Switch to text input  

**Recovery:**
- Retry conversation  
- Maintain context  

---

### Success Metrics

- Primary metric:  
  → % user hoàn thành ≥ 1 đoạn speaking ≥ 60s  

- Ngưỡng thành công:  
  → ≥ 40% users  

- Timeframe:  
  → 7 ngày sau onboarding  

---

### Dependencies & Constraints

- API:
  - OpenAI (speech + LLM)

- Tech:
  - Web app / mobile  

- Constraint:
  - Latency < 2s  
  - Cost per user thấp  

---

## 3. Hypothesis Table

### Hypothesis 1

> We believe that real-time AI speaking conversation  
will help shy students  
build confidence in speaking English.

We will know we are right when:
- ≥ 40% users speak ≥ 60s/session  
within 7 days  

**Riskiest assumption:**
> User vượt qua nỗi sợ và bắt đầu nói  

**Cheapest test:**
- Fake demo + Wizard of Oz  
- Quan sát user có nói không  

---

### Hypothesis 2

> AI feedback sau mỗi đoạn nói  
will help users improve and stay engaged  

We will know we are right when:
- ≥ 30% users quay lại session thứ 2  

**Riskiest assumption:**
> Feedback không làm user cảm thấy bị “judge”  

**Test:**
- A/B test: feedback vs no feedback  

---

## 4. PMF Scorecard

### Aha Moment
> User nói được 1 đoạn tiếng Anh liên tục và nhận feedback đúng → cảm thấy tự tin hơn  

---

### Actionable Metric
- % user hoàn thành ≥ 1 đoạn speaking ≥ 60s  
- % user quay lại ngày hôm sau  

---

### PMF Method

- [x] Retention Curve  
  → D7 retention ≥ 25%  

- [x] Aha Moment tracking  
  → ≥ 50% user đạt trong 2 ngày đầu  

---

### Vanity Metrics KHÔNG dùng

- Downloads  
- Page views  
- Total users  

---

## 5. AI Critique Log

**Điểm AI chỉ ra:**

1. In-Scope có thể quá nhiều  
   → Action: Accept → giảm còn 3 feature  

2. Fallback UX chưa rõ trigger  
   → Action: Accept → thêm condition cụ thể  

3. Metric có thể là vanity  
   → Action: Partial → thêm retention  

---

### Thay đổi lớn nhất
> Từ build nhiều feature → chuyển sang focus hành vi speaking  

---

## 6. Self-assessment

**Mắt xích yếu nhất:**
> PMF measurement (cần data thực)  

---

**Open questions:**

1. Làm sao giảm latency voice tốt hơn?  
2. Làm sao tạo cảm giác AI companion thật hơn?  

---

#  Notes

- MVP focus: chỉ test hành vi “dám nói”  
- Aha moment: nói được 1 đoạn + nhận feedback  
- Core insight: giảm fear → tăng speaking  
