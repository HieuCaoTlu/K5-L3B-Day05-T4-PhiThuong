# Bài nộp LAB5 — T4 Thương mại điện tử · Nhóm [TÊN NHÓM]

**Lớp:** L3B (13/11/2026) · **Bàn / zone:** ____ · **Track:** T4 — Trợ lý khiếu nại "hàng không đúng mô tả" (Trò chuyện với Shopee)

| Họ và Tên | Mã Học Viên | Việc đã làm |
|---|---|---|
| [Thành viên 1] | [MSHV] | §1–§2: lát cắt, 12 kịch bản rủi ro, chọn 2 kịch bản đắt nhất; §6 tiêu chí Pass/Fail; kịch bản demo |
| [Thành viên 2] | [MSHV] | §3: câu chữ AI nguyên văn cho ca A/B/C, phân loại lỗi, chọn nguyên tắc HAX/PAIR; trình bày phần "khi AI sai" |
| [Thành viên 3] | [MSHV] | §4: bảng mức tự chủ, tách tool `soan_hoan_tien`; §5: dựng prototype HTML, ảnh đánh số; trình bày prototype |

> Chính sách trong bài là **chính sách dựng** (khác Shopee thật, Shopee ghi 15 ngày): trả hàng trong **7 ngày** từ ngày nhận · người bán phản hồi trong **48 giờ** · nhân viên sàn xử lý trong **24 giờ** · xem xét lại trong 3 ngày. Đơn, shop, sản phẩm đều dựng.

---

## §1. Lát cắt & phạm vi

**Lát cắt (chép từ track):** Trong *Trò chuyện với Shopee*, khi **người mua** đang báo "hàng nhận không đúng mô tả", **AI tự hướng dẫn gửi yêu cầu trả hàng hoàn tiền, xin thêm bằng chứng, hay chuyển nhân viên**, để người mua biết đúng bước tiếp theo, và người bán không bị xử thua khi họ đúng.

**AI được thêm quyền gì so với hôm nay?** Hôm nay trợ lý chạy theo menu, người mua tự chọn đơn, chọn lý do, tự gửi. Khi tích hợp AI, trợ lý được: đọc đơn và **bản mô tả lưu lúc đặt**, đối chiếu với lời người mua, soạn sẵn yêu cầu, gửi mẫu xin bằng chứng, chuyển hồ sơ cho nhân viên. **Sai thì ai chịu:** hoàn sai → người bán mất tiền và hàng; từ chối hoặc vòng vo sai → người mua mất tiền và giữ hàng sai. Hướng sai đắt hơn với *từng lần* là hoàn sai (tiền đã về người mua, không đảo); hướng sai đắt hơn với *niềm tin dài hạn* là từ chối người mua đúng. Vì vậy thiết kế giữ AI **không ra phán quyết** về tiền.

**3 việc tính năng KHÔNG làm:**
1. **Không tự hoàn tiền và không tự đóng khiếu nại.** Cả hai đều chuyển tiền cho một bên và gần như không đảo được; người quyết phải là người bán (bên chịu thiệt đồng ý) hoặc nhân viên sàn.
2. **Không xử phạt shop, không kết luận hàng giả.** Đây là thẩm quyền của bộ phận Vi phạm, ảnh hưởng cả shop chứ không riêng một đơn; trong luồng khiếu nại, AI chỉ chuyển hồ sơ, không hứa phạt.
3. **Không làm trung gian liên lạc riêng** (không đưa số điện thoại, địa chỉ, tài khoản của bên kia). Lộ dữ liệu cá nhân thì không thu hồi được, và mở đường cho dàn xếp ngoài sàn.

*(Thêm: không xử lý đơn quá hạn trả hàng bằng ngoại lệ — chỉ nói rõ đã quá hạn bao lâu và chuyển nhân viên nếu người mua muốn xin xem xét.)* Prototype tuân thủ: không có nút "phạt shop", nút hoàn/từ chối chỉ có ở phía người bán/nhân viên, không có thông tin liên hệ nào hiện ra (ảnh 1, số 10–11).

**Đọc bảng tool:** chỉ đọc: *không có* — track không cấp tool đọc đơn, nên nhóm **thêm `xem_don_hang`** (đọc). Thay đổi trạng thái: tất cả 5 tool. Không đảo được: `soan_hoan_tien`, `tu_choi_khieu_nai`, `phat_shop` (khó), `yeu_cau_bang_chung` (tin đã tới). Việc người dùng sẽ đòi mà không có tool: "khoá shop ngay", "cho số điện thoại shop", "hoàn luôn hôm nay" → AI nói rõ không làm được và đường đi thay thế.

---

## §2. Kịch bản rủi ro

Ký hiệu lớp: ① nguồn sự thật · ② mơ hồ / thiếu · ③ ngoài phạm vi / thẩm quyền · ④ đặc thù domain.

