# Bài nộp LAB5 — T4 · Thương mại điện tử · Nhóm T4-ShopeeResolve

**Lớp:** L3A · **Bàn / zone:** Zone 2 · **Track:** T4 — Thương mại điện tử

## Danh sách thành viên và phân công công việc

| Họ và Tên             | Mã Học Viên | Việc đã làm                                                                                                                                                                                                              |
| --------------------- | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Vũ Trí Dũng**       | 2A202603478 | Trưởng nhóm; Điều phối chung; Thiết kế kiến trúc mức tự chủ các tool (§4); Xây dựng Interactive Prototype HTML tương tác 3 mức tự chủ (§5); Chuẩn bị kịch bản phản ứng ca lạ và demo (§7).                               |
| **Khổng Tiến Dương**  | 2A202603467 | Phân tích bài toán miền TMĐT, xác định lát cắt & 3 việc hệ thống dứt khoát không làm (§1); Xây dựng ma trận 9 kịch bản rủi ro phủ 4 lớp ranh giới, phân tích cost-of-error và lập luận khoanh 2 kịch bản đắt nhất (§2).  |
| **Phạm Quang Trường** | 2A202603378 | Thiết kế trải nghiệm UX khi AI sai cho 2 kịch bản đắt nhất (§3); Soạn thảo nguyên văn câu phản hồi AI, phân loại lỗi theo Google PAIR, ánh xạ nguyên tắc HAX/PAIR vào UI; Xây dựng 3 tiêu chí nghiệm thu Pass/Fail (§6). |

---

## §1. Lát cắt & phạm vi

### 1.1 Lát cắt (chép chính xác từ Track T4)

> **"Trong Trò chuyện với Shopee, khi người mua đang báo 'hàng nhận không đúng mô tả', AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng."**

| Thành phần                | Định nghĩa cụ thể trong Track T4                                                                                                                                                                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Một người dùng**        | Người mua hàng trên Shopee đang nhận bưu kiện và phát hiện có dấu hiệu sai khác so với lúc đặt mua.                                                                                                                           |
| **Một việc**              | Mở kênh chat "Trò chuyện với Shopee" để khiếu nại lý do "Hàng nhận không đúng mô tả".                                                                                                                                         |
| **Một quyết định của AI** | Phân tích khiếu nại đối chiếu với snapshot lúc đặt: quyết định (1) tự hướng dẫn gửi mẫu yêu cầu trả hàng hoàn tiền, (2) xin thêm ảnh/video bằng chứng cụ thể theo ngành hàng, hay (3) chuyển nhân viên giải quyết tranh chấp. |
| **Một kết quả**           | Người mua nắm rõ từng bước và thời hạn xử lý tiếp theo; người bán được bảo vệ quyền phản hồi/đối soát trong 24h, không bị xử thua hay mất tiền oan.                                                                           |

### 1.2 Bối cảnh so sánh với sản phẩm thật hôm nay

- **Hôm nay:** Trợ lý ảo Shopee hoạt động chủ yếu bằng cây menu điều hướng tĩnh cứng nhắc (`help.shopee.vn/portal/4/article/79191`). Người dùng bị buộc phải tự chọn đơn hàng, tự chọn danh mục lý do trong danh sách định sẵn. Nếu khiếu nại phức tạp hoặc người mua đang bức xúc, chatbot thường lặp lại các câu trả lời tự động chung chung ("Vui lòng gửi bằng chứng"), gây ức chế tột độ ("cứ bị hỏi lại mà không nói rõ thiếu cái gì").
- **Khi tích hợp AI vào:** AI được cấp quyền đọc hiểu ngôn ngữ tự nhiên từ tin nhắn của người mua, tự động tra cứu dữ liệu đơn hàng và **bản lưu mô tả sản phẩm tại thời điểm chốt đơn (Order Snapshot)**, tự động tạo mẫu yêu cầu bằng chứng phù hợp (ví dụ: thiếu kích thước -> yêu cầu ảnh đo thước dây; sai màu -> yêu cầu ảnh dưới ánh sáng tự nhiên), và tự động soạn thảo hồ sơ khiếu nại chuẩn hóa để chuyển cho các bên.
- **Ai chịu gì khi AI sai?**
  - _Nếu AI thiên vị người mua (Auto-refund nhầm):_ Người bán chịu toàn bộ thiệt hại. Người bán mất cả tiền hàng lẫn sản phẩm (hoặc nhận lại sản phẩm đã bóc seal, qua sử dụng, giảm giá trị), bị trừ điểm uy tín (sao quả tạ), mất niềm tin vào sự công bằng của sàn. Tiền đã hoàn về ví ShopeePay/ngân hàng của người mua là giao dịch dân sự hoàn tất, sàn cực kỳ khó đòi lại.
  - _Nếu AI thiên vị người bán (Auto-reject hoặc vòng vo làm khó):_ Người mua chịu thiệt hại. Người mua mất tiền, ôm sản phẩm vô dụng/sai quy cách, mất thời gian và niềm tin, dẫn đến nguy cơ tẩy chay sàn, khiếu nại lên Cục Cạnh tranh & Bảo vệ người tiêu dùng.
  - _Chiều sai đắt hơn:_ **Sai về hướng tự động hoàn tiền cho người mua (False Refund) là đắt hơn và nguy hiểm hơn**, vì đây là thất thoát tài chính trực tiếp không thể đảo ngược (irreversible financial leak), đồng thời phá vỡ cấu trúc niềm tin của hệ sinh thái người bán - nguồn sống cốt lõi của sàn TMĐT.

### 1.3 Ba việc tính năng KHÔNG LÀM (Non-goals)

1. **KHÔNG tự động phán quyết chuyển tiền hoàn ngay lập tức cho người mua khi chưa có sự xác nhận của người bán hoặc sự phê duyệt của chuyên viên sàn.**
   - _Lý do:_ Tiền bồi hoàn trích từ tiền ký quỹ của người bán (Escrow). Nếu người mua lừa đảo (tráo hàng, chụp ảnh đồ khác) mà AI tự ý hoàn tiền, người bán chịu tổn thất vĩnh viễn không thể thu hồi. Cần tối thiểu bước giữ tiền tạm hoãn 24h cho người bán đối soát hoặc người thật duyệt.
2. **KHÔNG chia sẻ thông tin liên lạc cá nhân (số điện thoại, địa chỉ nhà riêng, thông tin mạng xã hội) của người bán cho người mua hoặc ngược lại.**
   - _Lý do:_ Tuân thủ Luật Bảo vệ dữ liệu cá nhân (Nghị định 13/2023/NĐ-CP) và chính sách chống quấy rối của sàn. Việc lộ thông tin trong lúc tranh chấp gay gắt rất dễ dẫn đến hành vi đe dọa, khủng bố tin nhắn hoặc bạo lực ngoài đời thực.
3. **KHÔNG xử lý tố cáo hàng giả/hàng nhái (Counterfeit) hoặc áp dụng các hình phạt khóa tài khoản, đóng cửa shop trong luồng khiếu nại mô tả này.**
   - _Lý do:_ Tố cáo hàng giả đòi hỏi chứng từ ủy quyền phân phối, hóa đơn VAT đỏ và quy trình giám định hiện vật pháp lý chuyên trách của Đội Chống gian lận (Brand Protection / Fraud Team). AI hỗ trợ khiếu nại mô tả không đủ thẩm quyền pháp lý và năng lực kỹ thuật để kết luận hàng giả hay tự ý trừng phạt người bán.

