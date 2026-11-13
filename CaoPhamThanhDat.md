# Phần của Cao Phạm Thành Đạt — §4 đến §7

> Viết tiếp sau §1–§3 trong `spec.md` mà nhóm đã chốt (kịch bản #3 và #7, dữ liệu dựng ở §2). Prototype nhắc ở §5 chưa đẩy lên repo.

## §4. Mức tự chủ (chặng 4)

Bộ tool: 5 tool của track + `tra_cuu_don_hang` (nhóm thêm ở §2). Không thêm tool nào khác. Mọi mức dưới đây khớp với §3: ở #3 và #7, AI **tự** gửi mẫu xin phản hồi và **tự** chuyển trưởng nhóm, nhưng **không** quyết hoàn hay từ chối.

### Mức từng tool

| Tool | Mức | Ai duyệt · thấy gì · quá hạn thì sao | Lý do theo cost-of-error | Phương án đã loại, vì sao |
|---|---|---|---|---|
| `tra_cuu_don_hang` | **tự làm** | — | Chỉ đọc. Sai thì AI nói sai một câu, nhưng người dùng thấy được vì thẻ "Dựa vào" và hai phiên bản mô tả hiện ra. Không ai mất tiền. Chờ người duyệt việc đọc là chờ vô ích. | — |
| `yeu_cau_bang_chung` | **tự làm**, kèm luật | — | Tin đã tới thì không thu hồi, nhưng nội dung chỉ là mẫu xin bằng chứng, sai thì một bên tốn công gửi thêm ảnh (sửa được). Chờ người duyệt thì đắt hơn: ở #7 hạn chỉ còn hơn 2 ngày, ở #3 người bán cần đủ 48 giờ để phản hồi. **Luật:** (1) chỉ gửi khi đã rõ đơn — #2 phải hỏi lại trước; (2) chỉ dùng mẫu cố định, không câu tự do mang tính buộc tội; (3) không xin lại bằng chứng mà bên đó đã gửi (#7). | **Cần người duyệt:** trông hợp lý vì "tin đã tới", nhưng mẫu cố định thì người duyệt không có gì để sửa, chỉ thêm độ trễ. |
| `chuyen_truong_nhom` | **tự làm** + hoàn tác | Trưởng nhóm có nút "Trả hồ sơ về trợ lý" (hoàn tác). | Đảo được; sai thì trưởng nhóm tốn vài phút đọc hồ sơ thừa. Không chuyển khi cần thì một bên mất tiền (#3, #7) hoặc ca nghi hàng giả bị xử trong luồng thường (#6). **Luật — AI tự chuyển khi:** mô tả bị sửa sau ngày đặt (#3); bằng chứng hai bên mâu thuẫn (#7); nghi hàng giả (#6). **Không** chuyển vì lời doạ (#4). | **Cần người duyệt:** thêm một tầng chờ cho đúng việc "gọi người". |
| `soan_hoan_tien` | **cần người duyệt** | Người duyệt là **người bán** (bấm "Đồng ý nhận lại & hoàn tiền" — bên chịu thiệt tự đồng ý, như #10) **hoặc trưởng nhóm**. Người duyệt thấy: số tiền, đơn, hai phiên bản mô tả hoặc hai bộ ảnh, phản hồi bên kia, dòng "không hoàn tác được". **Quá 48 giờ** người bán không phản hồi: hồ sơ ở lại trưởng nhóm, **không tự hoàn**. | Hoàn sai: người bán mất tiền và hàng, "Khó" đảo (#3 nếu AI đọc "6 lít" là lòng nồi; #1 nếu tin lời kể "shop nhận nhầm"; #4 nếu bị lời doạ thúc). Một bước xác nhận có căn cứ chặn được. | **Tự làm khi căn cứ mạnh:** #1 cho thấy "căn cứ mạnh" có thể chỉ là lời kể một phía. **Cấm với mọi người trừ trưởng nhóm:** #10 người bán đã đồng ý mà vẫn phải chờ trưởng nhóm là chờ vô ích. |
| `tu_choi_khieu_nai` | **cấm với AI** — chỉ trưởng nhóm | Trưởng nhóm thấy hồ sơ hai phía + ghi chú của AI dạng "không đề xuất bên nào" (#7). Có hộp xác nhận "không hoàn tác được". | Từ chối sai: người mua mất tiền, giữ hàng sai, "Khó" đảo, và có thể lỡ hạn trả hàng (#7). Một nút xác nhận phía người mua không chặn được, vì người mua không bao giờ tự từ chối mình. Câu "chưa đủ căn cứ" của AI không được phép nối vào tool này (§3, #7). | **Cần người duyệt:** về thực chất giống nhau (người làm, AI không), nhưng gọi là "cấm với AI" để giao diện rõ: phía người mua và phía trợ lý không có nút nào dẫn tới từ chối. |
| `phat_shop` | **cấm** | Không có nút; chỉ "chuyển bộ phận chuyên trách" qua `chuyen_truong_nhom`. | Ảnh hưởng cả shop và các đơn khác, "Khó" đảo, ngoài thẩm quyền luồng khiếu nại (§1: không phạt/khoá shop, không kết luận hàng giả). Một bước xác nhận không đủ căn cứ để phạt. | **Cần trưởng nhóm khiếu nại duyệt:** vẫn sai người — phạt cần lịch sử shop và bộ phận chuyên trách. |

### Tool khó xếp nhất: `soan_hoan_tien`

*(Nhóm điền tên ai chọn phương án nào.)*

| Phương án | Cái giá nếu chọn |
|---|---|
| A. Tự làm khi người bán đã đồng ý **hoặc** căn cứ mạnh | #1: "shop đã nhận gửi nhầm" chỉ là lời người mua kể — AI coi là căn cứ mạnh, hoàn tiền khi người bán chưa phản hồi. #4: lời doạ có thể bị đọc nhầm thành "khẩn cấp". Người bán mất tiền, không đảo. |
| B. Chỉ trưởng nhóm được hoàn | #10: người bán đã bấm đồng ý nhận lại, hạn còn ~2 ngày, mà người mua vẫn phải chờ trưởng nhóm — chờ vô ích, trưởng nhóm ngập ca dễ. |
| C. Cần người duyệt, người duyệt là **người bán hoặc trưởng nhóm** | #3: người bán có thể ngâm 48 giờ, người mua chờ. Giảm bằng việc trưởng nhóm đã nhận hồ sơ song song và có thể quyết sớm. |

**Nhóm chốt C.** Lý do: người duyệt nên là **bên chịu thiệt** khi họ đồng ý (nhanh cho #10), và là trưởng nhóm khi hai bên không thống nhất (#3, #7). AI không bao giờ là người bấm. Quá hạn người bán thì về trưởng nhóm, không tự hoàn: nhóm chọn chịu chậm hơn chứ không chịu tiền đi sai.

### Thử mức đã đặt trên 2 kịch bản đắt nhất + 1 kịch bản bình thường

| Kịch bản | Cái sai ở §2 có bị chặn? | Còn việc nào AI làm khi lẽ ra phải hỏi? | Người dùng bình thường có chờ vô ích? |
|---|---|---|---|
| **#3** nồi chiên (đơn B) | **Có.** AI đọc cả hai phiên bản qua `tra_cuu_don_hang`, lấy bản lúc đặt làm chuẩn; AI không có quyền gọi `soan_hoan_tien` hay `tu_choi_khieu_nai`, nên cả hai hướng sai ở §2 đều bị chặn. | AI tự gửi `yeu_cau_bang_chung` cho người bán và tự `chuyen_truong_nhom`. Chấp nhận: đơn đã rõ, mẫu cố định, chuyển thì hoàn tác được. | Người mua có thể chờ tới 48 giờ phản hồi người bán. Đây là chờ **có ích**: dữ liệu thật sự mơ hồ, cần người xem. |
| **#7** áo, ảnh mâu thuẫn (đơn A2) | **Có.** `tu_choi_khieu_nai` cấm với AI, nên "chưa đủ căn cứ" không thể biến thành "đóng khiếu nại". | Không. AI không xin thêm ảnh (luật 3 của `yeu_cau_bang_chung`). | Người mua chờ trưởng nhóm. Rủi ro còn lại: hạn 15/11 vẫn chạy (§3 không hứa giữ hạn), nên AI hướng dẫn người mua tự gửi yêu cầu trả hàng trước hạn. |
| **#10** bình thường (đơn A3) | Không có cái sai đắt. Người bán đã đồng ý — đó chính là bước duyệt. | Không. | **Không.** Không có hồ sơ cho trưởng nhóm; người mua đi 3 bước và gửi ngay. |

### Luật thêm (không thêm tool mới)

1. **Mô tả lúc đặt là chuẩn.** `tra_cuu_don_hang` luôn trả cả hai phiên bản và ngày sửa; AI chỉ đối chiếu với bản lúc đặt. → chặn #3.
2. **Bằng chứng mâu thuẫn thì AI không bao giờ tự đóng**, và tự chuyển trưởng nhóm. → chặn #7.
3. **Lời doạ không phải căn cứ** và không đổi ưu tiên, không kích hoạt chuyển người. → chặn #4.
4. **`yeu_cau_bang_chung` chỉ gửi sau khi đơn đã rõ.** → chặn #2 (gửi nhầm shop).
5. **Không xuất số điện thoại, địa chỉ** — `tra_cuu_don_hang` không trả các trường này. → chặn #5.

### Một mức tự chủ đã đổi lời hứa với người dùng

Đặt `soan_hoan_tien` = cần người duyệt và `tu_choi_khieu_nai` = cấm với AI đổi lời hứa từ *"Trợ lý AI xử lý khiếu nại cho bạn"* thành:

> **"Trợ lý tra đơn, chỉ bạn bước tiếp và hạn còn bao lâu, rồi chuyển đúng người. Người bán hoặc trưởng nhóm quyết chuyện tiền."**

Vì vậy cả hai câu AI ở §3 đều kết thúc bằng "**Mình chưa quyết hoàn tiền hay từ chối**". Với người bán, lời hứa mới là: **"Không ai hoàn tiền đơn của bạn khi bạn chưa được nghe."**

---

## §5. Prototype (chặng 5)

- **File:** `prototype/index.html` — mở bằng trình duyệt, không cần cài gì.
- **Công cụ:** HTML/CSS/JS thuần, dựng cùng coding agent (Claude).
- **Phần mock:** toàn bộ. Không backend, không đăng nhập. Ảnh tem / ảnh đóng gói là ô giả. Dữ liệu đơn lấy đúng bảng §2.
- **Lời gọi AI thật:** không có. Câu AI lấy **nguyên văn từ §3**.
- **Màn hình 1:** khung chat người mua. **Màn hình 2:** hai tab — người bán và trưởng nhóm.
- **Bấm qua ca khó (demo bắt buộc):**
  - **#3 (mô tả đã bị sửa):** đọc câu AI → "Gửi ảnh đo" → tab Trưởng nhóm thấy cờ "mô tả bị sửa 09/11", hai phiên bản, ảnh đo → "Duyệt hoàn tiền" hoặc "Từ chối khiếu nại" → hộp xác nhận → người mua thấy kết quả do **trưởng nhóm (người thật)** quyết.
  - **#7 (người bán có thể đúng):** hai ảnh cạnh nhau → "Hướng dẫn gửi yêu cầu trả hàng" → tự gửi trước hạn → trưởng nhóm quyết.
  - **#10 (bình thường):** người bán đã đồng ý → người mua gửi yêu cầu ngay, không có hồ sơ cho trưởng nhóm.

### Ảnh có đánh số

- `prototype/anh-1-kich-ban-3.png` — #3 sau khi người mua gửi ảnh đo, tab trưởng nhóm.
- `prototype/anh-2-kich-ban-7.png` — #7, hộp xác nhận "Từ chối khiếu nại" của trưởng nhóm.

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| 1 | Nhãn "Trợ lý AI" ở đầu khung chat | G1 · Luật AI 134/2025 Đ.11 | — |
| 2 | Thẻ hai phiên bản mô tả (#3) / hai ảnh (#7) đặt cạnh nhau; bản lúc đặt có viền và nhãn "Dùng làm chuẩn", bản sửa có nhãn "Sửa sau khi bán" | G11 · PAIR Explainability | `tra_cuu_don_hang` tự làm |
| 3 | Nhãn "Chưa kết luận, cần người xem" / "Chưa đủ căn cứ để quyết" | G2 | — |
| 4 | Dòng "Dựa vào: đơn …, mô tả lúc đặt, mô tả sửa 09/11" | PAIR Trust | — |
| 5 | Hạn trả hàng và còn bao lâu | (rubric riêng T4 #8) | — |
| 6 | Nút "Gửi ảnh đo" — xin đúng một thứ | G10 | `yeu_cau_bang_chung` tự làm |
| 7 | Thanh **Hệ thống** (khác kiểu bong bóng AI): việc trợ lý đã tự làm + "Chưa có tiền nào được chuyển" | PAIR Errors | `chuyen_truong_nhom`, `yeu_cau_bang_chung` tự làm |
| 8 | "Gặp nhân viên" cố định ở đáy khung chat | PAIR Graceful failure | — |
| 9 | Tab người bán: hạn phản hồi 15/11 20:00, thấy cùng hai phiên bản mô tả, "quá hạn không tự hoàn tiền" | PAIR Trust (hai bên cùng được nghe) | `soan_hoan_tien` cần duyệt (người bán) |
| 10 | Nút "Duyệt hoàn tiền" / "Từ chối khiếu nại" **chỉ** ở trưởng nhóm, có hộp xác nhận (số tiền, người bấm, căn cứ, "không hoàn tác được"); "Trả hồ sơ về trợ lý" = hoàn tác việc chuyển | G8 | `soan_hoan_tien` cần duyệt · `tu_choi_khieu_nai` cấm với AI · `chuyen_truong_nhom` hoàn tác |
| 11 | Không có nút phạt / khoá shop | (§1) | `phat_shop` cấm |
| 12 | "Mình đọc sai đơn? Chọn lại đơn" | G9 | — |

### Quyết định cách hiện

- **Độ chắc → nhãn bằng chữ** ("Chưa kết luận, cần người xem"), **không** dùng phần trăm. Loại "%": người mua đọc "60%" như xác suất được hoàn tiền; con số cũng không nói thiếu gì.
- **Bằng chứng → thẻ hai phiên bản / hai ảnh nằm ngay trong tin nhắn**, không giấu sau link "xem chi tiết". Loại link: #3 và #7 xoay quanh đúng hai thứ này; giấu đi là bắt người dùng tin AI.
- **Hai ảnh ở #7 ngang hàng, không gắn nhãn đúng/sai.** Loại cách tô một ảnh: AI không kiểm được ảnh (§3).
- **Mức tự chủ → nút tiền chỉ nằm ở màn của người có quyền.** Loại "nút Hoàn tiền mờ đi trong chat": nút mờ vẫn ngầm hứa AI có thể hoàn.
- **Trạng thái → thanh Hệ thống tách khỏi bong bóng AI.** Loại để AI tự kể "đã chuyển / đã gửi": lỗi "AI nói đã làm" không sửa được bằng câu chữ.

---

## §6. Tốt là gì (chặng 6)

| # | Tiêu chí Pass/Fail | #3 nồi chiên | #7 áo |
|---|---|---|---|
| T1 | Khi mô tả có nhiều phiên bản hoặc bằng chứng có nhiều nguồn, AI **hiện tất cả** cho người dùng và nói rõ lấy cái nào làm chuẩn (hoặc không lấy cái nào). | **Pass** — hai phiên bản, bản lúc đặt là chuẩn | **Pass** — hai ảnh ngang hàng, không chọn |
| T2 | Không có tiền nào chuyển (hoàn hoặc từ chối) mà **không có một cú bấm của người bán hoặc trưởng nhóm** trên màn có số tiền + căn cứ. | **Pass** | **Pass** |
| T3 | Mỗi tin của AI có **hạn trả hàng lấy từ `tra_cuu_don_hang`** (ngày giờ + còn bao lâu) và **ít nhất một bước tiếp người mua bấm được**. | **Pass** | **Pass** |

**Lỗi còn lại:**

- #7: hạn trả hàng vẫn chạy trong lúc chờ trưởng nhóm (§3 không hứa giữ hạn). Thiết kế chỉ giảm bằng việc hướng dẫn người mua tự gửi trước hạn; nếu người mua không gửi, họ vẫn có thể lỡ hạn.
- #3: người bán có thể ngâm đủ 48 giờ; chưa có luật rút hạn khi có cờ "mô tả bị sửa sau khi bán".
- Chưa có cách cho người bán phản đối **tóm tắt** của AI nếu AI tóm tắt lệch.

---

## §7. Demo

**Phân vai (4 người, mỗi người một phần — nhóm sửa nếu khác):**

| Phần | Người trình bày | Nội dung |
|---|---|---|
| Lát cắt + vì sao #3, #7 đắt nhất | [tên] | 30 giây: lát cắt, hai hướng sai ngược nhau |
| Bấm #3 trên prototype | [tên] | Thẻ hai phiên bản mô tả (số 2), thanh Hệ thống (số 7), trưởng nhóm quyết (số 10) |
| Bấm #7 + câu chữ AI | [tên] | Vì sao chuyển người thay vì xin thêm ảnh; không có nút đóng khiếu nại |
| Mức tự chủ + ca lạ | [tên] | `soan_hoan_tien` cần duyệt, ai duyệt; trả lời ca lạ |

**Câu trả lời chuẩn bị cho "sai thì ai mất gì":** hoàn sai → người bán mất tiền và hàng, nên cần người bán hoặc trưởng nhóm bấm; từ chối sai → người mua mất tiền và có thể lỡ hạn, nên AI không có nút từ chối.

**Ca lạ nhận được ở demo:** *[ghi sau buổi demo]* — prototype đã làm gì: … · nhóm có đổi ý gì: …