| # | Tin người dùng nhắn | Lớp | AI có thể sai thế nào (trỏ về tool / dữ liệu) | Sản phẩm tốt nên làm gì | Nhãn |
|---|---|---|---|---|---|
| K1 | "Mình đặt áo size L mà nhận được size M… Shop cũng nhắn nhận là gửi nhầm rồi. Mình muốn trả hàng hoàn tiền thì làm sao?" *(tin mồi)* | ② ① | Tin "shop đã nhận" mà AI **không có tool đọc chat người mua–người bán** → gọi `soan_hoan_tien` ngay dựa trên lời kể một phía. Đúng vì may: lần này shop gửi nhầm thật. | Đối chiếu phân loại lúc đặt (L), nói rõ "mình không đọc được tin nhắn với shop", soạn yêu cầu; shop chỉ cần bấm "Đồng ý" → nhanh mà vẫn có người bên kia xác nhận. | sửa được |
| K2 | "hàng sai rồi, xử lý đi" *(tin mồi)* | ② | Không nói đơn nào, sai chỗ nào → AI đoán **đơn gần nhất** và gọi `yeu_cau_bang_chung` tới **nhầm shop** — tin đã tới, không thu hồi; shop vô can nhận khiếu nại. | Hỏi lại **một câu** kèm 3 đơn gần nhất dạng nút bấm; chưa gọi tool nào làm thay đổi. | sửa được |
| **K3** | "Nồi chiên ghi 6 lít mà mình đo lòng nồi chỉ được 5 lít. Không đúng mô tả. Hôm nay ko hoàn tiền là mình lên TikTok bóc phốt shop luôn đó" *(tin mồi + lời doạ)* | ① ④ | Bản mô tả lúc đặt ghi "**tổng 6 L, lòng nồi 5 L**" → người bán **đúng**. AI chỉ đọc tiêu đề "Nồi chiên 6L" hoặc bị lời doạ thúc → gọi `soan_hoan_tien` "cho êm" → người bán mất 1.290.000đ + hàng, không đảo được. Hoặc ngược lại AI gọi `tu_choi_khieu_nai` với câu "hàng đúng mô tả" → người mua bị phán thua bởi máy. | Hiện đúng dòng "lòng nồi 5 L" trong bản lưu lúc đặt; nói "**chưa đủ căn cứ**", không phán; lời doạ không đổi quy trình; để người mua chọn gửi ảnh đo / vẫn gửi yêu cầu / gặp nhân viên. | **không chấp nhận được** |
| **K4** | "tai nghe ko có chống ồn như quảng cáo, đeo y như tai thường. trả hàng kiểu gì" | ① ④ | Người bán **sửa mô tả lúc 20:14 12/11** (sau khi bán) từ "ANC −30 dB" thành "giảm ồn thụ động". AI đọc **mô tả hiện tại** → kết luận "đúng mô tả", gọi `tu_choi_khieu_nai` hoặc bảo người mua "không đủ điều kiện" → người mua mất 890.000đ, giữ hàng sai; shop gian lận được thưởng. | Đối chiếu **bản lưu lúc đặt**; hiện cờ "mô tả đã bị sửa sau khi bạn đặt" cho cả hai bên; soạn yêu cầu với căn cứ mạnh; `chuyen_truong_nhom` để nhân viên theo dõi việc sửa mô tả. | **không chấp nhận được** |
| K5 | "đồ fake 100%, tem mờ, khoá cái shop này lại cho mình" | ③ | AI gọi `phat_shop` hoặc **hứa** "Shopee sẽ xử lý nghiêm shop" — không tool nào được hứa vậy; phạt sai thì cả shop mất uy tín, đơn hàng khác bị ảnh hưởng. | Không hứa phạt; nói rõ nghi hàng giả do bộ phận chuyên trách xem xét; `chuyen_truong_nhom` gắn nhãn "nghi hàng giả"; luồng trả hàng của đơn này vẫn tiếp tục riêng. | không chấp nhận được |
| K6 | "cho mình số điện thoại shop, mình gọi thẳng cho nhanh" | ③ | Đơn hàng có thông tin liên hệ shop; AI trả ra luôn vì "người dùng hỏi". Lộ dữ liệu cá nhân, không thu hồi được. | Từ chối ngắn, đưa đường thay thế: chat với shop trong app, hoặc gửi yêu cầu để shop phải phản hồi trong 48h. | không chấp nhận được |
| K7 | "son không đúng màu như ảnh, hồng đất mà ra cam" (gửi ảnh dưới đèn vàng; mã màu trên tem khớp mô tả) | ① | Tem lô và mã màu khớp mô tả lúc đặt → người bán đúng. AI thấy ảnh "lệch màu" → `soan_hoan_tien`; hoặc AI so ảnh với ảnh và kết luận "đúng" → `tu_choi_khieu_nai`. | Nói "mã màu trên tem khớp mô tả; ảnh chụp dưới đèn có thể lệch màu"; xin **đúng 1 ảnh** dưới ánh sáng tự nhiên kèm tem qua `yeu_cau_bang_chung`; không đóng khiếu nại. | sửa được |
| K8 | "mình mua tháng trước, giờ mới dùng thấy không đúng mô tả, trả được không" (nhận 20/10) | ④ ① | AI không đọc ngày nhận → nói "bạn vẫn trả được" (bịa), người mua mất thêm thời gian; hoặc từ chối cụt. | Đọc ngày nhận qua `xem_don_hang`: "đã quá hạn 7 ngày từ 27/10"; đưa đường xin xem xét ngoại lệ với nhân viên, không hứa. | sửa được |
| K9 | "Tôi gửi ảnh rồi mà cứ bị hỏi lại!!" | ② | `yeu_cau_bang_chung` gửi lại **mẫu chung** dù đã có ảnh tem → người mua mất kiên nhẫn ("không ai nói tôi còn thiếu cái gì"). | Danh sách bằng chứng có tick: ✔ đã có, ☐ còn thiếu đúng 1 thứ và vì sao cần. Mỗi loại bằng chứng chỉ xin 1 lần. | nhẹ |
| K10 | "áo sai màu, với lại đơn nồi chiên hôm qua sao chưa giao" | ② ③ | AI gộp hai việc, gửi yêu cầu trả hàng cho cả đơn chưa giao, hoặc bỏ sót một việc. | Tách: xử lý khiếu nại áo trước; đơn chưa giao là luồng khác — đưa link theo dõi đơn. | nhẹ |
| K11 | (người bán, phía Seller Centre) "khách này doạ bóc phốt, hoàn luôn cho xong đi" | ④ | AI coi lời người bán là đồng ý → tự thực hiện `soan_hoan_tien` mà người bán chưa thấy số tiền, căn cứ. | Người bán phải bấm "Đồng ý trả hàng hoàn tiền" trên màn có số tiền + căn cứ; AI không bấm thay. | sửa được |
| K12 | (correction) "không phải đơn đó, đơn nồi chiên cơ" sau khi AI đã đối chiếu nhầm đơn | ② | AI đã soạn yêu cầu cho đơn sai; nếu đã `yeu_cau_bang_chung` thì tin đã tới shop sai. | Vì chỉ *soạn* (chưa gửi) nên bỏ bản nháp, hiện 3 đơn gần nhất để chọn lại; không có gì đã đi ra ngoài. | nhẹ |

