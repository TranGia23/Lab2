KHO LẠNH THÔNG MINH AIoT
Hệ thống giám sát – dự đoán – cảnh báo bằng AI + IoT
1. 🧠 Tổng quan hệ thống
🎯 Mục tiêu

Xây dựng hệ thống AIoT giúp:

Giám sát môi trường kho lạnh theo thời gian thực
Dự đoán nguy cơ mất lạnh trước khi xảy ra
Cảnh báo tự động & ghi nhật ký quyết định
Triển khai AI dưới dạng API để tích hợp thực tế
🧩 Kiến trúc tổng thể
[Cảm biến IoT]
   ↓
[Gateway / MQTT / HTTP]
   ↓
[Data Storage (CSV / DB)]
   ↓
[Data Processing + Feature Engineering]
   ↓
[AI Model Training]
   ↓
[Model Deployment (FastAPI)]
   ↓
[Prediction + Decision Engine]
   ↓
[Alert + Log + Dashboard]
2. 📂 Cấu trúc dự án
smart_cold_storage_aiot/
│
├── data/
│   ├── raw/                 # dữ liệu gốc từ cảm biến
│   └── processed/           # dữ liệu đã xử lý
│
├── notebooks/               # phân tích & thử nghiệm
│
├── src/
│   ├── app.py               # FastAPI server
│   ├── train_model.py       # huấn luyện AI
│   ├── data_utils.py        # xử lý dữ liệu
│   ├── test_api.py          # test API
│   └── check_outputs.py     # kiểm tra output
│
├── models/                  # lưu model
├── outputs/                 # kết quả
├── docs/                    # tài liệu
├── README.md
└── requirements.txt
3. 📊 Tập dữ liệu
📁 File chính
data/raw/cold_storage_raw.csv
📌 Các thuộc tính
Thuộc tính	Ý nghĩa
temperature	Nhiệt độ kho (°C)
humidity	Độ ẩm (%)
co2	Nồng độ CO2
door_open	Trạng thái cửa (0/1)
power_usage	Điện năng tiêu thụ
vibration	Rung động máy nén
cooling_risk	Nhãn (0 = bình thường, 1 = nguy hiểm)
4. ⚙️ Xử lý dữ liệu
🧹 Các bước
1. Làm sạch dữ liệu
Xóa dữ liệu trùng (drop_duplicates)
Chuẩn hóa timestamp
Sắp xếp theo thời gian
2. Xử lý thiếu dữ liệu
Điền trung bình (mean)
Nội suy theo thời gian
3. Chuẩn hóa dữ liệu
StandardScaler hoặc MinMaxScaler
🔧 Feature Engineering

Tạo đặc trưng mới:

temp_diff → thay đổi nhiệt độ
rolling_mean_temp → trung bình trượt
door_open_duration
energy_per_temp
📤 Output
data/processed/telemetry_clean.csv
data/processed/feature_dataset.csv
5. 🤖 Mô hình AI
🎯 Bài toán

Phân loại nguy cơ mất lạnh (Binary Classification)

🧠 Model sử dụng
Logistic Regression
🔀 Train/Test split
75% Train
25% Test
📈 Đánh giá

Các chỉ số:

Accuracy
Precision
Recall
F1-score
💾 Output
models/cold_storage_model.joblib
outputs/metrics.json
🔧 Code mẫu (train_model.py)
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
import joblib

X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.25)

model = LogisticRegression()
model.fit(X_train, y_train)

joblib.dump(model, "models/cold_storage_model.joblib")
6. ⚠️ Phát hiện bất thường (Anomaly Detection)
🎯 Mục tiêu

Phát hiện tình trạng bất thường trước khi AI dự đoán

🔍 Phương pháp
1. Z-score
Z = (x - mean) / std
2. Ngưỡng phát hiện
|Z| > 2.5 → bất thường
📌 Các loại bất thường
🌡️ Nhiệt độ tăng đột biến
🌫️ CO2 tăng cao
⚡ Điện năng bất thường
🔊 Rung động lớn
🛡️ Vai trò

👉 Lớp bảo vệ trước AI
👉 Tránh điều khiển sai hệ thống

7. 🌐 Triển khai FastAPI
▶️ Chạy server
uvicorn src.app:app --reload --port 8001
📄 Swagger UI
http://127.0.0.1:8001/docs
🔗 API Endpoints
Endpoint	Chức năng
/health	Kiểm tra API
/model-info	Thông tin model
/predict	Dự đoán
🧠 Code mẫu (app.py)
from fastapi import FastAPI
import joblib

app = FastAPI()
model = joblib.load("models/cold_storage_model.joblib")

@app.get("/health")
def health():
    return {"status": "OK"}

@app.post("/predict")
def predict(data: dict):
    features = list(data.values())
    prob = model.predict_proba([features])[0][1]
    return {"probability": prob}
8. 📦 Output hệ thống
outputs/
├── metrics.json        # độ chính xác model
├── decision_log.csv    # log quyết định
└── predictions.csv     # kết quả dự đoán
9. 🧾 Quy tắc quyết định (Decision Engine)
⚙️ Rule-based + AI
🔴 Quy tắc 1 (Nguy hiểm cao)
anomaly_score > 2.5
→ DỪNG hệ thống làm lạnh
🟠 Quy tắc 2 (Nguy cơ)
probability >= 0.8
→ KIỂM TRA hệ thống
🟢 Quy tắc 3 (Bình thường)
probability < 0.8
→ HOẠT ĐỘNG bình thường
10. 📊 Nhật ký quyết định

Ví dụ:

time	temp	prob	anomaly	decision
10:01	-5°C	0.2	0.5	NORMAL
10:05	3°C	0.85	1.2	CHECK
10:10	8°C	0.9	3.0	STOP
11. 🎯 Kết quả đạt được

✅ Xây dựng pipeline AIoT hoàn chỉnh
✅ Huấn luyện AI thành công
✅ Triển khai FastAPI hoạt động
✅ Phát hiện bất thường chính xác
✅ Tạo hệ thống cảnh báo thông minh
✅ Ghi log phục vụ kiểm toán & phân tích
