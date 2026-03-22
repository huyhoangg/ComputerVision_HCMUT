---
layout: default
title: Bài Tập Lớn 1
---

# 📚 BÀI TẬP LỚN SỐ 1: Phân Loại Dữ Liệu Ảnh, Văn Bản và Đa Phương Thức

**Thực hiện bởi:** Nhóm [Nhập Tên Nhóm] — **Thành viên:** [Tên bạn và bạn chung nhóm]
**GVHD:** [Tên Giảng Viên]

---

## 🔗 LIÊN KẾT NHANH MÃ NGUỒN VÀ VIDEO
- 💻 **Mã Nguồn (Github Source Code):** [Chèn link]
- 📼 **Video Thuyết Trình:** [Chèn link YouTube]
- 🎥 **Video Chạy Demo:** [Chèn link YouTube]

---

## 📑 BÁO CÁO THUYẾT MINH CHI TIẾT (REPORT)

### 1. Khám phá bài toán và Tập dữ liệu (EDA)
- **Ảnh (Image Classification):** [Bạn điền thông tin vào đây]
- **Văn bản (Text Classification):** Cấu hình tập tin tức `20 Newsgroups` với ~18.846 mẫu. Đây là một tập đặc trưng với điểm nhấn là độ giao thoa từ vựng rất lớn (ví dụ: Tôn giáo vs Vô thần, Phần cứng Mac vs Phần cứng IBM) khiến quá trình phân lớp dễ bị *Cạnh tranh Xác Suất (Probability Collision)*.
- **Đa phương thức (Multimodal):** Tập đa nhãn `MM-IMDb` (23 thể loại phim) sử dụng song song Poster và Text Cốt Truyện. Đặc thù của bộ này là độ Mất Cân Bằng Nhãn (Imbalanced Data) cực đoan: phim Drama rải rác khắp nơi nhưng thể loại ngách như Film-Noir hoặc Western lại đếm trên đầu ngón tay.

### 2. Cấu trúc, Huấn luyện, Đánh giá và Phân tích Mô hình (Models)
**2.1. Mô Hình Thị Giác Máy Tính (ẢNH)**
- [Phần này Bạn điền thông tin hoặc báo cáo phân tích kiến trúc CNN vs ViT vào đây do tính chất chưa hoàn thiện hiện tại].

**2.2. Mô Hình Xử Lý Ngôn Ngữ Tự Nhiên (VĂN BẢN)**
- Tuyến 1: Phân loại cơ bản bằng mạng **Bi-LSTM** (Mạng tuần tự) kế thừa biểu diễn từ không gian Embedding của `GloVe` (Cắt Tokenizer và Padding chuẩn).
- Tuyến 2: Khai báo Phân loại đỉnh cao bằng **Transformer (DistilBERT)**, vận dụng cơ chế *Self-Attention Đa Đầu* để đập bỏ rào cản tốc độ đọc hiểu và triệu chứng "Vanishing Gradient" của dòng Recurrent Neural Network.
- *Kết quả Cơ bản (Yêu cầu 60%):* LSTM đạt Test Accuracy khoảng `~61.2%`, trong khi DistilBERT đè bẹp hoàn toàn với độ chuẩn xác cực kỳ khủng `~70.0%`. Báo cáo F1-Score các lớp chi tiết qua ma trận Confusion.

**2.3. Mô Hình Đa Phương Thức (CLIP - ZERO-SHOT vs FEW-SHOT)**
- [Báo Cáo Dự Đoán Zero-Shot: Phần này Bạn điền thông số Accuracy/F1 Zero-Shot vào đây]
- **Chuyển giao Few-shot Classification (Linear Probing):** Nhóm tiến hành bảo lưu 100% trọng số của bộ sậu OpenAI CLIP siêu việt (`ViT-B/32`). Lôi đầu một hệ Hồi quy đa nhãn Logistic (`OneVsRestClassifier`) huấn luyện ép vào một phân bổ mẫu rất bé (~200 Posters) nhờ kỹ thuật Trích xích Véc-tơ Lai Tạo ngang (Late Fusion theo hệ số Alpha). *Kết quả làm F1 tăng vọt so với Zero-Shot vì trí não đã cọ xát với hàm phân phối lớp cục bộ.*

---

## 🚀 3. Báo Cáo Phân Tích Mở Rộng Chuyên Sâu (Hoàn Thành Điểm Thưởng 40%)
*Nhóm chọn đi sâu phân tích ở mảng Văn Bản và Đa Phương Thức (Multimodal)*

### 3.1. Sự Đánh Đổi Tối Ưu Hiệu Quả Mô Hình (Efficiency Trade-off)
**Sức Rướn Phần Cứng Của Text Classification:**

| Cấu trúc Mạng | Khối lượng Tham số | Tốc độ suy luận (1000 mẫu) | Kết luận Thực tiễn (Bản chất sự đánh đổi) |
| :--- | :--- | :--- | :--- |
| **Recurrent (Bi-LSTM)** | ~ **3.25 Triệu** | Siêu tốc (~ **0.25s**) | Trọng lượng mạng nhẹ, tiêu biến ít VRAM. Đẩy tốc độ truy xuất tới ngưỡng *Thời gian thực (Real-time)*. Phù hợp tuyệt vời để nén tích hợp lên mạch Edge Devices (ĐTDĐ, IoT). |
| **Transformer (DistilBERT)** | Khổng lồ (~ **66.3 Triệu**) | Tương đối Chậm | Khối lượng tài nguyên phình to gấp 20 lần, Inference chậm vì cơ chế xử lý ma trận xoay chiều sâu Q-K-V. Đổi lấy khả năng bảo toàn Độ chuẩn xác (Accuracy) cao cực độ tuyệt đối. |

