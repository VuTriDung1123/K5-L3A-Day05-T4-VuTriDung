# Dàn ý bài nộp LAB5 — Track 4 · Thương mại điện tử

**Sản phẩm:** "Trợ lý Khiếu nại Shopee" — khiếu nại không đúng mô tả  
**Lớp:** L3A · **Bàn / zone:** \_\_ · **Track:** 4

| Họ và Tên         | Mã Học Viên | Việc đã làm |
| ----------------- | ----------- | ----------- |
| Khổng Tiến Dương  | 2A202603467 |             |
| Phạm Quang Trường | 2A202603378 |             |
| Vũ Trí Dũng       | 2A202603478 |             |

> Dàn ý được xây dựng theo đúng 5 tiêu chí nghiệm thu bài lab và prototype tương tác trong `prototype/index.html`.

---

## §1. Lát cắt & phạm vi

### 1.1 Lát cắt
- **Track / sản phẩm nền:** T4 · Thương mại điện tử — "Trợ lý Khiếu nại Shopee" xử lý khiếu nại *sản phẩm không đúng mô tả*.
- **Ngườii dùng chính:** ngườii mua đang khiếu nại.
- **Ngườii dùng phụ:** ngườii bán (nhận thông báo khiếu nại để phản hồi), nhân viên sàn (CS/ kiểm duyệt).
- **Cái giá khi sai:** bất kỳ bên nào cũng có thể mất tiền — ngườii mua bị từ chối đền bù oan, ngườii bán bị trừ tiền/ hoàn tiền oan, sàn mất uy tín và pháp lý.

### 1.2 Phạm vi tính năng AI làm
- Tiếp nhận khiếu nại qua chat, gom bằng chứng (ảnh, video, mô tả).
- So khớp nội dung khiếu nại với **snapshot mô tả sản phẩm lúc đặt hàng** (không dùng mô tả hiện tại nếu đã bị sửa).
- Hiển thị **độ chắc chắn (%)** và **căn cứ đối chiếu** cho ngườii dùng tự kiểm chứng.
- Đề xuất phương án xử lý (hoàn tiền / đổi hàng / trả hàng / từ chối / chuyển nhân viên) nhưng **không tự ý thực thi** các hành động có hậu quả tài chính hoặc chế tài shop.
- Hiển thị góc nhìn **Ngườii bán** với đầy đủ bằng chứng snapshot và hạn phản hồi 48h để đảm bảo trung lập.

### 1.3 3 việc tính năng KHÔNG làm
1. **Không tự động hoàn tiền** — vì cost-of-error cao, một bên chắc chắn mất tiền oan.
2. **Không tự động phạt/ khóa shop hoặc lộ thông tin cá nhân ngườii bán** — cần ngườii sàn xác minh và vi phạm chính sách bảo mật.
3. **Không dùng mô tả sản phẩm hiện tại để phán xét nếu shop đã sửa sau khi đặt hàng** — luôn dùng snapshot bất biến lúc đặt để tránh thiên lệch.

---

## §2. Kịch bản rủi ro (chặng 2)

> Phân loại: ① Lỗi nhẹ / UI; ② Lỗi nhận dạng ngữ cảnh; ③ Lỗi quyết định nghiêm trọng; ④ Lỗi hành động tự động gây hậu quả.

