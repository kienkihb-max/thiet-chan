# Mô tả hình ảnh lưỡi bằng AI (bản thử nghiệm)

Trang web chụp hoặc chọn ảnh lưỡi, dùng AI khoanh vùng và gọi tên các đặc điểm hình ảnh (chất lưỡi, rêu lưỡi, nứt, hằn răng…) bằng tiếng Việt.

- **Ảnh không rời khỏi máy người dùng.** Mô hình được tải về trình duyệt và chạy ngay trên điện thoại/máy tính; trang không có máy chủ nhận ảnh.
- **Đây không phải công cụ chẩn đoán.** Kết quả chỉ là mô tả hình ảnh tự động, có thể sai. Kết luận do bác sĩ đưa ra sau khi khám.

## Thành phần

| File | Vai trò |
|---|---|
| `index.html` | Trang web |
| `model.onnx` | Mô hình đã huấn luyện (thêm vào sau khi chạy sổ tay huấn luyện) |
| `huan_luyen_thiet_chan.ipynb` | Sổ tay Google Colab để huấn luyện và xuất `model.onnx` |

Bộ ảnh huấn luyện (khoảng 2,34 GB) **không** nằm trong kho này; chỉ cần nó ở bước huấn luyện.

## Bước 1. Huấn luyện mô hình

1. Tải `shezhen_datasets1.zip` lên Google Drive (thư mục gốc *My Drive*).
2. Mở `huan_luyen_thiet_chan.ipynb` bằng Google Colab, bật GPU T4, chạy lần lượt các ô.
3. Ô cuối tải về file `model.onnx`.

## Bước 2. Đưa lên web (GitHub Pages)

1. Trên github.com: **New repository** → đặt tên (ví dụ `thiet-chan`) → chọn **Public** → **Create repository**.
2. **Add file → Upload files** → kéo thả `index.html`, `model.onnx`, `README.md`, `huan_luyen_thiet_chan.ipynb` → **Commit changes**.
3. **Settings → Pages** → mục *Branch* chọn `main` và `/ (root)` → **Save**.
4. Sau 1–2 phút, trang có địa chỉ dạng `https://<tên-tài-khoản>.github.io/thiet-chan/`.

Khi có mô hình mới, chỉ cần tải `model.onnx` mới lên đè file cũ.

## Kết quả mô hình v1 (17/11/2026) và cách trang hiển thị

YOLO11n, 60 vòng học, chấm trên 553 ảnh kiểm tra. AP50: 1,0 là hoàn hảo.

| Nhóm trên trang | Đặc điểm (AP50) | Cách hiển thị |
|---|---|---|
| A – hiển thị | Rêu trắng (0,94), Rêu vàng (0,82), Lưỡi gầy mỏng (0,71), Lưỡi đỏ (0,60) | "Thấy rõ" / "Có thể có" |
| B – hiển thị kèm lưu ý | Rêu bong (0,53), Rêu đen (0,39), Lưỡi bệu (0,30) | Kèm câu "AI còn hay nhầm, cần bác sĩ xác nhận" |
| C – không nhận định | Hằn răng (0,20), Lưỡi tím (0,14), Vết nứt (0,14), Điểm đỏ (0,01), Rêu trơn ướt (0,00) | Liệt kê ở mục "AI chưa đánh giá được" |
| Ẩn | Lưỡi bình thường, 7 nhãn vùng tạng phủ | Không hiển thị |

Ngưỡng từng đặc điểm nằm trong biến `FEATURES` ở `index.html`; thông tin bác sĩ nhận báo cáo nằm trong `CONFIG`.

Rêu bong, rêu đen, lưỡi bình thường có rất ít ảnh trong tập kiểm tra (1–3 ảnh) nên điểm của chúng chưa đáng tin.

## Luồng sử dụng

1. Người dùng chụp hoặc chọn ảnh; mô hình chạy ngay trong trình duyệt.
2. Trang kiểm tra ảnh (quá tối, quá chói, độ phân giải thấp, không thấy lưỡi).
3. Trang hiện tóm tắt theo Chất lưỡi / Hình thể / Rêu lưỡi, mô tả dễ hiểu từng đặc điểm và lưu ý yếu tố gây nhiễu (đồ ăn uống, ánh sáng).
4. Người dùng bấm "Gửi cho bác sĩ": điện thoại mở bảng chia sẻ (ví dụ Zalo), gửi ảnh gốc kèm bản mô tả. Bác sĩ xem và trả lời.

## Bảng nhãn

| Mã | Tên gốc | Tiếng Việt | Nhóm |
|---|---|---|---|
| 0 | jiankangshe | Lưỡi bình thường | Chất / hình thái lưỡi |
| 1 | botaishe | Rêu bong | Rêu lưỡi |
| 2 | hongshe | Lưỡi đỏ | Chất / hình thái lưỡi |
| 3 | zishe | Lưỡi tím | Chất / hình thái lưỡi |
| 4 | pangdashe | Lưỡi bệu | Chất / hình thái lưỡi |
| 5 | shoushe | Lưỡi gầy mỏng | Chất / hình thái lưỡi |
| 6 | hongdianshe | Lưỡi có điểm đỏ | Chất / hình thái lưỡi |
| 7 | liewenshe | Lưỡi nứt | Chất / hình thái lưỡi |
| 8 | chihenshe | Lưỡi có hằn răng | Chất / hình thái lưỡi |
| 9 | baitaishe | Rêu trắng | Rêu lưỡi |
| 10 | huangtaishe | Rêu vàng | Rêu lưỡi |
| 11 | heitaishe | Rêu đen | Rêu lưỡi |
| 12 | huataishe | Rêu trơn ướt | Rêu lưỡi |
| 13 | shenquao | Vùng thận lõm | Vùng tạng phủ |
| 14 | shenqutu | Vùng thận lồi | Vùng tạng phủ |
| 15 | gandanao | Vùng can đởm lõm | Vùng tạng phủ |
| 16 | gandantu | Vùng can đởm lồi | Vùng tạng phủ |
| 17 | piweiao | Vùng tỳ vị lõm | Vùng tạng phủ |
| 18 | xinfeitu | Vùng tâm phế lồi | Vùng tạng phủ |
| 19 | xinfeiao | Vùng tâm phế lõm | Vùng tạng phủ |

Mã nhãn theo file cấu hình YOLO của bộ dữ liệu gốc (20 nhãn). Danh sách `classes.txt` trong bản COCO có 21 nhãn (thêm `piweitu`), nên sổ tay huấn luyện có bước kiểm tra mã nhãn trước khi học. Nhóm "vùng tạng phủ" theo lý luận của nhóm tác giả bộ dữ liệu và không được hiển thị trên trang.

## Nguồn dữ liệu và giấy phép

- Dữ liệu: bộ TCM-Tongue, lưu trên Dryad (doi:10.5061/dryad.1c59zw48r), phát hành theo CC0. Ảnh chụp tại Trung Quốc; mô hình chưa được kiểm chứng trên người Việt Nam.
- Công cụ huấn luyện: Ultralytics YOLO11, giấy phép AGPL-3.0. Mô hình và trang web dùng nó nên được công khai mã nguồn theo cùng giấy phép; nếu thương mại hóa cần xem xét giấy phép doanh nghiệp của Ultralytics.
- Thư viện chạy mô hình trên trình duyệt: ONNX Runtime Web (MIT).