**Quyết định: 2 kịch bản đắt nhất = K3 (người bán đúng + lời doạ) và K4 (mô tả đã bị sửa).**
- **Vì sao đắt hơn:** cả hai là chỗ AI **trông như đúng** (K3: người mua đo thật, có số; K4: mô tả hiện tại đúng là "giảm ồn thụ động") nhưng gọi tool tiền sai hướng thì **không đảo được** (`soan_hoan_tien` / `tu_choi_khieu_nai`). Hai ca là **hai hướng sai ngược nhau**: K3 người bán bị xử thua oan, K4 người mua bị xử thua oan — đúng cái "sự trung lập" track yêu cầu thiết kế. K4 còn tệ ở chỗ thưởng cho hành vi gian lận và sửa câu chữ không cứu được (xem §3).
- **Suýt được chọn:** **K5 (khoá shop, nghi hàng giả)** — hậu quả lớn nhất về phạm vi (cả shop). Không chọn vì cách chặn rất rõ và rẻ: đặt `phat_shop` = cấm, không có nút → không còn quyết định UX khó. **K6 (số điện thoại)** — không đảo được, nhưng chỉ cần một luật cứng "không xuất thông tin liên hệ", không cần thiết kế lúc AI không chắc. K3, K4 thì AI *phải* ra một nhận định giữa hai bên, nên là chỗ khó nhất.
- **Kịch bản làm nhóm sợ nhất khi demo:** K3 — vì "hoàn cho xong" là phản xạ của cả người lẫn máy khi bị doạ.

---

## §3. Khi AI sai

### Kịch bản K3 — Nồi chiên: người bán đúng, người mua doạ bóc phốt

**AI nói (nguyên văn):**
> Mình là **trợ lý AI** của Shopee. Mình đối chiếu khiếu nại và hướng dẫn bước tiếp; mình **không tự hoàn tiền hay từ chối** — việc đó do người bán đồng ý hoặc nhân viên Shopee quyết.
> Mình đã mở đơn #2611-NC0417 (Nồi chiên không dầu BếpViệt AF-60). Bản mô tả lưu lúc bạn đặt ghi **dung tích tổng 6 L, lòng nồi 5 L** — số đo 5 L của bạn đang **khớp** với dòng lòng nồi.
> *[khối bản lưu mô tả]* · **Căn cứ "không đúng mô tả": CHƯA ĐỦ**
> Hạn gửi yêu cầu trả hàng: 23:59 18/11/2026 (còn 5 ngày).
> Về việc đăng lên mạng: đó là quyền của bạn. Yêu cầu của bạn vẫn được xử lý theo **cùng một quy trình và thời hạn** như mọi yêu cầu khác.
> **Bạn muốn làm gì tiếp?** [Lòng nồi đo được ít hơn 5 L — gửi ảnh đo] [Mình vẫn muốn gửi yêu cầu trả hàng] [Nói chuyện với nhân viên]

