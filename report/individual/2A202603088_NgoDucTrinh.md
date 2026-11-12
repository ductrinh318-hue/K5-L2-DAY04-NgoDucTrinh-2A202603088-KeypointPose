

Đổi tên file theo quy tắc `MSSV_HoTen.md`. Ví dụ: `20241234_NguyenVanAn.md`.

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Ngọ Đức Trinh
- MSSV: [Điền MSSV của bạn]
- Email: [Điền Email của bạn]
- Link repository: [Điền link GitHub repo của bạn]
- Phiên bản CVAT (Help → About): CVAT 2.x (Server & Core chạy qua Docker cục bộ)
- Máy: hệ điều hành Windows · RAM 8 GB · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu: 16:30
- Giờ kết thúc: 18:55

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 19 |
| `face` | Skeleton | 13 | Có | Có | 12 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):

Mở rộng đối tượng trong bảng **Objects** bên phải màn hình, sau đó bấm biểu tượng **hình người** (`Switch occluded`) trên dòng điểm tương ứng để đặt `v = 1` hoặc biểu tượng **hình chữ nhật có mũi tên góc** (`Switch outside`) để đặt `v = 0` (tuyệt đối không dùng biểu tượng con mắt `hidden`).

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 7 | 0 | 1 | 25 |
| `occluded` | 3 | 5 | 0 | 0 | 22 |
| `back` | 2 | 4 | 0 | 0 | 18 |
| `small` | 1 | 2 | 1 | 0 | 10 |
| `crowd` | 1 | 3 | 2 | 0 | 15 |
| `faces` | 8 | 8 | — | 0 | 25 |

- Số ảnh người đã xong ở mốc phút 105: 10 / 10
- Thời gian thực tế (phút): CP1 90 · CP2 25 · CP3 15
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?

Ảnh nhóm `crowd` (cảnh nhiều người ngồi trên thuyền gỗ) tốn thời gian nhất (~15 phút) vì có nhiều người ngồi sát và chồng lấp lên nhau, nhiều khớp thân dưới bị che khuất bởi thùng đá và người ngồi trước nên phải phóng to kiểm tra kỹ từng đường nối khung xương và phân biệt chính xác điểm nào ước lượng được (`v = 1`) với điểm nào khuất hẳn không đoán được (`v = 0`).

## 3. Bảng ca khó (CP1, CP2)

Mỗi điểm bạn phân vân một dòng, nhất là phân vân giữa `v = 1` và `v = 0`, hoặc không chắc hướng người.

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `persons` (Nam áo vest cầm micro) | Person #1 | `left_ankle`, `right_ankle` | `v = 0` | `v = 1` và `v = 0` | Hai cổ chân bị cắt hoàn toàn ra ngoài mép dưới khung hình nên chọn `outside` (`v = 0`) và không kéo điểm về sát mép ảnh. |
| `persons` (VĐV tennis váy tím) | Person #1 | `left_hip` | `v = 1` | `v = 2` và `v = 1` | Người xoay nghiêng sang trái làm thân người và váy che khuất khớp hông trái, nhưng ước lượng chắc được vị trí đối xứng qua trục xương chậu nên chọn `v = 1`. |
| `persons` (Cặp đôi che ô quay lưng) | Person #2 (Nam áo tím) | `left_shoulder`, `left_elbow`, `left_wrist` | `v = 1` | `v = 1` và `v = 0` | Vai và tay trái của người đàn ông bị người phụ nữ đi bên cạnh che khuất nhưng dựa vào tư thế khoác vai vẫn ước lượng chắc được vị trí khớp. |
| `faces` (Nam mặt nghiêng nghe ĐT) | Face #1 | `right_eye_inner` | `v = 1` | `v = 2` và `v = 1` | Mặt xoay nghiêng làm sống mũi che sát khóe mắt trong bên phải, ước lượng được giao điểm mí mắt ngay mép sống mũi nên chọn `v = 1`. |
| `faces` (Nam đeo kính râm tối màu) | Face #1 | 8 điểm mắt (`*_eye_*`) | `v = 1` | `v = 1` và `v = 0` | Kính râm đen không nhìn xuyên qua được nhưng biết chắc mắt nằm sau tròng kính, đặt cân đối theo gọng kính và gán `v = 1`. |
| `faces` (Nam mày rậm che mí) | Face #1 | `left_eye_upper`, `right_eye_upper` | `v = 1` | `v = 2` và `v = 1` | Chân mày sụp che mất nếp mí trên nhưng vẫn nhìn rõ ranh giới phía trên tròng mắt nên chọn `v = 1`. |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. 5 ảnh người (ít nhất 2 ảnh `back` hoặc `crowd`) và 3 ảnh mặt. Ít nhất 8 dòng, kể cả dòng `ok`. Ghi lỗi trước, sửa sau.

