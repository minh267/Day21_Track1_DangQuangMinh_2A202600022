# incident_playbook.md

## Tình huống giả định

9h30 sáng, một sinh viên đăng screenshot lên Facebook group:

> "App AI luyện nói tiếng Anh này sửa câu đúng của mình thành câu sai. Mình học theo 1 tuần mới phát hiện ra."

Bài post đạt 200 reactions và 80 comments trong 30 phút. Có dấu hiệu viral.

Goal trong 30 phút đầu: verify có thật không, stop bleeding, nhắn riêng cho user, và post public bằng giọng founder.

---

# Bước 1 - Verify trong 5 phút

## Check ở đâu?

- Helicone: filter theo `user_id`, `session_id`, timestamp trong screenshot; chụp lại prompt/response.
- Database: check user_id, thời gian, transcript speaking gốc, feedback AI đã trả.
- OpenAI dashboard: check model version, request error, latency, và spike bất thường.

## Kết luận trong 5 phút

- Nếu screenshot khớp log: coi là incident thật, chuyển sang Bước 2 ngay.
- Nếu chưa khớp log: vẫn soft kill feedback confidence thấp trong 10 phút, rồi tiếp tục điều tra.
- Nếu là photoshop rõ ràng: không accuse user public; nhắn riêng xin thêm `email/session time` để verify.

---

# Bước 2 - Stop the bleeding trong 10 phút

## Option chọn: Soft kill

Tạm tắt AI feedback tự động cho các case confidence thấp và chuyển sang fallback:

> "AI chưa chắc chắn về lỗi này. Bạn có thể thử nói lại hoặc chuyển sang text mode để nhận góp ý an toàn hơn."

## Lý do chọn soft kill

- Không cần tắt toàn bộ app.
- User vẫn luyện speaking được.
- Ngăn AI tiếp tục sửa sai hàng loạt.
- Founder có thời gian review log, rollback prompt, và update eval set.

## Nếu incident nặng hơn

- Nếu feedback sai >10 cases/ngày: hard stop toàn bộ auto-feedback trong ngày.
- Nếu chỉ 1 user cố tình prompt-inject: block user/session đó.
- Nếu lỗi đến từ prompt version mới: rollback prompt ngay trong LangSmith/PromptLayer.

---

# Bước 3 - Customer comm trong 20 phút

Hi bạn,

Mình là Minh, founder của app AI luyện nói tiếng Anh.

Mình vừa thấy bài post của bạn về việc AI sửa sai feedback speaking. Mình xin lỗi vì trải nghiệm này có thể khiến bạn học sai và mất niềm tin vào sản phẩm.

Mình đang làm 3 việc ngay:
1. Tạm tắt feedback tự động cho các case AI chưa chắc chắn.
2. Review lại log session của bạn trong Helicone/database.
3. Rollback prompt feedback nếu đúng là lỗi từ prompt version mới.

Để bù lại, mình gửi bạn **1 tháng sử dụng miễn phí ngay hôm nay** và muốn gọi bạn 10 phút để nghe kỹ lỗi bạn gặp. Mình không yêu cầu bạn xóa bài. Khi fix xong, mình sẽ update công khai kết quả và gửi bạn bản tóm tắt mình đã sửa gì.

Cảm ơn bạn vì đã chỉ ra vấn đề này. Đây là lỗi mình cần biết sớm, không phải lỗi mình muốn né.

- Minh

---

# Bước 4 - Public response trong 30 phút

Mình là Minh, founder app. Sáng nay AI feedback speaking đã sửa sai trong một số case. Mình đã tạm tắt feedback tự động khi AI không chắc, rollback prompt nghi ngờ lỗi, và đang review log. Xin lỗi vì trải nghiệm này. Mình sẽ cập nhật fix rõ ràng trong hôm nay.

---

# Bước 5 - Sau 24 giờ

- Post update ngắn: root cause, prompt/version nào lỗi, đã rollback hay chưa.
- Add case này vào eval set 50 speaking cases.
- Review 20 logs tiếp theo để chắc lỗi không lặp lại.
- Ghi postmortem 5 dòng trong Notion: what happened, impact, fix, owner, due date.
