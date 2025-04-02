# Genphysio: Tạo Sinh ECG & Hô Hấp từ PPG bằng Học Sâu 🩺💻

[![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch](https://img.shields.io/badge/PyTorch-1.10+-EE4C2C.svg)](https://pytorch.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

Repository này chứa mã nguồn cho đồ án giữa kỳ, tập trung vào việc tạo sinh có điều kiện tín hiệu Điện tâm đồ (ECG) và tín hiệu Hô hấp (Respiration) từ tín hiệu Quang phổ biến thể tích (PPG) bằng cách sử dụng các mô hình học sâu. Dự án khám phá và so sánh hiệu suất của ba kiến trúc: **U-Net**, **Generative Adversarial Network (GAN)**, và **Long Short-Term Memory (LSTM)**.

## 🎯 Mục tiêu

*   Xây dựng và huấn luyện các mô hình học sâu có khả năng tạo ra tín hiệu ECG và Respiration thực tế chỉ từ tín hiệu PPG đầu vào.
*   So sánh hiệu quả của các kiến trúc U-Net, GAN và LSTM cho bài toán này.
*   Đánh giá các mô hình dựa trên cả chỉ số định lượng (miền thời gian và tần số) và phân tích định tính (hình dạng sóng, phổ).

## ✨ Đặc điểm nổi bật

*   **Tiền xử lý Dữ liệu Toàn diện:** Quy trình chuẩn bị dữ liệu từ hai bộ dữ liệu công khai (BIDMC và CAPNO), bao gồm lấy mẫu lại (250Hz), lọc nhiễu (Butterworth), chuẩn hóa biên độ (Z-score/Min-Max) và phân đoạn.
*   **Triển khai 3 Mô hình:** Mã nguồn PyTorch rõ ràng cho các kiến trúc U-Net (với dual decoder), GAN (Generator & Discriminator) và LSTM.
*   **Huấn luyện Nâng cao:** Sử dụng các kỹ thuật như huấn luyện độ chính xác hỗn hợp (Mixed Precision), tối ưu hóa Adam, learning rate scheduling (Cosine Annealing) và dừng sớm (Early Stopping).
*   **Đánh giá Đa chiều:** Phân tích hiệu suất bằng RMSE, MAE, Lỗi tần số trội, Tương quan phổ và so sánh trực quan tín hiệu/phổ.
*   **Mã nguồn trong Notebook:** Toàn bộ quy trình được trình bày chi tiết trong một Jupyter Notebook duy nhất (`LeAnhTuan_giuaky.ipynb`).

## 📁 Cấu trúc Repository
```├── _results_files/ # (Dự kiến) Lưu trữ các kết quả, đồ thị, mô hình đã huấn luyện (.pth)
├── processed/ # (Dự kiến) Lưu trữ dữ liệu đã được tiền xử lý (.npz)
├── LeAnhTuan_giuaky.ipynb # Notebook chính chứa toàn bộ mã nguồn và quy trình
├── README.md # Tài liệu hướng dẫn này
└── requirements.txt # (Nên tạo) Danh sách các thư viện cần thiết
```

## 📊 Dữ liệu

*   **Nguồn:** Dự án sử dụng hai bộ dữ liệu công khai từ PhysioNet:
    1.  [BIDMC PPG and Respiration Dataset](https://physionet.org/content/bidmc/1.0.0/)
    2.  [CAPNO Database (IEEE TBME Respiratory Rate Benchmark)]([https://physionet.org/content/capnodb/1.0.0/](https://borealisdata.ca/dataset.xhtml?persistentId=doi:10.5683/SP2/NLB8IT))
*   **Tiền xử lý:** Các bước chính được thực hiện trong notebook `LeAnhTuan_giuaky.ipynb`:
    *   Đọc dữ liệu (`.dat`/`.hea`, `.mat`).
    *   Phân đoạn thành các cửa sổ 10 giây.
    *   Lấy mẫu lại tất cả tín hiệu về 250 Hz.
    *   Lọc nhiễu (Bandpass/Lowpass).
    *   Chuẩn hóa biên độ.
    *   Chia tập Train/Validation/Test (70%/15%/15%).
*   **Lưu trữ:** Dữ liệu đã xử lý có thể được lưu trong thư mục `processed/` dưới dạng file `.npz`. Người dùng có thể cần tự tải dữ liệu gốc từ PhysioNet và chạy các cell tiền xử lý trong notebook.

## 🚀 Cách sử dụng

1.  **Chuẩn bị Dữ liệu:**
    *   Tải dữ liệu gốc từ các link PhysioNet ở trên.
    *   Đặt dữ liệu vào cấu trúc thư mục phù hợp (hoặc cập nhật đường dẫn trong notebook).
    *   Chạy các cell tiền xử lý trong `LeAnhTuan_giuaky.ipynb` để tạo dữ liệu trong thư mục `processed/`.
2.  **Huấn luyện & Đánh giá:**
    *   Mở và chạy tuần tự các cell trong notebook `LeAnhTuan_giuaky.ipynb` bằng Jupyter Notebook/Lab hoặc Google Colab.
    *   Notebook sẽ thực hiện:
        *   Định nghĩa các mô hình (U-Net, GAN, LSTM).
        *   Huấn luyện từng mô hình.
        *   Đánh giá hiệu suất trên tập kiểm tra.
        *   Tạo các đồ thị so sánh và phân tích.
3.  **Xem Kết quả:**
    *   Các kết quả định lượng sẽ được in ra trong output của notebook.
    *   Các đồ thị trực quan hóa sẽ được hiển thị và có thể được lưu vào thư mục `_results_files/` (hoặc thư mục `models/` như đã đề cập trước đó - cần kiểm tra lại code lưu file).
    *   Các file trọng số mô hình tốt nhất (`.pth`) cũng có thể được lưu tại đây.