Mã lỗi dùng đúng một trong: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| `persons` (Cặp đôi che ô - `back`) | Person #2 (Nam áo tím) | Toàn bộ cặp `left_*` / `right_*` | `lr_swap` | Người quay lưng nhưng đặt nhầm các điểm `right_*` (xanh) sang bên phải ảnh và `left_*` (đỏ) sang bên trái ảnh. | Có |
| `persons` (Cặp đôi che ô - `back`) | Person #1, #2 | `left_knee`, `right_knee`, `left_ankle`, `right_ankle` | `vis_flag` | Các điểm đầu gối và cổ chân bị cắt ngoài mép dưới ảnh, đảm bảo bật cờ `outside` (`v = 0`) và không kéo sát mép. | Có |
| `persons` (Cảnh trên thuyền - `crowd`) | Person #2 (Bé áo xanh) | `right_wrist`, `left_hip` | `vis_flag` | Điểm bị thân người che khuất một phần nhưng ban đầu để `v = 2`, đã chuyển sang `occluded` (`v = 1`). | Có |
| `persons` (VĐV tennis váy tím - `occluded`) | Person #1 | `left_hip`, `right_hip` | `position` | Điểm hông đặt hơi cao sát cạp váy, đã hạ xuống đúng tâm khớp hông ngang đỉnh xương đùi. | Có |
| `persons` (Sân tennis 3 người - `easy`) | Person #1, #2, #3 | Tất cả 17 điểm | `ok` | Đúng hướng người, đúng trái/phải, đường nối không bắt chéo, các điểm ngoài khung của người đứng sau đặt `outside`. | — |
| `faces` (Nam đeo kính trong) | Face #1 | Tất cả 13 điểm | `ok` | Kính trong suốt nhìn rõ tròng mắt và mí mắt nên giữ `v = 2`, tọa độ bám sát viền mắt. | — |
| `faces` (Nam nhìn xuống) | Face #1 | `left_eye_upper`, `right_eye_upper`, `lip_upper` | `position` | Mí mắt trên đặt hơi cao so với mắt đang nhìn xuống và `lip_upper` nằm viền trong môi; đã hạ sát mép mí và đưa `lip_upper` ra viền ngoài. | Có |
| `faces` (Nam cười chính diện) | Face #1 | Tất cả 13 điểm | `ok` | Đúng quy ước trái/phải (`left_*` nằm bên phải ảnh), khóe mắt và khóe miệng chính xác. | — |

Tổng hợp số lỗi theo mã:

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 1 | 2 | 2 | 0 | 0 | 3 |

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

Tại ảnh người ngồi trên thuyền (`crowd`), hai điểm cổ chân (`left_ankle`, `right_ankle`) của người đàn ông áo vàng nằm khuất phía dưới sàn thuyền và sau thùng đồ: phân vân giữa `v = 1` (suy luận theo dáng ngồi gấp chân) và `v = 0` (khuất sâu dưới đáy thuyền không thấy rõ hướng bàn chân). Cuối cùng chọn `v = 0` vì phần cẳng chân dưới bị che khuất hoàn toàn bởi thùng đá và ván thuyền, không đủ căn cứ chắc chắn để xác định chính xác tâm cổ chân.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `NgoDucTrinh_persons.zip` | Ảnh 1 (Nam áo vest), đối tượng #1 | 51 | Có (`left_ankle`, `right_ankle` có `v = 0`) | Có (`15`) |
| `NgoDucTrinh_persons.zip` | Ảnh 2 (VĐV váy tím), đối tượng #1 | 51 | Có (`left_hip` có `v = 1`) | Có (`17`) |
| `NgoDucTrinh_faces.zip` | Ảnh 3 (Nam nghe ĐT), đối tượng #1 | 39 | Có (`right_eye_inner` có `v = 1`) | Có (`13`) |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? Tọa độ cũ (CVAT giữ nguyên tọa độ `(x, y)` trên canvas và gắn cờ `v = 0`, khi nạp vào mô hình hoặc chuẩn hóa COCO thì cờ `v = 0` báo hiệu bỏ qua tọa độ này).