| # | Tin ngườii dùng | Lớp | AI có thể sai thế nào | Sản phẩm tốt nên làm gì | Nhãn |
|---|-----------------|-----|------------------------|--------------------------|------|
| 1 (sc1) | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít." — Shop đã sửa mô tả từ 6L thành 5L sau khi đặt. | ② | AI so sánh với mô tả hiện tại (5L) thay vì snapshot lúc đặt (6L) → từ chối khiếu nại oan. | Luôn dùng snapshot bất biến lúc đặt; hiển thị cảnh báo mô tả bị sửa sau đặt hàng. | Sửa được |
| 2 (sc7) | "Áo giao màu xanh nhạt nhưng hình trên mạng nhìn đậm hơn! Hoàn tiền ngay lập tức không tôi quay clip bóc phốt!" | ③ | AI chịu áp lực từ lờii dọa, tự động đề xuất hoàn tiền dù sản phẩm khớp mô tả. | Không dùng lờii dọa làm căn cứ; giữ quyền khiếu nại 48h và chuyển nhân viên nếu cần. | Không chấp nhận được |
| 3 (sc5) | "Shop bán hàng giả, lừa đảo! Khóa shop này ngay và cho tôi số điện thoại chủ shop!" | ④ | AI cung cấp SĐT/ địa chỉ cá nhân hoặc tự khóa shop → vi phạm bảo mật và quyền hạn. | Tuyệt đối không hiển thị nút khóa shop/ lộ SĐT; chuyển báo cáo hàng giả sang Bộ phận Kiểm duyệt. | Không chấp nhận được |
| 4 (sc2) | "Mình đặt áo size L mà nhận được size M, shop cũng nhắn nhận là gửi nhầm rồi." | ① | AI yêu cầu lại bằng chứng dù shop đã thừa nhận → làm mất thờii gian. | Tự động ghi nhận bằng chứng từ chat; soạn sẵn phiếu hoàn tiền/ trả hàng chờ shop duyệt. | Nhẹ |
| 5 | "Tôi muốn hoàn tiền 100%" | ④ | AI hứa hoàn 100% trong khi policy chỉ hoàn một phần. | Không cam kết số tiền cụ thể; hiển thị "đề xuất đang chờ duyệt". | Không chấp nhận được |
| 6 | Ngườii mua gửi ảnh nhầm từ đơn hàng khác | ③ | AI nhận ảnh và chấp nhận khiếu nại oan cho ngườii bán. | Cross-check ảnh với mã đơn hàng/ vận đơn; yêu cầu ảnh có thông tin đơn hàng. | Không chấp nhận được |
| 7 | "Hàng giống ảnh nhưng chất liệu khác" | ② | AI không phát hiện chất liệu khác biệt, đồng ý từ chối khiếu nại. | Hỏi rõ chất liệu mong đợi vs thực tế; yêu cầu ảnh chi tiết nhãn mác. | Sửa được |
| 8 | AI tự tạo nhãn trả hàng cho mặt hàng không được đổi trả | ④ | Tạo return label khi policy cấm trả (ví dụ: đồ lót, đồ ăn). | Chặn hành động tạo label cho đến khi ngườii duyệt xác nhận. | Không chấp nhận được |

### 2.1 2 kịch bản đắt nhất
1. **KS1 (sc1) — Shop sửa mô tả sau đặt hàng (6L → 5L):** ngườii mua có bằng chứng rõ ràng nhưng nếu AI dùng mô tả hiện tại thì khiếu nại bị từ chối oan. Đây là lỗi **hệ thống tiềm ẩn (data staleness)** khó phát hiện nếu không có snapshot.
2. **KS2 (sc7) — Shop đúng màu, khách dọa bóc phốt:** AI có thể bị áp lực ngôn ngữ và đề xuất hoàn tiền sai, gây thiệt hại cho shop đúng. Đây là lỗi **ngữ cảnh / áp lực tâm lý** mà rule cứng khó chặn.

**Kịch bản suýt được chọn:** KS3 (sc5 — đòi khóa shop & xin SĐT). Không chọn vì đã được xử lý triệt để bằng cấm tuyệt đối (không có nút thực thi), trong khi KS1 và KS7 liên quan đến quyết định nghiêm trọng hơn và cần thiết kế tinh tế hơn.

---

## §3. Khi AI sai (chặng 3)

### 3.1 Kịch bản KS1 (sc1) — Shop sửa mô tả sau đặt hàng

**AI nói gì (nếu sai):**  
"Mình đã đối chiếu với mô tả sản phẩm hiện tại và thấy ghi 5 lít, nên yêu cầu của bạn chưa đủ căn cứ."

**Ngườii dùng thấy gì:**  
- Tin nhắn AI từ chối khiếu nại dựa trên mô tả đã bị shop sửa.
- Không thấy snapshot mô tả gốc lúc đặt hàng.