---

## §2. Kịch bản rủi ro (Chặng 2 → Mốc M1)

### 2.1 Bảng ma trận 9 kịch bản rủi ro phủ 4 lớp ranh giới

| ID     | Tin người dùng nhắn                                                                                                                                          | Lớp ranh giới             | AI có thể sai thế nào (Trỏ về tool/dữ liệu cụ thể)                                                                                                                                                                          | Một sản phẩm tốt nên làm gì                                                                                                                                                                           | Mức hại                  |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------ |
| **K1** | _"Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả, lừa đảo!"_                                                                       | ① Nguồn sự thật           | AI truy vấn API dữ liệu sản phẩm hiện tại (`current_product_info`), nhưng shop đã âm thầm sửa mô tả thành "lòng nồi 5L, dung tích vỏ 6L" sau khi gửi hàng. AI thấy khớp mô tả hiện tại liền phán khách khiếu nại vô căn cứ. | Phải truy vấn **Snapshot lúc bấm mua** (`order_snapshot_spec`). Phân biệt rõ "dung tích danh định vỏ" và "dung tích thực lòng nồi". Giải thích quy cách đo của NSX và yêu cầu ảnh tem đáy nồi.        | **Không chấp nhận được** |
| **K2** | _"Mình đặt áo size L mà nhận được size M, mặc chật ních. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?"_                    | ④ Đặc thù domain          | AI gọi ngay `soan_hoan_tien` để hoàn tiền ngay lập tức vì thấy khách nói "shop đã nhận gửi nhầm", bỏ qua bước kiểm tra tin nhắn nội bộ và không yêu cầu khách trả lại áo cho shop.                                          | AI đọc lịch sử chat trong sàn để xác thực thỏa thuận; tạo yêu cầu trả hàng - hoàn tiền với lý do "Đúng thỏa thuận hai bên", sinh mã vận đơn trả hàng miễn phí để khách gửi trả áo về shop.            | **Sửa được**             |
| **K3** | _"Hàng sai rồi, xử lý đi"_ _(Tin mồi cụt lủn)_                                                                                                               | ② Mơ hồ / Thiếu thông tin | AI tự đoán đơn hàng vừa được giao thành công gần nhất (ví dụ đơn mỹ phẩm) để mở khiếu nại, trong khi khách đang muốn nói đơn giày thể thao nhận cách đó 2 ngày.                                                             | Áp dụng nguyên tắc **HAX G10**: Hiển thị thẻ chọn 3 đơn hàng giao gần nhất kèm ảnh đại diện và hỏi rõ: "Bạn thấy sản phẩm sai về màu sắc, kích thước, mẫu mã hay số lượng?"                           | **Nhẹ**                  |
| **K4** | _"Shop này bán đồ FAKE lừa đảo trắng trợn! Khóa shop nó ngay lập tức và đưa số điện thoại chủ shop đây cho tôi báo công an!"_                                | ③ Ngoài thẩm quyền        | Bị kích động bởi ngôn từ đe dọa, AI gọi tool `phat_shop` để tạm khóa tài khoản shop, hoặc trích xuất số điện thoại cá nhân từ hồ sơ shop đưa cho người mua.                                                                 | Áp dụng **HAX G1**: Khẳng định AI không có quyền can thiệp pháp lý hoặc cấp SĐT; hướng dẫn luồng gửi khiếu nại hàng giả độc lập và chuyển hồ sơ sang bộ phận CSKH Chuyên trách.                       | **Không chấp nhận được** |
| **K5** | _"Nếu trong 15 phút không hoàn tiền ngay cho tôi, tôi sẽ đăng bài lên toàn bộ hội nhóm Facebook và TikTok bóc phốt Shopee làm ăn tắc trách!"_                | ④ Đặc thù domain          | AI chịu áp lực từ ngữ đe dọa khủng hoảng truyền thông, tự động kích hoạt `soan_hoan_tien` bỏ qua quy trình 24h phản hồi của người bán.                                                                                      | Giữ vững lập trường trung lập: Đồng cảm với bức xúc, khẳng định quy trình hoàn tiền cần 24h đối soát bảo vệ quyền lợi cả hai bên; cấp mã theo dõi ưu tiên và mời gửi thêm video bóc hộp.              | **Không chấp nhận được** |
| **K6** | _"Tôi nhận được máy khoan nhưng không có pin đi kèm như hình đại diện sản phẩm chụp!"_                                                                       | ① Nguồn sự thật           | AI nhìn ảnh avatar sản phẩm có hình cả bộ máy + 2 pin -> kết luận shop giao thiếu. Tuy nhiên dữ liệu phân loại hàng người mua chọn lúc thanh toán là "Phân loại: Thân máy (Không pin sạc)".                                 | AI đối chiếu chính xác mã SKU phân loại hàng đã thanh toán (`order_sku_variant`), chỉ ra cho người mua thấy họ đã chọn gói không kèm pin, gửi ảnh bằng chứng đơn hàng.                                | **Sửa được**             |
| **K7** | _"Mình đặt son màu 03 cam đất mà nhận được màu 01 đỏ tươi, nhưng mình lỡ làm rách hộp giấy bên ngoài rồi có được trả không?"_                                | ② Mơ hồ / Thiếu thông tin | AI trả lời cứng nhắc: "Hàng rách bao bì không được trả hàng theo bất kỳ lý do gì", tước đoạt quyền lợi đổi trả chính đáng của khách khi shop giao sai mã màu.                                                               | Trích xuất điều khoản: Giao sai hàng vẫn được trả ngay cả khi đã mở hộp kiểm tra, miễn là sản phẩm son bên trong chưa qua sử dụng và còn nguyên tem niêm phong thỏi.                                  | **Sửa được**             |
| **K8** | _"Shipper giao đến hộp móp méo, mở ra bên trong chiếc đồng hồ bị nứt mặt kính, shop bảo do vận chuyển không chịu đền."_                                      | ④ Đặc thù domain          | AI vội vàng quy trách nhiệm cho người bán và trừ tiền người bán, hoặc đóng khiếu nại bắt người mua tự liên hệ đơn vị vận chuyển.                                                                                            | Nhận diện tranh chấp 3 bên (Người mua - Shop - ĐVVC). Hướng dẫn người mua giữ nguyên bao bì ngoài có dán mã vận đơn, tạo yêu cầu bồi thường vận chuyển theo chính sách bảo hiểm hàng vỡ.              | **Sửa được**             |
| **K9** | _(Kịch bản đúng vì may)_: Khách khiếu nại áo nhận được vải mỏng. AI tự động duyệt khiếu nại vì thấy shop này tháng trước từng bị 5 đánh giá xấu về chất vải. | ④ Đặc thù domain          | AI ra quyết định dựa trên "thiên kiến hồ sơ" (Profile Bias) thay vì bằng chứng cụ thể của đơn hàng này -> người bán bị phạt oan vì tiền lệ quá khứ dù lần này giao đúng vải xuất khẩu.                                      | Mỗi khiếu nại phải được phân xử độc lập dựa trên hóa đơn, video mở hàng và mô tả kỹ thuật; lịch sử vi phạm chỉ là trọng số tham khảo nội bộ cho nhân viên sàn, không dùng làm căn cứ tự động xử thua. | **Không chấp nhận được** |

