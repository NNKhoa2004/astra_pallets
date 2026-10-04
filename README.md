# Astra Pallets

Nhận diện và phân đoạn pallet từ camera RGB Astra Pro bằng YOLO. Model có một lớp `pallets` và được lưu tại `models/best_v2.pt`.

## Cài đặt

Dự án chạy trên Linux hoặc WSL2 có giao diện đồ họa, sử dụng camera V4L2 tại `/dev/video0`. Môi trường đã chạy dùng Python 3.12.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Chương trình tự dùng CUDA nếu PyTorch nhận GPU, nếu không sẽ dùng CPU.

## Kết nối camera trên WSL2

Cắm camera vào máy. Với usbipd-win đã cài trên Windows, kiểm tra BUSID của `Astra Pro HD Camera` bằng PowerShell:

```powershell
usbipd list
usbipd bind --busid <BUSID>
usbipd attach --wsl --busid <BUSID>
```

Lệnh `bind` cần PowerShell chạy với quyền quản trị và chỉ cần khi thiết bị chưa được chia sẻ. BUSID có thể thay đổi khi đổi cổng USB.

Kiểm tra `/dev/video0` đã xuất hiện trong WSL và người dùng có quyền truy cập camera. Nếu camera dùng đường dẫn khác, sửa biến `DEVICE` trong script tương ứng.

## Chạy

Chạy từng script riêng để tránh tranh chấp camera:

```bash
# Kiểm tra camera: 640x480, 15 FPS
python src/test_camera.py

# Nhận diện pallet: 640x480, 30 FPS, ngưỡng confidence 0.70
python src/test_pallets.py

# Chụp ảnh dữ liệu: 1280x720, 30 FPS
python src/capture_rgb.py
```

Nhấn **Q** trong cửa sổ camera để thoát. Với `capture_rgb.py`, nhấn **S** để lưu ảnh vào `data/rgb/` (thư mục tự tạo). Ảnh chụp và môi trường Python không được đưa vào Git.