- **Người dùng thấy:** khối "Mô tả lúc bạn đặt — bản lưu 06/11 09:12" với dòng *lòng nồi 5 L* được tô vàng; dòng xanh "người bán không sửa mô tả sau khi bạn đặt"; nhãn vàng **CHƯA ĐỦ** + 3 chip ✔/✖; hộp hạn trả hàng; 3 nút lựa chọn; link "Mình đối chiếu sai?"; nút "Gặp nhân viên" luôn ở đáy.
- **Làm gì tiếp:** (a) gửi ảnh đo nếu < 5 L → AI xin đúng 1 ảnh; (b) vẫn gửi yêu cầu → AI soạn **bản nháp**, người mua bấm Gửi; (c) gặp nhân viên. **Nếu không làm gì:** không có gì được gửi, không ai bị trừ tiền; hạn 18/11 vẫn chạy và được nhắc rõ.
- **Ai được báo, sau bao lâu:** chưa ai khi người mua chỉ đang hỏi (cuộc trò chuyện với trợ lý *không* tự gửi gì cho người bán). Khi người mua bấm Gửi → **người bán** được báo ngay, có 48h, thấy *cùng* bản mô tả lúc đặt + tóm tắt trung lập (không thấy lời doạ, không thấy SĐT). Người bán không đồng ý hoặc quá 48h → **nhân viên sàn** nhận hồ sơ, xử lý trong 24h; hồ sơ ghi "tin có lời doạ — đã gắn nhãn, không dùng làm căn cứ, không đổi ưu tiên".
- **Quyết định — trả lời kèm giới hạn** (nói rõ "chưa đủ căn cứ" + đưa đường đi), **không** hỏi lại, **không** dừng chuyển người ngay.
  - *Loại "hỏi lại một câu":* thông tin đã đủ (đơn rõ, số đo rõ, bản mô tả có sẵn). Hỏi thêm chỉ làm người mua thấy bị vòng vo — đúng nỗi đau "cứ bị hỏi lại".
  - *Loại "dừng và chuyển người ngay":* lời doạ sẽ trở thành "vé ưu tiên" — ai doạ thì gặp người nhanh hơn, không công bằng với người bán và người mua khác. Nút gặp nhân viên vẫn có, nhưng là lựa chọn của người mua, không phải phản xạ của AI.
  - *Loại "trả lời 'hàng đúng mô tả' và đóng":* đó là phán quyết, AI không có quyền (`tu_choi_khieu_nai` = cấm), và số đo của người mua có thể vẫn < 5 L. Câu "chưa đủ căn cứ" mở đường cho bằng chứng mới; câu "đúng mô tả" thì đóng lại.
- **Loại lỗi:** không phải lỗi hệ thống. Với người mua đây là **lỗi ngữ cảnh**: hệ thống và mô tả đều đúng, nhưng tiêu đề "Nồi chiên 6L" khiến người dùng hiểu 6 L là lòng nồi. Vì vậy AI **không nói "bạn đọc nhầm"** (đổ lỗi người dùng) mà *đặt hai dòng cạnh nhau* để người dùng tự thấy, và đưa đường đi tiếp. Cách sửa gốc là thiết kế: hồ sơ cho nhân viên ghi "tiêu đề có thể gây hiểu nhầm" để sàn phản hồi người bán về cách đặt tiêu đề — không phạt.

### Kịch bản K4 — Tai nghe: mô tả đã bị sửa sau khi bán

**AI nói (nguyên văn):**
> Mình là **trợ lý AI** của Shopee — mình hướng dẫn và soạn yêu cầu; hoàn tiền do người bán đồng ý hoặc nhân viên duyệt.
> Đơn #2611-TN0233 (Tai nghe Sona Q3). Lúc bạn đặt, mô tả ghi **chống ồn chủ động (ANC) đến −30 dB**. Sau đó người bán đã sửa mô tả — mình đối chiếu theo **bản lúc bạn đặt**, không theo bản hiện tại.
> *[khối bản lưu + cờ đỏ "Mô tả đã bị người bán sửa lúc 20:14 12/11 — sau khi bạn đặt"]* · **Căn cứ "không đúng mô tả": MẠNH**
> Hạn gửi yêu cầu trả hàng: 23:59 15/11/2026 (còn 2 ngày).
> **Mình soạn sẵn yêu cầu trả hàng cho bạn nhé?** [Soạn giúp mình] [Nói chuyện với nhân viên]

- **Người dùng thấy:** cờ đỏ "đã bị sửa", gạch ngang giá trị cũ → mới; nhãn xanh **MẠNH**; chip "☐ Video khi bật ANC — giúp xử lý nhanh hơn, không bắt buộc"; sau khi bấm → bản nháp viền cam "chưa gửi" có Gửi / Thêm video / Huỷ; sau khi gửi → thanh **Hệ thống** "Chưa có tiền nào được chuyển".
- **Làm gì tiếp:** đọc bản nháp rồi Gửi; có thể thêm video. **Nếu không làm gì:** hạn còn 2 ngày được nhắc; không gì được gửi.
- **Ai được báo:** ngay khi phát hiện sửa mô tả sau bán, AI **tự** `chuyen_truong_nhom` (đảo được) để trưởng nhóm theo dõi — *không* kết luận, không phạt. Khi người mua gửi → người bán có 48h, thấy hai cột "mô tả lúc đặt / mô tả hiện tại (bạn sửa 20:14 12/11)". Người bán không đồng ý → nhân viên duyệt hoàn tiền trên màn hình xác nhận có số tiền, căn cứ, "không hoàn tác được".
- **Quyết định — trả lời kèm căn cứ + soạn sẵn**, không hỏi lại, không tự hoàn.
  - *Loại "tự hoàn tiền vì căn cứ mạnh":* căn cứ mạnh về *mô tả*, nhưng AI chưa thấy *hàng* (có thể người mua chưa bật ANC đúng cách). Hoàn tiền không đảo được; một lượt người bán phản hồi 48h là cái giá chấp nhận được.
  - *Loại "xin bằng chứng trước khi soạn":* hạn chỉ còn 2 ngày; bắt video trước có thể làm người mua lỡ hạn. Video là "giúp nhanh hơn", không chặn việc gửi.
  - *Loại "chuyển người ngay":* người mua không cần người để biết bước tiếp; nhân viên đã được báo song song qua `chuyen_truong_nhom`.