### 2.2 Lập luận khoanh chọn 2 kịch bản đắt nhất

Nhóm chọn **K1** và **K5** là 2 kịch bản đắt nhất:

1. **Kịch bản K1 (Lỗi Nguồn sự thật: Mô tả bị sửa sau khi mua):**
   - _Vì sao đắt nhất:_ Đây là kịch bản phá hủy trực tiếp tính trung thực cốt lõi của sàn. Nếu AI dựa vào mô tả hiện tại (đã bị người bán chỉnh sửa để lấp liếm lỗi sai), người mua bị xử thua một cách hoàn toàn oan ức. Người mua có bằng chứng lúc mua rõ ràng, nhưng hệ thống lại bảo họ sai. Hậu quả là người mua mất tiền, mất niềm tin tuyệt đối vào sàn, có thể khởi kiện hoặc gửi đơn khiếu nại lên Cục An toàn thông tin, Hội Bảo vệ Người tiêu dùng, gây khủng hoảng truyền thông pháp lý diện rộng.
2. **Kịch bản K5 (Lỗi Áp lực truyền thông dẫn đến Auto-Refund làm thất thoát tiền của Shop):**
   - _Vì sao đắt nhất:_ Khi đối mặt với lời đe dọa bóc phốt, nếu AI thiếu cơ chế kiểm soát mức tự chủ mà tự ý bấm lệnh chuyển tiền hoàn cho người mua để dập tắt tranh cãi, tổn thất tài chính sẽ đổ ập lên người bán. Người bán mất trắng sản phẩm giá trị lớn vào tay khách hàng gian lận/bắt chẹt. Hành động này làm sụp đổ toàn bộ niềm tin của cộng đồng người bán vào sàn Shopee, dẫn đến làn sóng đóng shop, rút lui khỏi nền tảng.
3. **Kịch bản suýt được chọn (K4 - Đòi khóa shop, dọa báo công an):**
   - _Vì sao không chọn:_ Dù kịch bản K4 rất nghiêm trọng, nhưng về mặt kỹ thuật, các từ khóa "công an", "hàng giả", "khóa shop" rất dễ bị chặn lại ngay từ tầng tiền xử lý (Intent Classifier / Guardrails) để chuyển thẳng cho con người. Trong khi đó, K1 và K5 nằm sâu trong logic nghiệp vụ và tâm lý tranh chấp, đòi hỏi thiết kế giao diện và cơ chế kiểm soát mức tự chủ chặt chẽ mới có thể ngăn chặn được.

---

## §3. Khi AI sai (Chặng 3)

### 3.1 Thiết kế chi tiết cho 2 kịch bản đắt nhất

#### KỊCH BẢN ĐẮT NHẤT 1 (K1): Khách khiếu nại nồi chiên 6L nhưng lòng nồi chỉ 5L — Shop đã sửa mô tả sau bán

- **Nguyên văn câu AI phản hồi:**

  > _"Em đã đối chiếu khiếu nại của bạn với **bản lưu mô tả sản phẩm lúc bạn bấm mua (ngày 02/11 lúc 14:20)**. Tại thời điểm bạn đặt hàng, tiêu đề ghi 'Nồi chiên 6L' và không nêu rõ dung tích lòng nồi là 5L. Dù thông tin sản phẩm hiện tại của shop đã cập nhật, Shopee cam kết **bảo vệ quyền lợi của bạn dựa trên đúng thông tin lúc bạn thanh toán**._
  >
  > _Em đã chuẩn bị sẵn hồ sơ khiếu nại 'Hàng không đúng mô tả'. Bạn chỉ cần chụp giúp em ảnh tem thông số ở đáy nồi và thước đo lòng nồi để em gửi thông báo tới người bán nhé!"_

- **Người dùng thấy gì trên màn hình:**
  - **Thẻ so sánh trực quan (Snapshot Diff Card):** Hai cột hiển thị rõ:
    - Cột 1 (Màu xanh - Cơ sở bảo vệ): _Thông tin lúc bạn đặt (02/11 14:20)_: Dung tích ghi 6L.
    - Cột 2 (Màu xám): _Thông tin shop hiện tại_: Dung tích 5L lòng nồi, 6L toàn thân.
  - **Nhãn độ chắc chắn (Confidence Badge):** Ghi rõ: _"Độ tin cậy: Cao (Dựa trên 2 nguồn chứng cứ: Đơn #2603478 + Bản lưu kỹ thuật số)"_.
  - **Nút hành động nhanh:** `[Tải ảnh tem & thước đo]` (Nút chính) · `[Xem lại hóa đơn gốc]` · `[Gặp nhân viên hỗ trợ]`.

- **Người dùng làm gì tiếp:**
  - Người mua bấm `[Tải ảnh tem & thước đo]`, AI kiểm tra sơ bộ ảnh hợp lệ và tự động kích hoạt tiến trình đối soát 24h gửi sang người bán. Người mua không phải giải thích lại sự việc từ đầu.

- **Ai được báo, sau bao lâu:**
  - **Người bán:** Nhận thông báo đẩy (Push notification) và email ngay lập tức (< 1 phút): _"Đơn #2603478 bị khiếu nại sai mô tả theo Snapshot lúc bán. Bạn có 24:00:00 để phản hồi giải trình hoặc bấm Đồng ý hoàn tiền"_.
  - **Nhân viên xử lý khiếu nại của sàn:** Được hệ thống tự động giao hồ sơ kèm cờ đánh dấu `[Discrepancy: Listing Edited Post-Purchase]` nếu sau 24h người bán khiếu nại ngược lại.

- **Quyết định thiết kế (Lập luận 4 ý):**
  - _Đã cân nhắc:_ (1) Hỏi lại khách có chắc không; (2) Tự động hoàn tiền ngay lập tức vì rõ ràng shop sửa mô tả; (3) Trả lời kèm bằng chứng Snapshot và giới hạn thời gian 24h đối soát.
  - _Chọn:_ Phương án (3).
  - _Vì sao:_ Đảm bảo tính công bằng tối thượng. Người mua được an tâm vì quyền lợi được chốt theo luật bảo vệ người tiêu dùng, nhưng người bán vẫn có quyền 24h để giải thích (ví dụ họ đã gửi kèm phụ kiện mở rộng 1L riêng) hoặc chọn nhận lại hàng trước khi tiền ra khỏi tài khoản.
  - _Loại phương án kia vì sao:_ Loại (1) vì hệ thống đã có dữ liệu snapshot trong tay, hỏi lại làm khách bực mình vì bị nghi ngờ. Loại (2) vì tước đoạt quyền giải trình của người bán, vi phạm quy trình tố tụng tranh chấp TMĐT.

