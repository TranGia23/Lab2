KHO LẠNH THÔNG MINH AIOT
Hệ thống AIoT cho kho lạnh thông minh
1. Giới thiệu
Đề tài xây dựng trợ giúp AIoT hệ thống:

Giám sát kho lạnh bằng cảm biến IoT
Dự đoán nguy cơ mất lạnh bằng AI
Sinh cảnh báo và nhật ký quyết định
Triển khai mô hình AI bằng FastAPI
Hệ thống đường ống
Raw Data
   ↓
Clean Data
   ↓
Feature Engineering
   ↓
Train AI Model
   ↓
Save Model
   ↓
FastAPI Deployment
   ↓
Prediction & Decision Layer
Dự án cấu trúc
smart_cold_storage_aiot/
│
├── data/
│   ├── raw/
│   └── processed/
│
├── notebooks/
│
├── src/
│   ├── app.py
│   ├── train_model.py
│   ├── data_utils.py
│   ├── test_api.py
│   └── check_outputs.py
│
├── models/
│
├── outputs/
│
├── docs/
│
├── README.md
└── requirements.txt
2. Tập dữ liệu
Tệp dữ liệu chính
data/raw/cold_storage_raw.csv
Các trường dữ liệu
Nhiệt độ
Độ ẩm
CO2
Cửa mở
Mức tiêu thụ điện năng
Rung động máy nén
Rủi ro làm mát
3. Xử lý dữ liệu
Tòa án thực hiện:

dữ liệu chồng chéo
Xử lý giá trị bị thiếu
Dấu thời gian Chuẩn
Kỹ thuật đặc trưng
Tạo bộ dữ liệu đặc trưng cho AI
Đầu ra sinh ra
data/processed/telemetry_clean.csv
data/processed/feature_dataset.csv
4. Mô hình AI
Người mẫu sử dụng
Logistic Regression
Phân chia giai đoạn huấn luyện/kiểm tra
75% Đào tạo
Bài kiểm tra 25%
Mô hình đầu ra
models/cold_storage_model.joblib
outputs/metrics.json
5. Phát hiện bất thường
thông qua sử dụng:

Sự bất thường về nhiệt độ
Sự bất thường của CO2
Phát hiện điểm Z
điểm đến
Phát hiện dữ liệu bất ngờ
Hỗ trợ lớp an toàn
Tránh điều khiển kho lạnh hệ thống
6. Triển khai FastAPI
Chạy API
uvicorn src.app:app --reload --port 8001
Tài liệu Swagger
http://127.0.0.1:8001/docs
Điểm cuối API
Điểm cuối	năng lượng
/health	kiểm tra API
/model-info	Mô hình thông tin
/predict	Dự đoán AI
7. Tệp đầu ra
outputs/
├── metrics.json
├── decision_log.csv
└── predictions.csv

models/
└── cold_storage_model.joblib
8. Quy tắc quyết định
Quy tắc 1
anomaly_score > 2.5
→ DỪNG_CHẾ ĐỘ ĐIỀU KHIỂN_T

Quy tắc 2
probability >= 0.8
→ KIỂM TRA LÀM MÁT

Quy tắc 3
Normal condition
→ BÌNH THƯỜNG

9. Kết quả đạt được
Thông thống đã:

Xây dựng hoàn thiện đường ống AIoT
Huấn luyện mô hình AI thành công
Triển khai API FastAPI
Nhật ký quyết định của Sinh
Phát hiện dị thường
Tạo lớp an toàn cho kho lạnh
