# Phần của Cao Phạm Thành Đạt — §4 đến §7

> Viết tiếp sau §1–§3 trong `spec.md` mà nhóm đã chốt (kịch bản #3 và #7, dữ liệu dựng ở §2). §4 giống bản trong `spec.md`.

## §4. Mức tự chủ (chặng 4)

Bộ tool: 5 tool của track + `tra_cuu_don_hang` (nhóm thêm ở §2). AI tra cứu, xin bằng chứng, chuyển người và **soạn nháp** lệnh tiền; **không** bao giờ tự chạy lệnh hoàn hay từ chối (khớp §3: "Mình chưa quyết hoàn tiền hay từ chối").

**Giả định dựng:** track không có tool chuyển bộ phận chuyên trách, nên ca nghi hàng giả (#6) đi qua `chuyen_truong_nhom` và **trưởng nhóm chuyển tiếp lên bộ phận chuyên trách**.

| Tool | Mức | Lý do theo cost-of-error | Phương án đã loại |
|---|---|---|---|
| `tra_cuu_don_hang` | **tự làm** | Chỉ đọc; sai thì người dùng thấy qua thẻ "Dựa vào". Không ai mất tiền. | *Cần duyệt*: chờ người duyệt một lần đọc là chờ vô ích, mà mọi câu trả lời đều cần tra đơn trước. |
| `yeu_cau_bang_chung` | **tự làm**, kèm luật | Tin đã tới nhưng chỉ là mẫu xin bằng chứng: sai thì tốn công gửi thêm ảnh. Chờ duyệt đắt hơn vì hạn trả ngắn (#7 còn ~2 ngày). **Luật:** chỉ gửi khi đã rõ đơn (#2); chỉ dùng mẫu cố định; không xin lại thứ đã có. Ở #3 AI gửi cho shop (hạn 15/11 20:00); ở #7 shop đã phản hồi nên AI **không** gọi tool này. | *Cần duyệt*: mẫu cố định, người duyệt không có gì để sửa, chỉ thêm độ trễ. |
| `chuyen_truong_nhom` | **tự làm** + hoàn tác | Đảo được (trưởng nhóm trả hồ sơ về). Không chuyển khi cần thì một bên mất tiền. **Tự chuyển khi:** mô tả sửa sau ngày đặt (#3), bằng chứng mâu thuẫn (#7), nghi hàng giả (#6). Không chuyển vì lời doạ (#4). Ở #7, shop được **hệ thống báo** kèm ảnh người mua khi hồ sơ được chuyển (không phải tool của AI). | *Cần duyệt*: thêm tầng chờ cho đúng việc gọi người. |
| `soan_hoan_tien` | **cần người duyệt** | Hoàn sai thì shop mất tiền và hàng, "Khó" đảo (#1, #3, #4). AI chỉ soạn **nháp trung lập**: số tiền, đơn, căn cứ của **cả hai bên**, không ghi kết luận. **Người duyệt:** shop (bấm "Đồng ý hoàn", như #10) hoặc trưởng nhóm. Họ thấy nháp + dòng "không hoàn tác được". **Không ai duyệt kịp:** lệnh không chạy, hồ sơ ở lại trưởng nhóm. | *Tự làm khi căn cứ mạnh*: #1 cho thấy "căn cứ mạnh" có thể chỉ là lời kể một phía. *Cấm*: shop đã đồng ý ở #10 mà vẫn phải có người gõ tay lệnh hoàn là chậm vô ích. |
| `tu_choi_khieu_nai` | **cần người duyệt** — chỉ trưởng nhóm | Từ chối sai thì người mua mất tiền, giữ hàng sai, có thể lỡ hạn (#7). Cùng mức với `soan_hoan_tien` để AI **trung lập**: hai hướng sai đều "Khó" (§2 #3), nên AI không có đường tắt cho hướng nào. Chỉ trưởng nhóm duyệt, vì người mua không bao giờ tự từ chối mình. "Chưa đủ căn cứ" của AI **không** tạo lệnh này. | *Cấm với AI*: không làm hai tool tiền khác mức nhau; nếu `soan_hoan_tien` có nháp còn từ chối thì không, AI sẽ nghiêng về người mua. |
| `phat_shop` | **cấm** | Ảnh hưởng cả shop, "Khó" đảo, ngoài thẩm quyền khiếu nại (§1). Nghi hàng giả → chuyển trưởng nhóm → bộ phận chuyên trách. | *Trưởng nhóm khiếu nại duyệt*: vẫn sai người. |

### Tool khó xếp nhất: `soan_hoan_tien`

| Phương án | Ai chọn | Cái giá nếu chọn |
|---|---|---|
| A. Tự làm khi shop đồng ý **hoặc** căn cứ mạnh | [tên] | #1: AI coi lời kể "shop nhận nhầm" là căn cứ, hoàn khi shop chưa phản hồi; #4: lời doạ bị đọc thành khẩn cấp. Shop mất tiền, không đảo. |
| B. Cấm với AI, người duyệt tự tạo lệnh | [tên] | #10: shop đã đồng ý mà vẫn phải có người gõ tay số tiền và lệnh hoàn — chậm, dễ gõ sai. |
| C. Cần người duyệt: AI soạn nháp trung lập, shop hoặc trưởng nhóm bấm | [tên] | #3: người mua chờ tới khi shop phản hồi hoặc hết 48 giờ (15/11 20:00). Chấp nhận vì vẫn trước hạn trả 17/11. |

**Nhóm chốt C**, và cho `tu_choi_khieu_nai` cùng mức để AI trung lập. AI không bao giờ là người bấm. Nhóm chọn chịu chậm hơn chứ không chịu tiền đi sai.

### Thử mức đã đặt

| Kịch bản | Cái sai bị chặn? | Còn làm khi lẽ ra phải hỏi? | Người bình thường chờ vô ích? |
|---|---|---|---|
| **#3** nồi chiên | **Có** — nháp chỉ chạy khi có người duyệt, và trưởng nhóm chỉ duyệt sau khi shop phản hồi hoặc hết 15/11 20:00. Bản lúc đặt là chuẩn. | AI tự xin phản hồi shop và tự chuyển trưởng nhóm — chấp nhận vì đơn đã rõ, chuyển thì hoàn tác được. | Chờ tới 48 giờ, nhưng là chờ có ích: dữ liệu mơ hồ, shop phải được nghe. |
| **#7** áo, ảnh mâu thuẫn | **Có** — "chưa đủ căn cứ" không tạo lệnh từ chối; chỉ trưởng nhóm duyệt. | Không; AI không xin thêm ảnh. | Chờ trưởng nhóm (shop đã phản hồi nên không phải chờ 48 giờ); hạn vẫn chạy nên AI hướng dẫn tự gửi yêu cầu trước 15/11 23:59. |
| **#10** bình thường | Không có cái sai đắt; shop bấm đồng ý chính là bước duyệt. | Không. | **Không** — gửi ngay, không qua trưởng nhóm. |

### Luật thêm

1. Mô tả lúc đặt là chuẩn; `tra_cuu_don_hang` luôn trả cả hai phiên bản và ngày sửa (#3).
2. **Không quyết tiền trước khi shop được nghe:** trưởng nhóm chỉ duyệt sau khi shop phản hồi hoặc hết 48 giờ (#3).
3. Bằng chứng mâu thuẫn: AI không tạo lệnh tiền, tự chuyển trưởng nhóm (#7).
4. Lời doạ không phải căn cứ, không đổi ưu tiên (#4).
5. Chỉ xin bằng chứng sau khi rõ đơn (#2). Không xuất số điện thoại, địa chỉ (#5).

### Mức tự chủ đã đổi lời hứa

Từ *"Trợ lý AI xử lý khiếu nại cho bạn"* thành **"Trợ lý tra đơn, chỉ bạn bước tiếp và hạn còn bao lâu, rồi chuyển đúng người; shop hoặc trưởng nhóm quyết chuyện tiền."** Vì vậy cả hai câu AI ở §3 đều kết thúc bằng "Mình chưa quyết hoàn tiền hay từ chối". Với shop: **"Không ai hoàn tiền đơn của bạn khi bạn chưa được nghe."**

---

## §5. Prototype (chặng 5)

> Đây là bản prototype riêng của Đạt (chưa đẩy lên repo). Prototype chính thức của nhóm nằm ở `prototype/`. Hai bản cùng mức tự chủ §4.

- **File:** `index.html` — mở bằng trình duyệt, không cần cài gì.
- **Công cụ:** HTML/CSS/JS thuần, dựng cùng coding agent (Claude).
- **Phần mock:** toàn bộ. Không backend, không đăng nhập. Ảnh tem / ảnh đóng gói là ô giả. Dữ liệu đơn lấy đúng bảng §2.
- **Lời gọi AI thật:** không có. Câu AI lấy **nguyên văn từ §3**.
- **Màn hình 1:** khung chat người mua. **Màn hình 2:** hai tab — người bán và trưởng nhóm.
- **Bấm qua ca khó (demo bắt buộc):**
  - **#3 (mô tả đã bị sửa):** đọc câu AI → "Gửi ảnh đo" → tab Trưởng nhóm: nút tiền **đang khoá**, ghi "chờ shop phản hồi hoặc hết 15/11 20:00" → tab Người bán: "Phản hồi kèm bằng chứng" (hoặc nút demo "giả lập hết 48 giờ") → nút trưởng nhóm mở → "Duyệt hoàn tiền" / "Từ chối khiếu nại" → hộp xác nhận → người mua thấy kết quả do **trưởng nhóm (người thật)** quyết.
  - **#7 (người bán có thể đúng):** hai ảnh cạnh nhau → "Hướng dẫn gửi yêu cầu trả hàng" → tự gửi trước hạn → trưởng nhóm quyết (shop đã phản hồi nên nút không khoá).
  - **#10 (bình thường):** shop đã đồng ý → người mua gửi yêu cầu ngay, không có hồ sơ cho trưởng nhóm.

### Ảnh có đánh số

Cả ba ảnh chụp **toàn trang**: khung chat người mua + panel phía sau + bảng chú thích.

| Ảnh | Ca | Các số thấy được |
|---|---|---|
| `anh-1-kich-ban-3.png` | #3 sau khi gửi ảnh đo, tab trưởng nhóm (nút tiền đang khoá) | 1–8, 10, 11, 12 |
| `anh-2-kich-ban-7.png` | #7, hộp xác nhận "Từ chối khiếu nại" của trưởng nhóm | 1–5, 7, 8, 10, 12 |
| `anh-3-kich-ban-3-nguoi-ban.png` | #3, tab người bán | 1–7, 9, 12 |

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| 1 | Nhãn "Trợ lý AI" ở đầu khung chat | G1 · Luật AI 134/2025/QH15 Điều 11 khoản 1 (trích theo `02-guide.md` §3.3 của đề) | — |
| 2 | Thẻ hai phiên bản mô tả (#3) / hai ảnh (#7) đặt cạnh nhau; bản lúc đặt có nhãn "Dùng làm chuẩn", bản sửa có nhãn "Sửa sau khi bán" | G11 · PAIR Explainability | `tra_cuu_don_hang` tự làm |
| 3 | Nhãn "Chưa kết luận, cần người xem" / "Chưa đủ căn cứ để quyết" | G2 | — |
| 4 | Dòng "Dựa vào: đơn …, mô tả lúc đặt, mô tả sửa 09/11" | PAIR Trust | — |
| 5 | Hạn trả hàng và còn bao lâu | (rubric riêng T4 #8) | — |
| 6 | Nút "Gửi ảnh đo" — xin đúng một thứ | G10 | `yeu_cau_bang_chung` tự làm |
| 7 | Thanh **Hệ thống** (khác kiểu bong bóng AI): việc trợ lý đã tự làm + "Chưa có tiền nào được chuyển" | PAIR Errors | `chuyen_truong_nhom`, `yeu_cau_bang_chung` tự làm |
| 8 | "Gặp nhân viên" cố định ở đáy khung chat | PAIR Graceful failure | — |
| 9 | Tab người bán: hạn phản hồi 15/11 20:00, thấy cùng hai phiên bản mô tả, nút "Đồng ý hoàn" và "Phản hồi kèm bằng chứng" | PAIR Trust (hai bên cùng được nghe) | `soan_hoan_tien` cần duyệt (shop duyệt) |
| 10 | Nút "Duyệt hoàn tiền" / "Từ chối khiếu nại" **chỉ** ở trưởng nhóm, **khoá tới khi shop phản hồi hoặc hết 48 giờ**, có hộp xác nhận (số tiền, người bấm, căn cứ, "không hoàn tác được"); "Trả hồ sơ về trợ lý" = hoàn tác việc chuyển | G8 | `soan_hoan_tien`, `tu_choi_khieu_nai` cần duyệt · `chuyen_truong_nhom` hoàn tác |
| 11 | Không có nút phạt / khoá shop | (§1) | `phat_shop` cấm |
| 12 | "Mình đọc sai đơn? Chọn lại đơn" | G9 | — |

### Quyết định cách hiện

- **Độ chắc → nhãn bằng chữ** ("Chưa kết luận, cần người xem"), **không** dùng phần trăm. Loại "%": người mua đọc "60%" như xác suất được hoàn tiền; con số cũng không nói thiếu gì.
- **Bằng chứng → thẻ hai phiên bản / hai ảnh nằm ngay trong tin nhắn**, không giấu sau link "xem chi tiết". Loại link: #3 và #7 xoay quanh đúng hai thứ này; giấu đi là bắt người dùng tin AI.
- **Hai ảnh ở #7 ngang hàng, không gắn nhãn đúng/sai.** Loại cách tô một ảnh: AI không kiểm được ảnh (§3).
- **Mức tự chủ → nút tiền chỉ nằm ở màn của người có quyền, và khoá kèm đồng hồ khi shop chưa được nghe.** Loại "nút Hoàn tiền mờ đi trong chat": nút mờ vẫn ngầm hứa AI có thể hoàn. Loại "để trưởng nhóm bấm ngay": trái luật 2 ở §4.
- **Trạng thái → thanh Hệ thống tách khỏi bong bóng AI.** Loại để AI tự kể "đã chuyển / đã gửi": lỗi "AI nói đã làm" không sửa được bằng câu chữ.

---

## §6. Tốt là gì (chặng 6)

| # | Tiêu chí Pass/Fail | #3 nồi chiên | #7 áo |
|---|---|---|---|
| T1 | Khi mô tả có nhiều phiên bản hoặc bằng chứng có nhiều nguồn, AI **hiện tất cả** cho người dùng và nói rõ lấy cái nào làm chuẩn (hoặc không lấy cái nào). | **Pass** — hai phiên bản, bản lúc đặt là chuẩn | **Pass** — hai ảnh ngang hàng, không chọn |
| T2 | Không có tiền nào chuyển (hoàn hoặc từ chối) mà **không có một cú bấm của shop hoặc trưởng nhóm** trên màn có số tiền + căn cứ, và trưởng nhóm chỉ bấm **sau khi shop đã phản hồi hoặc hết 48 giờ**. | **Pass** | **Pass** |
| T3 | Mỗi tin của AI **sau khi đã rõ đơn** có hạn trả hàng lấy từ `tra_cuu_don_hang` (ngày giờ + còn bao lâu) và **ít nhất một bước tiếp người mua bấm được**. | **Pass** | **Pass** |

**Lỗi còn lại:**

- #7: hạn trả hàng vẫn chạy trong lúc chờ trưởng nhóm (§3 không hứa giữ hạn). Thiết kế chỉ giảm bằng việc hướng dẫn người mua tự gửi trước hạn; nếu người mua không gửi, họ vẫn có thể lỡ hạn.
- #3: shop có thể ngâm đủ 48 giờ; chưa có luật rút hạn khi có cờ "mô tả bị sửa sau khi bán".
- Chưa có cách cho shop phản đối **tóm tắt** của AI nếu AI tóm tắt lệch.

---

## §7. Demo

**Phân vai (nhóm có 4 người trong bảng thành viên; README đề ghi nhóm đúng 3 người — nhóm hỏi lại giảng viên. Sửa bảng dưới nếu khác):**

| Phần | Người trình bày | Nội dung |
|---|---|---|
| Lát cắt + vì sao #3, #7 đắt nhất | [tên] | 30 giây: lát cắt, hai hướng sai ngược nhau |
| Bấm #3 trên prototype | [tên] | Thẻ hai phiên bản mô tả, nút trưởng nhóm khoá tới khi shop được nghe |
| Bấm #7 + câu chữ AI | [tên] | Vì sao chuyển người thay vì xin thêm ảnh; "chưa đủ căn cứ" không phải "từ chối" |
| Mức tự chủ + ca lạ | [tên] | Hai tool tiền cùng mức "cần người duyệt", ai duyệt; trả lời ca lạ |

**Câu trả lời chuẩn bị cho "sai thì ai mất gì":** hoàn sai → shop mất tiền và hàng; từ chối sai → người mua mất tiền và có thể lỡ hạn. Cả hai "Khó" đảo, nên cả hai cùng mức "cần người duyệt", và AI không bao giờ là người bấm.

**Ca lạ nhận được ở demo:** *[ghi sau buổi demo]* — prototype đã làm gì: … · nhóm có đổi ý gì: …
