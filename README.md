# 🎮 Chill Block Blast: Player Engagement & Churn Prediction

[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)](https://pandas.pydata.org/)
[![Scikit-Learn](https://img.shields.io/badge/Scikit_Learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)](https://scikit-learn.org/)

## 📝 Tổng quan Dự án (Executive Summary)
Dự án phân tích dữ liệu đo lường hành vi (telemetry data) cho tựa game giải đố **Chill Block Blast**. Mục tiêu cốt lõi là xây dựng luồng xử lý dữ liệu từ A-Z để tìm ra nguyên nhân gốc rễ dẫn đến quyết định gỡ/bỏ game của người dùng (Churn), đồng thời huấn luyện mô hình Machine Learning dự đoán người chơi có nguy cơ rời bỏ với độ chính xác lên tới **95%**.

Kết quả phân tích định lượng chỉ ra rằng: Sự bế tắc về độ khó là rào cản lớn nhất, trong khi việc cung cấp vật phẩm hỗ trợ (Godot icons) là chìa khóa chiến lược để giữ chân người chơi.

## 🎯 Mục tiêu Kinh doanh (Business Objectives)
1. **Dự đoán Rời bỏ (Churn Prediction):** Nhận diện sớm người chơi có nguy cơ gỡ game thông qua mô hình học máy.
2. **Khai phá Nguyên nhân (Root-cause Analysis):** Định lượng mức độ tác động của các yếu tố hành vi (số lần thua, số lượng vật phẩm sử dụng, số phiên chơi).
3. **Đề xuất Giải pháp (Actionable Insights):** Cung cấp các chiến lược can thiệp bằng dữ liệu (Data-driven) để đội ngũ phát triển tối ưu hóa trải nghiệm người dùng (UX).

---

## 🛠️ Phương pháp & Kỹ thuật (Methodology)

### 1. Data Engineering & Xử lý Mất cân bằng dữ liệu (Imbalanced Data)
Dữ liệu hành vi thô ban đầu gặp hiện tượng mất cân bằng cực đoan, dẫn đến hiện tượng mô hình học vẹt (Dự đoán 100% Churn).
* **Giải pháp:** Xây dựng luồng ETL bằng `Pandas/NumPy`, áp dụng kỹ thuật gán nhãn động dựa trên phân vị **(Quantile-based thresholding)**.
* **Kết quả:** Tái tạo thành công phân bổ dữ liệu thực tế (70% Churn - 30% Retain), tạo ra vùng nhiễu (overlap) hợp lý giúp mô hình AI phân tích đặc trưng sâu sắc hơn.

### 2. Machine Learning Modeling
Sử dụng hai thuật toán học máy có tính diễn giải cao (White-box models) để phục vụ cho việc ra quyết định kinh doanh:
* **Decision Tree Classifier:** Trực quan hóa các luật (rules) phân loại hành vi người chơi.
* **Logistic Regression:** Trích xuất trọng số (Coefficient) để đánh giá mức độ tác động tuyến tính của từng biến độc lập.

---

## 📊 Phân tích & Đánh giá Kết quả (Results & Insights)

### 1. Hiệu suất Mô hình (Model Performance)
Hệ thống AI đạt hiệu suất xuất sắc và ổn định trên tập kiểm thử (1000 users):
* **Logistic Regression:** Accuracy **93%**, Recall (Lớp Churn) đạt **96%**.
* **Decision Tree:** Accuracy **95%**, Recall (Lớp Churn) đạt **1.00**.

> Mô hình đủ độ tin cậy để tích hợp vào hệ thống, giúp tự động nhận diện sớm những người chơi đang nản chí trước khi họ quyết định xóa game.

![Báo cáo Mô hình]([Chèn link ảnh báo cáo classification report hoặc confusion matrix vào đây])

### 2. Phân tích Nguyên nhân Cốt lõi (Feature Coefficients)
Từ mô hình Logistic Regression, hai thái cực đối lập định hình hành vi người dùng đã được phát hiện:

* 🔴 **Kẻ hủy diệt trải nghiệm - Tổng số lần thất bại (`total_fails` | Trọng số: +2.91):**
Trọng số dương rất lớn. Người chơi càng thất bại nhiều tại một màn chơi, tỷ lệ rời bỏ càng tăng vọt. Sự bế tắc tạo ra trải nghiệm ức chế.
* 🟢 **Vị cứu tinh - Vật phẩm hỗ trợ (`icons_or_hints_used` | Trọng số: -3.31):**
Trọng số âm lớn nhất. Nếu người chơi sử dụng nhiều icon Godot hỗ trợ qua màn, tỷ lệ họ ở lại với game tăng lên rất cao, triệt tiêu hoàn toàn sự ức chế do thất bại.

![Feature Importance]([Chèn link ảnh Feature Importance hoặc Trọng số Logistic Regression vào đây])
![Biểu đồ Boxplot EDA]([Chèn link ảnh biểu đồ Boxplot hoặc KDE phân phối vào đây])

---

## 💡 Đề xuất Giải pháp (Business Recommendations)

 Dựa trên các phát hiện từ dữ liệu, tôi đề xuất 3 chiến lược hành động:

1. **Điều chỉnh Độ khó Động (Dynamic Difficulty Adjustment - DDA):** Tích hợp logic vào engine game. Nếu hệ thống ghi nhận người chơi thua liên tiếp 3-4 lần tại một màn chơi, hệ thống tự động can thiệp giảm độ khó (thả các khối block dễ ghép hơn).
2. **Hệ thống Trợ giúp Chủ động (Proactive Hint System):** Tự động tặng hoặc gợi ý sử dụng Godot icons miễn phí cho nhóm người chơi đang rơi vào trạng thái bế tắc trước khi họ chạm đến ngưỡng chịu đựng.
3. **Thử nghiệm A/B (A/B Testing):** Triển khai hai giải pháp trên cho một tập Test Group và so sánh tỷ lệ giữ chân (Retention Rate) với Control Group để đo lường tỷ suất hoàn vốn (ROI).

---

## 📂 Cấu trúc Kho lưu trữ (Repository Structure)

```text
├── data/
│   └── chill_block_blast_features.csv    # Dataset sau khi đã ETL & Feature Engineering
├── notebooks/
│   └── Churn_Prediction_Analysis.ipynb   # Source code xử lý dữ liệu, EDA và Machine Learning
├── images/                               # Thư mục chứa các hình ảnh biểu đồ trong báo cáo
└── README.md
