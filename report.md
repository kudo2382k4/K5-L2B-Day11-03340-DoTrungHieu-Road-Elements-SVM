# Báo cáo cá nhân — Ngày 11
Họ tên: Đỗ Trung Hiếu               Mã học viên: 03340  
Phiên bản CVAT (Help → About): CVAT v2.82.0  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): 2026-11-21 10:02:10 (front) / 10:02:27 (fisheye)    Số lần nộp lại sau Rework: 0  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Đúng | Đủ 6 label hình học: `lane` (polyline), `road_curb` (polyline), `crosswalk` (polygon), `drivable` (polygon), `traffic_light` (rectangle), `traffic_sign` (rectangle). |
| Thuộc tính và giá trị | Đúng | Chuẩn theo guideline mục 1: `lane` (position L1, R1, L2, R2, other; style solid, dashed; color white, yellow, other; kind single, double), `drivable` (area_type direct, alternative), `traffic_light` (color red, yellow, green, off, unknown; type vehicle, pedestrian, beacon; facing front, side, back; edge_zone), `traffic_sign` (group prohibitory, mandatory, warning, information, supplementary, other; facing front, side, back; edge_zone). |
| 4 tag ảnh | Đúng | Đủ 4 label Tag: `camera`, `timeofday`, `weather`, `source`, mỗi tag có thuộc tính select `value`. |
Cách kết thúc polyline trên bản CVAT này: Click các điểm dọc theo tâm vệt sơn (từ gần xe ra xa), sau đó bấm phím **N** (hoặc click đúp chuột trái tại điểm cuối) để kết thúc polyline.  
Cách đặt tag ảnh trên bản CVAT này: Chọn công cụ **Tag** ở thanh công cụ bên trái (hoặc phím tắt tương ứng), chọn label tag cần đặt (`camera`, `timeofday`, `weather`, `source`), click vào khung vẽ canvas, sau đó mở bảng **Objects → Details** để chọn giá trị (`value`) thích hợp.  

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 30 | 2 | 3 | 23 | 30 | 24 |
| front-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| fisheye | 8 | 9 | 10 | 9 | 10 | 7 | 11 |
| fisheye-extra (đã làm __ / 4) | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