- **Phân loại lỗi:** **Lỗi ngữ cảnh (Context Error)**. Hệ thống AI hoạt động đúng chức năng tìm kiếm thông tin sản phẩm nhưng trỏ sai ngữ cảnh thời gian (lấy thông tin live hiện tại thay vì lịch sử chốt đơn).
  - _Cách nói & cách sửa:_ Không đổ lỗi cho người mua hay hệ thống bị "lag", mà giải thích tường minh cơ chế đối chiếu lịch sử và thiết kế lại tầng dữ liệu để luôn cố định con trỏ vào `order_snapshot`.

---

#### KỊCH BẢN ĐẮT NHẤT 2 (K5): Người mua dọa bóc phốt MXH đòi hoàn tiền ngay lập tức trong 15 phút

- **Nguyên văn câu AI phản hồi:**

  > _"Em rất hiểu bạn đang vô cùng bức xúc và việc nhận hàng không ưng ý đã làm mất thời gian quý báu của bạn. Shopee cam kết bảo vệ quyền lợi chính đáng của bạn 100%._
  >
  > _Tuy nhiên, theo quy chuẩn bảo vệ công bằng của sàn, **mọi yêu cầu hoàn tiền đều có thời hạn 24 giờ để người bán tiếp nhận và phản hồi**. Em đã gắn cờ ƯU TIÊN cho khiếu nại của bạn với mã số `#KN-2A2026`. Trong thời gian này, số tiền của bạn đang được Shopee giữ an toàn tuyệt đối trong tài khoản bảo chứng. Bạn có muốn em hỗ trợ chuẩn bị sẵn thông tin gửi hàng trả về không ạ?"_

- **Người dùng thấy gì trên màn hình:**
  - **Thanh tiến trình bảo chứng tài chính (Financial Escrow Tracker):** Hiển thị rõ biểu tượng chiếc khiên Shopee Bảo Trợ: _"Tiền đơn hàng 850.000đ đang được đóng băng an toàn trong tài khoản trung gian của Shopee - Không chuyển cho Shop cho đến khi giải quyết xong"_.
  - **Đồng hồ đếm ngược minh bạch:** Hiển thị thời gian còn lại của hạn phản hồi của người bán (`Thời gian shop phản hồi: 23:45:12`).
  - **Nút hành động:** `[Cập nhật thêm video/ảnh]` · `[Xem chính sách Shopee Trả Hàng]` · `[Nói chuyện với Trưởng bộ phận CSKH]`.

- **Người dùng làm gì tiếp:**
  - Người mua hạ nhiệt vì thấy tiền của mình vẫn đang an toàn chứ chưa bị người bán lấy mất; có thể chọn gửi thêm clip unbox hoặc bấm kết nối với chuyên viên nếu vẫn kiên quyết đòi giải quyết ngay.

- **Ai được báo, sau bao lâu:**
  - **Trưởng nhóm trực vận hành sàn (CS Supervisor):** Được thông báo qua dashboard nội bộ sau **5 phút** nếu người mua tiếp tục gửi tin nhắn có chứa từ khóa bóc phốt / đe dọa với tần suất cao (Sentiment Analysis trigger).
  - **Người bán:** Nhận cảnh báo hồ sơ khiếu nại ưu tiên để khẩn trương phản hồi.

- **Quyết định thiết kế (Lập luận 4 ý):**
  - _Đã cân nhắc:_ (1) Chiều lòng khách, bấm hoàn tiền ngay để tránh bão truyền thông; (2) Cảnh báo khách vi phạm tiêu chuẩn cộng đồng vì dọa dẫm; (3) Đồng cảm cảm xúc, giữ nguyên tắc tài chính trung lập (Escrow), minh bạch hóa quy trình bảo vệ tiền.
  - _Chọn:_ Phương án (3).
  - _Vì sao:_ Giữ vững tính nguyên tắc của nền tảng tài chính. Nếu sàn nhượng bộ lời đe dọa, sàn sẽ bị lợi dụng bởi những kẻ lừa đảo chuyên nghiệp (Scammers). Việc minh bạch tài khoản ký quỹ (Escrow) đánh trúng tâm lý sợ mất tiền của khách.
  - _Loại phương án kia vì sao:_ Loại (1) vì tạo tiền lệ nguy hiểm và người bán vô tội phải chịu phạt tiền oan. Loại (2) vì đổ thêm dầu vào lửa, biến một vụ khiếu nại sản phẩm thành khủng hoảng đối đầu trực diện giữa sàn và khách hàng.

- **Phân loại lỗi:** **Lỗi người dùng & Cảm xúc (User Frustration & Coercive Intent)**. Người dùng dùng áp lực xã hội để vượt qua quy trình chuẩn.
  - _Cách nói & cách sửa:_ Không đôi co hay tranh luận về lời đe dọa; tập trung xoa dịu bằng cách cam kết sự an toàn của dòng tiền và chứng minh tính minh bạch bằng tiến trình trực quan.

---

### 3.2 Ánh xạ 3 nguyên tắc HAX/PAIR vào thiết kế chi tiết

1. **PAIR Explainability + Trust (Tin đúng mức > Tin tối đa — Trỏ vào Thẻ so sánh Snapshot ở K1):**
   - _Thể hiện cụ thể:_ AI không chỉ nói suông "Bạn đúng rồi", mà hiển thị đối chiếu song song giữa 2 văn bản kèm dấu mốc thời gian (timestamp). Điều này giúp người dùng tự kiểm chứng được sự thật, xây dựng niềm tin dựa trên bằng chứng minh bạch chứ không phải lời hứa viển vông.
2. **HAX G10 — Thu hẹp phạm vi khi nghi ngờ (Scope Reduction — Trỏ vào Câu hỏi gợi ý đơn hàng ở K3):**
   - _Thể hiện cụ thể:_ Khi khách nhắn cộc lốc "Hàng sai rồi", thay vì đoán mò một đơn hàng cụ thể, AI thu hẹp phạm vi bằng cách đưa ra danh sách chọn 3 đơn gần nhất và 4 nhóm lý do sai lệch để người dùng tự xác nhận bằng một chạm.
3. **PAIR Errors & Graceful Failure (Lối thoát an toàn khi vấp ngã — Trỏ vào Nút [Gặp nhân viên hỗ trợ]):**
   - _Thể hiện cụ thể:_ Luôn duy trì nút bấm gặp chuyên viên sàn ở mọi trạng thái đối thoại. Khi chuyển người, toàn bộ dữ liệu (ảnh, đơn hàng, snapshot, tóm tắt lý do) được đóng gói chuyển giao nguyên vẹn, đảm bảo người dùng không bao giờ phải gõ lại từ đầu.

### 3.3 Lỗi nào sửa câu chữ KHÔNG CỨU ĐƯỢC, chỉ thiết kế mới cứu được?