- **Loại lỗi:** nếu AI đọc mô tả hiện tại và kết luận "đúng mô tả" thì đó là **lỗi hệ thống** (thiết kế dữ liệu sai), không phải lỗi người dùng. Cách nói khi lỗi kiểu này bị phát hiện: xin lỗi thẳng, nói "mình đã đối chiếu nhầm bản mô tả", và sửa bằng dữ liệu, không bằng câu chữ.

### Lỗi mà sửa câu chữ không cứu được
1. **"So với mô tả hiện tại" (K4).** Không câu chữ nào cứu được nếu AI chỉ đọc được bản hiện tại: nó sẽ nói tự tin rằng hàng đúng mô tả. **Thiết kế cứu:** tool `xem_don_hang` (nhóm thêm) trả về **bản chụp mô tả lúc đặt + lịch sử sửa**; AI chỉ được đối chiếu với bản chụp; mọi lần sửa sau thời điểm đặt hiện thành cờ cho *cả* người mua, người bán và nhân viên.
2. **"AI nói đã hoàn tiền / đã gửi" khi chưa làm.** Câu chữ của AI không phải nguồn sự thật về trạng thái tiền. **Thiết kế cứu:** mọi trạng thái (đã gửi, hạn phản hồi, "chưa có tiền nào được chuyển", ai đã quyết) do **thanh Hệ thống** in ra từ sự kiện thật, khác hẳn kiểu bong bóng AI (ảnh 1, số 7). AI không có tool tự hoàn nên không thể có câu "đã hoàn" mà đúng.

### Nguyên tắc HAX/PAIR (trỏ vào chỗ cụ thể)
- **G1 + Luật AI 134/2025 Đ.11** → nhãn "Trợ lý AI" ở đầu khung chat và câu mở đầu "mình **không tự hoàn tiền hay từ chối**" (ảnh 1, số 1). Nói rõ giới hạn quyền ngay câu đầu để người mua không chờ một phán quyết AI không có.
- **G11 + PAIR Explainability & Trust** → khối "Mô tả lúc bạn đặt — bản lưu [giờ]" với dòng được tô (ảnh 1/2, số 2): người dùng *tự kiểm* căn cứ, tin đúng mức chứ không tin tối đa.
- **G2** → nhãn căn cứ CHƯA ĐỦ / MẠNH + chip ✔/✖/☐ (số 3): nói AI chắc tới đâu và còn thiếu gì — giải đúng nỗi đau "không ai nói tôi còn thiếu cái gì".
- **G10** → K2/K12: "Bạn đang nói về đơn nào?" + 3 nút đơn (bước "Sai đơn"); K3: một câu "Bạn muốn làm gì tiếp?" + 3 lựa chọn (số 4).
- **G8/G9** → bản nháp "chưa gửi" với Gửi / Sửa / Huỷ (số 6); "Thôi, tiếp tục với trợ lý" hoàn tác việc chuyển nhân viên (số 12).
- **G15** → "Mình đối chiếu sai? Sai đơn / sai chỗ — sửa lại" dưới mỗi lần đối chiếu (số 8).
- **PAIR Errors + Graceful failure** → nút "Gặp nhân viên" cố định ở đáy khung chat ở mọi bước (số 12).

---

## §4. Mức tự chủ

**Thay đổi bộ tool (nhóm thêm / tách):**
- **Thêm `xem_don_hang`** (chỉ đọc: đơn, ngày nhận, hạn trả, *bản chụp mô tả lúc đặt*, lịch sử sửa mô tả). Không có tool này thì rubric "so với mô tả lúc đặt" không thể làm — AI sẽ đoán.
- **Thêm `soan_yeu_cau_tra_hang`** (tạo bản nháp yêu cầu; *người mua* bấm Gửi). Tách "soạn" khỏi "gửi" → chặn K2/K12 (soạn nhầm đơn thì bỏ nháp, chưa có gì đi ra ngoài).
- **Tách `soan_hoan_tien`** — tên gọi là "soạn" nhưng theo bảng track nó *thực hiện* hoàn tiền. Nhóm tách thành **`de_xuat_hoan_tien`** (AI soạn đề xuất: số tiền, căn cứ — tự làm) và **`thuc_hien_hoan_tien`** (chuyển tiền — cần người duyệt). Chặn K3, K11: AI có thể *đề xuất* nhưng tiền chỉ đi khi có người bấm.

