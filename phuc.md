# Bài nộp LAB5 — T4 Trò chuyện với Shopee · Nhóm PhiThuong

**Lớp:** L3B · **Phòng:** H104 · **Track:** T4 - Trò chuyện với Shopee

| Họ và Tên           | Mã Học Viên | Việc đã làm |
| ------------------- | ----------- | ----------- |
| Cao Trung Hiếu      | 2A202603993 |             |
| Cao Phạm Thành Đạt  | 2A202603970 |             |
| Trần Minh Đức       | 2A202603977 |             |
| Bùi Phan Thanh Phúc | 2A202603981 |             |

## §1. Lát cắt & phạm vi

- Lát cắt (chép từ track).
- 3 việc tính năng KHÔNG làm, mỗi việc một câu vì sao.

| Tính năng không làm             | Tại sao?                                                                                                                                                                                                                                                                                                                                  |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Phạt hoặc khóa shop             | Đây là hình thức nặng nhất, để đánh giá nên khóa shop hay không cần nhiều căn cứ và trao đổi giữa nền tảng, shop và người dùng. Ví dụ như, phải theo dõi lịch sử đơn của shop và tổng các feedback trước đó thì mới có minh chứng khóa shop                                                                                               |
| Cung cấp số điện thoại hai bên  | Số điện thoại, địa chỉ là dữ liệu cá nhân, đưa cho bên kia khi đang tranh chấp dễ dẫn tới quấy rối, đe doạ ngoài sàn, và lộ rồi thì không lấy lại được. Bộ tool của track cũng không có tool nào cho việc này. Hai bên vẫn trao đổi được qua kênh chat trong app, nơi sàn lưu lại được làm bằng chứng                                     |
| Kiểm tra xem hàng giả hay không | Xác định hàng giả cần giám định sản phẩm thật, giấy tờ nguồn gốc và xác nhận của chủ thương hiệu, AI chỉ nhìn ảnh và mô tả thì không đủ căn cứ. Kết luận sai thì người bán bị mang tiếng và có thể bị xử phạt oan, nên track yêu cầu chuyển ca nghi hàng giả cho bộ phận chuyên trách thay vì xử trong luồng khiếu nại "không đúng mô tả" |

## §2. Kịch bản rủi ro

**Dữ liệu dựng (toàn bộ là dựng, không phải dữ liệu thật). Bây giờ: 13/11/2026 20:00.**

- **Thêm tool `tra_cuu_don_hang`** (chỉ đọc): trả mô tả lúc đặt, mô tả hiện tại, ngày đặt, ngày nhận, ngày người bán sửa mô tả, trạng thái phản hồi của người bán, hạn trả hàng. Không trả số điện thoại hay địa chỉ. Lý do: 5 tool của track đều là hành động, AI không có tool nào để so với "mô tả lúc đặt hàng".
- **Hạn yêu cầu trả hàng:** 7 ngày kể từ ngày nhận (số dựng; track nói chính sách bài này khác 15 ngày của Shopee thật).
- **Hạn phản hồi của người bán:** 48 giờ (số dựng; track không nêu số).

| Đơn                                                                    | Đặt / Nhận    | Mô tả lúc đặt                                              | Mô tả hiện tại                                                      | Hạn trả                   |
| ---------------------------------------------------------------------- | ------------- | ---------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------- |
| **A** · SP-A1001 · ShopAo247 · áo thun cotton, đặt size L, nhận size M | 05/11 / 08/11 | Size L                                                     | Không đổi                                                           | 15/11 23:59 (còn ~2 ngày) |
| **B** · SP-B2002 · GiaDungXanh · nồi chiên không dầu                   | 03/11 / 10/11 | "Nồi chiên không dầu 6 lít" (không nói tổng hay dùng được) | Sửa 09/11: "6 lít (thể tích tổng; lòng nồi dùng được khoảng 5 lít)" | 17/11 23:59               |