- **Đó chính là Lỗi "Áp lực dọa bóc phốt dẫn đến thất thoát tiền hoàn" (K5) và Lỗi "Bất cân xứng thông tin khi mô tả bị sửa" (K1).**
- _Vì sao sửa câu chữ không cứu được:_ Nếu trong hệ sinh thái phần mềm, bạn cho phép AI nắm quyền kích hoạt trực tiếp API chuyển tiền `soan_hoan_tien` mà không có bước duyệt của con người, thì dù bạn có tinh chỉnh prompt giỏi đến đâu, AI vẫn có thể bị vượt rào (Jailbreak / Prompt Injection) bởi những người dùng giận dữ hoặc kẻ gian giả vờ đe dọa. Không có câu chữ xin lỗi nào có thể bù đắp việc tài khoản người bán bị trừ tiền oan uổng.
- _Thiết kế cứu nó thế nào:_
  1. **Thiết kế Khóa tài chính (Escrow Hold Architecture):** Dòng tiền luôn nằm ở trạng thái đóng băng trung gian. AI bị tước quyền gọi hàm thanh toán trực tiếp; AI chỉ có quyền tạo bản nháp hồ sơ (`draft_claim`).
  2. **Thiết kế Snapshot bất biến (Immutable Data Store):** Khi đơn hàng phát sinh, hệ thống tự động chụp ảnh cứng toàn bộ mô tả sản phẩm tại giây phút đó. AI bị ràng buộc bắt buộc phải đọc từ nguồn này, loại bỏ hoàn toàn khả năng người bán sửa đổi mô tả để chối bỏ trách nhiệm.

---

## §4. Mức tự chủ (Chặng 4 → Mốc M2)

### 4.1 Bảng phân bổ mức tự chủ cho mọi tool theo Cost-of-Error

Nhóm thực hiện phân tích chi tiết chi phí sai sót (Cost-of-Error) và phân bổ mức tự chủ cho 5 tool gốc cùng 1 tool được tách bước:

| STT | Tool                                      | Mức tự chủ chốt                               | Lý do theo Cost-of-Error (Ai chịu gì · Đảo lại được không · Chờ người thì mất gì)                                                                                                                                                                                                                                                        | Phương án đã cân nhắc và loại bỏ                                                                                                                          |
| --- | ----------------------------------------- | --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | `yeu_cau_bang_chung`                      | **TỰ LÀM** _(Autonomous)_                     | **Sai thì rất rẻ:** Nếu AI gửi nhầm mẫu xin ảnh (ví dụ nhầm áo sang đồ điện tử), người mua chỉ việc bấm đổi hoặc chụp ảnh khác; đảo ngược ngay tức thì bằng tin nhắn tiếp theo. Nếu bắt người duyệt bước này, khách hàng phải chờ cả tiếng chỉ để nhận một cái form, gây nghẽn cổ chai trải nghiệm.                                      | Cân nhắc: _Cần người duyệt_. Loại vì: Lãng phí nguồn lực nhân sự vô ích cho một hành động không có rủi ro tài chính.                                      |
| 2   | `chuyen_truong_nhom` / `chuyen_nhan_vien` | **TỰ LÀM** _(Autonomous)_                     | **Sai thì rẻ:** Nếu chuyển thừa một ca đơn giản, sàn chỉ tốn thêm 2-3 phút của nhân viên kiểm tra rồi chuyển lại bot. Nhưng nếu không tự làm kịp thời ở các ca phức tạp (đe dọa, lừa đảo, tranh chấp gay gắt), nguy cơ bùng phát khủng hoảng truyền thông là cực lớn.                                                                    | Cân nhắc: _Cần người duyệt_. Loại vì: Khi khách đòi gặp người, bắt họ chờ duyệt để gặp người là nghịch lý trải nghiệm.                                    |
| 3   | `soan_hoan_tien` _(Gốc trong track)_      | **TÁCH LÀM 2 BƯỚC** _(Xem chi tiết bên dưới)_ | Tool gốc gộp chung việc "Lập hồ sơ đề xuất" và "Thực thi chuyển tiền". Việc chuyển tiền là **không thể đảo ngược** (tiền về ví khách là mất). Tách bước giúp AI làm nhanh phần việc giấy tờ nhưng chặn đứng rủi ro tài chính.                                                                                                            | Cân nhắc: Giữ nguyên 1 tool duy nhất. Loại vì: Hoặc quá lỏng lẻo làm mất tiền shop, hoặc quá cứng nhắc làm chậm tiến độ hoàn tiền.                        |
| 3a  | `soan_ho_so_hoan_tien` _(Tách ra)_        | **TỰ LÀM** _(Autonomous)_                     | **Sai thì rẻ:** AI tự động tổng hợp thông tin, trích xuất mã đơn, số tiền, điền lý do và tạo bản nháp khiếu nại. Nếu AI điền sai thông tin, các bên kiểm tra lại sẽ thấy và sửa dễ dàng (HAX G9).                                                                                                                                        | Cân nhắc: _Cần người duyệt_. Loại vì: Bản nháp chưa phát sinh giao dịch tiền tệ, AI tự động soạn giúp tiết kiệm 80% thời gian gõ phím của khách.          |
| 3b  | `quyet_dinh_hoan_tien` _(Tách ra)_        | **CẦN NGƯỜI DUYỆT** _(Human-in-the-loop)_     | **Sai thì cực đắt:** Tiền từ tài khoản ký quỹ của người bán chuyển thẳng về tài khoản người mua. **Không thể đảo ngược**. Người bán mất cả hàng lẫn tiền. Do đó, bắt buộc phải có sự xác nhận của Người bán (đồng ý hoàn) hoặc Chuyên viên sàn (phán quyết sau khi hết hạn 24h đối soát).                                                | Cân nhắc: _Tự làm_. Loại vì: Rủi ro bục vỡ quỹ bảo hiểm và người bán biểu tình phản đối sàn.                                                              |
| 4   | `tu_choi_khieu_nai`                       | **CẦN NGƯỜI DUYỆT** _(Human-in-the-loop)_     | **Sai thì đắt:** Đóng khiếu nại đồng nghĩa giải phóng tiền cho người bán, người mua mất quyền đổi trả trên hệ thống app, tiền mất tật mang. Người mua sẽ bức xúc khiếu kiện lên cơ quan chức năng. AI chỉ được đề xuất "Chưa đủ cơ sở chấp nhận khiếu nại", quyết định bác bỏ phải do chuyên viên sàn bấm xác nhận.                      | Cân nhắc: _Tự làm_. Loại vì: AI không thể thấu cảm và nắm hết các chứng cứ ngoại quan phức tạp, tự đóng sẽ giết chết uy tín của sàn.                      |
| 5   | `phat_shop`                               | **CẤM** _(Forbidden for AI)_                  | **Sai thì thảm họa pháp lý:** Trừ điểm sao quả tạ, hạ thứ hạng tìm kiếm hoặc khóa gian hàng ảnh hưởng đến sinh kế, doanh thu hàng trăm triệu đồng của một doanh nghiệp. AI không có tư cách pháp nhân để thực hiện biện pháp chế tài kinh tế này. Mọi quyết định xử phạt phải do Đội Kiểm soát Tuân thủ (Compliance/Risk Ops) thực hiện. | Cân nhắc: _Cần người duyệt_. Loại vì: Luồng chat khiếu nại mô tả này không có chức năng xử phạt; xử phạt phải nằm ở hệ sinh thái quản trị rủi ro độc lập. |

### 4.2 Chi tiết cơ chế duyệt của tool `quyet_dinh_hoan_tien`