| Tool | Mức | Ai duyệt · thấy gì · quá hạn thì sao | Lý do theo cost-of-error | Phương án đã loại |
|---|---|---|---|---|
| `xem_don_hang` *(thêm)* | **tự làm** | — | Chỉ đọc, sai thì không ai mất gì ngoài một câu trả lời sai mà người dùng thấy được (vì bản lưu hiện ra). Chờ người duyệt việc đọc là chờ vô ích. | — |
| `soan_yeu_cau_tra_hang` *(thêm)* | **tự soạn → người mua bấm Gửi** | Người mua thấy: đơn, số tiền, lý do, đính kèm, lưu ý "người bán có thể không đồng ý". Không bấm → không gì xảy ra, hạn vẫn được nhắc. | Soạn sai = rẻ (bỏ nháp). Gửi sai = shop nhận khiếu nại oan, tin đã tới. Một cú bấm của chính người mua chặn được. | *AI tự gửi*: nhanh hơn ~1 bấm nhưng sai đơn (K2) là không thu hồi được. |
| `yeu_cau_bang_chung` | **tự làm**, kèm luật | — | Sai thì tốn công một lượt gửi ảnh (nhẹ), không mất tiền; chờ người duyệt thì người mua lỡ hạn trả hàng (K4 còn 2 ngày) — chờ đắt hơn sai. **Luật:** chỉ dùng mẫu cố định (không câu chữ tự do có thể buộc tội), mỗi loại bằng chứng xin 1 lần, phải ghi rõ đã có gì / thiếu gì (K9), chỉ gửi cho đơn người mua đã xác nhận (K2). | *Cần người duyệt*: tin đã tới nên trông như cần duyệt, nhưng nội dung là mẫu cố định; bước duyệt chỉ thêm độ trễ. |
| `chuyen_truong_nhom` | **tự làm** + hoàn tác | — | Đảo được; sai thì nhân viên tốn vài phút đọc hồ sơ thừa. Không chuyển khi cần (K4 sửa mô tả, K5 nghi hàng giả) thì một bên mất tiền. **Luật:** lời doạ *không* là lý do chuyển/ưu tiên. | *Cần duyệt*: thêm một tầng chờ cho đúng việc "gọi người". |
| `de_xuat_hoan_tien` *(tách)* | **tự làm** | — | Chỉ là bản đề xuất trong hồ sơ, không có tiền đi. | — |
| `thuc_hien_hoan_tien` *(tách)* | **cần người duyệt** | **Người bán** bấm "Đồng ý trả hàng hoàn tiền" (bên chịu thiệt tự đồng ý) **hoặc nhân viên** bấm "Duyệt hoàn tiền". Người duyệt thấy: số tiền, người nhận, mô tả lúc đặt vs lời người mua, phản hồi bên kia, dòng đỏ "không hoàn tác được" (ảnh 3). **Quá 48h người bán không trả lời → tự chuyển nhân viên, *không* tự hoàn.** Nhân viên quá 24h → nhắc trưởng nhóm. | Hoàn sai: người bán mất tiền + hàng, gần như không đảo. Một bước xác nhận có căn cứ chặn được K3, K11. | *Tự làm khi căn cứ mạnh* (K1, K4) — xem "tool khó nhất". *Cấm*: người bán tự đồng ý hoàn mà cũng phải chờ nhân viên thì K1 chờ vô ích. |
| `tu_choi_khieu_nai` | **cấm với AI** (chỉ nhân viên) | Nhân viên thấy tóm tắt 2 phía + đề xuất AI ghi rõ "đề xuất, không phải quyết định". | Từ chối sai: người mua mất tiền, giữ hàng sai, niềm tin vào sàn mất ngay. AI có thể đúng về số liệu (K3) nhưng không có quyền phán; một nút xác nhận của *người mua* cũng không chặn được (người mua không bao giờ tự từ chối mình). Nên AI không có nút này; chỉ nói "chưa đủ căn cứ". | *Cần duyệt*: về thực chất giống "nhân viên làm, AI đề xuất" — nhóm gọi là **cấm với AI** để rõ trên giao diện: phía người mua không có nút nào dẫn tới từ chối. |
| `phat_shop` | **cấm** | Không có nút; chỉ "Chuyển bộ phận Vi phạm" (ảnh 1, số 11). | Ảnh hưởng cả shop và các đơn khác, ngoài thẩm quyền luồng khiếu nại; xác nhận một lần không đủ căn cứ để phạt. | *Cần duyệt nhân viên khiếu nại*: vẫn sai người — phạt thuộc bộ phận chuyên trách. |

### Tool khó xếp nhất: `soan_hoan_tien` (→ `thuc_hien_hoan_tien`)
- **Mỗi người từng chọn:** TV1 — *tự làm khi căn cứ mạnh* (K1 shop đã nhận nhầm, K4 mô tả bị sửa: chờ 48h là bắt người mua đúng chịu thiệt). TV2 — *cấm với AI, chỉ nhân viên* (K3: căn cứ trông mạnh vẫn có thể sai; hoàn không đảo được). TV3 — *cần người duyệt*, nhưng chưa rõ ai.
- **Cái giá mỗi phía:** tự làm → K3 nếu AI đọc nhầm tiêu đề "6L" là căn cứ mạnh: người bán mất 1.290.000đ oan; K11 người bán "nói" hoàn cho xong là AI hoàn luôn. Chỉ nhân viên → K1: shop đã nhận gửi nhầm mà người mua vẫn phải chờ nhân viên 24h+, nhân viên ngập ca dễ.
- **Nhóm chốt:** **cần người duyệt, và người duyệt là bên chịu thiệt** — người bán bấm Đồng ý (nhanh cho K1, thường vài giờ), hoặc nhân viên khi hai bên không thống nhất. AI chỉ *đề xuất*. Quá hạn người bán → nhân viên, không phải tự hoàn: chọn chịu chậm hơn chứ không chịu tiền đi sai.

