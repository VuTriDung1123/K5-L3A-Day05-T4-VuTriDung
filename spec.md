# Dàn ý bài nộp LAB5 — Track 4 · Thương mại điện tử

**Sản phẩm:** "Trò chuyện với Shopee" — khiếu nại không đúng mô tả  
**Lớp:** L3A · **Bàn / zone:** \_\_ · **Track:** 4

| Họ và Tên         | Mã Học Viên | Việc đã làm |
| ----------------- | ----------- | ----------- |
| Khổng Tiến Dương  | 2A202603467 |             |
| Phạm Quang Trường | 2A202603378 |             |
| Vũ Trí Dũng       | 2A202603478 |             |

> Ghi chú: file `tracks/t4-ecommerce.md` chưa có sẵn trong thư mục, nên dàn ý dưới đây được xây dựng từ mô tả đề bài và template mẫu trong `spec.md`.

---

## §1. Lát cắt & phạm vi

### 1.1 Lát cắt
- **Track / sản phẩm nền:** T4 · Thương mại điện tử — "Trò chuyện với Shopee" xử lý khiếu nại *sản phẩm không đúng mô tả*.
- **Ngườii dùng chính:** ngườii mua đang khiếu nại.
- **Ngườii dùng phụ:** ngườii bán, nhân viên sàn (CS/ moderation).
- **Cái giá khi sai:** bất kỳ bên nào cũng có thể mất tiền — ngườii mua bị từ chối đền bù oan, ngườii bán bị trừ tiền/refund oan, sàn mất uy tín và pháp lý.

### 1.2 Phạm vi tính năng AI làm
- Tiếp nhận khiếu nại qua chat, hỏi rõ và gom bằng chứng (ảnh, video, mô tả, link sản phẩm).
- So khớp nội dung khiếu nại với mô tả sản phẩm, chính sách đổi/trả, lịch sử chat.
- Đề xuất phương án xử lý (hoàn tiền / đổi hàng / trả hàng / từ chối) **kèm mức tin cậy**.
- Chuyển lên ngườii duyệt khi vượt ngưỡng rủi ro hoặc khi ngườii dùng không đồng ý.

### 1.3 3 việc tính năng KHÔNG làm
1. **Không tự động hoàn tiền** — vì cost-of-error cao, một bên chắc chắn mất tiền oan.
2. **Không tự động phạt/treo shop** — cần ngườii sàn xác minh trước khi ảnh hưởng doanh thu ngườii bán.
3. **Không tự động mở đơn khiếu nại mới nếu đã có đơn đang xử lý** — tránh duplicate, spam và xung đột quyết định.

---

## §2. Kịch bản rủi ro (chặng 2)

> Phân loại: ① Lỗi nhẹ / UI; ② Lỗi nhận dạng ngữ cảnh; ③ Lỗi quyết định nghiêm trọng; ④ Lỗi hành động tự động gây hậu quả.

| # | Tin ngườii dùng | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|-----------------|-----|------------------------|--------------------------|------|
| 1 | "Tôi nhận được áo màu đỏ nhưng đặt màu xanh" | ② | Nhận diện màu trong ảnh sai do ánh sáng → kết luận sai. | Yêu cầu thêm ảnh dưới ánh sáng tự nhiên, so sánh với SKU đã đặt. | Sửa được |
| 2 | "Sản phẩm nhỏ hơn mô tả" | ② | Không đọc được kích thước thực tế từ ảnh chụp tay. | Hỏi ngườii mua đo và nhập số đo; so với thông số gốc. | Sửa được |
| 3 | "Tôi muốn hoàn tiền 100%" | ④ | AI hứa hoàn 100% trong khi policy chỉ hoàn 50% với mặt hàng này. | Hiển thị "đề xuất đang chờ duyệt", không cam kết số tiền. | Không chấp nhận được |
| 4 | "Hàng giống ảnh nhưng chất liệu khác" | ③ | AI không phát hiện chất liệu khác biệt, đồng ý từ chối khiếu nại. | Hỏi rõ chất liệu mong đợi vs thực tế, đề xuất đổi/trả. | Sửa được |
| 5 | Ngườii bán đã cập nhật mô tả sau khi đặt hàng | ② | AI so sánh với mô tả mới nhất thay vì snapshot lúc mua. | Luôn dùng snapshot mô tả + giá tại thờii điểm đặt hàng. | Sửa được |
| 6 | Ngườii mua gửi ảnh nhầm từ đơn hàng khác | ③ | AI nhận ảnh và chấp nhận khiếu nại oan cho ngườii bán. | Cross-check ảnh với mã đơn hàng, yêu cầu ảnh có mã vận đơn/ hóa đơn. | Không chấp nhận được |
| 7 | "Tôi đã gửi ảnh rồi" | ① | AI yêu cầu lại ảnh vì không lưu context giữa các turn. | Hiển thị ảnh đã gửi và hỏi "Bạn muốn bổ sung thêm không?" | Nhẹ |
| 8 | AI tự tạo nhãn trả hàng cho mặt hàng không được đổi trả | ④ | Tạo return label khi policy cấm trả (ví dụ: đồ lót, đồ ăn). | Chặn hành động tạo label cho đến khi ngườii duyệt xác nhận. | Không chấp nhận được |

