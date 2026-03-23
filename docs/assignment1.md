---
layout: default
title: Assignment 1
---

# 🧠 BÀI TẬP LỚN SỐ 1  
## Phân loại Ảnh, Văn bản & Đa phương thức

---

## 👥 THÔNG TIN NHÓM
- **Nhóm:** GROUP_11  
- **Thành viên:**
  - Nguyễn Huy Hoàng - 2570197  
  - Nguyễn Anh Khoa - 2211612  
- **Giảng viên:** Lê Thành Sách  

---

## 🎥 DEMO & PRESENTATION

- 🔗 **Video Demo:**  
👉 [Xem tại đây](#)

- 🔗 **Video Báo Cáo (YouTube):**  
👉 [Xem tại đây](#)

---

## 💻 SOURCE CODE

- 🔗 **GitHub Repository:**  
👉 [Xem code tại đây](#)

---

# 📑 NỘI DUNG BÁO CÁO

---

# 🖼️ 1. IMAGE DATASET (CNN vs ViT)

## 1.1 📊 Bài toán & Dataset (EDA)
- Mô tả bài toán phân loại ảnh
- Giới thiệu dataset
- Phân tích dữ liệu:
  - Số lượng ảnh mỗi class
  - Visualization sample
  - Nhận xét

👉 [Xem chi tiết](#)

---

## 1.2 ⚙️ Dataset, Dataloader & Augmentation
- Preprocessing ảnh
- Data augmentation (flip, rotate, normalize,...)
- Pipeline DataLoader

👉 [Xem chi tiết](#)

---

## 1.3 🤖 Mô hình & Huấn luyện
### 🔹 CNN
- Kiến trúc (ResNet, EfficientNet,...)
- Fine-tune pretrained

### 🔹 Vision Transformer (ViT)
- Kiến trúc ViT
- Fine-tune pretrained

👉 [Xem chi tiết](#)

---

## 1.4 📈 Kết quả & So sánh
- Bảng so sánh:
  - Accuracy
  - Loss
- Biểu đồ training
- So sánh CNN vs ViT

👉 [Xem chi tiết](#)

---

## 1.5 🧠 Nhận xét
- Khi nào CNN tốt hơn?
- Khi nào ViT tốt hơn?

---

# 📝 2. TEXT DATASET (RNN vs Transformer)

## 2.1 📊 Bài toán & Dataset (EDA)
- Mô tả bài toán text classification
- Dataset (ví dụ: 20 Newsgroups)
- Phân tích:
  - Độ dài văn bản
  - Phân phối label

👉 [Xem chi tiết](#)

---

## 2.2 ⚙️ Dataset, Dataloader & Preprocessing
- Tokenization
- Padding
- Embedding (GloVe / Word2Vec)

👉 [Xem chi tiết](#)

---

## 2.3 🤖 Mô hình & Huấn luyện
### 🔹 RNN (LSTM / BiLSTM)
- Kiến trúc
- Training

### 🔹 Transformer
- BERT / DistilBERT
- Fine-tune

👉 [Xem chi tiết](#)

---

## 2.4 📈 Kết quả & So sánh
- Accuracy / F1-score
- So sánh:
  - RNN vs Transformer

👉 [Xem chi tiết](#)

---

## 2.5 🧠 Nhận xét
- Ưu nhược điểm từng model
- Khả năng học long-term dependency

---

# 🔗 3. MULTIMODAL (Zero-shot vs Few-shot)

## 3.1 📊 Bài toán & Dataset
- Mô tả bài toán đa phương thức
- Dataset (image + text)

👉 [Xem chi tiết](#)

---

## 3.2 🤖 Phương pháp

### 🔹 Zero-shot Classification
- Sử dụng model pretrained (CLIP,...)
- Không cần fine-tune

### 🔹 Few-shot Classification
- Fine-tune với ít dữ liệu
- Prompt / adapter / training nhẹ

👉 [Xem chi tiết](#)

---

## 3.3 📈 Kết quả & So sánh
- So sánh:
  - Accuracy
  - Khả năng tổng quát hóa
- Zero-shot vs Few-shot

👉 [Xem chi tiết](#)

---

## 3.4 ⚡ Efficiency (phần bonus rất quan trọng)
- So sánh:
  - Accuracy vs Model size
  - Inference time
- Thử:
  - Quantization / pruning (nếu có)

👉 [Xem chi tiết](#)

---

## 3.5 🧠 Nhận xét
- Khi nào dùng zero-shot?
- Khi nào cần few-shot?

---

# 📌 TỔNG KẾT
- Tổng hợp kết quả cả 3 bài toán
- Insight chính:
  - CNN vs ViT
  - RNN vs Transformer
  - Zero-shot vs Few-shot
- Hạn chế
- Hướng phát triển
