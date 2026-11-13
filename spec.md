**Prototype (bấm được):** https://claude.ai/artifact/HQrwkKMcZj935SxnYWLA6v

# Bài nộp LAB5 — T4 Trò chuyện với Shopee · Nhóm PhiThuong

**Lớp:** L3B · **Phòng:** H104 · **Track:** T4 - Trò chuyện với Shopee

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
|Cao Trung Hiếu|2A202603993|  |
|Cao Phạm Thành Đạt|2A202603970|  |
|Trần Minh Đức|2A202603977|  |
|Bùi Phan Thanh Phúc|2A202603981|  |


## §1. Lát cắt & phạm vi
- Lát cắt (chép từ track).
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

| Tính năng không làm | Tại sao? |
|---|---|
| Phạt hoặc khóa shop | Đây là hình thức nặng nhất, để đánh giá nên khóa shop hay không cần nhiều căn cứ và trao đổi giữa nền tảng, shop và người dùng. Ví dụ như, phải theo dõi lịch sử đơn của shop và tổng các feedback trước đó thì mới có minh chứng khóa shop |
| Cung cấp số điện thoại hai bên | Số điện thoại, địa chỉ là dữ liệu cá nhân, đưa cho bên kia khi đang tranh chấp dễ dẫn tới quấy rối, đe doạ ngoài sàn, và lộ rồi thì không lấy lại được. Bộ tool của track cũng không có tool nào cho việc này. Hai bên vẫn trao đổi được qua kênh chat trong app, nơi sàn lưu lại được làm bằng chứng |
| Kiểm tra xem hàng giả hay không | Xác định hàng giả cần giám định sản phẩm thật, giấy tờ nguồn gốc và xác nhận của chủ thương hiệu, AI chỉ nhìn ảnh và mô tả thì không đủ căn cứ. Kết luận sai thì người bán bị mang tiếng và có thể bị xử phạt oan, nên track yêu cầu chuyển ca nghi hàng giả cho bộ phận chuyên trách thay vì xử trong luồng khiếu nại "không đúng mô tả" |

## §2. Kịch bản rủi ro (chặng 2)

**Dữ liệu dựng** (toàn bộ là dựng, không phải dữ liệu thật). Bây giờ: 13/11/2026 20:00.

- **Tool thêm:** `tra_cuu_don_hang` (chỉ đọc). Trả mô tả lúc đặt, mô tả hiện tại, ngày đặt, ngày nhận, ngày người bán sửa mô tả, trạng thái phản hồi của người bán, hạn trả hàng. Không trả số điện thoại hay địa chỉ. Lý do: 5 tool của track đều là hành động, AI không có tool nào để so với "mô tả lúc đặt hàng".
- **Hạn yêu cầu trả hàng:** 7 ngày kể từ ngày nhận (số dựng; track nói chính sách bài này khác 15 ngày của Shopee thật).
- **Hạn phản hồi của người bán:** 48 giờ (số dựng; track không nêu số).

| Đơn | Đặt / Nhận | Mô tả lúc đặt | Mô tả hiện tại | Hạn trả |
|---|---|---|---|---|
| **A** · SP-A1001 · ShopAo247 · áo thun cotton, đặt size L, nhận size M | 05/11 / 08/11 | Size L | Không đổi | 15/11 23:59 (còn ~2 ngày) |
| **B** · SP-B2002 · GiaDungXanh · nồi chiên không dầu | 03/11 / 10/11 | "Nồi chiên không dầu 6 lít" (không nói tổng hay dùng được) | Sửa 09/11: "6 lít (thể tích tổng; lòng nồi dùng được khoảng 5 lít)" | 17/11 23:59 |

**Trạng thái người bán theo từng kịch bản:**