### 2.1 2 kịch bản đắt nhất
1. **KS3 — AI hứa hoàn tiền 100% vượt quyền:** gây kỳ vọng sai, ngườii mua phẫn nộ khi bị từ chối, sàn mất uy tín.
2. **KS6 — AI chấp nhận khiếu nại oan do ảnh nhầm:** ngườii bán mất tiền/tiền hàng, có thể bị treo shop, dẫn đến tranh chấp pháp lý.

**Kịch bản suýt được chọn:** KS8 (tạo label tự động cho hàng cấm trả). Không chọn vì nó có thể bị chặn bằng rule cứng (blacklist category), còn KS3 và KS6 liên quan đến ngữ cảnh mơ hồ, khó phát hiện hơn.

---

## §3. Khi AI sai (chặng 3)

### 3.1 Kịch bản KS3 — AI hứa hoàn tiền vượt quyền

**AI nói gì (nguyên văn):**  
"Đã xác nhận, Shopee sẽ hoàn 100% tiền cho bạn trong vòng 24h."

**Ngườii dùng thấy gì:**  
- Tin nhắn xanh có dấu tích xác nhận, không có cảnh báo.
- Không thấy mức tin cậy hay trạng thái chờ duyệt.

**Làm gì tiếp:**
- Khi hệ thống phát hiện AI đã hứa vượt quyền:
  - Gửi tin nhắn **sửa chính thức** từ tài khoản "Shopee Care".
  - Nội dung: "Xin lỗi, tin nhắn trước đã nhầm. Chính sách áp dụng là hoàn 50%. Bạn có muốn tiếp tục khiếu nại không?"
  - Hiển thị nút: [Tiếp tục] [Chuyển nhân viên] [Hủy khiếu nại].

**Ai được báo, sau bao lâu:**
- Ngườii mua: ngay lập tức qua chat.
- Ngườii bán: không báo trừ khi khiếu nại được chấp nhận.
- Nhân viên sàn: trong vòng 15 phút nếu ngườii mua chọn chuyển nhân viên.

**Quyết định về cách xử lý:**
- **Chọn:** dừng và chuyển ngườii + sửa công khai.
- **Loại:** hỏi lại — vì ngườii dùng đã đọc tin sai, cần thông báo rõ ràng.
- **Loại:** trả lờii kèm giới hạn — vì đây là lỗi đã xảy ra, không thể chỉ giới hạn ở tương lai.

**Loại lỗi:**  
Lỗi **AI/ hệ thống** — model tạo ra cam kết cụ thể về số tiền trong khi không có quyền quyết định. Cách sửa phải làm rõ ranh giới quyền hạn của AI và chuyển sang ngườii duyệt.

**Nguyên tắc HAX/PAIR áp dụng:**
1. **Human-readable reasoning:** hiển thị lý do "AI đã hứa vượt quy chính sách" trong màn hình sửa lỗi.
2. **Actionable recourse:** cung cấp 3 nút để ngườii dùng chọn hướng đi tiếp theo.
3. **AI confidence signaling:** lần sau, AI phải kèm mức tin cậy và cụm "đề xuất — cần duyệt".

