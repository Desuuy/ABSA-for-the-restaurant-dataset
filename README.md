# 🍽️ Vietnamese Restaurant Review Aspect-Based Sentiment Analysis

## 📌 Giới thiệu
Dự án này thực hiện **Aspect-Based Sentiment Analysis (ABSA)** trên **VLSP 2018 Restaurant Dataset** bằng tiếng Việt.  
**Mục tiêu:** dự đoán **cảm xúc** (Positive, Negative, Neutral) cho từng **khía cạnh** (aspect) trong đánh giá của khách hàng về nhà hàng.

### Các thành phần chính:
- **Tiền xử lý dữ liệu tiếng Việt**: loại bỏ nhiễu, chuẩn hóa chính tả, xử lý teencode, tách từ.
- **Word2Vec embeddings**: biểu diễn từ dựa trên mô hình `wiki.vi.model.bin`.
- **BiLSTM đa nhiệm**: dự đoán đồng thời nhiều khía cạnh và cảm xúc.
- **Đánh giá mô hình**: Accuracy, Precision, Recall, F1-score, Confusion Matrix.
- **Dự đoán thử**: nhập câu tiếng Việt và trả kết quả phân tích.

## 📂 Cấu trúc thư mục
├── preprocessing.py # Module tiền xử lý tiếng Việt

├── nlp_model.py # Định nghĩa & huấn luyện mô hình BiLSTM

├── data/

│ ├── raw/ # Dataset gốc (train/dev/test)

│ ├── processed/ # Dataset đã tiền xử lý

│ └── wiki.vi.model.bin/ # Mô hình Word2Vec tiếng Việt

├── results/ # Lưu biểu đồ, báo cáo kết quả

└── README.md


---

## 📊 Dữ liệu
Sử dụng **VLSP 2018 SA - Restaurant Dataset** gồm 3 file:
- `1-VLSP2018-SA-Restaurant-train.csv`
- `2-VLSP2018-SA-Restaurant-dev.csv`
- `3-VLSP2018-SA-Restaurant-test.csv`

**Cấu trúc dữ liệu:**
- **Review**: câu đánh giá của khách.
- **Các cột aspect**: giá trị `0` (Not Mentioned), `1` (Positive), `2` (Negative), `3` (Neutral).

---

## 🛠️ Tiền xử lý
Pipeline tiền xử lý gồm:
1. Xóa HTML, emoji, URL, email, số điện thoại, hashtag.
2. Chuẩn hóa unicode tiếng Việt.
3. Chuẩn hóa dấu theo **VinAI** hoặc thuật toán **Behitek**.
4. Thay thế **teencode** & từ viết tắt đặc thù domain nhà hàng.
5. Sửa lỗi chính tả bằng mô hình [`bmd1905/vietnamese-correction-v2`](https://huggingface.co/bmd1905/vietnamese-correction-v2).
6. Tách từ bằng **VnCoreNLP**.
7. Xuất CSV chứa câu gốc và câu đã xử lý.

---

## 🧠 Mô hình
- **Embedding Layer**: khởi tạo từ Word2Vec.
- **BiLSTM**: trích xuất ngữ cảnh hai chiều.
- **Linear Layer**: dự đoán toàn bộ các khía cạnh cùng lúc.
- **Masked Loss Function**: bỏ qua khía cạnh không được đề cập (`label=0`).

---

## 📈 Đánh giá
- **Aspect Detection**: nhận diện khía cạnh được đề cập.
- **Sentiment Classification**: phân loại cảm xúc cho khía cạnh đó.

**Metric sử dụng**:
- Accuracy
- Precision / Recall / F1-score (macro, weighted)
- Confusion Matrix

---

## 🚀 Cách chạy

### Cài đặt môi trường
```
pip install underthesea gensim torchinfo seaborn scipy
pip install vncorenlp emoji transformers
```
### Data Preprocessing
```
python preprocessing.py
```
### Running Model
```
python nlp_model.py
```
### Test 
```
from nlp_model import predict_sentiment
text = "Món ăn ngon nhưng phục vụ chậm."
print(predict_sentiment(text))
