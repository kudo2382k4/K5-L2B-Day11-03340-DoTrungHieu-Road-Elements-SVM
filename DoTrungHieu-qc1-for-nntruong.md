# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): nntruong        Người kiểm (học viên): DoTrungHieu  
Ngày: 2026-11-21                          Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `front (1).zip`, `fisheye (1).zip` (chưa nhận manifest, coi là revision 1)  
Số ảnh kiểm: 6 (+ 0 ảnh thêm)   Số nhãn đã kiểm (shape + tag): 104  

Ảnh kiểm: A01, A07, A08, A12 (camera trước), B03 (fisheye_rear), B06 (fisheye_left).

| Ảnh | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A01 | 9 | 4 | 13 |
| A07 | 10 | 4 | 14 |
| A08 | 22 | 4 | 26 |
| A12 | 11 | 4 | 15 |
| B03 | 23 | 4 | 27 |
| B06 | 5 | 4 | 9 |
| **Tổng** | **80** | **24** | **104** |

Cách kiểm: mở hai file bằng `road_check lint` (mở được, đúng label), rồi vẽ nhãn lên ảnh gốc đã tăng sáng, phóng to 2–6 lần ở từng vạch, hộp đèn/biển và mép polygon. Độ lệch polyline đo bằng cách dò tâm vệt sơn vuông góc với đường tại mỗi 5 px.

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 6 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 8 | 2 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 15 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 15 | 1 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 6 | 0 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 15 | 2 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 20 | 7 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 20 | 6 | nặng (1) / chính (5) |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 45 | 5 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 30 | 2 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 21 | 1 | chính |
| QC1-12 file | lỗi file | 2 | 0 | nhẹ / nặng |

Ghi chú số mục kiểm: QC1-02 = L1 và R1 của 4 ảnh trước; QC1-03/04/06 = 15 polyline lane; QC1-05 = 6 vạch đứt; QC1-07 = 9 drivable + 9 crosswalk + 2 road_curb; QC1-08 = 16 hộp đèn đã gán + 4 đèn bị thiếu; QC1-09 = 29 biển + 16 đèn; QC1-10 = 29 biển đã gán + 1 biển bị thiếu; QC1-11 = 5 lane fisheye + 16 hộp `edge_zone` ở B03.

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-02 | A07 | lane L1 (không có) | nặng | Thiếu L1: vạch trắng liền sát dải phân cách bê tông bên trái, thấy rõ từ (0,565) tới (440,362) |
| 2 | QC1-02 | A08 | lane L1 (không có) | nặng | Thiếu L1: vạch đứt trắng mờ từ (240,428) tới (516,300); polygon direct của bạn đã đi theo đúng vạch này nên vạch là thấy được |
| 3 | QC1-08 | B03 | traffic_light #20 (1154,410) | nặng | Đèn đang sáng xanh (thấy rõ chấm xanh khi phóng to) nhưng gán `color = red`, `facing = front` |
| 4 | QC1-04 | B03 | lane L1 #5 | chính | Dừng sớm: polyline chỉ từ (770,349) tới (678,286); vạch đứt còn thấy tới khoảng (957,630) gần cản sau |
| 5 | QC1-06 | B03 | lane L1 #5 | chính | `style = solid`, đúng là `dashed` (vạch đứt chia làn) |
| 6 | QC1-06 | B06 | lane other #0 | chính | Hàng khối trắng dọc dải phân cách là vạch đứt: `style = solid`, đúng là `dashed` |
| 7 | QC1-07 | A07 | drivable direct #8 | chính | Khoét vùng lái quanh xe phía trước (mép polygon chạy vòng chân chiếc xe bạc); guideline: "không khoét lỗ quanh xe khác" |
| 8 | QC1-07 | A08 | drivable direct #17 | chính | Dừng ở mép trên crosswalk (y≈420), bỏ cả vùng crosswalk và mặt đường tới táp-lô dù xe đi được |
| 9 | QC1-07 | A08 | drivable alternative #18 | chính | Cùng lỗi như #8: dừng trước crosswalk |
| 10 | QC1-07 | A08 | crosswalk #19 (892,323)–(1269,401) | chính | Thừa: polygon nằm trên vỉa hè bên phải, không có vạch ngựa vằn |
| 11 | QC1-07 | A08 | crosswalk #20 (0,270)–(380,296) | chính | Thừa: polygon nằm trên vỉa hè bên trái, không có vạch ngựa vằn |
| 12 | QC1-07 | B03 | drivable direct #8 / alternative #9 | chính | Ranh giới hai vùng lệch 30–60 px sang phải so với vạch đứt L1 ở đoạn gần xe, direct tràn sang làn bên |
| 13 | QC1-07 | B03 | crosswalk #19 (1038,392) | chính | Thừa: polygon phủ vỉa hè bên phải, không có vạch ngựa vằn |
| 14 | QC1-08 | A07 | traffic_light (không có) | chính | Thiếu đèn xanh nhỏ ở (702,244)–(710,254), cao 10 px |
| 15 | QC1-08 | A08 | traffic_light (không có) | chính | Thiếu đèn xe xanh ở (650,210)–(657,221) |
| 16 | QC1-08 | A08 | traffic_light (không có) | chính | Thiếu đèn đi bộ bàn tay cam ở (697,241)–(709,256) |
| 17 | QC1-08 | A08 | traffic_light (không có) | chính | Thiếu đèn đi bộ hình người trắng ở (775,231)–(783,242) |
| 18 | QC1-08 | A08 | traffic_light #3 (764,165) | chính | Hộp chỉ ôm bóng đèn xanh trên cùng, không ôm cả vỏ cụm đèn (vỏ kéo tới y≈200) |
| 19 | QC1-09 | B03 | traffic_sign #14 (839,239) | chính | Biển chữ nhật xanh chỉ dẫn: `group = prohibitory`, đúng là `information` |
| 20 | QC1-09 | B03 | traffic_sign #15 (836,276) | chính | Biển tròn xanh mũi tên (bắt buộc đi theo hướng): `prohibitory`, đúng là `mandatory` |
| 21 | QC1-09 | B03 | traffic_sign #16 (913,301) | chính | Biển tròn xanh mũi tên: `prohibitory`, đúng là `mandatory` |
| 22 | QC1-09 | B03 | traffic_sign #17 (1001,276) | chính | Biển xanh nhỏ trên cột: `prohibitory`, đúng là `information` |
| 23 | QC1-09 | B03 | traffic_sign #13 (1123,352) | chính | Biển tròn xanh "đi bên phải": `prohibitory`, đúng là `mandatory` |
| 24 | QC1-10 | A12 | traffic_sign (không có) | chính | Thiếu biển xanh nhỏ ở (719,306)–(731,320), cao 14 px |
| 25 | QC1-10 | B03 | traffic_sign #21 (1188,405) | chính | Thừa: vật trong hộp là mảng cửa/tường của tòa nhà, không phải biển |
| 26 | QC1-11 | B06 | lane other #0 | chính | Polyline không bám tâm khối vạch: lệch trung bình 4,4 px, lớn nhất 14 px, 24/61 điểm đo vượt 5 px |