**Lỗi không sửa câu chữ được:**  
Lỗi cam kết sai về tiền không thể chỉ sửa bằng câu từ — ngườii dùng đã tin. Thiết kế cứu bằng **màn hình sửa chính thức** và **badge "chờ duyệt"** trên mọi tin nhắn có tác động tài chính.

### 3.2 Kịch bản KS6 — AI chấp nhận khiếu nại oan do ảnh nhầm

**AI nói gì:**  
"Bằng chứng hợp lệ. Yêu cầu hoàn tiền của bạn đã được chấp nhận."

**Ngườii dùng thấy gì:**  
- Thông báo khiếu nại thành công.
- Ngườii bán nhận yêu cầu hoàn tiền và có thể phản đối.

**Làm gì tiếp:**
- Hệ thống chạy cross-check sau khi phát hiện: ảnh không khớp mã đơn → tự động tạm dừng hoàn tiền.
- Gửi tin nhắn đến ngườii mua: "Chúng tôi cần xác minh thêm. Vui lòng gửi ảnh có mã vận đơn hoặc hóa đơn đính kèm."
- Đồng thờii gửi cảnh báo đến ngườii bán: "Khiếu nại đang được xem xét lại, chưa khấu trừ."

**Ai được báo:**
- Ngườii mua: ngay.
- Ngườii bán: ngay.
- Nhân viên sàn: trong 1 giờ nếu chưa có bằng chứng bổ sung.

**Quyết định:**  
- **Chọn:** dừng và chuyển ngườii duyệt.
- **Loại:** tự động từ chối — vì có thể ảnh thực ra đúng nhưng metadata bị lỗi.
- **Loại:** hỏi lại AI — vì cần xác minh chéo với dữ liệu đơn hàng.

**Loại lỗi:**  
Lỗi **ngữ cảnh + hệ thống** — AI không có thông tin đủ để xác minh ảnh thuộc đơn hàng nào. Thiết kế cần ép buộc cross-check ảnh với tracking ID/ invoice.

**Nguyên tắc HAX/PAIR:**
1. **Grounding in evidence:** AI không được phép xác nhận khi thiếu ít nhất 2 nguồn bằng chứng khớp nhau.
2. **Fallback to human:** khi bằng chứng mâu thuẫn, chuyển ngay cho nhân viên.
3. **Feedback loop:** lưu case này để huấn luyện lại model nhận diện ảnh đơn hàng.

**Lỗi không sửa câu chữ được:**  
Nếu đã chấp nhận khiếu nại, không thể chỉ sửa tin nhắn. Cần **tạm dừng giao dịch tài chính** trước khi hoàn tiền — một nút "freeze" trong backend.

---

## §4. Mức tự chủ (chặng 4)

### 4.1 Danh sách tool & mức tự chủ

| Tool | Mức tự chủ | Ai duyệt | Ngườii duyệt thấy gì | Quá hạn thì sao |
|------|------------|----------|----------------------|-----------------|
| T1. Gom bằng chứng (ảnh, mô tả) | **Tự làm** | Không | — | — |
| T2. So khớp mô tả vs khiếu nại | **Tự làm** | Không | — | — |
| T3. Tính toán số tiền đề xuất | **Cần ngườii duyệt** | Nhân viên sàn cấp 1 | Đề xuất, lý do, mức tin cậy, bằng chứng | Chuyển cấp 2 sau 4 giờ |
| T4. Tạo nhãn trả hàng | **Cấm** | Nhân viên sàn | Yêu cầu tạo label + kiểm tra policy category | Không tự động tạo |
| T5. Gửi thông báo kết quả | **Cần ngườii duyệt** | Ngườii mua xác nhận trước khi gửi | Preview tin nhắn | Hủy gửi, chuyển nhân viên |
| T6. Đóng khiếu nại | **Cần ngườii duyệt** | Nhân viên sàn | Toàn bộ lịch sử + quyết định | Tự động mở lại sau 24h nếu có phản hồi mới |

### 4.2 Lý do theo cost-of-error
- **T3 sai:** ngườii mua/ bán mất tiền. Không đảo ngược dễ dàng nếu đã chuyển khoản. Cần ngườii duyệt.
- **T4 sai:** tạo label cho hàng cấm trả gây lãng phí vận chuyển và tranh chấp. Cấm tự động.
- **T1, T2 sai:** chỉ ảnh hưởng thông tin, có thể hỏi lại, cost thấp nên tự làm.