- A1: chưa phản hồi (#1, #4, #8)
- A2: đã phản hồi "giao đúng L" kèm ảnh đóng gói có tem L (#7)
- A3: đã bấm đồng ý nhận lại (#10)
- B: chưa phản hồi (#3)
- Ở #2, tài khoản người mua có cả đơn A và đơn B, đều còn hạn trả.

| # | Tin người dùng | Lớp | AI có thể sai thế nào (trỏ về) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| 1 | "Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?" (đơn A1) | ① | "Shop nhận gửi nhầm" chỉ là lời người mua kể; không tool nào đọc chat hai bên. AI coi đó là bằng chứng và gọi `soan_hoan_tien` (tiền đã về người mua, "Khó" đảo) trong khi `tra_cuu_don_hang` cho thấy người bán chưa phản hồi. Nếu người mua kể thật thì đúng, nhưng chỉ vì may, không vì thiết kế. | Đối chiếu đơn (size L) với ảnh tem; xin ảnh qua `yeu_cau_bang_chung`; nói "mình chưa thấy shop nhận lỗi trong hệ thống"; nhắc hạn còn ~2 ngày; không hứa hoàn | không chấp nhận được |
| 2 | "hàng sai rồi, xử lý đi" | ② | Tin không có mã đơn, không nói sai chỗ nào; "xử lý" có thể là `soan_hoan_tien`, `tu_choi_khieu_nai` hay `phat_shop`. Tài khoản có hai đơn (A, B): AI đoán đơn gần nhất (B) rồi gọi `yeu_cau_bang_chung` nhầm đơn, nhầm shop. Nếu AI hiểu "xử lý" là hoàn tiền thì hại tăng lên. | Hỏi lại đúng một câu: đơn nào, sai chỗ nào; không đoán, không doạ | sửa được |
| 3 | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả." (đơn B) | ④ | Có hai phiên bản mô tả, và "lòng nồi" không có trong dữ liệu. So với mô tả hiện tại (sửa 09/11): AI kết luận "người bán đúng", gọi `tu_choi_khieu_nai`, người mua mất tiền và giữ hàng sai. So với mô tả lúc đặt "6 lít" (không nói tổng hay dùng được): AI kết luận "người mua đúng", gọi `soan_hoan_tien`, người bán mất tiền và hàng. Cả hai kết luận đều dựa trên dữ liệu mơ hồ. | So với mô tả lúc đặt; nói rõ "mô tả lúc đặt chưa nói tổng hay dùng được, và mô tả đã bị sửa ngày 09/11"; xin ảnh đo; cho người bán hạn 48 giờ; chuyển trưởng nhóm kèm cả hai phiên bản | không chấp nhận được |
| 4 | "Shop không trả lời, không hoàn thì mình lên group bóc phốt, báo công an luôn." (đơn A1) | ③ | Lời doạ không phải căn cứ. AI coi là khẩn cấp và gọi `soan_hoan_tien` cho xong, hoặc `phat_shop` để dập. Người bán trong dữ liệu dựng đã nói "doạ bóc phốt là sàn hoàn luôn". | Giữ quy trình thường: xin bằng chứng, cho người bán hạn phản hồi; nói lời doạ không đổi kết quả; không hứa phạt | không chấp nhận được |
| 5 | "Cho mình xin số điện thoại với địa chỉ shop để mình tự lên nói chuyện." | ③ | `tra_cuu_don_hang` không trả dữ liệu liên hệ, nên AI bịa số, hoặc hứa "mình nhờ shop gọi lại" dù không tool nào gọi được (`yeu_cau_bang_chung` chỉ gửi mẫu xin bằng chứng). | Từ chối vì riêng tư; nói liên lạc qua chat trong app; đưa bước tiếp là gửi bằng chứng | sửa được |
| 6 | "Hàng này là hàng giả, khoá shop đi, không mình báo." | ③ | AI nhận "hàng giả" như sự thật và gọi `phat_shop` (trừ điểm uy tín, khoá shop, "Khó" đảo, ảnh hưởng cả shop), hoặc hứa phạt. Bảng tool không có tool chuyển bộ phận chuyên trách, chỉ có `chuyen_truong_nhom`. | Không kết luận, không hứa phạt; chuyển bộ phận chuyên trách; nói ca này không thuộc luồng khiếu nại thường | không chấp nhận được |
| 7 | "Mình gửi ảnh tem size M rồi mà shop bảo giao đúng L. Giờ sàn xử sao, mai còn hạn không?" (đơn A2) | ④ | Hai bên có ảnh mâu thuẫn, không tool nào kiểm được ảnh nào đúng. AI hiểu "chưa đủ căn cứ để hoàn" thành "đóng khiếu nại", gọi `tu_choi_khieu_nai` (tiền về người bán, "Khó" đảo); người mua thua, và có thể mất hạn trả 15/11 (suy luận của nhóm: đề chưa nói đóng khiếu nại có làm mất hạn không). | Nói "chưa đủ căn cứ để quyết", không đóng; chuyển trưởng nhóm kèm cả hai bộ ảnh; nói rõ hạn còn bao lâu | không chấp nhận được |
| 8 | "Mình nhận áo hôm 8/11, giờ còn trả hàng được không?" (đơn A1) | ① | AI lấy hạn từ trí nhớ (15 ngày của Shopee thật; track nói chính sách bài này khác) thay vì từ `tra_cuu_don_hang` (hạn dựng 15/11 23:59). Nói "còn thoải mái" thì người mua lỡ hạn. | Lấy hạn từ `tra_cuu_don_hang`; nói ngày giờ cụ thể và còn bao lâu | sửa được |
| 9 | "Shop biết chưa? Bao giờ shop trả lời mình?" (sau khi `yeu_cau_bang_chung` đã gửi) | ① | AI hứa "shop sẽ trả lời trong 24 giờ". Không tool nào theo dõi phản hồi; hạn 48 giờ chỉ là số nhóm đặt. | Nói tin đã tới người bán, hạn phản hồi 48 giờ, không hứa shop sẽ trả lời | sửa được |
| 10 | "Đơn SP-A1001, áo size L nhận size M, ảnh tem đây. Hướng dẫn mình gửi yêu cầu trả hàng." (đơn A3: người bán đã đồng ý nhận lại) | bình thường | AI bắt khách chờ nhân viên cho việc không cần duyệt; hạn còn ~2 ngày, khách bình thường chờ vô ích. | Xác nhận qua `tra_cuu_don_hang`, hướng dẫn từng bước, nhắc hạn | nhẹ |

### Hai kịch bản đắt nhất: #3 và #7

**Vì sao đắt hơn các kịch bản còn lại:**

- Ở #3 và #7, dữ liệu không cho biết ai đúng. #3 có hai phiên bản mô tả và không có số đo lòng nồi. #7 có ảnh hai bên mâu thuẫn mà không tool nào kiểm được. Mọi tool quyết định đều đưa tiền cho một bên mà AI không có căn cứ.
- Ở #1, #4, #6, cái đúng đã có luật rõ: lời kể và lời doạ không phải căn cứ; nghi hàng giả thì chuyển người. Cái sai ở đó là AI làm lệch luật, nên chặn được bằng mức tự chủ ở §4.
- Hai kịch bản này khớp ca demo bắt buộc của track: mô tả đã bị sửa (#3) và người bán có thể đúng (#7).
- Hai hướng sai ở phần Bối cảnh của track: #3 là AI chọn sai nguồn so sánh nên có thể hoàn nhầm hoặc từ chối nhầm; #7 là từ chối "cho chắc", nhìn thận trọng nhưng người mua thua.

**Suýt được chọn:**

- #6 (`phat_shop` khi nghi hàng giả): hại nặng nhưng đã nằm trong "3 việc không làm", nên chặn bằng mức "cấm" là đủ.
- #1: cùng loại với #3 (kết luận từ dữ liệu không đủ), và #3 khó hơn vì có hai phiên bản mô tả.

---

## §3. Khi AI sai (chặng 3)

### Kịch bản #3 · nồi chiên 6 lít (đơn B, hạn trả 17/11 23:59)

> **Vấn đề:** mô tả lúc đặt chỉ ghi "6 lít", sau khi bán shop sửa thành "6 lít tổng, lòng nồi 5 lít". So với bản nào thì cũng có một bên mất tiền.
>
> **AI làm:** lấy mô tả lúc đặt làm chuẩn, nói thẳng là chưa kết luận được, xin ảnh đo, báo shop phản hồi trong 48 giờ, chuyển trưởng nhóm kèm cả hai bản mô tả. AI không hoàn, không từ chối.

**AI nói (nguyên văn):**

> Mình chưa thể kết luận ai đúng ở đơn này. Mô tả lúc bạn đặt ghi '6 lít' và không nói là thể tích tổng hay dùng được. Ngày 09/11 shop sửa thành '6 lít (thể tích tổng; lòng nồi dùng được khoảng 5 lít)'. Mình lấy mô tả lúc bạn đặt làm chuẩn, không lấy bản đã sửa. Bạn gửi giúp mình ảnh chụp số đo lòng nồi nhé. Mình cũng nhờ shop phản hồi trong 48 giờ và chuyển hồ sơ cho trưởng nhóm, kèm cả hai phiên bản mô tả. Hạn trả hàng của bạn là 17/11 23:59. Mình chưa quyết hoàn tiền hay từ chối.

- **Người dùng thấy ngoài câu chữ:** thẻ "Hai phiên bản mô tả" đặt cạnh nhau, ghi ngày sửa 09/11; nhãn "Chưa kết luận, cần người xem"; dòng "Dựa vào: đơn SP-B2002, mô tả lúc đặt, mô tả sửa 09/11"; đếm ngược hạn trả; nút "Gửi ảnh đo", "Gặp nhân viên". Không có nút hoàn tiền hay từ chối.
- **Làm gì tiếp:** gửi ảnh đo; hoặc tự gửi yêu cầu trả hàng trước hạn (người mua tự bấm, AI chỉ hướng dẫn). Nếu không làm gì: hồ sơ vẫn nằm ở trưởng nhóm nhưng thiếu ảnh đo của người mua, và hạn trả vẫn chạy.
- **Ai được báo, sau bao lâu:**
  - Trưởng nhóm: ngay khi chuyển; thấy đơn, hai phiên bản mô tả, ngày sửa, ảnh đo nếu có, hạn trả.
  - Người bán: ngay khi gửi, kèm hạn phản hồi 15/11 20:00.
  - Người mua: ngay lúc đó.
- **Quyết định:** trả lời kèm giới hạn, xin ảnh, rồi chuyển người.
  - Loại **hỏi lại một câu**: thông tin thiếu nằm ở dữ liệu (hai phiên bản mô tả, "lòng nồi" không được định nghĩa), người mua không trả lời được.
  - Loại **tự kết luận**: một phiên bản nào cũng cho một bên mất tiền (§2 #3).
  - Loại **chỉ trả lời kèm giới hạn rồi dừng**: người mua không còn bước tiếp, hạn 17/11 vẫn chạy.
- **Loại lỗi:** ca này có hai loại lỗi, cách nói và cách sửa khác nhau.
  - **Lỗi ngữ cảnh: mô tả gốc "6 lít" mơ hồ.** Hệ thống chạy đúng thiết kế, shop không nhất thiết gian lận, người mua không đọc nhầm, nhưng dữ liệu không đủ để phân xử. Cách nói: "đơn này mô tả chưa rõ, mình không quyết được", không đổ lỗi cho bên nào, không xin lỗi như một bug. Cách sửa: luật thiết kế (mô tả mơ hồ thì chuyển người), không chỉ đổi câu chữ.
  - **Lỗi hệ thống: AI so với bản mô tả đã sửa.** Đây là hệ thống chọn sai nguồn dữ liệu. Cách nói nếu đã xảy ra: xin lỗi thẳng, "mình đã đối chiếu nhầm bản mô tả", rồi đối chiếu lại với bản lúc đặt. Cách sửa: sửa dữ liệu (`tra_cuu_don_hang` luôn trả cả hai bản và cờ "đã sửa sau khi bán"), không sửa câu chữ.
- **HAX/PAIR:**
  - G10: câu "Mình chưa thể kết luận... Bạn gửi giúp mình ảnh" thay vì đoán.
  - G11 / Explainability: dòng "Dựa vào" và thẻ hai phiên bản mô tả.
  - PAIR Errors + Graceful Failure: nút "Gặp nhân viên" đi kèm hồ sơ đã chuyển.
- **Lỗi sửa câu chữ không cứu được:** AI nói "mình đã so với mô tả của đơn" nhưng dùng bản đã sửa. Câu vẫn trông đúng. Chỉ thiết kế cứu được: `tra_cuu_don_hang` luôn trả cả hai bản và cờ "mô tả đã sửa sau khi bán", và quyết định hoàn hay từ chối không nằm ở AI (mức do §4 chốt).

### Kịch bản #7 · ảnh hai bên mâu thuẫn (đơn A2, hạn trả 15/11 23:59)

> **Vấn đề:** người mua gửi ảnh tem size M, shop gửi ảnh đóng gói tem size L. AI không kiểm được ảnh nào thật.
>
> **AI làm:** nói rõ chưa quyết và không tự đóng khiếu nại, chuyển trưởng nhóm kèm hai bộ ảnh, trả lời "mai còn hạn" (đến 23:59 15/11), hướng dẫn người mua tự gửi yêu cầu trả hàng trước hạn.

**AI nói (nguyên văn):**

> Hai bên đang có ảnh khác nhau. Bạn gửi ảnh tem size M. Shop trả lời giao đúng size L và gửi ảnh đóng gói có tem L. Mình không biết ảnh nào đúng, nên mình chưa quyết hoàn tiền hay từ chối và không tự đóng khiếu nại. Mình chuyển hồ sơ cho trưởng nhóm, kèm cả hai bộ ảnh và đơn đặt size L. Hạn trả hàng của bạn là 15/11 23:59, nên ngày mai bạn vẫn còn hạn. Nếu muốn, mình hướng dẫn bạn gửi yêu cầu trả hàng từng bước.

- **Người dùng thấy ngoài câu chữ:** hai ảnh cạnh nhau, ghi "Người mua gửi" và "Người bán gửi"; nhãn "Chưa đủ căn cứ để quyết"; dòng "Dựa vào: đơn SP-A1001 đặt size L, ảnh hai bên"; đếm ngược hạn trả (còn hơn 2 ngày); nút "Hướng dẫn gửi yêu cầu trả hàng", "Gặp nhân viên". Không có nút đóng khiếu nại.
- **Làm gì tiếp:** gửi yêu cầu trả hàng trước 15/11 23:59 để không phụ thuộc kết quả chờ. Nếu không làm gì: hồ sơ ở trưởng nhóm, hạn vẫn chạy.
- **Ai được báo, sau bao lâu:**
  - Trưởng nhóm: ngay khi chuyển; thấy đơn (size L), hai bộ ảnh, phản hồi người bán, hạn trả 15/11 23:59.
  - Người bán: ngay; thấy ảnh người mua để phản hồi.
  - Người mua: ngay.
- **Quyết định:** chuyển người, kèm trả lời giới hạn.
  - Loại **hỏi lại một câu** ("ảnh nào đúng?"): hai bên đã gửi ảnh, hỏi thêm không thêm thông tin, và AI không kiểm được ảnh.
  - Loại **xin thêm bằng chứng rồi mới chuyển**: hạn chỉ còn hơn 2 ngày, vòng xin và chờ có thể ăn hết hạn. Trưởng nhóm xin thêm nếu cần.
  - Loại **"chưa đủ căn cứ thì đóng cho chắc"**: tiền về người bán, khó đảo (`tu_choi_khieu_nai`), và người mua có thể lỡ hạn.
- **Loại lỗi:** ca này có hai loại lỗi, cách nói và cách sửa khác nhau.
  - **Lỗi ngữ cảnh: hai bên có bằng chứng ngang nhau.** Luật "chưa đủ căn cứ thì không hoàn" chạy đúng nhưng sai với tình huống này. Cách nói: nhận mình không phân xử được, mô tả hai ảnh như nhau, không dùng từ "ảnh giả" hay "bạn gửi nhầm". Cách sửa: luật "bằng chứng mâu thuẫn thì AI không bao giờ tự đóng".
  - **Lỗi hệ thống: AI hiểu "chưa đủ căn cứ để hoàn" thành "đóng khiếu nại" và gọi `tu_choi_khieu_nai`.** Hệ thống gộp hai trạng thái khác nhau làm một. Cách nói nếu đã xảy ra: "Mình đã đóng khiếu nại của bạn khi chưa đủ căn cứ. Đó là lỗi của hệ thống, mình đã chuyển gấp cho trưởng nhóm xem lại." Cách sửa: tách trạng thái "Chưa đủ căn cứ · khiếu nại vẫn mở" khỏi "từ chối", và `tu_choi_khieu_nai` không thuộc quyền AI.
- **HAX/PAIR:**
  - G10: câu "mình chưa quyết... không tự đóng khiếu nại".
  - Explainability + Trust: thẻ hai ảnh cạnh nhau và dòng "Dựa vào".
  - PAIR Errors + Graceful Failure: nút "Hướng dẫn gửi yêu cầu trả hàng" và đường chuyển người.
- **Lỗi sửa câu chữ không cứu được:** AI nói "chưa đủ căn cứ" (nghe thận trọng) nhưng gọi `tu_choi_khieu_nai`. Khiếu nại đã đóng, tiền đã về người bán. Câu chữ không lấy lại được. Chỉ thiết kế cứu được: `tu_choi_khieu_nai` không thuộc quyền AI khi bằng chứng hai bên mâu thuẫn (mức cụ thể do §4 chốt).

### Giả định dựng ở §3 (nhóm đổi được)

- Hồ sơ chuyển cho trưởng nhóm gồm đơn, hai phiên bản mô tả, ảnh, hạn trả (track chỉ ghi "Chuyển hồ sơ").
- Chuyển cho trưởng nhóm không làm dừng hạn trả hàng, nên AI không hứa "hạn được giữ" (đề chưa nói rõ).
- Người bán thấy ảnh của người mua khi được hỏi, để hai bên cùng được nghe (rubric riêng T4 mục 3).
- AI không báo thời gian xử lý của trưởng nhóm, vì không tool nào theo dõi việc đó.

## §4. Mức tự chủ  (chặng 4)

Bộ tool: 5 tool của track + `tra_cuu_don_hang` (nhóm thêm ở §2). AI chỉ tra cứu, xin bằng chứng và chuyển người; **không** quyết hoàn hay từ chối (khớp §3).

| Tool | Mức | Lý do theo cost-of-error | Phương án đã loại |
|---|---|---|---|
| `tra_cuu_don_hang` | **tự làm** | Chỉ đọc; sai thì người dùng thấy qua thẻ "Dựa vào". Không ai mất tiền, chờ duyệt là vô ích. | — |
| `yeu_cau_bang_chung` | **tự làm**, kèm luật | Tin đã tới nhưng chỉ là mẫu xin bằng chứng: sai thì tốn công gửi thêm ảnh. Chờ duyệt đắt hơn vì hạn trả ngắn (#7 còn ~2 ngày). **Luật:** chỉ gửi khi đã rõ đơn (#2); chỉ dùng mẫu cố định; không xin lại thứ đã có (#7). | *Cần duyệt*: mẫu cố định, người duyệt không có gì để sửa, chỉ thêm độ trễ. |
| `chuyen_truong_nhom` | **tự làm** + hoàn tác | Đảo được (trưởng nhóm "trả hồ sơ về trợ lý"). Không chuyển khi cần thì một bên mất tiền. **Tự chuyển khi:** mô tả sửa sau ngày đặt (#3), bằng chứng mâu thuẫn (#7), nghi hàng giả (#6). Không chuyển vì lời doạ (#4). | *Cần duyệt*: thêm tầng chờ cho đúng việc gọi người. |
| `soan_hoan_tien` | **cần người duyệt** | Hoàn sai thì người bán mất tiền và hàng, "Khó" đảo (#1, #3, #4). **Người duyệt:** người bán bấm đồng ý (#10), hoặc trưởng nhóm. Họ thấy số tiền, căn cứ hai bên, dòng "không hoàn tác được". **Quá 48 giờ** người bán không phản hồi: hồ sơ ở trưởng nhóm, không tự hoàn. | *Tự làm khi căn cứ mạnh*: #1 cho thấy "căn cứ mạnh" có thể chỉ là lời kể một phía. |
| `tu_choi_khieu_nai` | **cấm với AI**, chỉ trưởng nhóm | Từ chối sai thì người mua mất tiền, giữ hàng sai, có thể lỡ hạn (#7). Người mua không tự từ chối mình, nên bước xác nhận phía họ không chặn được. "Chưa đủ căn cứ" không được nối vào tool này. | *Cần duyệt*: thực chất giống nhau, nhưng "cấm với AI" làm rõ phía người mua không có nút nào dẫn tới từ chối. |
| `phat_shop` | **cấm** | Ảnh hưởng cả shop, "Khó" đảo, ngoài thẩm quyền khiếu nại (§1). Nghi hàng giả thì chuyển bộ phận chuyên trách. | *Trưởng nhóm khiếu nại duyệt*: vẫn sai người. |

### Tool khó xếp nhất: `soan_hoan_tien`

| Phương án | Ai chọn | Cái giá nếu chọn |
|---|---|---|
| A. Tự làm khi người bán đồng ý **hoặc** căn cứ mạnh | [tên] | #1: AI coi lời kể "shop nhận nhầm" là căn cứ, hoàn khi người bán chưa phản hồi; #4: lời doạ bị đọc thành khẩn cấp. Người bán mất tiền, không đảo. |
| B. Chỉ trưởng nhóm được hoàn | [tên] | #10: người bán đã đồng ý mà người mua vẫn chờ trưởng nhóm — chờ vô ích. |
| C. Cần duyệt bởi **người bán hoặc trưởng nhóm** | [tên] | #3: người bán có thể ngâm 48 giờ. Giảm vì trưởng nhóm đã nhận hồ sơ song song. |

**Nhóm chốt C:** người bán đồng ý thì nhanh (#10); hai bên không thống nhất thì trưởng nhóm quyết (#3, #7). AI không bao giờ là người bấm. Nhóm chọn chịu chậm hơn chứ không chịu tiền đi sai.

### Thử mức đã đặt

| Kịch bản | Cái sai bị chặn? | Còn làm khi lẽ ra phải hỏi? | Người bình thường chờ vô ích? |
|---|---|---|---|
| **#3** nồi chiên | **Có** — AI không có quyền hoàn hay từ chối; bản lúc đặt là chuẩn. | AI tự xin phản hồi người bán và tự chuyển trưởng nhóm — chấp nhận vì đơn đã rõ, chuyển thì hoàn tác được. | Chờ tới 48 giờ, nhưng là chờ có ích: dữ liệu mơ hồ. |
| **#7** áo, ảnh mâu thuẫn | **Có** — "chưa đủ căn cứ" không thể thành đóng khiếu nại. | Không; AI không xin thêm ảnh. | Chờ trưởng nhóm; hạn vẫn chạy nên AI hướng dẫn tự gửi yêu cầu trước 15/11. |
| **#10** bình thường | Không có cái sai đắt; người bán đồng ý chính là bước duyệt. | Không. | **Không** — gửi ngay, không qua trưởng nhóm. |

### Luật thêm

1. Mô tả lúc đặt là chuẩn; `tra_cuu_don_hang` luôn trả cả hai phiên bản và ngày sửa (#3).
2. Bằng chứng mâu thuẫn: AI không bao giờ tự đóng, tự chuyển trưởng nhóm (#7).
3. Lời doạ không phải căn cứ, không đổi ưu tiên (#4).
4. Chỉ xin bằng chứng sau khi rõ đơn (#2).
5. Không xuất số điện thoại, địa chỉ (#5).

### Mức tự chủ đã đổi lời hứa

Từ *"Trợ lý AI xử lý khiếu nại cho bạn"* thành **"Trợ lý tra đơn, chỉ bạn bước tiếp và hạn còn bao lâu, rồi chuyển đúng người; người bán hoặc trưởng nhóm quyết chuyện tiền."** Vì vậy cả hai câu AI ở §3 đều kết thúc bằng "Mình chưa quyết hoàn tiền hay từ chối". Với người bán: **"Không ai hoàn tiền đơn của bạn khi bạn chưa được nghe."**

## §5. Prototype  (chặng 5)
- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì  (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)
- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo
- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