### Thử mức đã đặt
| Ca | Cái sai ở §2 có bị chặn? | Còn việc nào AI làm khi lẽ ra phải hỏi? | Người dùng bình thường có chờ vô ích? |
|---|---|---|---|
| **K3 nồi chiên** | Có. AI không có nút hoàn/từ chối; lời doạ không đổi luồng; người bán thấy cùng bản lưu và phản hồi; nhân viên quyết. | `yeu_cau_bang_chung` tự làm — nhưng chỉ khi người mua chọn "lòng nồi < 5 L"; không tự xin. | Người mua có thể chờ 48h + 24h nếu cố khiếu nại — đây là chờ *có ích* vì căn cứ đang nghiêng về người bán. |
| **K4 tai nghe** | Có. AI đọc bản lưu lúc đặt, cờ "đã sửa" hiện cho mọi bên; `tu_choi_khieu_nai` không có ở phía AI. | `chuyen_truong_nhom` tự làm khi thấy sửa mô tả sau bán — đảo được, chấp nhận. | Có rủi ro: người bán có thể ngâm 48h dù sai. Chấp nhận vì nhân viên đã được báo song song và có thể duyệt sớm (không phải đợi hết 48h nếu có cờ sửa mô tả). |
| **K1 áo (bình thường)** | Không có cái sai đắt; AI không tin lời "shop đã nhận" mà để shop bấm. | Không. | Ít: shop đã nhận nhầm thì bấm Đồng ý ngay; người mua chỉ thêm 1 bước Gửi. |

### Một mức tự chủ đã đổi lời hứa với người dùng
Đặt `thuc_hien_hoan_tien` = cần người duyệt đổi lời hứa từ *"Trợ lý AI giải quyết khiếu nại cho bạn"* thành **"Trợ lý chỉ bạn đúng bước tiếp, soạn sẵn yêu cầu và nói rõ còn thiếu gì; người bán hoặc nhân viên Shopee quyết chuyện tiền"**. Vì vậy câu đầu tiên của AI ở mọi ca là "mình không tự hoàn tiền hay từ chối" — nếu không, người mua sẽ chờ một phán quyết mà AI không được phép đưa. Với người bán, lời hứa mới là **"không ai hoàn tiền đơn của bạn khi bạn chưa được nghe"**.

---

## §5. Prototype

- **File:** [`prototype/index.html`](prototype/index.html) — mở bằng trình duyệt, không cần cài đặt. Công cụ: HTML/CSS/JS thuần, dựng cùng coding agent (Claude). Không có backend, không có lời gọi AI thật — mọi câu AI là kịch bản soạn theo §3.
- **Bấm qua ca khó (demo bắt buộc):** chọn **Ca A** (người bán đúng) → "Mình vẫn muốn gửi yêu cầu trả hàng" → "Gửi yêu cầu" → tab *Người bán*: "Không đồng ý + gửi bằng chứng" → tab *Nhân viên*: "Từ chối khiếu nại" → hộp xác nhận → người mua thấy kết quả do **nhân viên (người thật)** quyết. **Ca B** (mô tả đã bị sửa) có cùng luồng, kết thúc bằng nhân viên duyệt hoàn tiền. **Ca C** là ca bình thường: người bán bấm Đồng ý, không cần nhân viên.
- **Màn hình 1** = khung chat người mua. **Màn hình 2** = phía sau, hai tab: người bán (Seller Centre, hạn phản hồi) và nhân viên (hồ sơ hai phía).

**Ảnh đánh số:** `prototype/anh-1-ca-A-nguoi-ban-dung.png` (ca A tới lúc nhân viên nhận hồ sơ), `prototype/anh-2-ca-B-mo-ta-bi-sua.png` (ca B, phía người bán), `prototype/anh-3-nhan-vien-xac-nhan.png` (hộp xác nhận của nhân viên).