*(Tổng cộng: 168 shape + 80 tag = 248 mục kiểm trên 20 ảnh bắt buộc).*

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A05 | `lane` (L1, R1) | Vẽ vạch giả định sát mép xe đỗ hay không vẽ L1/R1 | Không vẽ L1 và R1 | Đường ngõ hẹp xe đỗ hai bên, mặt đường thực tế không có vạch kẻ sơn. Guideline quy định "Không vẽ vạch mà ảnh không có", chấp nhận lint báo QC1-02 và xin miễn trừ theo checkpoint. |
| A06 | `lane` (R1) | Vẽ R1 sát dải phân cách hay chỉ gán road_curb | Chỉ gán `road_curb` và lane L1, không vẽ R1 | Làn đường dưới cầu vượt có dải phân cách bê tông khối cứng bên phải, không có vạch kẻ sơn dưới chân dải phân cách. Mép tiếp giáp là `road_curb`. |
| A09 | `lane` (R1) | Vẽ vạch biên R1 hay để mép lề đường | Không vẽ R1 | Bên phải là bó vỉa lề đường, không có vạch sơn biên đường. Tuân thủ nguyên tắc không tự vẽ vạch không có. |
| A10 | `traffic_sign` | Gán biển báo nhỏ ở xa (≈ 25×7 px) hay bỏ qua | Đã gán hộp `traffic_sign` #7 | Biển báo nhìn thấy được dù chiều cao 7 px (< 8 px ngưỡng bắt buộc). Lint báo cảnh báo nhẹ [QC1-10 nhẹ], người kiểm xác nhận không bắt buộc gán. |
| A12 | `lane` (R1) | Vẽ R1 giả định hay không vẽ | Không vẽ R1 | Xe đang qua nút giao, bên phải L1 là khoảng đường rộng xe đỗ, không có vạch sơn phân chia làn xe bên phải. |
| B01, B02 | `drivable` (direct) | Phủ trùm qua vạch qua đường tới mặt đường xa hay chỉ phủ vạch qua đường | Phủ phần vạch qua đường và mặt đường xe lăn bánh trực tiếp | Ở fisheye góc rộng, vạch đi bộ crosswalk lớn choán phần lớn tầm nhìn gần; vùng direct bao quát phần đường xe di chuyển qua giao lộ. Chờ ý kiến thống nhất từ Lead. |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: ngongoctruong  Quyết định nhận được: **Hold**  
| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-02 | 0 (5 ca chờ lead) | Đồng ý. Các ảnh A05 (thiếu L1, R1), A06 (thiếu R1), A09 (thiếu R1), A12 (thiếu R1) đều thực tế không có vạch sơn; đã tuân thủ đúng quy tắc không vẽ vạch không có. | Không sửa (giữ nguyên quy tắc không vẽ vạch ảo; chờ lead/coach xác nhận miễn lỗi theo CHECKPOINTS). |
| QC1-07 | 0 (2 ca chờ lead) | Đồng ý. B01, B02 vùng drivable ở fisheye cần sự thống nhất từ lead về phạm vi mở rộng qua vạch đi bộ. | Chờ kết luận của lead (nếu lead yêu cầu mở rộng sẽ cập nhật ở Revision 2). |
| QC1-10 | 1 | Đồng ý. Box traffic_sign #7 ở A10 có chiều cao 7 px (< 8 px), là mức nhẹ (minor), không bắt buộc gán. | Không sửa (đã gán đúng biển báo, mức nhẹ không trừ điểm và không bắt buộc gỡ bỏ). |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `DoTrungHieu-front.zip` | Có (`road_check lint` mở thành công, 0 lỗi cú pháp) | 7 / 7 (A01: 3 drivable, 4 lane) | 5 / 5 (A02: 2 drivable, 3 lane) | Có (`camera=front` đủ 12/12 ảnh) |
| `DoTrungHieu-fisheye.zip` | Có (`road_check lint` mở thành công, 0 lỗi cú pháp) | 4 / 4 (B01: 1 crosswalk, 1 drivable, 1 lane, 1 road_curb) | 10 / 10 (B02: 2 crosswalk, 1 drivable, 1 road_curb, 5 traffic_light, 1 traffic_sign) | Có (`camera=fisheye_*` đủ 8/8 ảnh) |

## 6. Ảnh chụp màn hình
1. **Tab Labels của project**: Đủ 6 label hình học và 4 label tag với đầy đủ thuộc tính/giá trị (`docs/report_screenshots/Screenshot 2026-11-21 092548.png` hoặc `docs/images/huong-dan-cvat/02-project-task.jpg`).
2. **Một ảnh camera trước đã gán đủ (làn, vùng lái, đèn/biển)**: Ảnh A01 / A08 với các vạch L1, R1, L2, R2, vùng drivable direct/alternative, hộp traffic_light, traffic_sign (`docs/images/huong-dan-cvat/08-lane-L1-dashed.jpg`, `10-drivable-direct.jpg`).
3. **Một ảnh fisheye đã gán, phóng to đoạn polyline cong**: Ảnh B01 camera góc rộng WoodScape với vạch cong polyline bám sát cung đường mép ngoài và vùng drivable (`docs/report_screenshots/Screenshot 2026-11-21 094439.png`).
4. **Một đoạn XML có `<polyline label="lane">`**:
```xml
<polyline label="lane" source="manual" occluded="0" points="739.00,719.00;710.00,704.00;680.00,687.70;650.00,670.50;620.00,652.50;590.00,633.50;560.00,613.70;530.00,593.10;500.00,571.50;470.00,549.20;438.00,524.30" z_order="0">
  <attribute name="position">R1</attribute>
  <attribute name="style">dashed</attribute>
  <attribute name="color">white</attribute>
  <attribute name="kind">single</attribute>
</polyline>
```