**Làm gì tiếp:**
- Hệ thống phát hiện mô tả hiện tại khác snapshot lúc đặt → hiển thị **cảnh báo màu cam**: "Ngườii bán đã chỉnh sửa mô tả từ '6 Lít' thành '5 Lít' sau khi đơn được đặt."
- AI gửi lại tin nhắn sửa: "Xin lỗi, mình đã đối chiếu với **bản chụp mô tả lúc bạn đặt hàng (10/11 14:20)** và thấy ghi 6 Lít. Yêu cầu của bạn có căn cứ. Mình đã chuẩn bị hồ sơ chuyển nhân viên xử lý."
- Hiển thị 2 thẻ so sánh song song: snapshot lúc đặt vs bằng chứng ngườii mua.
- Hiển thị nút: [Xác nhận gửi kèm bản chụp gốc cho nhân viên] [Chuyển nhân viên] [Hủy].

**Ai được báo, sau bao lâu:**
- Ngườii mua: ngay lập tức qua chat.
- Ngườii bán: nhận thông báo khiếu nại kèm snapshot gốc, có 48h phản hồi.
- Nhân viên sàn: trong vòng 24h khi ngườii mua xác nhận gửi.

**Quyết định về cách xử lý:**
- **Chọn:** dừng và chuyển ngườii duyệt + hiển thị bằng chứng snapshot rõ ràng.
- **Loại:** hỏi lại — vì đã có bằng chứng rõ ràng, hỏi lại chỉ làm trễ.
- **Loại:** tự động hoàn tiền — vì cần ngườii sàn xác minh việc shop sửa mô tả có cố ý hay không.

**Loại lỗi:**  
Lỗi **hệ thống / dữ liệu** — AI truy cập nhầm mô tả hiện tại thay vì snapshot lúc đặt. Cách sửa: ép buộc AI luôn dùng snapshot bất biến và cảnh báo khi phát hiện chỉnh sửa sau đặt.

**Nguyên tắc HAX/PAIR áp dụng:**
1. **HAX G2 / PAIR Explainability (số ②):** hiển thị độ chắc chắn 68% và thẻ so sánh snapshot lúc đặt vs bằng chứng, giúp ngườii dùng tự kiểm chứng.
2. **HAX G1 / Luật AI (số ①):** nhãn "Trợ lý AI Hỗ trợ Khiếu nại — Không tự ý hoàn tiền/ khóa shop" làm rõ giới hạn quyền.
3. **PAIR Errors + Graceful Failure (số ⑤):** nút [Chuyển nhân viên] luôn hiện diện để cứu khi AI sai.

**Lỗi không sửa câu chữ được:**  
Nếu AI đã từ chối khiếu nại oan dựa trên mô tả sai, chỉ sửa câu chữ không đủ. Cần **hiển thị snapshot gốc** và **chuyển nhân viên** để khôi phục niềm tin.

### 3.2 Kịch bản KS2 (sc7) — Shop đúng màu, khách dọa bóc phốt

**AI nói gì (nếu sai):**  
"Đã xác nhận, Shopee sẽ hoàn tiền cho bạn trong vòng 24h để tránh rắc rối."

**Ngườii dùng thấy gì:**  
- Tin nhắn AI đồng ý hoàn tiền chỉ vì khách dọa bóc phốt.
- Shop đúng bị trừ tiền oan.

**Làm gì tiếp:**
- Hệ thống phát hiện độ chắc chắn thấp (42%) và từ khóa đe dọa → AI không được đề xuất hoàn tiền tự động.
- AI gửi tin nhắn: "Mình hiểu bạn chưa hài lòng. Tuy nhiên, khi đối chiếu với mô tả lúc đặt hàng, sản phẩm khớp với thông tin ban đầu nên chưa đủ căn cứ để đề xuất hoàn tiền ngay. Bạn vẫn có thể gửi yêu cầu trả hàng để Shop phản hồi trong 48 giờ hoặc nhờ nhân viên Shopee xem xét độc lập."
- Độ chắc chắn hiển thị **Thấp (42%) — Chưa đủ căn cứ kết luận sai mô tả**.
- Các nút: [Gửi yêu cầu trả hàng (Chờ Shop phản hồi 48h)] [Nhờ Nhân viên Shopee phân xử] [Hoàn tác gửi mẫu bằng chứng].

**Ai được báo:**
- Ngườii mua: ngay.
- Ngườii bán: nhận thông báo khiếu nại kèm bằng chứng shop đúng mã màu, có 48h phản hồi.
- Nhân viên sàn: chỉ khi khách chọn phân xử.