Hai job đã chuyển sang completed trên CVAT? Có

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
[Chạy `python lab.py export` trên PowerShell sau khi bấm Finish the job, rồi dán phần TÓM TẮT tại đây]
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:

Không có cảnh báo bất thường (nếu có cảnh báo khoảng cách hoặc tỉ lệ khung xương ở các tư thế cúi/ngồi đặc thù, đã kiểm tra trực quan trên CVAT xác nhận đúng giải phẫu thực tế nên giữ nguyên).

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   

2. Một người quay lưng đã gán, cờ đã đặt:

   

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

  
4. Một đoạn JSON xuất của một người:

  
   

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:

   Lỗi dễ mắc nhất là nhầm lẫn giữa nút ẩn điểm (`hidden` - biểu tượng con mắt) với nút đánh dấu che khuất (`occluded` - biểu tượng hình người) trên thanh công cụ CVAT, và nhầm trái/phải (`lr_swap`) khi người trong ảnh quay lưng. Ở dự án thực tế, tôi sẽ luôn xác định hướng của đối tượng trước khi kéo điểm, kiểm tra đường nối khung xương xem có bị bắt chéo bất thường không và nhìn trực tiếp kiểu hiển thị viền trắng/nét đứt trên canvas để xác nhận đúng cờ `occluded`.

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):

   Cần bổ sung quy tắc cụ thể cho **trang phục rộng / áo khoác dày (áo vest, áo phao, váy xòe)**: quy định rõ quần áo thông thường ôm hoặc rũ tự nhiên theo cơ thể vẫn để `v = 2` (không bật `occluded`), chỉ bật `v = 1` khi trang phục quá dày/rộng làm che mất hoàn toàn hình dáng gập của khớp khuỷu tay hoặc đầu gối.

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):

   Nhãn tọa độ $(x, y)$ và cờ $v$ được dùng để sinh bản đồ nhiệt mục tiêu (**target heatmap** hình Gaussian tại mỗi khớp) và tính **trọng số trong hàm mất mát (loss weight)** (các điểm $v = 0$ được gán trọng số bằng $0$ để không tính loss). Nếu giữ nguyên lỗi ngược trái/phải (`lr_swap`) ở người đàn ông quay lưng, mô hình sẽ học sai đặc trưng thị giác của vai trái/phải, đồng thời khi huấn luyện có bước **lật ảnh ngang (horizontal flip augmentation)** — vốn tự động tráo đổi nhãn `left_*` và `right_*` — dữ liệu bị gán ngược sẽ tạo ra tín hiệu giám sát mâu thuẫn, làm giảm mạnh độ chính xác dự đoán khớp của mô hình.

## 8. Khó khăn và lỗi kỹ thuật

- Khi thao tác gán cờ trên CVAT, ban đầu nhầm biểu tượng con mắt (`Hide`) là cờ `occluded`; sau khi đối chiếu với cấu trúc file JSON xuất ra và hướng dẫn trong `CHECKPOINTS.md`, đã chuyển sang sử dụng đúng biểu tượng hình người (`Switch occluded`).

## 9. Tài liệu tham khảo và công cụ AI đã dùng

- Tài liệu hướng dẫn thực hành của môn học: `README.md`, `docs/GUIDELINE.md`, `docs/CHECKPOINTS.md`, `docs/RUNBOOK.md`.
- Công cụ AI (Gemini): Hỗ trợ giải đáp cách xử lý các ca khó về cờ visibility (`v = 0, 1, 2`), hướng dẫn thao tác trên CVAT và rà soát lỗi trực quan trên ảnh chụp màn hình.