**Bấm giờ Sự Bất Cân Bằng Tốc Độ Của Mô Hinh Multimodal (CLIP):** Thực tiễn bấm giờ đếm ngược cho thấy cơ chế "Mắt thần Nhìn Ảnh" (Image Encoder quét 224x224 điểm ảnh màu) tốn nhiều giây xử lý hơn cực kì nhiều so với "Não bộ Đọc Chữ" (Text Encoder Tokenization 77 ký tự chữ). Sự thắt cổ chai suy luận Đa phương thức luôn nằm ở module Nhận Diện Hình Ảnh ViT đồ sộ!

### 3.2 Tinh chỉnh Chiến Lược Huấn Luyện Học Máy (Fine-tune & Data Imbalance Handling)

**A. Đấu Trường Lựa Chọn Cấu Trúc Text Classification (Full-FineTune vs Freeze Backbone)**

| Loại Chiến lược Tinh Chỉnh | Vùng Trọng số | Thời lượng đào tạo | Lý giải Hệ quả Đánh Đổi |
| :--- | :--- | :--- | :--- |
| **Full Fine-Tuning** | 100% Cấu trúc (~66 Triệu) | Phút - Hàng Giờ | Tiêu hao VRAM nặng nhưng Model tương thích trọn vẹn đặc trưng phân phối ngữ nghĩa cục bộ khiến Accuracy đạt Đỉnh Tối Đa. |
| **Đóng Băng Xương Sống (Linear Probing)** | Dưới 1% (~600 Ngàn tham số) | Siêu Việt (1-3 phút) | Tính đạo hàm tốc độ bàn thờ giúp học cực kỳ nhanh. Hệ lụy là não bộ "Chống đối" từ chối hòa nhập văn phong Báo chí khiến Điểm số Accuracy bị tụt hạng. Rất lý tưởng cho kế hoạch Xây dựng *Rapid Prototyping (Demo Mẫu Nhanh)*. |

**B. Tuyệt Năng Giải Cứu Dữ Liệu Lệch Lớp ở Hệ Multimodal (Imbalanced Data Reweighting):**
Tập dữ liệu MM-IMDb vô cùng nghiêng lệch (Thiên kiến đa số). Việc vô tâm huấn luyện Hồi quy Few-Shot ở mức cơ sở vô tình đẩy các thể loại phim Nhỏ Lẻ (Film-Noir, Western...) rơi vào vòng xoáy bị máy tính nuốt trọn bỏ qua do mẫu bé. 
=> **Cách nhóm đột phá:** Gắn bùa chú ma thuật Cân Bằng Lớp `class_weight='balanced'` vào thuật toán học máy. Điều này đóng vai trò ép máy phải *Nhân hệ số phạt Tạ siêu nặng* mỗi khi tiên đoán lầm các Phim hiếm, buộc nó nắn nót độ chính xác rải đều cho 23 nhãn. Kết thúc, điểm **F1-Macro vụt bứt phá tăng vọt** cứu rỗi hoàn toàn tính chất Công Bằng Thuật Toán đối với những Lớp Lép Vế!

### 3.3 Phân Tích Cạm Bẫy Nhiễu Loạn Ngữ Nghĩa Nguyên Sinh (Error Analysis)
Nhóm lôi cổ những phân loại sai lầm "điển hình kinh điển" ở cả hệ Văn Bản trơn trượt lẫn Đa phương thức:
- **Cạm bẫy Không Gian Text (Cạnh Tranh Xác Suất):** Hàng loạt chứng cứ trong Notebook phơi bày thực trạng các Cụm `Tôn giáo` và `Vô thần` liên tục bị máy trộn lẫn. Lý giải cho vấn đề mấu chốt đến từ các mốc tự vựng hạt nhân (VD: `"God"`, `"Bible"`) phủ dày ở toàn cõi không gian Dữ liệu của 2 Chủ đề, ép AI (cả Transformer và LSTM) đâm sầm vào nhau vì điểm mù Xác suất Bắt Cặp từ khóa.
- **Cạm Bẫy Đa Giác Quan Multimodal (Ảnh Lừa Tổ Hợp Chữ):** Có những lúc Não Văn Bản và Não Hình Ảnh của mô hình CLIP cãi vã nhau chan chát. Các Poster Thể Loại Hài Hước (Comedy) có phong cách thiết kế Đâm Chém Troll cực ngầu (Kinh dị - Thriller). Lúc này thị giác máy tính gào thét báo kết quả Thriller dẫn tới Sự cố Chập Mạch Véc-Tơ Dung Hợp (Fused Features Noise), đánh sập cả hệ thống Few-Shot Probing. Đây chính là hệ lụy không thể tránh khỏi của Dữ liệu Nhiễu Phức Tạp Đa phương thức do Tình người đan xen vào máy! Đã có hàng loạt chứng cứ móc mộc văn bản ở Jupyter để trình báo. Mảng Multimodal bị nhiễu do chính cấu trúc mâu thuẫn sinh học!