Điểm làm tốt, ghi để công bằng: A01 gần như không có lỗi; bạn gán được hai biển chỉ dẫn xanh trên cao ở A01 và cụm biển cấm đỗ ở A08 (990,184) mà người kiểm cũng bỏ sót. Lane ở A01, A07, A08, A12 bám tâm sơn tốt (lệch trung bình dưới 1,5 px).

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A12 | Xe đang giữa giao lộ, nằm gần đường kéo dài của vạch đứt. Bạn chọn xe ở làn trái (mép vùng gạch chéo = L1, vạch đứt = R1); có thể hiểu xe ở làn phải (vạch đứt = L1). Làn nào là làn đang đi? | |
| B03 | Camera sau: bạn gán vạch đứt bên phải ảnh là R2. Bên phải ảnh camera sau là bên trái xe. Quy ước position ở camera sau theo ảnh hay theo xe? | |
| B03 | Hai hộp đèn #3 (369,249) và #10 (399,236) gán `red`, `facing = front`, nhưng khi phóng to chỉ thấy vỏ tối, không thấy bóng đỏ sáng. Nếu GT là `unknown/back` thì đây là lỗi nặng QC1-08. | |
| A08 | Vạch L1 rất mờ (lỗi #2). Lead có coi là "thấy được" không? Nếu không, lỗi #2 được miễn. | |
| B06 | Camera bên: vùng `direct` của bạn kéo qua cả làn tàu điện tới mép xa. Phạm vi `direct` ở camera bên dừng ở đâu? | |

## Quyết định
Lỗi nặng: 3   Lỗi chính: 23 / 104 nhãn = 22,1 %   Lỗi nhẹ: 0   Ca chờ lead: 5  
Quyết định (Release / Hold / Rework): **Rework**  
Lý do (theo luật của phiếu): còn 3 lỗi nặng (thiếu L1 ở A07 và A08, sai màu đèn hướng về xe ở B03), mỗi lỗi nặng là đủ để Rework; thêm vào đó tỉ lệ lỗi chính 22,1 % vượt ngưỡng 5 %. Ca mơ hồ không đổi kết luận: kể cả khi lead miễn lỗi #2 thì vẫn còn 2 lỗi nặng, nên không ra Hold.

Loại lỗi: bỏ sót = thiếu nhãn phải có; thừa = gán cái không phải đối tượng; sai phạm vi = hình lệch, sai chiều, dừng sai chỗ, tràn vùng. Ba loại này theo cách chia lỗi gán nhãn của ISO/IEC 5259-4 (mục 8.4.2); "sai thuộc tính" và "lỗi file" là khóa học thêm. Cách xếp mức: [thang mức lỗi](docs/qc_severity_road_v1.0.md).
