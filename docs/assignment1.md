---
layout: default
title: Assignment 1
---

# BÀI TẬP LỚN SỐ 1  
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
👉 [Xem tại đây](https://github.com/huyhoangg/ComputerVision_HCMUT)

- 🔗 **Video Báo Cáo (YouTube):**  
👉 [Xem tại đây](https://youtu.be/ZdF2tTKzBVw?si=Qj_MPWYKR6kgglMU)

---

## 💻 SOURCE CODE

- 🔗 **GitHub Repository:**  
👉 [Xem code tại đây](#)

---

## 📑 NỘI DUNG BÁO CÁO
**Nội dung bao gồm:**
- 📊 Exploratory Data Analysis (EDA)  
- ⚙️ Dataset, Dataloader & Preprocessing  
- 🤖 Model Training & Fine-tuning  
- 📈 Evaluation & Comparison  
- 🧠 Discussion & Analysis  

---

# 1. IMAGE DATASET (CNN vs ViT)

- So sánh hai nhóm mô hình:
  - CNN (ResNet, MobileNet)
  - Vision Transformer (ViT)
- Sử dụng pretrained + fine-tune
- [Pipeline training và evaluation](https://colab.research.google.com/drive/1Dx8y9QQjvoaXHMDYFWouAm0BXyxn1NEu?usp=sharing)
- Đánh giá và so sánh:
  - Accuracy, Loss
  - Biểu đồ training
  - Hiệu năng so với mô hình MobileNet
- [Biểu đồ](https://colab.research.google.com/drive/1TY5PcesLzqQ6m-NxRHBXk3nhmXdXY8PY?usp=sharing)
---

# 2. TEXT DATASET (RNN vs Transformer)

- So sánh hai nhóm mô hình:
  - RNN (LSTM / BiLSTM)
  - Transformer (BERT, DistilBERT,...)
- Xử lý dữ liệu:
  - Tokenization, Padding, Embedding
- Đánh giá:
  - Accuracy, F1-score
- [Pipeline training và evaluation](https://colab.research.google.com/drive/1ovVxwUNdwiCuf6oW4HjFr0IOWQXv9A-0?usp=sharing)

---

# 3. MULTIMODAL (Zero-shot vs Few-shot)

- So sánh hai cách tiếp cận:
  - Zero-shot (CLIP,...)
  - Few-shot (fine-tune với ít dữ liệu)
- Đánh giá:
  - Accuracy
  - Khả năng tổng quát hóa
- [Pipeline training và evaluation](https://colab.research.google.com/drive/1bpuvxkobtwV_2vXgpMnSsEp6ve6LnVNt?usp=sharing)
---

# 📌 TỔNG KẾT

- So sánh tổng thể:
  - CNN vs ViT  
  - RNN vs Transformer  
  - Zero-shot vs Few-shot  

- Insight chính:
  - Hiệu quả từng loại mô hình theo từng bài toán
  - Trade-off giữa performance và chi phí tính toán  

- Hạn chế:
  - Dataset nhỏ / chưa đa dạng  
  - Chưa tối ưu hyperparameter toàn diện  