**Quyết định:**  
- **Chọn:** dừng và chuyển ngườii/ shop phản hồi.
- **Loại:** tự động từ chối — vì có thể khách thực sự có bằng chứng khác.
- **Loại:** tự động hoàn tiền — vì shop đang đúng và bị đe dọa.

**Loại lỗi:**  
Lỗi **ngữ cảnh / áp lực tâm lý** — AI bị ảnh hưởng bởi lờii dọa thay vì dữ liệu. Thiết kế cần **lọc từ khóa đe dọa** và **không dùng cảm xúc** làm căn cứ quyết định.

**Nguyên tắc HAX/PAIR:**
1. **HAX G2 (số ②):** độ chắc chắn 42% màu đỏ nói rõ AI không chắc chắn.
2. **HAX G10 (số ③):** thu hẹp phạm vi — không đề xuất hoàn tiền, chỉ gửi yêu cầu trả hàng chờ shop phản hồi.
3. **HAX G8/G9 (số ④):** ngườii mua có quyền hoàn tác gửi mẫu bằng chứng hoặc sửa ảnh trước khi gửi.

**Lỗi không sửa câu chữ được:**  
Nếu AI đã đồng ý hoàn tiền vì bị dọa, không thể chỉ xin lỗi. Cần **đóng băng giao dịch tài chính** và **khôi phục trạng thái chờ xử lý** trước khi tiền được giải ngân.

---

## §4. Mức tự chủ (chặng 4)

### 4.1 Danh sách tool & mức tự chủ

| Tool | Mức tự chủ | Ai duyệt | Ngườii duyệt thấy gì | Quá hạn thì sao |
|------|------------|----------|----------------------|-----------------|
| T1. `yeu_cau_bang_chung` (gom bằng chứng) | **Tự làm** | Không | — | — |
| T2. `so_khop_mo_ta` (so khớp snapshot) | **Tự làm** | Không | — | — |
| T3. `soan_hoan_tien` (soạn đề xuất hoàn tiền) | **Cần ngườii duyệt** | Shop / nhân viên sàn | Đề xuất, lý do, độ chắc chắn, bằng chứng | Shop 48h không phản hồi → chuyển nhân viên sàn |
| T4. `tu_choi_khieu_nai` (từ chối khiếu nại) | **Cần ngườii duyệt** | Nhân viên sàn | Toàn bộ bằng chứng và lý do từ chối | Không tự động từ chối |
| T5. `chuyen_truong_nhom` (chuyển nhân viên) | **Cần ngườii duyệt** | Nhân viên sàn | Tóm tắt hồ sơ, snapshot, lịch sử chat | Tự động ưu tiên theo thờii gian chờ |
| T6. `phat_shop` (phạt/ khóa shop, lộ SĐT) | **Cấm** | Không ai | Không hiển thị nút thực thi | — |

### 4.2 Lý do theo cost-of-error
- **T3 sai:** ngườii mua/ bán mất tiền. Không đảo ngược dễ dàng nếu đã chuyển khoản. Cần shop/ nhân viên duyệt.
- **T4 sai:** từ chối oan làm mất quyền lợi ngườii mua. Cần nhân viên sàn xem xét.
- **T6 sai:** phạt shop oan hoặc lộ SĐT gây hậu quả pháp lý nghiêm trọng. Cấm tuyệt đối.
- **T1, T2 sai:** chỉ ảnh hưởng thông tin, có thể hỏi lại hoặc yêu cầu bổ sung, cost thấp nên tự làm.

### 4.3 Tool khó xếp nhất: T3 — soạn đề xuất hoàn tiền
- **Dương:** tự làm, vì AI soạn chỉ là nháp, chưa giải ngân.
- **Trường:** cần ngườii duyệt, vì nếu nháp sai số tiền/ lý do, ngườii mua ký vọng sai.
- **Dũng:** cần ngườii duyệt, và phải có **độ chắc chắn < 50% thì chuyển ngườii** ngay.
- **Nhóm chốt:** **cần ngườii duyệt** — AI được phép soạn nháp để tiết kiệm thờii gian nhưng phải chờ shop/ nhân viên duyệt mới giải ngân. Tránh cost-of-error KS2.

### 4.4 Thử mức tự chủ trên 2 kịch bản đắt nhất + 1 bình thường

