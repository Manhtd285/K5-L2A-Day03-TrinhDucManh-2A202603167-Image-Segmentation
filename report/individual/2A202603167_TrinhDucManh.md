# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603167 (TrinhDucManh)
- Ngày / CVAT local: 11/11/2026 / CVAT local
- Công cụ đã dùng: CVAT local (Polygon, Brush) + Pre-labeling gợi ý từ SegFormer/Mask2Former + SAM (`sam2.1_b`) rồi import vào CVAT để rà soát và chỉnh sửa thủ công

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3 | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | 3 |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, người phụ nữ mặc áo dài đi bộ qua vạch kẻ đường ở chính giữa ảnh (`person`).
- Class và quy tắc tôi dùng để chọn biên: Class `person`. Vẽ Polygon bám sát đường viền tà áo dài trắng, cánh tay, đầu và bàn chân nhìn thấy; dừng biên ở sát mặt đường vạch sang đường (không lấy phần bóng đổ dưới chân) và đi vòng theo mép che khuất phía trước chiếc xe máy chạy ngang phía sau.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Gợi ý tự động (Mask2Former + SAM) bắt khá chuẩn biên của người đi bộ chính giữa, nhưng ở chiếc xe máy phía sau người phụ nữ này lại bị tách làm 2 mask rời nhau (`#7 motorcycle` phần đuôi xe bên trái và `#15 motorcycle` phần bánh trước bên phải), đồng thời nhận nhầm tấm áp phích người mẫu trên cửa kính tiệm CHANEL phía sau (`#17 person`) là người thật. Tôi đã xóa mask `#17 person` trên biển quảng cáo và gộp 2 nửa xe máy bị che khuất về cùng 1 instance `motorcycle`.
- Nếu không dùng gợi ý: Đã dùng gợi ý sau khi tự vẽ object đầu tiên và kiểm tra/chỉnh sửa lại trên CVAT.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `cp6_coverage`, ảnh `7daa6479-67988f3f.jpg`, vùng vỉa hè lát gạch xám bên phải chiếc xe buýt trắng MTA (`2555`) dưới hàng cột tòa nhà.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: Sai lớp (`road` vs `sidewalk`) và phủ vùng (`coverage`).
- Bằng chứng tôi nhìn thấy: Mặt gạch vỉa hè bên phải bó vỉa có màu xám sẫm rất giống màu nhựa đường nên gợi ý ban đầu gán nhầm phần lớn vỉa hè phía dưới bên phải thành `road`, đồng thời phần xe buýt `2555` và xe tải `CAPITAL` ở giữa đường không thuộc 7 class của `cp6_coverage` (`road`, `sidewalk`, `building`, `vegetation`, `sky`, `car`, `person`) từng bị gán nhầm một phần sang `car`.
- Quy tắc và hành động sửa: Áp dụng quy tắc phân biệt `road`–`sidewalk` theo bó vỉa và chức năng (toàn bộ phần bên phải đường gờ bó vỉa dọc theo thân xe buýt đi vào dưới hàng cột tòa nhà là `sidewalk`), và để trống (unlabeled/background) vùng xe buýt + xe tải vì không nằm trong `classes.json` của trạm `cp6_coverage`.
- Sau sửa đã Save và export lại chưa? Đã chỉnh sửa, bấm Save trên CVAT và export lại `cp6_coverage.zip` đúng chuẩn `Segmentation mask 1.1`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Đã chạy `python scripts/inspect_submissions.py --dir submissions` đạt `[OK]` cả 9/9 task / chưa có điểm reference. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `medium_instance` / `000000181542.jpg` — Chiếc xe bán tải màu bạc ở giữa nền sau (`#19`) và poster người trên kính tiệm CHANEL (`#17`) | (a) Xe bán tải là `car` hay `truck`; (b) Hình người trên biển quảng cáo CHANEL có tính là `person` không? | Trong COCO, xe pickup/SUV cỡ lớn thường gán `truck` hoặc `car`; còn hình in 2D trên biển quảng cáo không phải người thật trong cảnh giao thông. | Xóa mask `#17 person` trên biển quảng cáo CHANEL; giữ mask xe bán tải . |
| 2. `cp5_occlusion` / `000000336232.jpg` — Hai chiếc taxi màu vàng ở góc dưới bên phải | (a) Gộp chung vùng màu vàng thành 1 mask `car` lớn hay (b) Tách thành 2 instance `car` riêng biệt (chiếc taxi phía trước che một phần chiếc taxi phía sau)? | Có 2 đường viền kính chắn gió và nắp capo tách biệt của 2 xe taxi vàng chạy song song/nối đuôi nhau (cùng class sát nhau/che nhau). | Tách rõ thành 2 instance `car` riêng biệt theo đúng quy tắc "Hai vật cùng lớp sát nhau vẫn là hai instance", không để SAM gộp chung 2 xe vàng làm một. |
| 3. `cp4_curb` / `7d83710e-4697c3b2.jpg` — Dải đất/cỏ quanh gốc cây nằm giữa bó vỉa lòng đường (`road`) và lối đi bê tông (`sidewalk`) + mũi xe ego ở đáy ảnh | (a) Tô dải đất quanh gốc cây vào `sidewalk` / tô mũi xe vào `road`, hay (b) Để trống (background) cả dải đất (`terrain`) và nắp capo xe ego? | `classes.json` của `cp4_curb` chỉ có 2 lớp `road` và `sidewalk`. Theo chuẩn BDD100K/Cityscapes, dải đất/cỏ là `terrain` và nắp capo là ego-vehicle, không thuộc `road` hay `sidewalk`. | Chỉ tô lòng đường nhựa đến mép bó vỉa là `road`, phần bê tông vỉa hè bên phải là `sidewalk`, và để trống dải đất quanh gốc cây cùng nắp capo xe ở đáy ảnh. |