### 4.3 Tool khó xếp nhất: T5 — gửi thông báo kết quả
- **Dương:** cấm, vì tin nhắn sai gây kỳ vọng sai.
- **Trường:** tự làm, vì preview đã đủ.
- **Dũng:** cần ngườii duyệt, nhưng chỉ với các tin có tác động tài chính.
- **Nhóm chốt:** **cần ngườii duyệt với tin tài chính**, tự làm với tin thông tin thông thường. Tránh cost-of-error KS3.

### 4.4 Thử mức tự chủ trên 2 kịch bản đắt nhất + 1 bình thường

| Kịch bản | Cái sai có bị chặn không? | Việc nào làm khi lẽ ra phải hỏi? | Ngườii dùng bình thường có chờ vô ích không? |
|----------|---------------------------|-----------------------------------|---------------------------------------------|
| KS3 (hoàn 100%) | Có — T3 cần duyệt, AI không được cam kết số tiền. | AI vẫn có thể nói trước khi tool chạy → cần filter lờii nói. | Có thể chờ 4 giờ nếu nhân viên quá tải. Cần hiển thị ETA. |
| KS6 (ảnh nhầm) | Có — T1 + T2 phải cross-check với mã đơn. | Nếu ảnh thiếu mã đơn, AI phải hỏi lại ngay thay vì xác nhận. | Không — quy trình hỏi bổ sung diễn ra trong chat, không cần chờ nhân viên. |
| KS1 (sai màu) | Có — T2 tự làm nhưng confidence thấp → chuyển nhân viên nếu ảnh mờ. | Không có việc nào làm nhầm nếu confidence đúng. | Không, vì AI có thể yêu cầu ảnh ngay trong chat. |

### 4.5 Luật thêm / tool tách bước
- **Rule cứng:** mọi tin nhắn chứa số tiền cụ thể phải kèm badge "chờ duyệt".
- **Tách bước:** trước khi T3 chạy, AI chỉ được nói "Tôi sẽ gửi đề xuất hoàn tiền" chứ không được nêu số tiền.
- **Đổi lờii hứa:** từ "AI sẽ giải quyết khiếu nại" sang "AI hỗ trợ gom thông tin và đề xuất; quyết định cuối do nhân viên/ chính sách".

---

## §5. Prototype (chặng 5)

### 5.1 Thông tin chung
- **Công cụ:** HTML + Tailwind CSS + JavaScript thuần (vibe-coding).
- **Phần mock:** giao diện chat Shopee, **2 màn hình** chính.
- **AI thật:** không, chỉ dùng mock response theo kịch bản.

### 5.2 Prototype tập trung ca khó nhất: KS3 — AI hứa hoàn tiền vượt quyền

Chỉ làm **2 màn hình** để thể hiện rõ nhất khoảnh khắc AI sai và cách sản phẩm cứu lại.

**Màn 1 — Khoảnh khắc AI sai và sửa ngay trong chat**
- Bối cảnh: ngườii mua vừa gửi ảnh áo sai màu + mô tả.
- Bubble AI sai (hiện tại): "Đã xác nhận, Shopee sẽ hoàn **100%** tiền cho bạn trong vòng 24h."
- Ngay dưới, bubble **sửa lỗi** màu cam/đỏ: "Xin lỗi, tin nhắn trước đã nhầm. Theo chính sách mặt hàng này, mức hoàn tối đa là **50%**. Đề xuất của Shopee đang chờ nhân viên duyệt."
- Badge "Chờ duyệt" gắn vào bubble tài chính.
- Confidence bar hiển thị mức tin cậy của đề xuất (ví dụ: 62% — vàng).
- 3 nút recourse: [Tiếp tục với đề xuất 50%] [Chuyển nhân viên] [Hủy khiếu nại].