## 7. Tự đánh giá
- **Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20**:  
  - Mã lỗi xuất hiện nhiều nhất trong lint là **QC1-02** (thiếu L1/R1 ở 5 vị trí: A05, A06, A09, A12). Tuy nhiên, đây là do mặt đường thực tế không có vạch sơn phân làn hoặc vạch biên (đường hẻm hẹp xe đỗ kín, chân dải phân cách bê tông, bó vỉa lề đường, ngã tư giao lộ). Tuân thủ nguyên tắc cốt lõi "không vẽ vạch mà ảnh không có", học viên không vẽ vạch bịa. Cảnh báo này được đưa vào danh sách ca mơ hồ để xin miễn trừ từ Coach/Lead theo đúng CHECKPOINTS.  
  - Ngoài ra có 1 cảnh báo **QC1-10** (nhẹ) do gán biển báo ở xa cao 7 px (< ngưỡng 8 px).  
  - **Cách tránh ở Ngày 20**:  
    1. Chạy `road_check lint` thường xuyên trong quá trình làm bài để nắm bắt sớm các cảnh báo.  
    2. Chủ động lập danh sách các vị trí không có vạch sơn thực tế và xin xác nhận miễn lỗi từ Coach/Lead ngay trong phiên làm việc.  
    3. Dùng công cụ đo pixel trên CVAT để kiểm tra kích thước đối tượng ở xa, ưu tiên gán đầy đủ mọi đèn/biển từ 8 px trở lên và tránh mất thời gian vào đối tượng quá nhỏ dưới ngưỡng.  

- **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào**:  
  - **Vị trí nhãn đi vào mô hình (Slide 16, 17, 22)**:  
    1. *Polyline vạch làn*: Huấn luyện mô hình phát hiện làn đường (Lane Detection). Với kiến trúc **Row-anchor** (UFLD, LaneATT, CLRNet), polyline được nội suy tọa độ $x$ tại các hàng $y$ cố định làm ground-truth vị trí vạch; với mô hình phân đoạn (SCNN, LaneNet, YOLOP), polyline được rasterize thành mask.  
    2. *Thuộc tính làn* (`position`, `style`, `color`, `kind`): Đi vào nhánh phân loại ngữ nghĩa (classification branch), giúp xác định vai trò làn xe chạy (egolane L1/R1 vs làn lân cận L2/R2) và quy tắc giao thông (vạch đứt cho phép vượt vs vạch liền cấm chuyển làn).  
    3. *Polygon vùng lái* (`drivable direct/alternative`): Cung cấp ground-truth cho đầu Semantic Segmentation xác định không gian di chuyển an toàn (free-space).  
    4. *Bounding box đèn & biển* (`traffic_light`, `traffic_sign`): Huấn luyện Object Detector và bộ phân loại màu tín hiệu / nhóm biển báo.  
    5. *Tag camera*: Định hướng phân tập dữ liệu (train/val/test) và áp dụng cấu hình góc nhìn tương ứng.  
  - **Phân tích một lỗi làm model học sai**:  
    - Ví dụ lỗi thuộc tính vạch làn **QC1-06** (`style = solid` bị nhầm thành `dashed` hoặc ngược lại):  
      Nếu vạch sơn liền (`solid` - cấm đè vạch/cấm chuyển làn) bị gán nhầm thành vạch đứt (`dashed`), nhánh phân loại của mô hình sẽ học sai đặc trưng thị giác của vạch cấm. Khi hệ thống tự hành ADAS (như Hỗ trợ giữ làn khẩn cấp ELK hoặc Hỗ trợ chuyển làn tự động ALC) hoạt động, xe sẽ hiểu sai rằng làn bên cạnh được phép chuyển làn, có thể dẫn đến hành vi chuyển làn nguy hiểm đè qua dải phân cách hoặc vào luồng xe ngược chiều. Ngược lại, nếu vạch đứt bị gán thành vạch liền, hệ thống sẽ ngăn cản vô lý thao tác chuyển làn an toàn của người lái.  

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | không có làn | 666.6 | 600.3 | 540.0 | 484.5 |
| x của bạn kia | không có làn | 670.0 | 596.6 | 540.2 | 483.8 |