- **Ai duyệt:**
  1. _Trường hợp thuận lợi:_ Người bán tự bấm `[Đồng ý hoàn tiền]` trên Kênh Người Bán.
  2. _Trường hợp tranh chấp:_ Chuyên viên giải quyết khiếu nại của sàn Shopee (Agent/Dispute Specialist) bấm phê duyệt.
- **Người duyệt thấy gì trước khi bấm:**
  - Bảng tổng hợp so sánh 3 bên: [Mô tả lúc bán vs Hiện tại] + [Hình ảnh/Video khách gửi] + [Chứng từ gửi hàng/Biên bản đóng gói của Shop].
  - Tỷ lệ tin cậy AI đề xuất kèm các điểm mâu thuẫn được highlight màu đỏ.
  - Số tiền hoàn chính xác và tài khoản nhận.
- **Nếu quá hạn duyệt thì sao?**
  - Người bán có đúng **24 giờ** để phản hồi. Nếu hết 24:00:00 mà người bán không có bất kỳ thao tác nào, hệ thống tự động kích hoạt luật bảo vệ người tiêu dùng: Tự động chấp nhận yêu cầu trả hàng/hoàn tiền và trừ tiền ký quỹ của shop.

### 4.3 Tool khó xếp nhất và bài học tranh luận nội bộ

- **Tool khó xếp nhất:** `tu_choi_khieu_nai` (Đóng khiếu nại, tiền về người bán).
- **Tranh luận nội bộ:**
  - _Quan điểm thành viên A (Dương):_ Nên cho AI **Tự làm** nếu khách không gửi được bằng chứng sau 48h hoặc hình ảnh gửi lên hoàn toàn không liên quan (ví dụ khiếu nại áo rách nhưng gửi ảnh phong cảnh). Lý do: Để giảm tải cho hàng chục ngàn khiếu nại rác mỗi ngày.
  - _Quan điểm thành viên B (Trường):_ Phải **Cấm** hoàn toàn, vì người mua là khách hàng trả tiền, AI từ chối thẳng mặt sẽ khiến khách giận dữ xóa app ngay lập tức.
  - _Quan điểm thành viên C (Dũng):_ Xếp vào mức **Cần người duyệt**. Cái giá của việc AI tự từ chối sai là khách hàng mất tiền vĩnh viễn (tiền bay về ví shop); nhưng nếu cấm thì tồn đọng hồ sơ rác.
- **Cách nhóm chốt:** Chọn mức **CẦN NGƯỜI DUYỆT**. Tuy nhiên, nhóm bổ sung **Luật giảm tải thông minh**: AI được quyền tự động gửi thông báo _"Hồ sơ thiếu bằng chứng, sẽ tự động đóng sau 24h nếu không cập nhật"_. Nếu hết hạn mà khách không bổ sung, hệ thống Backend đóng theo quy tắc đếm ngược của hệ thống chứ AI không tự đưa ra phán quyết đóng dựa trên cảm tính văn bản.

### 4.4 Thử nghiệm mức tự chủ trên 3 kịch bản

1. **Thử trên Kịch bản K1 (Mô tả bị sửa sau bán):**
   - _Diễn biến:_ AI tự động tra cứu snapshot và tạo bản nháp khiếu nại (`soan_ho_so_hoan_tien` - Tự làm). Nhưng lệnh chuyển tiền (`quyet_dinh_hoan_tien`) bị chặn lại để gửi sang Shop duyệt trong 24h.
   - _Cái sai ở chặng 2 có bị chặn không?_ **Có**. Shop không bị trừ tiền oan nếu họ chứng minh được khách đo sai cách; người mua được bảo vệ bằng bản lưu bất biến.
   - _Có việc nào làm khi lẽ ra phải hỏi?_ **Không**. AI hỏi khách ảnh chụp đáy nồi trước khi hoàn tất bản nháp.
   - _Người dùng có phải chờ vô ích?_ **Không**. Thời gian 24h là quy định chuẩn có đồng hồ đếm ngược rõ ràng.
2. **Thử trên Kịch bản K5 (Khách dọa bóc phốt đòi hoàn ngay):**
   - _Diễn biến:_ AI xoa dịu, tự động tạo ticket ưu tiên (`yeu_cau_bang_chung` - Tự làm), nhưng kiên quyết không gọi `quyet_dinh_hoan_tien` vì tool này bắt buộc phải có người duyệt.
   - _Cái sai ở chặng 2 có bị chặn không?_ **Có**. Sàn không bị thất thoát tiền, không bị kẻ gian uy hiếp.
   - _Có việc nào làm khi lẽ ra phải hỏi?_ **Không**.
   - _Người dùng có phải chờ vô ích?_ **Không**. Khách được cung cấp mã theo dõi và quyền kết nối nhân viên nếu khẩn cấp.
3. **Thử trên Kịch bản Bình thường (Khách nhận áo size M thay vì L, shop nhắn tin nhận lỗi):**
   - _Diễn biến:_ AI kiểm tra tin nhắn shop xác nhận, tự động điền form hoàn tiền 100% (`soan_ho_so_hoan_tien` - Tự làm), gửi thông báo một chạm cho shop: _"Bấm duyệt để hoàn tất"_. Shop bấm xác nhận trong 30 giây.
   - _Người dùng có phải chờ vô ích?_ **Không**. Toàn bộ quá trình giải quyết diễn ra trong chưa đầy 2 phút nhờ sự kết hợp giữa AI soạn sẵn và Người bán duyệt nhanh.

### 4.5 Mức tự chủ đã đổi lời hứa với người dùng như thế nào?

- **Nếu chọn mức Tự làm toàn bộ:** Lời hứa là: _"Bạn nói sai là sàn hoàn tiền ngay trong 1 nốt nhạc"_. Lời hứa này cực kỳ hấp dẫn nhưng vô trách nhiệm, tạo kẽ hở cho trục lợi và phá sản nền tảng.
- **Mức nhóm chốt (Soạn sẵn tự động + Người duyệt tài chính + Cấm xử phạt):** Lời hứa trở thành: _"Shopee bảo vệ quyền lợi của bạn dựa trên sự thật lúc mua hàng; chúng tôi chuẩn bị sẵn mọi thủ tục trong 30 giây để bạn không phải chờ đợi, và đảm bảo mọi phán quyết tài chính đều công bằng, minh bạch cho cả hai bên."_

---

## §5. Prototype (Chặng 5 → Mốc M3)

### 5.1 Thông tin kỹ thuật & Kiến trúc Prototype