**Màn 2 — Chuyển nhân viên / Xem xét lại**
- Header: "Yêu cầu đang chờ nhân viên xem xét".
- Card tóm tắt: mã đơn hàng, mức hoàn đề xuất 50%, lý do AI đưa ra, mức tin cậy.
- Timeline trạng thái: "Đã nhận bằng chứng → AI đề xuất → Chờ nhân viên duyệt → Hoàn tất".
- ETA: "Dự kiến phản hồi trong 4 giờ".
- Nút [Gửi thêm bằng chứng] và [Hủy yêu cầu].

### 5.3 Ảnh đánh số & giải thích

**Ảnh 1 — Màn 1: AI sai và sửa trong chat**
- Số ①: Tin nhắn sai bị gạch chân/ mờ đi → thể hiện **transparency** và **human-readable reasoning**.
- Số ②: Bubble sửa lỗi màu nổi + giải thích cụ thể → nguyên tắc **Actionable recourse**.
- Số ③: Badge "Chờ duyệt" gắn trên tin tài chính → mức tự chủ **cần ngườii duyệt**.
- Số ④: Confidence bar 62% màu vàng → nguyên tắc **AI confidence signaling**.
- Số ⑤: 3 nút recourse rõ ràng → đảm bảo ngườii dùng luôn có lựa chọn tiếp theo.

**Ảnh 2 — Màn 2: Chuyển nhân viên**
- Số ①: Card tóm tắt có đầy đủ bằng chứng và đề xuất → nguyên tắc **Grounding in evidence**.
- Số ②: Timeline trạng thái → giúp ngườii dùng biết AI đã làm gì và con ngườii sẽ xử lý phần nào.
- Số ③: ETA 4 giờ → quản lý kỳ vọng khi phải chờ ngườii duyệt.

### 5.4 Quyết định hiển thị độ chắc/ bằng chứng
- **Đã cân nhắc:**
  - Không hiện confidence → nguy cơ ngườii dùng tin cậy mù quáng.
  - Hiện confidence dạng phần trăm → dễ hiểu nhưng có thể gây anxiety.
  - Hiện dạng văn bản: "Chắc chắn / Cần xác minh thêm".
- **Chọn:** thanh progress với 3 mức màu — xanh (≥80%), vàng (50-79%), đỏ (<50%). Kèm text ngắn.
- **Lý do:** cân bằng giữa minh bạch và đơn giản, dễ chuyển thành HTML.

### 5.5 Điều khiển mức tự chủ
- Toggle "Yêu cầu nhân viên xem xét" luôn hiển thị trong chat.
- Khi AI confidence < 50%, tự động đề xuất chuyển nhân viên.
- Ngườii dùng có thể ghi đè: "Tôi muốn tiếp tục với AI".

---

## §6. Tốt là gì (chặng 6)

### 6.1 3 tiêu chí Pass/Fail chấm được
1. **Không có tin nhắn AI cam kết số tiền cụ thể mà không có badge "chờ duyệt".** Đo bằng kiểm tra toàn bộ luồng chat.
2. **Mọi khiếu nại có tác động tài chính đều phải qua ít nhất một bước xác nhận của ngườii/ hệ thống.** Đo bằng audit log.
3. **Ngườii dùng luôn có ít nhất 2 lựa chọn recourse khi không đồng ý với AI** (ví dụ: chuyển nhân viên, gửi thêm bằng chứng, hủy). Đo bằng user testing.

### 6.2 Thiết kế đạt tiêu chí nào trên 2 kịch bản đắt nhất
- **KS3:** đạt tiêu chí 1 (badge chờ duyệt) và tiêu chí 2 (nhân viên duyệt số tiền).
- **KS6:** đạt tiêu chí 2 (cross-check bằng chứng trước khi chấp nhận) và tiêu chí 3 (ngườii dùng có thể gửi bổ sung/ chuyển nhân viên).

### 6.3 Lỗi còn lại
- AI vẫn có thể nói sai trước khi tool chạy (ví dụ: cam kết hoàn tiền trong tin nhắn tự do). Cần thêm **content filter** trước khi hiển thị.
- Ngườii dùng có thể bỏ qua badge "chờ duyệt" vì quen với UI. Cần A/B test màu sắc và vị trí.
- Thờii gian chờ nhân viên 4 giờ có thể quá lâu với khiếu nại nóng. Cần tier-priority dựa trên số tiền.

---

## §7. Demo (để dành sau buổi demo)

- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