Trả lời 3 câu:

1. **Năm số x đó chính là đích mà một model row-anchor học cho vạch này. Nếu bạn dừng polyline sớm 40 px, hàng nào đổi thành "không có làn"?**  
   - Các hàng neo được đặt cách đều nhau 40 px theo trục tung ($y \in \{720, 680, 640, 600, 560\}$).  
   - Nếu dừng polyline sớm 40 px ở phía gần xe (đáy ảnh, ví dụ điểm đầu dừng ở $y=640$ thay vì $y=680$), thì hàng $y=680$ sẽ nằm ngoài phạm vi bao của vạch và đổi thành "không có làn".  
   - Tương tự, nếu dừng polyline sớm 40 px ở phía xa (đầu mút xa dừng ở $y=600$ thay vì vươn tới $y=560$), thì hàng $y=560$ sẽ mất điểm nội suy và đổi thành "không có làn".  

2. **So với bạn được kiểm (cùng ảnh): ở hàng 720 và hàng 560, hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn (slide 25)?**  
   - Ở hàng $y=720$: Cả hai bạn đều không có điểm chạm tới $y=720$ (điểm bắt đầu ở $y=719$ và $y=680$), cả hai đều là "không có làn" (lệch 0 px). Tại hàng gần xe nhất có vạch là $y=680$, hai bạn lệch nhau $|666.6 - 670.0| = 3.4\text{ px}$.  
   - Ở hàng $y=560$: Hai bạn lệch nhau $|484.5 - 483.8| = 0.7\text{ px}$.  
   - **Lệch ở hàng gần xe (hàng 720 / 680 - đáy ảnh)** ảnh hưởng tới LKA (Lane Keeping Assist) và LDW (Lane Departure Warning) **nhiều hơn rất nhiều**:  
     Theo bài giảng (slide 21 / 25), camera đặt trên xe có góc nhìn phối cảnh (perspective projection). Ở đáy ảnh, các pixel biểu diễn mặt đường sát ngay trước đầu xe (khoảng cách thực tế chỉ ~5 m), nơi biên độ an toàn giữa bánh xe và mép vạch chỉ còn khoảng 20–80 cm. Sai lệch pixel ở cự ly gần tương ứng trực tiếp với độ lệch vị trí ngang thực tế của xe trong làn, quyết định tức thì hành vi đánh lái khẩn cấp của LKA hoặc cảnh báo LDW. Ngược lại, ở hàng xa ($y=560$), điểm ảnh cách xe hàng chục đến hàng trăm mét, chủ yếu dùng cho dự báo độ cong dài hạn (path planning) và hệ thống có đủ thời gian bù trừ khi xe di chuyển tới gần.  

3. **Nếu polyline vẽ từ xa về gần (ngược quy tắc 2), cách tính trên có đổi không? Script nào sẽ gặp khó?**  
   - Về mặt toán học: Tọa độ $x$ tại giao điểm của polyline với đường ngang $y = \text{const}$ (nội suy tuyến tính) **hoàn toàn không đổi**, vì đoạn thẳng hình học trong không gian 2D là duy nhất bất kể thứ tự hai điểm đầu mút.  
   - Tuy nhiên, các **script xử lý downstream** sẽ gặp khó:  
     - Các script nội suy hoặc chuyển đổi định dạng giả định danh sách tọa độ có $y$ giảm dần đơn điệu từ gần xe ra xa. Nếu vẽ ngược, thuật toán duyệt tìm cặp điểm kẹp sẽ bị sai hoặc cần thêm bước đảo mảng (`points.reverse()`).  
     - Script trích xuất hướng vector làn đường (xác định chiều di chuyển của luồng xe).  
     - Script gắn cờ che khuất (`occluded`) và xác định điểm bắt đầu của vạch sát cản xe để đo độ lệch tâm làn.
