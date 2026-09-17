# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602228
- Ngày / CVAT local:17/9/2026
- Công cụ đã dùng: CVAT

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | submissions/easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | submissions/medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic |submissions/hard_panoptic.zip |2 / 2 | 30 |
| cp1_holes | submissions/cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | submissions/cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | submissions/cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | submissions/cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | submissions/cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | submissions/cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `000000181542.jpg` — người phụ nữ mặc áo dài trắng đứng giữa làn đường, khoảng trung tâm ảnh ngay trước vạch sang đường. Đây là object đầu tiên tôi tự vẽ bằng Polygon trước khi bật bất kỳ gợi ý nào.
- Class và quy tắc tôi dùng để chọn biên: class `person`. Quy tắc biên: chỉ vẽ phần thân người **nhìn thấy rõ** từ đỉnh đầu đến bàn chân; dừng mask theo đường viền cuối cùng còn nhìn thấy của vạt áo. Phần chân trái gần xe máy bên trái bị che một phần — tôi dừng mask ở mép cuối nhìn thấy rõ, không đoán phần khuất.
- Nếu không dùng gợi ý: **không dùng**. Quyết định gán nhãn: tay trái của người phụ nữ bị cánh tay người lái xe máy khuất một phần nhỏ. Tôi vẫn xếp đây là **một instance duy nhất** vì quy tắc "vật bị che vẫn là một instance", và chỉ vẽ đến điểm cuối của phần nhìn thấy, không cố nối phần bị che.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: `medium_instance` / `000000181542.jpg` / hai người trên xe máy bên trái (người lái và người ngồi sau, cùng một chiếc xe).
- Lỗi thuộc loại: **gộp-tách** — ban đầu tôi vẽ người lái và người ngồi sau thành một mask `person` duy nhất vì hai người ép sát nhau, rất khó phân biệt đường viền.
- Bằng chứng tôi nhìn thấy: sau khi phóng to vùng đó trong CVAT, thấy rõ hai chiếc mũ bảo hiểm riêng biệt và hai đường vai tách nhau; danh sách Objects chỉ hiện một object cho cả hai người đó.
- Quy tắc và hành động sửa: quy tắc "hai vật cùng lớp sát nhau vẫn là **hai instance**". Tôi xóa mask cũ, dùng Polygon vẽ lại lần lượt từng người thành hai instance riêng, kiểm tra danh sách Objects thấy đủ hai `person`, rồi bấm Save.
- Sau sửa đã Save và export lại chưa? **Đã Save** trong CVAT; đã export lại `medium_instance.zip` định dạng COCO 1.0 và đặt lại vào `submissions/`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): chưa có điểm (release ground truth chưa được công bố, Actions chỉ kiểm cấu trúc ZIP). Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `000000181542.jpg` — người đi xe máy ngoài cùng bên phải, bị cắt sát mép ảnh, chỉ thấy phần vai và tay (~20–30 px) | (a) Gán nhãn `person` cho phần cơ thể rất nhỏ còn thấy ở mép ảnh phải; (b) bỏ qua vì quá nhỏ và bị cắt gần hết | Quy tắc: "chỉ vẽ phần nhìn thấy"; không có hướng dẫn ngưỡng diện tích tối thiểu trong `classes.json` | Tôi chọn gán nhãn (a) để không bỏ sót vật có thật. **Hỏi coach**: có ngưỡng diện tích tối thiểu (pixel) cho `person` bị cắt mép không? |
| `000000373353.jpg` — taxi màu vàng phía trước double-decker bus; phần đầu xe bị bus che ~30% | (a) Vẽ mask `car` ôm toàn bộ xe taxi kể cả phần bị bus che (đoán biên phía sau); (b) chỉ vẽ phần taxi nhìn thấy, dừng tại mép che của bus | Quy tắc: "chỉ vẽ phần nhìn thấy, không tự đoán phần bị che" → ủng hộ (b) rõ ràng | Tôi chọn (b): mask taxi dừng đúng tại đường mép bus, không đoán phần khuất. Quy tắc đã đủ rõ ở ca này. |
| `000000458325.jpg` — người đi ván trượt (skateboard) đi giữa đường, ván không có class trong task | (a) Gán nhãn `person` cho người trên ván, bỏ ván; (b) bỏ qua cả người lẫn ván vì "phương tiện" không có class | `classes.json` chỉ có `person, bicycle, car, motorcycle, bus, truck`; người đứng trên ván vẫn là người, ván trượt không phải một trong sáu class | Tôi chọn (a): người đi ván vẫn là `person`; ván trượt không vẽ vì không có class tương ứng. **Hỏi coach**: cách xử lý khi phương tiện của người không có trong danh sách class — chỉ nhãn người hay bỏ hẳn cả hai? |