| Số | Thể hiện gì | Nguyên tắc | Mức tự chủ |
|---|---|---|---|
| 1 | Nhãn "Trợ lý AI" + câu "mình không tự hoàn tiền hay từ chối" | G1, Luật AI Đ.11 | (lời hứa §4) |
| 2 | Khối "Mô tả lúc bạn đặt — bản lưu [giờ]", dòng tô vàng, cờ "đã/không bị sửa"; phía nhân viên: "Đối chiếu của trợ lý (đề xuất, không phải quyết định)" | G11, PAIR Explainability | `xem_don_hang` tự làm |
| 3 | Nhãn căn cứ CHƯA ĐỦ / MẠNH + chip ✔ ✖ ☐ | G2 | — |
| 4 | Một câu hỏi + 2–3 lựa chọn bấm | G10 | — |
| 5 | Hạn gửi yêu cầu, còn bao lâu, tính từ ngày nhận | (rubric riêng #8) | — |
| 6 | Bản nháp viền cam "chưa gửi" với Gửi / Sửa / Huỷ | G8, G9 | `soan_yeu_cau_tra_hang`: tự soạn → người mua gửi |
| 7 | Thanh **Hệ thống** (kiểu khác bong bóng AI): "Chưa có tiền nào được chuyển", hạn phản hồi, ai đã quyết | PAIR Errors (sửa lỗi "AI nói đã làm") | — |
| 8 | "Mình đối chiếu sai? Sai đơn / sai chỗ — sửa lại" | G15, G9 | — |
| 9 | Phía người bán: hạn 48h đếm ngược, hai cột mô tả lúc đặt / hiện tại, "quá hạn → chuyển nhân viên, không tự hoàn" | PAIR Trust (hai bên cùng được nghe) | — |
| 10 | Nút hoàn / từ chối **chỉ** ở người bán và nhân viên, có hộp xác nhận ghi số tiền, người bấm, căn cứ, "không hoàn tác được" | — | `thuc_hien_hoan_tien` cần duyệt · `tu_choi_khieu_nai` cấm với AI |
| 11 | Không có nút phạt shop — chỉ link "Chuyển bộ phận Vi phạm" | G1 (giới hạn) | `phat_shop` cấm |
| 12 | Nút "Gặp nhân viên" cố định; sau khi chuyển có "Thôi, tiếp tục với trợ lý (hoàn tác)" | PAIR Graceful failure, G8 | `chuyen_truong_nhom` tự làm + hoàn tác |

**Quyết định cách hiện:**
- **Độ chắc → nhãn căn cứ bằng chữ + danh sách ✔/✖**, không dùng phần trăm (ví dụ "72%"). Loại %: người mua đọc "72%" như xác suất được hoàn tiền; con số cũng không chỉ ra *thiếu gì*. Danh sách ✔/✖ trả lời đúng nỗi đau "không ai nói tôi còn thiếu cái gì".
- **Bằng chứng → bản lưu mô tả hiện ngay trong tin nhắn**, không để trong link "xem chi tiết". Loại link: K3/K4 xoay quanh đúng một dòng mô tả; giấu nó đi là bắt người dùng tin AI.
- **Mức tự chủ → hai màn hình riêng cho hai bên**, nút tiền chỉ nằm ở màn của người có quyền. Loại cách "một màn hình chat có nút Hoàn tiền mờ đi": nút mờ vẫn hứa hẹn với người mua rằng AI *có thể* hoàn.
- **Trạng thái → thanh Hệ thống tách khỏi bong bóng AI.** Loại cách để AI tự nói "đã gửi": xem §3, lỗi không sửa được bằng câu chữ.
- Khác với prototype mẫu (đổi mức tự chủ bằng toggle), prototype của nhóm cho thấy **hai phía của cùng một quyết định** — người mua thấy gì và người bán thấy gì cùng lúc.

---

## §6. Tốt là gì

| # | Tiêu chí Pass/Fail | K3 nồi chiên | K4 tai nghe |
|---|---|---|---|
| T1 | Mọi nhận định "đúng/không đúng mô tả" của AI trích **bản mô tả lúc đặt** (có giờ lưu) và hiện nó cho người dùng; không bao giờ dựa trên mô tả hiện tại. | Pass | Pass (có cờ đã sửa) |
| T2 | Không có tiền nào chuyển (hoàn hoặc trả về người bán) mà **không có một cú bấm của người bán hoặc nhân viên** trên màn có số tiền + căn cứ. | Pass | Pass |
| T3 | Mỗi tin của AI kết thúc bằng **đúng một bước tiếp theo bấm được** + hạn trả hàng còn bao lâu; khi xin bằng chứng thì ghi rõ đã có gì / thiếu gì. | Pass | Pass |

**Lỗi còn lại:**
- K3: nếu người mua đo sai cách và thật sự lòng nồi < 5 L, họ phải tự biết bấm "gửi ảnh đo" — AI không chủ động hướng dẫn cách đo. Chưa sửa.
- K4: người bán có thể ngâm đủ 48h; nhóm chỉ giảm bằng việc báo nhân viên song song, chưa có luật "có cờ sửa mô tả thì rút hạn xuống 24h".
- Chưa có màn hình cho người bán đọc lại tóm tắt AI và phản đối *tóm tắt* (nếu AI tóm tắt lệch).

---

## §7. Demo

- **Kịch bản demo (≈4 phút):** TV1 nói lát cắt + vì sao K3/K4 đắt nhất (30") → TV3 bấm Ca A từ đầu tới lúc nhân viên quyết, chỉ số 2, 3, 7, 10 → TV2 giải thích câu AI nói với lời doạ và vì sao không "hoàn cho xong" → TV3 chuyển Ca B, chỉ cờ "mô tả đã bị sửa" → cả nhóm nhận ca lạ.
- **Câu trả lời chuẩn bị cho "sai thì ai mất gì":** hoàn sai → người bán mất tiền + hàng (vì vậy cần người bán/nhân viên bấm); từ chối sai → người mua mất tiền (vì vậy AI không có nút từ chối).
- **Ca lạ nhận được ở demo:** *[ghi sau buổi demo]* — prototype đã làm gì: … · nhóm có đổi ý gì: …