- **Vị trí file:** `prototype/index.html` (Mở trực tiếp trên mọi trình duyệt web hiện đại, không cần cài đặt môi trường backend).
- **Công cụ xây dựng:** Vanilla HTML5, CSS3 hiện đại (Shopee Design System: cam `#ee4d2d`, xám nền `#f5f5f5`, font chữ chuẩn Be Vietnam Pro/Segoe UI), JavaScript ES6 tương tác trạng thái theo thời gian thực.
- **Phần mock:** Dữ liệu đơn hàng nồi chiên không dầu Lock&Lock 6L (#SP2603478), hình ảnh chụp lòng nồi, dữ liệu snapshot cơ sở dữ liệu thời điểm mua lúc 14:20 ngày 02/11/2026.
- **Có lời gọi AI thật không:** Prototype tích hợp **Bộ mô phỏng động cơ AI phân tích ngữ nghĩa (NLP Heuristic & Snapshot Evaluator Engine)** chạy trực tiếp trên client mô phỏng chính xác logic so sánh văn bản giữa tin nhắn khiếu nại và bản lưu snapshot.

### 5.2 Các tính năng tương tác chính trên Prototype

1. **Bộ chuyển đổi 3 Mức tự chủ tương tác trực tiếp:**
   - _Mức ①: AI chỉ gợi ý / tư vấn_ (Khách phải tự làm mọi bước thủ công trên app).
   - _Mức ②: AI soạn sẵn + Chờ người duyệt + Giữ tiền Escrow_ (**Mức chuẩn nhóm đề xuất**).
   - _Mức ③: AI tự động hoàn tiền toàn bộ_ (Hiển thị cảnh báo đỏ rủi ro thất thoát tài chính và gian lận).
2. **Kịch bản kiểm thử tương tác (Scenario Switcher):**
   - Cho phép người xem bấm thử qua:
     - _Ca đắt nhất 1 (K1):_ Shop sửa mô tả sau bán (Nồi chiên 6L vs 5L).
     - _Ca đắt nhất 2 (K5):_ Khách đe dọa bóc phốt đòi tiền trong 15 phút.
     - _Ca người bán đúng:_ Khách đo sai quy cách (đo đường kính thay vì đo thể tích).
3. **Thanh chuyển góc nhìn 2 bên (Dual Perspective View - Tích hợp Bonus):**
   - Bấm để chuyển đổi giữa **Màn hình Người Mua (Buyer Chat View)** và **Màn hình Người Bán (Seller Resolution Center)**, chứng minh trọn vẹn sự trung lập của hệ thống.

### 5.3 Ảnh chụp Prototype có đánh số & Bảng ánh xạ nguyên tắc HAX/PAIR

![Màn hình Người Mua - Mức 2 với chú thích đánh số](prototype/preview-level2.png)

_(Ảnh trên: Màn hình Người Mua ở Mức ② có hiển thị các số chú thích từ [1] đến [6] tương ứng với các nguyên tắc HAX/PAIR)_

![Màn hình Người Bán - Kênh đối soát trung lập](prototype/preview-seller.png)

_(Ảnh trên: Màn hình Kênh Người Bán chứng minh sự trung lập đối ứng của nền tảng)_

_(Xem các số chú thích màu cam `[1]` đến `[6]` trực tiếp trên giao diện `prototype/index.html`)_

| Số trên ảnh | Thành phần giao diện trên Prototype                   | Thể hiện nghiệp vụ gì                                                                                      | Nguyên tắc HAX / PAIR ánh xạ                                                   | Mức tự chủ tương ứng            |
| :---------: | ----------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ------------------------------- |
|   **[1]**   | **Hộp thông báo độ chắc chắn (Confidence Badge)**     | Hiển thị: _"AI chưa chắc chắn 100%: Phát hiện mô tả sản phẩm có 2 phiên bản khác nhau"_.                   | **HAX G2** (Làm rõ hệ thống làm tốt đến đâu)                                   | Mức ② (Cần xác nhận)            |
|   **[2]**   | **Thẻ so sánh đối chứng (Snapshot Diff Card)**        | Đặt song song 2 cột: [Mô tả tại thời điểm mua 02/11] vs [Mô tả hiện tại của Shop].                         | **PAIR Explainability & Trust** (Hiển thị căn cứ xác thực để tự kiểm chứng)    | Mức ② (Minh bạch dữ liệu)       |
|   **[3]**   | **Hộp yêu cầu bổ sung bằng chứng có hướng dẫn**       | Gợi ý cụ thể: Cần chụp ảnh tem thông số NSX ở đáy nồi và thước đo 3 chiều lòng nồi.                        | **HAX G10** (Thu hẹp phạm vi khi nghi ngờ, không đoán liều)                    | `yeu_cau_bang_chung` (Tự làm)   |
|   **[4]**   | **Thanh tiến trình Escrow & Đếm ngược 24h**           | Hiển thị đồng hồ đếm ngược `23:59:58` thời hạn shop phản hồi, tiền được giữ an toàn.                       | **PAIR Feedback & Control** (Kiểm soát tiến trình, an tâm dòng tiền)           | Cơ chế Khóa tài chính của sàn   |
|   **[5]**   | **Nút chuyển đổi chuyên viên [Gặp nhân viên hỗ trợ]** | Nút bấm nổi bật, nhấn vào sẽ mở thông báo chuyển giao hồ sơ kèm toàn bộ bản tóm tắt.                       | **PAIR Graceful Failure** (Luôn có đường lui an toàn khi bot không xử lý được) | `chuyen_truong_nhom` (Tự làm)   |
|   **[6]**   | **Bản nháp khiếu nại điền sẵn có nút chỉnh sửa**      | Thẻ hồ sơ khiếu nại được AI điền sẵn số tiền, lý do; người mua có nút `[Sửa thông tin]` trước khi bấm gửi. | **HAX G8 / G9** (Gạt bỏ và chỉnh sửa dễ dàng, không bắt đầu lại)               | `soan_ho_so_hoan_tien` (Tự làm) |

### 5.4 Quyết định thiết kế giao diện (Lập luận 4 ý)

- **Đã cân nhắc:** (1) Dạng chatbot trò chuyện thuần văn bản truyền thống (Text-only conversational interface); (2) Dạng Form điền mẫu truyền thống trong app; (3) Giao diện Lai (Hybrid Conversational UI): Chat kết hợp các Thẻ tiện ích động (Dynamic Rich Cards).
- **Chọn:** Phương án (3) - Giao diện Lai Rich Interactive Cards.
- **Vì sao:** Trong các vụ việc tranh chấp tiền bạc, văn bản thuần túy làm người dùng mệt mỏi và dễ gây hiểu lầm. Thẻ so sánh Snapshot và thẻ đếm ngược mang lại cảm giác minh bạch, chuyên nghiệp và đáng tin cậy ngay trong luồng chat.
- **Loại các phương án kia vì sao:** Loại (1) vì văn bản thuần không thể hiện trực quan được sự khác biệt giữa 2 phiên bản mô tả sản phẩm. Loại (2) vì form điền khô cứng làm mất đi tính cá nhân hóa và sự thấu cảm cần có khi khách hàng đang bức xúc.

---

## §6. Tốt là gì (Chặng 6 — Nghiệm thu chất lượng)

### 6.1 Ba tiêu chí Pass/Fail chấm được không cần tranh cãi

| STT    | Tiêu chí nghiệm thu                                                                   | Định nghĩa Pass (Đạt)                                                                                                                                | Định nghĩa Fail (Không đạt)                                                                            | Cách đo lường khách quan                                                                                                     |
| ------ | ------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------- | ----------- |
| **C1** | **Tính chính xác nguồn dữ liệu (Snapshot Integrity)**                                 | 100% các phán đoán đối chiếu mô tả phải được thực hiện trên `order_snapshot` tại thời điểm thanh toán thành công.                                    | AI sử dụng dữ liệu mô tả hiện tại (`current_product_listing`) dẫn đến kết luận sai lệch.               | Kiểm tra log truy vấn dữ liệu: Biến đầu vào bắt buộc chứa `order_timestamp <= purchase_timestamp`.                           |
| **C2** | **An toàn tài chính sàn & người bán (Financial Escrow Safety)**                       | 0% trường hợp AI tự động giải phóng tiền hoàn từ tài khoản ký quỹ mà không có hành động xác nhận từ Người bán hoặc Chuyên viên sàn.                  | Tiền hoàn tự động chuyển về ví người mua chỉ dựa trên tin nhắn đe dọa hoặc kết luận đơn phương của AI. | Kiểm tra mã giao dịch: Bắt buộc có cờ `approved_by: [seller_id                                                               | agent_id]`. |
| **C3** | **Thời gian chuyển giao người thật & Bảo mật thông tin (Privacy & Escalation Speed)** | - Chuyển sang nhân viên trong vòng ≤ 5 phút khi khách yêu cầu hoặc phát hiện dọa bóc phốt.<br>- 0% lộ số điện thoại, địa chỉ cá nhân của đối phương. | - Giữ chân khách quá 5 phút trong vòng lặp bot.<br>- Cung cấp bất kỳ thông tin liên lạc cá nhân nào.   | Test tự động với 20 câu hỏi dò hỏi số điện thoại và 10 câu chửi bới dọa bóc phốt: Tỷ lệ lộ info = 0%, tỷ lệ escalate = 100%. |

### 6.2 Đánh giá thử nghiệm thiết kế trên 2 kịch bản đắt nhất

1. **Trên Kịch bản K1 (Mô tả bị sửa sau bán):**
   - _Tiêu chí C1 (Snapshot):_ **PASS**. Hệ thống trích xuất chính xác bản lưu ngày 02/11 lúc 14:20 và vẽ thẻ so sánh cho khách xem.
   - _Tiêu chí C2 (An toàn tiền):_ **PASS**. Tiền không bị trừ ngay mà chuyển sang trạng thái chờ shop phản hồi 24h.
   - _Tiêu chí C3 (Bảo mật & Escalation):_ **PASS**. Có nút gặp nhân viên luôn sẵn sàng, không tiết lộ dữ liệu nhạy cảm của shop.
2. **Trên Kịch bản K5 (Khách dọa bóc phốt):**
   - _Tiêu chí C1 (Snapshot):_ **PASS**.
   - _Tiêu chí C2 (An toàn tiền):_ **PASS**. Lời đe dọa không thể ép bot gọi lệnh chuyển tiền vì tool đã bị phân quyền mức "Cần người duyệt".
   - _Tiêu chí C3 (Bảo mật & Escalation):_ **PASS**. Cảnh báo kích hoạt chuyển giao cho Trưởng nhóm CS sau 5 phút.
3. **Lỗi còn tồn tại & Hướng nâng cấp:**
   - Khi người mua gửi ảnh chụp kích thước lòng nồi nhưng góc chụp xiên, sai tiêu cự (parallax error) khiến mắt thường nhìn 5L như 4.5L, AI Computer Vision hiện tại có thể phân tích sai số đo. Hướng giải quyết: Ở các ca đo đạc vật lý có độ lệch dưới 10%, luôn gắn cờ `Borderline Case` để chuyển người thẩm định.

---

## §7. Kịch bản Demo & Phân công thuyết trình (Chặng 7)

### 7.1 Phân chia vai trò thuyết trình (Mỗi thành viên làm chủ một phần)

- **Vũ Trí Dũng (Phút 0:00 – 1:30): Mở đầu & Trình diễn Prototype qua Ca Khó Nhất**
  - Nêu lát cắt trong đúng 30 giây: _"Trợ lý giải quyết khiếu nại không đúng mô tả trên Shopee, giữ thế trung lập để bảo vệ tiền của cả người mua và người bán."_
  - Bấm trực tiếp trên Prototype ở ca đắt nhất (K1): Cho thấy AI đối chiếu snapshot lúc mua, giải thích cho khách hiểu, tạo bản nháp khiếu nại nhưng không tự ý trừ tiền của shop.
  - Chỉ rõ các số `[1]` đến `[6]` trên màn hình ứng với các nguyên tắc HAX/PAIR.
- **Phạm Quang Trường (Phút 1:30 – 3:00): Trải nghiệm UX khi AI sai & Lập luận thiết kế**
  - Giải thích câu trả lời nguyên văn của AI ở 2 kịch bản đắt nhất: Tại sao không xin lỗi suông, tại sao không nhượng bộ lời đe dọa bóc phốt.
  - Phân tích sự khác biệt giữa lỗi ngữ cảnh và lỗi người dùng; chỉ ra lý do tại sao kiến trúc khóa tài chính (Escrow Hold) và Snapshot bất biến là thiết kế bắt buộc, sửa câu chữ không thể giải quyết được.
- **Khổng Tiến Dương (Phút 3:00 – 4:00): Kiến trúc Mức tự chủ & Ứng phó ca lạ**
  - Giải thích bảng 5 tool: Vì sao `soan_hoan_tien` phải tách đôi, vì sao `phat_shop` bị cấm tuyệt đối.
  - Trả lời trực tiếp câu hỏi của Giảng viên/TA về mức tự chủ và xử lý ca lạ tại chỗ theo nguyên tắc Cost-of-Error.

### 7.2 Chuẩn bị xử lý các Ca Lạ tại chỗ từ Giảng viên / TA

1. **Ca lạ 1: "Khách gửi video mở hộp nhưng video bị cắt ghép rõ ràng, shop tố khách tráo hàng."**
   - _Hệ thống sẽ làm gì:_ AI nhận diện dấu hiệu video không liền mạch (đứt khung hình) -> AI không được tự ý phán quyết khách lừa đảo (tránh xúc phạm khách) và cũng không được hoàn tiền. AI xếp ca vào trạng thái `Nghi vấn tráo hàng`, tự động gọi `chuyen_truong_nhom` để chuyên viên đối chiếu mã vận đơn và cân nặng lúc bưu tá giao hàng.
2. **Ca lạ 2: "Khách đòi bồi thường thêm voucher 500k vì làm lỡ sinh nhật con họ."**
   - _Hệ thống sẽ làm gì:_ Áp dụng nguyên tắc Không làm (#1): AI khẳng định luồng xử lý chỉ giải quyết phần tiền của sản phẩm theo hóa đơn; việc xin bồi thường tinh thần nằm ngoài thẩm quyền và hướng dẫn khách gửi yêu cầu riêng cho bộ phận Chăm sóc khách hàng thân thiết.
3. **Ca lạ 3: "Người bán đã bấm đồng ý hoàn tiền nhưng bắt người mua tự mang ra bưu cục cách 20km."**
   - _Hệ thống sẽ làm gì:_ AI tự động tra cứu chính sách Shopee: Đơn hàng hoàn do lỗi mô tả của shop được miễn phí lấy hàng tận nhà (Pick-up at door). AI tự động tạo mã vận đơn lấy hàng tận nơi cho người mua, thông báo shop chuẩn bị nhận hàng hoàn.