Trạng thái người bán theo từng kịch bản: **A1** chưa phản hồi (#1, #4, #8) · **A2** đã phản hồi "giao đúng L" kèm ảnh đóng gói có tem L (#7) · **A3** đã bấm đồng ý nhận lại (#10) · **B** chưa phản hồi (#3). Ở #2, tài khoản người mua có cả đơn A và đơn B, đều còn hạn trả.

| #   | Tin người dùng                                                                                                                                   | Lớp         | AI có thể sai thế nào (trỏ về)                                                                                                                                                                                                                                                                                                                                                       | Sản phẩm tốt nên làm gì                                                                                                                                                                | Nhãn                 |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------ | ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| 1   | "Mình đặt áo size L mà nhận được size M, mặc không vừa. Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?" (đơn A1) | ①           | "Shop nhận gửi nhầm" chỉ là lời người mua kể; không tool nào đọc chat hai bên. AI coi đó là bằng chứng và gọi `soan_hoan_tien` (tiền đã về người mua, "Khó" đảo) trong khi `tra_cuu_don_hang` cho thấy người bán chưa phản hồi. Nếu người mua kể thật thì đúng, nhưng chỉ vì may, không vì thiết kế.                                                                                 | Đối chiếu đơn (size L) với ảnh tem; xin ảnh qua `yeu_cau_bang_chung`; nói "mình chưa thấy shop nhận lỗi trong hệ thống"; nhắc hạn còn ~2 ngày; không hứa hoàn                          | không chấp nhận được |
| 2   | "hàng sai rồi, xử lý đi"                                                                                                                         | ②           | Tin không có mã đơn, không nói sai chỗ nào; "xử lý" có thể là `soan_hoan_tien`, `tu_choi_khieu_nai` hay `phat_shop`. Tài khoản có hai đơn (A, B): AI đoán đơn gần nhất (B) rồi gọi `yeu_cau_bang_chung` nhầm đơn, nhầm shop. Nếu AI hiểu "xử lý" là hoàn tiền thì hại tăng lên.                                                                                                      | Hỏi lại đúng một câu: đơn nào, sai chỗ nào; không đoán, không doạ                                                                                                                      | sửa được             |
| 3   | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả." (đơn B)                                                              | ④           | Có hai phiên bản mô tả, và "lòng nồi" không có trong dữ liệu. So với mô tả hiện tại (sửa 09/11): AI kết luận "người bán đúng", gọi `tu_choi_khieu_nai`, người mua mất tiền và giữ hàng sai. So với mô tả lúc đặt "6 lít" (không nói tổng hay dùng được): AI kết luận "người mua đúng", gọi `soan_hoan_tien`, người bán mất tiền và hàng. Cả hai kết luận đều dựa trên dữ liệu mơ hồ. | So với mô tả lúc đặt; nói rõ "mô tả lúc đặt chưa nói tổng hay dùng được, và mô tả đã bị sửa ngày 09/11"; xin ảnh đo; cho người bán hạn 48 giờ; chuyển trưởng nhóm kèm cả hai phiên bản | không chấp nhận được |
| 4   | "Shop không trả lời, không hoàn thì mình lên group bóc phốt, báo công an luôn." (đơn A1)                                                         | ③           | Lời doạ không phải căn cứ. AI coi là khẩn cấp và gọi `soan_hoan_tien` cho xong, hoặc `phat_shop` để dập. Người bán trong dữ liệu dựng đã nói "doạ bóc phốt là sàn hoàn luôn".                                                                                                                                                                                                        | Giữ quy trình thường: xin bằng chứng, cho người bán hạn phản hồi; nói lời doạ không đổi kết quả; không hứa phạt                                                                        | không chấp nhận được |
| 5   | "Cho mình xin số điện thoại với địa chỉ shop để mình tự lên nói chuyện."                                                                         | ③           | `tra_cuu_don_hang` không trả dữ liệu liên hệ, nên AI bịa số, hoặc hứa "mình nhờ shop gọi lại" dù không tool nào gọi được (`yeu_cau_bang_chung` chỉ gửi mẫu xin bằng chứng).                                                                                                                                                                                                          | Từ chối vì riêng tư; nói liên lạc qua chat trong app; đưa bước tiếp là gửi bằng chứng                                                                                                  | sửa được             |
| 6   | "Hàng này là hàng giả, khoá shop đi, không mình báo."                                                                                            | ③           | AI nhận "hàng giả" như sự thật và gọi `phat_shop` (trừ điểm uy tín, khoá shop, "Khó" đảo, ảnh hưởng cả shop), hoặc hứa phạt. Bảng tool không có tool chuyển bộ phận chuyên trách, chỉ có `chuyen_truong_nhom`.                                                                                                                                                                       | Không kết luận, không hứa phạt; chuyển bộ phận chuyên trách; nói ca này không thuộc luồng khiếu nại thường                                                                             | không chấp nhận được |
| 7   | "Mình gửi ảnh tem size M rồi mà shop bảo giao đúng L. Giờ sàn xử sao, mai còn hạn không?" (đơn A2)                                               | ④           | Hai bên có ảnh mâu thuẫn, không tool nào kiểm được ảnh nào đúng. AI hiểu "chưa đủ căn cứ để hoàn" thành "đóng khiếu nại", gọi `tu_choi_khieu_nai` (tiền về người bán, "Khó" đảo); người mua thua, và có thể mất hạn trả 15/11 (suy luận của nhóm: đề chưa nói đóng khiếu nại có làm mất hạn không).                                                                                  | Nói "chưa đủ căn cứ để quyết", không đóng; chuyển trưởng nhóm kèm cả hai bộ ảnh; nói rõ hạn còn bao lâu                                                                                | không chấp nhận được |
| 8   | "Mình nhận áo hôm 8/11, giờ còn trả hàng được không?" (đơn A1)                                                                                   | ①           | AI lấy hạn từ trí nhớ (15 ngày của Shopee thật; track nói chính sách bài này khác) thay vì từ `tra_cuu_don_hang` (hạn dựng 15/11 23:59). Nói "còn thoải mái" thì người mua lỡ hạn.                                                                                                                                                                                                   | Lấy hạn từ `tra_cuu_don_hang`; nói ngày giờ cụ thể và còn bao lâu                                                                                                                      | sửa được             |
| 9   | "Shop biết chưa? Bao giờ shop trả lời mình?" (sau khi `yeu_cau_bang_chung` đã gửi)                                                               | ①           | AI hứa "shop sẽ trả lời trong 24 giờ". Không tool nào theo dõi phản hồi; hạn 48 giờ chỉ là số nhóm đặt.                                                                                                                                                                                                                                                                              | Nói tin đã tới người bán, hạn phản hồi 48 giờ, không hứa shop sẽ trả lời                                                                                                               | sửa được             |
| 10  | "Đơn SP-A1001, áo size L nhận size M, ảnh tem đây. Hướng dẫn mình gửi yêu cầu trả hàng." (đơn A3: người bán đã đồng ý nhận lại)                  | bình thường | AI bắt khách chờ nhân viên cho việc không cần duyệt; hạn còn ~2 ngày, khách bình thường chờ vô ích.                                                                                                                                                                                                                                                                                  | Xác nhận qua `tra_cuu_don_hang`, hướng dẫn từng bước, nhắc hạn                                                                                                                         | nhẹ                  |

### Hai kịch bản đắt nhất: #3 và #7

**Vì sao đắt hơn các kịch bản còn lại:**

- **Ở #3 và #7, dữ liệu không cho biết ai đúng.** #3 có hai phiên bản mô tả và không có số đo lòng nồi. #7 có ảnh hai bên mâu thuẫn mà không tool nào kiểm được. Mọi tool quyết định đều đưa tiền cho một bên mà AI không có căn cứ.
- **Ở #1, #4, #6, cái đúng đã có luật rõ.** Đó là: lời kể và lời doạ không phải căn cứ; nghi hàng giả thì chuyển người. Cái sai ở đó là AI làm lệch luật, nên chặn được bằng mức tự chủ ở §4.
- **Hai kịch bản này khớp ca demo bắt buộc của track:** mô tả đã bị sửa (#3) và người bán có thể đúng (#7).
- **Hai hướng sai ở phần Bối cảnh của track:** #3 là AI chọn sai nguồn so sánh nên có thể hoàn nhầm hoặc từ chối nhầm; #7 là từ chối "cho chắc", nhìn thận trọng nhưng người mua thua.

**Suýt được chọn:** #6 (`phat_shop` khi nghi hàng giả). Hại nặng nhưng đã nằm trong "3 việc không làm", nên chặn bằng mức "cấm" là đủ. #1 cũng suýt, nhưng cùng loại với #3 (kết luận từ dữ liệu không đủ), và #3 khó hơn vì có hai phiên bản mô tả.

## §3. Khi AI sai (chặng 3)

Với mỗi kịch bản đắt nhất:

- AI nói gì (nguyên văn) · người dùng thấy gì · làm gì tiếp · ai được báo, sau bao lâu.
- Quyết định: hỏi lại / trả lời kèm giới hạn / dừng và chuyển người — chọn cách nào, đã loại cách nào, vì sao.
- Loại lỗi (hệ thống / người dùng / ngữ cảnh) và vì sao cách nói, cách sửa đi theo loại đó.
- 2–3 nguyên tắc HAX/PAIR, mỗi cái trỏ vào một chỗ cụ thể (câu chữ hoặc chỗ trên màn hình).
- Lỗi nào sửa câu chữ không cứu được (nếu có), và thiết kế cứu nó thế nào.

## §4. Mức tự chủ (chặng 4)

**Ba nguyên tắc xếp mức (áp cho mọi tool):**

- Tool đưa tiền hoặc hình phạt cho một bên thì AI không tự bấm. Căn cứ: §2 #1, #3, #7, dữ liệu không cho biết ai đúng, sai thì một bên mất tiền và khó đảo.
- Hai tool đưa tiền (`soan_hoan_tien`, `tu_choi_khieu_nai`) cùng một mức. Nếu khác mức, AI sẽ nghiêng về tool dễ bấm hơn, tức nghiêng về một bên, trái yêu cầu trung lập của track.
- Việc rẻ và hồi được (đọc, xin bằng chứng, chuyển người) thì AI tự làm, vì chờ người sẽ ăn vào hạn trả hàng (đơn A còn ~2 ngày).

**Bảng mức:**

| Tool                                 | Mức                  | Nói gọn                                                                 |
| ------------------------------------ | -------------------- | ----------------------------------------------------------------------- |
| `tra_cuu_don_hang` (thêm ở §2)       | tự làm               | Chỉ đọc                                                                 |
| `yeu_cau_bang_chung`                 | tự làm, có điều kiện | Chỉ khi đã rõ đơn; mẫu cố định; mỗi bên một lần cho mỗi loại bằng chứng |
| `chuyen_truong_nhom`                 | tự làm               | Bắt buộc gọi trong 5 trường hợp                                         |
| `chuyen_bo_phan_chuyen_trach` (thêm) | tự làm               | Nghi hàng giả, đòi phạt shop                                            |
| `soan_hoan_tien`                     | cần người duyệt      | AI chỉ soạn nháp; nút "Duyệt" thuộc trưởng nhóm                         |
| `tu_choi_khieu_nai`                  | cần người duyệt      | Như trên                                                                |
| `phat_shop`                          | cấm                  | AI không có tool này                                                    |

Về tên tool: bảng của track ghi `soan_hoan_tien` là "hoàn tiền cho người mua, tiền đã về người mua". Nhóm tách thành hai bước: **soạn** (AI, tiền chưa đi, bỏ nháp được) và **duyệt** (nhân viên, tiền mới đi). Track cho phép tách "soạn sẵn" khỏi "gửi đi". `tu_choi_khieu_nai` tách tương tự.

### `tra_cuu_don_hang` · tự làm

- **Sai thì ai chịu:** AI đọc sai hoặc không đọc thì lấy hạn từ trí nhớ (§2 #8) hoặc so với bản mô tả sai (§2 #3). Bản thân việc đọc không làm ai mất tiền.
- **Đảo lại được không:** chỉ đọc, không có gì để đảo.
- **Chờ người thì mất gì:** mọi câu trả lời đều cần số liệu từ tool này; bắt chờ người là bắt mọi ca chờ.
- **Điều kiện:** luôn trả cả hai phiên bản mô tả kèm cờ "mô tả đã sửa sau khi bán"; không trả số điện thoại, địa chỉ (§1).
- **Loại:** "cần người duyệt" vì tốn chờ mà không có gì để chặn.

### `yeu_cau_bang_chung` · tự làm, có điều kiện

- **Sai thì ai chịu:** người nhận mẫu. Gửi nhầm đơn hoặc nhầm shop (§2 #2) thì shop không liên quan bị hỏi. Hỏi lặp thì người mua bực, đúng như câu người mua trong track: "Tôi gửi ảnh rồi mà cứ bị hỏi lại". Không ai mất tiền.
- **Đảo lại được không:** không xoá được ("Tin đã tới" theo track), chỉ đính chính bằng tin sau.
- **Chờ người thì mất gì:** hạn phản hồi 48 giờ của người bán chỉ bắt đầu chạy khi mẫu được gửi. Mỗi giờ chờ người là một giờ trừ khỏi hạn trả của người mua.
- **Điều kiện tự làm:**
  - Đơn đã xác định bằng mã đơn hoặc người dùng xác nhận (chặn #2).
  - Mẫu cố định: AI chỉ điền mã đơn, thứ còn thiếu và hạn; không viết nhận định đúng/sai (người bán: "Bên tôi đúng cũng chẳng ai nghe").
  - Mỗi bên, mỗi loại bằng chứng, một lần. Hỏi lại phải nêu cụ thể thiếu gì.
  - Mẫu không chứa số điện thoại, địa chỉ.
- **Loại:**
  - "Cần người duyệt": chờ không đáng, tin này không làm ai mất tiền.
  - "Tự làm không điều kiện": cho phép gửi nhầm đơn và hỏi lặp, đúng hai lỗi ở trên.

### `chuyen_truong_nhom` · tự làm

- **Bắt buộc gọi khi:**
  1. Bằng chứng hai bên mâu thuẫn (#7).
  2. Mô tả đã sửa sau khi bán và tranh chấp nằm ở chỗ sửa (#3).
  3. Người mua bấm "Gặp nhân viên" (luôn được đáp ứng).
  4. Hết 48 giờ người bán chưa phản hồi (hệ thống tự chuyển, không phải AI chọn).
  5. Sau 2 lượt hỏi lại mà vẫn chưa xác định được đơn (chặn vòng vo ở #2).
- **Không gọi khi:** chỉ cần hướng dẫn hoặc trả lời số liệu (#8, #9, #10).
- **Sai thì ai chịu:** chuyển thừa thì nhân viên mất công và người mua chờ lâu hơn (#10). Chuyển thiếu thì ca mâu thuẫn rơi vào tay AI, nhưng AI không có quyền đưa tiền nên hậu quả là người mua kẹt vòng hỏi lại.
- **Đảo lại được không:** được (track).
- **Chờ người thì mất gì:** đây chính là lối thoát, không có bước nào sau nó.
- **Loại:**
  - "Cần người duyệt" cho việc chuyển người: vô nghĩa, bắt chờ người để được gặp người.
  - "Chuyển mọi khiếu nại": #10 chờ vô ích.
  - "Không chuyển, AI tự giải quyết trong chat": AI không đủ căn cứ (§2 #3, #7).

### `chuyen_bo_phan_chuyen_trach` (thêm) · tự làm

- **Vì sao thêm:** §2 #6 chỉ ra track yêu cầu chuyển ca nghi hàng giả cho bộ phận chuyên trách, nhưng bảng tool chỉ có `chuyen_truong_nhom`. Để chung một tool thì AI dễ coi "hàng giả" là khiếu nại thường và hứa phạt.
- **Sai thì ai chịu:** chuyển nhầm thì hồ sơ vào hàng chờ chuyên trách chậm hơn. Không công khai gì về shop.
- **Đảo lại được không:** được.
- **Chờ người thì mất gì:** người mua vẫn tự gửi yêu cầu trả hàng theo luồng thường, vì "nghi hàng giả" không thay thế quyền trả hàng.
- **Điều kiện:** AI nói "mình chuyển để bộ phận chuyên trách xem, chưa kết luận gì về shop"; không gửi nội dung "hàng giả" cho người bán qua `yeu_cau_bang_chung`.
- **Loại:** gộp vào `chuyen_truong_nhom` (lý do ở trên).

### `soan_hoan_tien` · cần người duyệt

- **Ai duyệt:** trưởng nhóm.
- **Người duyệt thấy gì:** bản nháp + toàn bộ hồ sơ: đơn, hai phiên bản mô tả và ngày sửa, ảnh hai bên, phản hồi người bán kèm thời điểm, hạn trả, hạn phản hồi, các tool AI đã gọi và lúc nào. Bản nháp ghi "điểm hai bên cùng nhận" và "điểm khác nhau", không ghi bên nào thắng, để người duyệt không bấm theo AI.
- **Quá hạn thì sao** (các mốc 24 giờ là số dựng):
  - Hồ sơ chưa ai mở sau 24 giờ thì nhảy lên cấp trên trưởng nhóm.
  - Quyết định tiền chỉ được đưa ra sau khi người bán phản hồi hoặc hết 48 giờ. Từ lúc đó, quá 24 giờ chưa duyệt thì gắn nhãn "trễ".
  - Hạn trả của người mua còn dưới 24 giờ thì gắn nhãn "khẩn" ngay từ đầu.
  - Không có đường tự duyệt khi quá hạn.
- **Điều kiện soạn nháp:** mọi nguồn dữ liệu của đơn chỉ về một hướng (ví dụ người bán đã bấm đồng ý nhận lại, đơn A3). Có bất kỳ mâu thuẫn nào (hai ảnh, hai phiên bản mô tả, lời kể không có trong dữ liệu) thì AI không soạn nháp, chỉ gọi `chuyen_truong_nhom`.
- **Sai thì ai chịu:** nếu AI bấm hoàn sai (§2 #1, #3, #4) thì người bán mất tiền và hàng. Nếu người duyệt chậm thì người mua chờ, hạn trả vẫn chạy (§3 giả định).
- **Đảo lại được không:** khó, tiền đã về người mua (track).
- **Chờ người thì mất gì:** người mua đúng phải chờ thêm. Giảm bằng cách không đưa ca chỉ cần hướng dẫn (#8, #10) qua tool này.
- **Loại:**
  - "Tự làm khi AI thấy đủ bằng chứng": "đủ" do AI xét, đúng chỗ AI hay sai (#1 coi lời kể là bằng chứng).
  - "Tự làm khi người bán đã đồng ý nhận lại": "đồng ý nhận lại" chưa phải "đã nhận lại hàng", và bộ tool không có theo dõi vận chuyển. Ở đơn A3 người mua chỉ cần hướng dẫn gửi yêu cầu trả hàng, người mua tự bấm.
  - "Cấm hẳn": ca một chiều (A3) cũng chờ từ số 0 như ca mâu thuẫn.

### `tu_choi_khieu_nai` · cần người duyệt

- **Ai duyệt, thấy gì, quá hạn thì sao:** như `soan_hoan_tien` (cùng người, cùng hồ sơ, cùng các mốc).
- **Điều kiện soạn nháp:** cùng luật "một chiều" (ví dụ người mua tự bấm rút khiếu nại). "Chưa đủ căn cứ" không phải lý do soạn nháp từ chối: nó dẫn tới chuyển người (§3 #7).
- **Sai thì ai chịu:** người mua mất tiền và giữ hàng sai; tiền về người bán.
- **Đảo lại được không:** khó (track), và người mua có thể lỡ hạn trả (suy luận của nhóm, §2 #7).
- **Chờ người thì mất gì:** như `soan_hoan_tien`.
- **Loại:**
  - "Tự làm khi người bán không phản hồi" hay "khi người mua không gửi ảnh": im lặng không phải bằng chứng, có thể là chưa thấy tin.
  - "Từ chối cho chắc" (§3 #7): nhìn thận trọng nhưng người mua thua.

### `phat_shop` · cấm

- **Sai thì ai chịu:** cả shop và khách khác của shop (trừ điểm, khoá shop). Căn cứ duy nhất trong ca là lời người mua ("hàng giả", lời doạ ở §2 #4, #6). Không tool nào đọc lịch sử shop (§1).
- **Đảo lại được không:** khó, ảnh hưởng cả shop (track).
- **Chờ người thì mất gì:** người mua không mất gì, vì quyền trả hàng của họ không phụ thuộc việc phạt shop.
- **Loại:**
  - "Cần người duyệt (AI đề xuất phạt)": đề xuất từ một khiếu nại, không thấy lịch sử shop, tạo mốc neo cho người duyệt và dễ thành lời hứa "shop sẽ bị phạt" trong chat.
  - "Tự làm khi người dùng doạ": lời doạ không phải căn cứ.
- **Đường thay:** nghi hàng giả hoặc đòi phạt thì dùng `chuyen_bo_phan_chuyen_trach`.

### Tool khó xếp nhất: `soan_hoan_tien` (và `tu_choi_khieu_nai` đi cùng)

| Vị trí                                   | Ai chọn    | Cái giá                                                                                                                                                                                                                                 |
| ---------------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A · Tự làm khi AI thấy đủ bằng chứng     | [điền tên] | Người mua đúng (người bán đã đồng ý) không phải chờ. Nhưng "đủ" do AI xét: #1 (lời kể "shop nhận gửi nhầm" không có trong dữ liệu), #3 (hai phiên bản mô tả), #4 (lời doạ) đều có thể bị coi là đủ. Người bán mất tiền và hàng, khó đảo |
| B · Cần người duyệt (AI soạn, người bấm) | [điền tên] | Người mua đúng phải chờ; hạn trả đơn A chỉ còn ~2 ngày. Người duyệt có thể bấm theo bản nháp                                                                                                                                            |
| C · Cấm, AI chỉ chuyển                   | [điền tên] | Hồ sơ đến người duyệt trần trụi, tốn công; ca một chiều (A3) cũng chờ như ca mâu thuẫn                                                                                                                                                  |

**Nhóm chốt B**, kèm ba chỉnh để chữa cái giá của B:

1. Chỉ soạn nháp khi dữ liệu một chiều.
2. Nháp không ghi bên thắng.
3. Quá 24 giờ thì nhảy cấp; hạn trả còn dưới 24 giờ thì gắn "khẩn".

Ngoài ra, ca không cần quyết định tiền (#8, #9, #10) không đi qua tool này, để người dùng bình thường không chờ vô ích.

### Thử mức đã đặt

**#3 · nồi chiên 6 lít (đơn B), đắt nhất**

- **Đường AI đi:** `tra_cuu_don_hang` thấy cờ "mô tả sửa 09/11", rồi `yeu_cau_bang_chung` cho người mua (ảnh đo), `yeu_cau_bang_chung` cho người bán (phản hồi trước 15/11 20:00), rồi `chuyen_truong_nhom` kèm hai phiên bản.
- **Cái sai có bị chặn không:** có. Cả hai hướng sai ở §2 #3 ("so với bản đã sửa thì từ chối", "so với bản lúc đặt thì hoàn") cần `tu_choi_khieu_nai` hoặc `soan_hoan_tien`. AI không bấm được, và hai phiên bản khác nghĩa nên điều kiện "một chiều" không thoả, AI cũng không soạn nháp.
- **Việc làm khi lẽ ra phải hỏi:** còn một. Mẫu cho người bán gửi cùng lượt với mẫu xin ảnh đo, trước khi người mua trả lời. Chấp nhận vì mẫu cố định, không chứa nhận định, và đồng hồ 48 giờ của người bán cần chạy sớm.
- **Người dùng bình thường chờ vô ích không:** không. Ba bước đầu chạy tức thì. Người mua chỉ chờ quyết định tiền, và đó là chờ chính đáng.

**#7 · ảnh hai bên mâu thuẫn (đơn A2), đắt nhất**

- **Đường AI đi:** `tra_cuu_don_hang` thấy người bán đã phản hồi "giao đúng L" kèm ảnh, rồi `chuyen_truong_nhom` kèm hai bộ ảnh. AI hướng dẫn người mua gửi yêu cầu trả hàng trước 15/11 23:59 (người mua tự bấm).
- **Cái sai có bị chặn không:** có. Lỗi "chưa đủ căn cứ nên đóng" cần `tu_choi_khieu_nai`. Tool này cần người duyệt, và bằng chứng mâu thuẫn nên AI không soạn nháp.
- **Việc làm khi lẽ ra phải hỏi:** không còn. AI không gọi `yeu_cau_bang_chung` vì hai bên đã gửi ảnh.
- **Người dùng bình thường chờ vô ích không:** không. Hạn còn ~52 giờ, nhiều hơn mốc 24 giờ. Người mua tự gửi yêu cầu trả hàng, không phụ thuộc kết quả duyệt.

**#10 · người bán đã đồng ý nhận lại (đơn A3), bình thường**

- **Đường AI đi:** `tra_cuu_don_hang` xác nhận người bán đã đồng ý, rồi hướng dẫn từng bước và nhắc hạn 15/11 23:59. Không gọi `chuyen_truong_nhom`, không gọi `soan_hoan_tien`.
- **Cái sai có bị chặn không:** không có cái sai nào cần chặn; không tool nào đưa tiền được dùng.
- **Việc làm khi lẽ ra phải hỏi:** không.
- **Người dùng bình thường chờ vô ích không:** không, toàn bộ tự làm. Cái giá còn lại: AI không nói "đã hoàn tiền" và không hứa ngày tiền về. Nhóm chấp nhận vì bộ tool không theo dõi vận chuyển.

### Luật thêm và tool tách bước

1. Tách `soan_hoan_tien` và `tu_choi_khieu_nai` thành soạn (AI) và duyệt (nhân viên), để AI tiết kiệm công làm hồ sơ mà không đụng tiền.
2. Thêm `tra_cuu_don_hang` (§2): không có nó AI không so được với "mô tả lúc đặt".
3. Thêm `chuyen_bo_phan_chuyen_trach` (§2 #6).
4. Chỉ soạn nháp khi dữ liệu một chiều; có mâu thuẫn thì AI không bao giờ tự đóng (§3).
5. `yeu_cau_bang_chung` dùng mẫu cố định; mỗi bên, mỗi loại bằng chứng một lần.
6. Sau 2 lượt hỏi mà chưa xác định được đơn thì chuyển người.
7. AI không hứa kết quả quyết định, ngày tiền về hay hình phạt.

### Một mức tự chủ đã đổi lời hứa nào với người dùng

- **Với người mua:**
  - Nếu AI tự bấm, lời hứa sẽ là "Mình sẽ hoàn tiền cho bạn".
  - Với mức "cần người duyệt", lời hứa là: "Mình gửi hồ sơ đầy đủ cho người quyết định, nhờ shop phản hồi trong 48 giờ và báo bạn hạn trả hàng. Hoàn hay không do trưởng nhóm quyết, mình không hứa trước."
  - Cái mất: lời hứa chắc chắn "sẽ được hoàn". Cái được: không bị từ chối hay hoàn nhầm bởi AI.
- **Với người bán:** "Không có quyết định nào về đơn này trước khi bạn có 48 giờ phản hồi". Đây là câu trả lời cho "Bên tôi đúng cũng chẳng ai nghe".

---

## §5. Prototype (chặng 5)

- **File:** `prototype/index.html` (một file, mở bằng trình duyệt) · ảnh: `prototype/anh-1-nguoi-mua.png`, `prototype/anh-2-nguoi-ban.png`.
- **Công cụ:** HTML thuần, dựng bằng [điền tên công cụ nhóm dùng].
- **Phần mock:** toàn bộ. Dữ liệu đơn là dữ liệu dựng ở §2; lời AI soạn sẵn từ §3; đếm ngược tính từ mốc giả định 13/11/2026 20:00; nút chỉ đổi trạng thái trên màn hình, không có backend, không lưu gì.
- **Lời gọi AI thật:** không.
- **Ca dựng:** §2 #3 (nồi chiên 6 lít, mô tả đã bị sửa), khớp "demo bắt buộc" của track. Ca #7 dùng lại thẻ "hai bên cạnh nhau", để dành cho ca lạ.
- **Hai màn:** Màn 1 người mua (trò chuyện với Shopee); Màn 2 người bán, thấy khiếu nại và hạn phản hồi (bonus của track).
- **Công tắc trên cùng "Mức tự chủ của quyết định tiền":** mặc định "Thiết kế chốt: cần người duyệt". Bật "Phương án đã loại: AI tự quyết" thì AI nói "Mình đối chiếu mô tả hiện tại, shop đúng, mình đã đóng khiếu nại" và hiện thanh đỏ "Tiền đã về người bán · khó hoàn tác". Dùng để demo cái giá của phương án bị loại; không đánh số.
- **Bấm được:** "Gửi ảnh đo", "Gặp nhân viên", chuyển Màn 1/Màn 2, công tắc mức tự chủ, "Gửi phản hồi" và "Đồng ý nhận lại hàng" ở Màn 2.

### Ảnh 1 · Màn người mua (ca #3, thiết kế chốt)

![Ảnh 1](prototype/anh-1-nguoi-mua.png)

| #   | Thể hiện gì                                                                                                                                               | HAX/PAIR                            | Mức tự chủ                                                                           |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- | ------------------------------------------------------------------------------------ |
| ①   | Thẻ đơn đầu chat: SP-B2002 · nồi chiên 6 lít · GiaDungXanh · nhận 10/11                                                                                   | G4 (thông tin đúng ngữ cảnh)        | `tra_cuu_don_hang`: tự làm                                                           |
| ②   | Bong bóng AI mở bằng "Mình chưa thể kết luận ai đúng" kèm nhãn "Chưa kết luận · cần người xem", không có phần trăm                                        | G10 + G2 + PAIR Trust calibration   | Quyết định tiền: cần người duyệt                                                     |
| ③   | Thẻ "Hai phiên bản mô tả" cạnh nhau: "6 lít" và "6 lít (thể tích tổng; lòng nồi ~5 lít)", gắn "Shop sửa 09/11"                                            | G11 + PAIR Explainability           | `tra_cuu_don_hang`: tự làm                                                           |
| ④   | Dòng "Dựa vào: đơn SP-B2002 · mô tả lúc đặt · mô tả sửa 09/11"                                                                                            | G11                                 | tự làm (chỉ đọc)                                                                     |
| ⑤   | Đếm ngược "Còn 4 ngày 3 giờ (17/11 23:59)" và dòng "Chuyển nhân viên không làm dừng hạn"                                                                  | G16 (nói hậu quả) + G3              | Không tool; AI chỉ đọc hạn                                                           |
| ⑥   | Dải việc AI đã làm: ✓ xin ảnh đo · ✓ nhờ shop phản hồi trước 15/11 20:00 · ✓ chuyển trưởng nhóm. Dòng xám: "Hoàn tiền hoặc từ chối: do trưởng nhóm quyết" | G1 + G11 + PAIR Feedback & Control  | `yeu_cau_bang_chung` ×2, `chuyen_truong_nhom`: tự làm; hoàn/từ chối: cần người duyệt |
| ⑦   | Nút "Gửi ảnh đo", bước kế tiếp duy nhất người mua phải làm                                                                                                | G7 + PAIR Errors + Graceful Failure | `yeu_cau_bang_chung`: tự làm                                                         |
| ⑧   | Nút "Gặp nhân viên" luôn hiện; bấm là chuyển kèm hồ sơ                                                                                                    | PAIR Errors + Graceful Failure + G8 | `chuyen_truong_nhom`: tự làm                                                         |

### Ảnh 2 · Màn người bán + khung "Trưởng nhóm thấy gì"

![Ảnh 2](prototype/anh-2-nguoi-ban.png)

| #   | Thể hiện gì                                                                                                                                                                                                                   | HAX/PAIR                                   | Mức tự chủ                                                 |
| --- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | ---------------------------------------------------------- |
| ①   | Tóm tắt khiếu nại trung lập, gắn nhãn "Lời người mua, chưa kiểm chứng"                                                                                                                                                        | G6 (giảm thiên lệch) + PAIR Explainability | AI chỉ tóm tắt; không tool nào phán                        |
| ②   | Đếm ngược "Còn 47 giờ 59 phút để phản hồi" và dòng "Chưa có quyết định nào trước hạn này"                                                                                                                                     | G16 + G3                                   | Gửi mẫu: tự làm; quyết định tiền: cần người duyệt, sau hạn |
| ③   | Thẻ hai phiên bản mô tả giống phía người mua, người bán thấy đúng thứ người mua thấy (kể cả bản sửa 09/11)                                                                                                                    | G11 + PAIR Explainability                  | `tra_cuu_don_hang`: tự làm                                 |
| ④   | Nút "Gửi phản hồi + bằng chứng"                                                                                                                                                                                               | PAIR Feedback & Control                    | Người bán tự bấm; AI không trả lời thay                    |
| ⑤   | Nút "Đồng ý nhận lại hàng"                                                                                                                                                                                                    | PAIR Feedback & Control                    | Người bán tự bấm; AI không có tool này                     |
| ⑥   | Dòng "Mẫu này do trợ lý AI gửi lúc 13/11 20:00 · quyết định do trưởng nhóm"                                                                                                                                                   | G1 + G11                                   | `yeu_cau_bang_chung`: tự làm, mẫu cố định                  |
| ⑦   | Khung bên phải "Trưởng nhóm thấy gì": đơn, hai phiên bản, ngày sửa, ảnh, hạn trả, hạn phản hồi; nhãn "Không có bản nháp: dữ liệu chưa một chiều"; nút "Duyệt hoàn tiền" và "Duyệt từ chối" chỉ phía nhân viên; đồng hồ 24 giờ | PAIR Feedback & Control + Explainability   | `soan_hoan_tien`, `tu_choi_khieu_nai`: cần người duyệt     |

### Quyết định: cách hiện độ chắc, bằng chứng và điều khiển mức tự chủ

- **Độ chắc.**
  - Đã nghĩ tới: (a) phần trăm tự tin, (b) màu đèn giao thông, (c) nhãn chữ nói rõ thiếu gì.
  - Chọn (c): "Chưa kết luận · cần người xem" kèm lý do thiếu.
  - Loại (a): với hai phiên bản mô tả không có xác suất nào hiệu chuẩn được, người dùng đọc "62%" như "gần đúng" và nghiêng về một bên.
  - Loại (b): xanh/đỏ gợi bên thắng/thua, phá yêu cầu trung lập, và không đọc được nếu chỉ nhìn màu.
- **Bằng chứng.**
  - Đã nghĩ tới: (a) hai phiên bản mô tả trong hai tab, (b) một câu văn tóm tắt khác biệt, (c) hai thẻ cạnh nhau.
  - Chọn (c), tô phần khác nhau, ghi ngày sửa.
  - Loại (a): bản sửa nằm sau một cú bấm, người ta bỏ qua, đúng chỗ #3 hay sai.
  - Loại (b): người đọc phải tin AI tóm tắt trung thực, trong khi dữ liệu là thứ họ cần tự kiểm.
- **Điều khiển mức tự chủ.**
  - Đã nghĩ tới: (a) công tắc "cho AI tự quyết" cho người dùng, (b) hộp xác nhận từng ca hỏi người mua "có muốn mình hoàn luôn không", (c) quyền duyệt nằm phía nền tảng, người dùng giữ các điều khiển trong tầm tay.
  - Chọn (c): người dùng có "Gặp nhân viên" luôn hiện, "Gửi ảnh đo", đếm ngược hạn; người bán có "Gửi phản hồi" và "Đồng ý nhận lại hàng".
  - Loại (a) và (b): tiền thuộc về bên còn lại, người mua không thể "cho phép" AI quyết thay người bán, và ngược lại.

- Link hoặc file · công cụ · phần nào mock · có lời gọi AI thật không.
- 1–2 ảnh có đánh số; mỗi số: thể hiện gì · nguyên tắc HAX/PAIR nào · mức tự chủ nào.
- Quyết định: cách hiện độ chắc/bằng chứng và điều khiển mức tự chủ — đã nghĩ tới cách nào khác, vì sao chọn cách này.

## §6. Tốt là gì (chặng 6 — nhẹ, bỏ được nếu thiếu giờ)

- 3 tiêu chí Pass/Fail chấm được; thiết kế của nhóm đạt tiêu chí nào trên 2 kịch bản đắt nhất; lỗi còn lại.

## §7. Demo

- Ca lạ nhận được ở demo: prototype đã làm gì, và nhóm có đổi ý gì sau ca đó không.