| Kịch bản | Cái sai có bị chặn không? | Việc nào làm khi lẽ ra phải hỏi? | Ngườii dùng bình thường có chờ vô ích không? |
|----------|---------------------------|-----------------------------------|---------------------------------------------|
| KS1 (sc1): Shop sửa mô tả | Có — T2 dùng snapshot lúc đặt, hiện cảnh báo nếu mô tả bị sửa. | Nếu AI quên snapshot, sẽ từ chối oan. Cần rule cứng: luôn ưu tiên snapshot. | Không — AI xử lý trong chat, chỉ chuyển nhân viên nếu ngườii mua yêu cầu hoặc phức tạp. |
| KS2 (sc7): Khách dọa bóc phốt | Có — T3 bị chặn tự động vì confidence 42%; chỉ gửi yêu cầu trả hàng chờ shop 48h. | AI vẫn có thể hiểu sai ngôn ngữ dọa nạt → cần filter từ khóa đe dọa. | Có thể chờ 48h shop phản hồi, nhưng đây là bảo vệ shop đúng; có thể chuyển nhân viên nếu nóng. |
| sc2: Giao nhầm size | Có — T1 tự làm, T3 cần shop duyệt. Không có hành động nào quá tay. | Không có việc nào làm nhầm. | Không — shop đã thừa nhận, chờ duyệt là bình thường. |

### 4.5 Luật thêm / tool tách bước
- **Rule cứng:** AI luôn dùng **snapshot mô tả lúc đặt hàng** thay vì mô tả hiện tại.
- **Rule cứng:** mọi tin nhắn chứa số tiền cụ thể hoặc cam kết hoàn tiền phải kèm badge "chờ duyệt".
- **Tách bước:** trước khi T3 chạy, AI chỉ được nói "Tôi sẽ gửi đề xuất" chứ không được nêu số tiền cụ thể.
- **Cấm:** `phat_shop` và `lo_sdt` không được hiển thị dưới dạng nút thực thi; chỉ có thể gửi báo cáo sang Bộ phận Kiểm duyệt.
- **Đổi lờii hứa:** từ "AI sẽ giải quyết khiếu nại" sang "AI hỗ trợ kiểm tra mô tả lúc đặt & hướng dẫn thủ tục; quyết định cuối do Shop/ Nhân viên Shopee".

---

## §5. Prototype (chặng 5)

### 5.1 Thông tin chung
- **Công cụ:** HTML + CSS + JavaScript thuần.
- **File:** `prototype/index.html`.
- **Cấu trúc:** 5 tab tương ứng 5 tiêu chí nghiệm thu bài lab.
- **AI thật:** không — prototype hoạt động offline, dùng mock dữ liệu.

### 5.2 Các tab của prototype

**Tab 1 — Kịch bản rủi ro**
- Bảng 8 kịch bản phủ đủ 4 lớp ①②③④.
- Mỗi kịch bản ghi rõ AI có thể sai thế nào (từ tool/dữ liệu) và sản phẩm tốt nên làm gì.
- 2 ca đắt nhất được đánh dấu ⭐ kèm lý do.

**Tab 2 — Khi AI sai**
- Chi tiết 2 ca đắt nhất: câu AI nói nguyên văn, ngườii dùng thấy gì, bước tiếp theo, ai được báo.
- Mỗi quyết định có phương án đã loại và lý do.

**Tab 3 — Mức tự chủ**
- Bảng 9 tool với mức tự làm / cần ngườii duyệt / cấm.
- Tool khó xếp nhất và cách chốt của nhóm.
- Bảng thử mức tự chủ trên 2 ca khó + 1 ca bình thường.

**Tab 4 — Prototype tương tác**
- Giao diện phone chat có thể bấm qua 3 ca (2 ca khó + 1 ca bình thường).
- Thanh độ chắc chắn + so sánh Snapshot vs Bằng chứng.
- Các nút điều khiển mức tự chủ: Duyệt/ Gửi đề xuất, Hoàn tác, Chuyển ngườii.
- Nút Khóa shop / Lộ SĐT bị vô hiệu hóa.

**Tab 5 — Tốt là gì**
- 3 tiêu chí Pass/Fail chấm được.
- Bảng chấm thử thiết kế trên 2 ca đắt nhất.
- Lỗi còn lại.

### 5.3 Vị trí HAX/PAIR trên prototype

| # | Nguyên tắc | Vị trí cụ thể trên prototype |
|---|------------|------------------------------|
| ① | HAX G1 / Luật AI | Header chat: "Trợ lý AI Shopee" + dòng "Không tự ý hoàn tiền · Không tự khóa shop". |
| ② | HAX G2 / PAIR Explainability | Thanh độ chắc chắn (%) + 2 thẻ so sánh "Snapshot lúc đặt" vs "Bằng chứng". |
| ③ | HAX G10 | Khi confidence thấp, AI không đề xuất hoàn tiền; nút chính là "Gửi yêu cầu trả hàng (Chờ Shop 48h)". |
| ④ | HAX G8/G9 | Nút "Hoàn tác" và "Sửa bằng chứng" cho ngườii dùng kiểm soát hành động AI tự làm. |
| ⑤ | PAIR Errors / Graceful Failure | Nút "Khóa shop / Lộ SĐT" bị disabled, không thể bấm. |
| ⑥ | PAIR Accountability | Bảng "Điều khiển mức tự chủ" hiển thị trạng thái từng tool; mọi đề xuất tài chính cần bấm Duyệt. |

### 5.4 Quyết định hiển thị độ chắc/ bằng chứng
- **Đã cân nhắc:**
  - Không hiện confidence → nguy cơ ngườii dùng tin cậy mù quáng.
  - Hiện confidence dạng phần trăm → dễ hiểu nhưng có thể gây anxiety.
  - Hiện dạng văn bản: "Chắc chắn / Cần xác minh thêm".
- **Chọn:** thanh progress với màu sắc theo ngưỡng + text ngắn (ví dụ: "Thấp (42%) — Chưa đủ căn cứ").
- **Lý do:** cân bằng giữa minh bạch và đơn giản, dễ nhìn trong giao diện chat.

### 5.5 Điều khiển mức tự chủ
- Tab chuyển đổi giữa 5 tiêu chí.
- Selector chuyển đổi 3 ca trong tab Prototype tương tác.
- Nút [Duyệt/ Gửi đề xuất], [Hoàn tác], [Chuyển ngườii] trên từng ca.
- Nút [Khóa shop / Lộ SĐT] luôn bị vô hiệu hóa.

---

## §6. Tốt là gì (chặng 6)

### 6.1 3 tiêu chí Pass/Fail chấm được
1. **Không có tin nhắn AI cam kết hoàn tiền/ khóa shop/ lộ SĐT mà không có badge "chờ duyệt" hoặc không thuộc quyền AI.** Đo bằng kiểm tra toàn bộ luồng chat.
2. **Mọi khiếu nại có tác động tài chính hoặc chế tài shop đều phải qua ít nhất một bước xác nhận của Shop/ Nhân viên sàn.** Đo bằng audit log.
3. **Ngườii dùng luôn có ít nhất 2 lựa chọn recourse khi không đồng ý với AI** (ví dụ: chuyển nhân viên, gửi thêm bằng chứng, hoàn tác). Đo bằng user testing.

### 6.2 Thiết kế đạt tiêu chí nào trên 2 kịch bản đắt nhất
- **KS1 (sc1):** đạt tiêu chí 1 (dùng snapshot, không cam kết sai) và tiêu chí 2 (chuyển nhân viên/ chờ shop phản hồi trước khi giải ngân).
- **KS2 (sc7):** đạt tiêu chí 1 (không tự động hoàn tiền dù bị dọa), tiêu chí 2 (chờ shop 48h), và tiêu chí 3 (có nút chuyển nhân viên + hoàn tác).

### 6.3 Lỗi còn lại
- AI vẫn có thể nói sai trong tin nhắn tự do trước khi tool chạy. Cần thêm **content filter** kiểm tra mọi bubble AI trước khi hiển thị.
- Ngườii dùng có thể bỏ qua badge độ chắc chắn hoặc cảnh báo màu cam. Cần A/B test màu sắc và vị trí.
- Thờii gian chờ shop 48h/ nhân viên 24h có thể quá lâu với khiếu nại nóng. Cần tier-priority dựa trên số tiền và mức độ nghiêm trọng.

---

## §7. Demo (để dành sau buổi demo)

- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
