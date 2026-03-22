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
**Sức Rướn Phần Cứng Của Text Classification & Kỹ thuật Nén Mô hình (Compression):**

![Biểu đồ so sánh LSTM vs DistilBERT vs TinyBERT](Bieu_Do_Tradeoff.png) <!-- Ghi chú: Chèn ảnh biểu đồ plot vào đây -->

*Nhận xét biểu đồ (Discussion):*
1. **Trục Accuracy (Độ chính xác)** cho thấy sự tiến bộ rõ rệt của cơ chế Attention khi các họ Transformer (DistilBERT/TinyBERT) đều đè bẹp LSTM.
2. Tuy nhiên, đánh đổi lại, **trục Parameter (Dung lượng) và trục Thời gian (Time)** phơi bày sự cồng kềnh khủng khiếp của mạng DistilBERT. Nó nặng gấp hơn 20 lần và chạy chậm hơn gần 20 lần so với LSTM, ngốn cực kỳ nhiều tài nguyên RAM của hệ thống.
3. Sự xuất hiện của **TinyBERT (Siêu nén)** đã tạo ra tính đột phá về Hiệu Quả Mô Hình (Efficiency): Nó hạ số lượng tham số xuống 4.38 Triệu (Gần bằng LSTM), đem tốc độ Inference chớp nhoáng quay trở lại (chỉ ~6.4s), nhưng vẫn giữ được mức Accuracy cực tốt nhờ kế thừa toàn vẹn tinh hoa của kiến trúc mạng Transformer! Đây chính là cốt lõi của việc nén mô hình (Knowledge Distillation) để Deploy.

**Bấm giờ Sự Bất Cân Bằng Tốc Độ Của Mô Hinh Multimodal (CLIP):** Thực tiễn bấm giờ đếm ngược cho thấy cơ chế "Mắt thần Nhìn Ảnh" (Image Encoder quét 224x224 điểm ảnh màu) tốn nhiều giây xử lý hơn cực kì nhiều so với "Não bộ Đọc Chữ" (Text Encoder Tokenization 77 ký tự chữ). Sự thắt cổ chai suy luận Đa phương thức luôn nằm ở module Nhận Diện Hình Ảnh ViT đồ sộ!

### 3.2 Tinh chỉnh Chiến Lược Huấn Luyện Học Máy (Fine-tune & Data Imbalance Handling)

**A. Đấu Trường Lựa Chọn Cấu Trúc Text Classification (Full-FineTune vs Freeze Backbone)**

Để đảm bảo tính công bằng trong thử nghiệm (Fair Comparison), nhóm tiến hành huấn luyện hai chiến lược trên cùng một kiến trúc DistilBERT và cùng lặp chính xác `3 Epochs`. Kết quả đem lại góc nhìn rất sâu sắc về sự đánh đổi (Trade-off):

| Tiêu chí | 1. Fine-tune Toàn bộ (Unfreeze) | 2. Đóng băng Xương sống (Freeze Backbone) |
| :--- | :--- | :--- |
| **Cơ chế Huấn luyện** | Rã đông toàn bộ 6 lớp Transformer. Cập nhật mọi ma trận Attention từ đầu đến chân. | Khóa chết thân mạng (Backbone). Chỉ cho phép cập nhật duy nhất lớp phân loại (Classifier) cuối cùng. |
| **Số tham số Update** | Toàn bộ **~67,000,000** tham số phải được backpropagate. | Trượt dốc thảm hại chỉ còn **605,972** tham số kích hoạt. (Giảm hơn 100 lần hệ số tính toán!) |
| **Thời gian Train (3 Epochs)** | **Dài nhất** (Ước tính gấp nhiều lần). | Chỉ tốn **550 giây** (~9 phút). Siêu tốc vì Gradient không truyền ngược qua 6 Lớp Transformer. |
| **Độ chính xác (Accuracy)**| **Chạm Đỉnh Nhất: 0.6980** | **Kẹt Ở Ngưỡng Cụt: 0.6058** |

**Sự kiện "Bức tường kính" (Plateau Effect):** Dù lớp Classifier ở Freeze Backbone đã có hệ số Loss giảm rất đẹp qua từng vòng (`57%` -> `58%` -> `60%`), nó vẫn bị kẹt cứng ở ngưỡng trần **0.6058**, cách một khoảng rất xa so với mức **0.6980** của Fine-tune toàn bộ.
**Đánh giá:** Bản chất cốt tủy của Wikipedia (dữ liệu pretrained) "khớp lệch" hoàn toàn với văn phong tán gẫu lóng của diễn đàn 20 Newsgroups. Khi "khóa xích" Backbone lại, ta tước đoạt đi khả năng "uốn nắn" ngữ cảnh khiến Classifier "lực bất tòng tâm" không thể vươn xa hơn được. Thậm chí `0.6058` chỉ ngang bằng kỷ lục ban đầu `0.6152` của mạng LSTM cạn truyền thống!

**B. Tuyệt Năng Giải Cứu Dữ Liệu Lệch Lớp ở Hệ Multimodal (Imbalanced Data Reweighting):**
Tập dữ liệu MM-IMDb vô cùng nghiêng lệch (Thiên kiến đa số). Việc vô tâm huấn luyện Hồi quy Few-Shot ở mức cơ sở vô tình đẩy các thể loại phim Nhỏ Lẻ (Film-Noir, Western...) rơi vào vòng xoáy bị máy tính nuốt trọn bỏ qua do mẫu bé. 
=> **Cách nhóm đột phá:** Gắn bùa chú ma thuật Cân Bằng Lớp `class_weight='balanced'` vào thuật toán học máy. Điều này đóng vai trò ép máy phải *Nhân hệ số phạt Tạ siêu nặng* mỗi khi tiên đoán lầm các Phim hiếm, buộc nó nắn nót độ chính xác rải đều cho 23 nhãn. Kết thúc, điểm **F1-Macro vụt bứt phá tăng vọt** cứu rỗi hoàn toàn tính chất Công Bằng Thuật Toán đối với những Lớp Lép Vế!

### 3.3 Phân Tích Cạm Bẫy Nhiễu Loạn Ngữ Nghĩa Nguyên Sinh (Error Analysis)
Nhóm lôi cổ những phân loại sai lầm "điển hình kinh điển" ở cả hệ Văn Bản trơn trượt lẫn Đa phương thức:
- **Cạm bẫy Không Gian Text (Cạnh Tranh Xác Suất Nhóm Phần Cứng/Hardware):** Điển hình nhất là nhóm `comp.os.ms-windows.misc` và `comp.sys.ibm.pc.hardware` có F1-score cực kỳ thấp (LSTM: 0.54, DistilBERT: 0.64) do dùng chung một bộ từ vựng dày đặc (*RAM, drive, disk, CPU*).
    - **Trường hợp The Generic Text:** Khi gặp văn bản đánh giá chung chung *"like them, good response on service, buying one"*, do thiếu hụt Context vựng chuyên ngành, LSTM bị nhiễu đoán thành `rec.autos` (do từ service), DistilBERT đoán thành `misc.forsale` (do buying one). Cả 2 đều đoán sai nhãn PC Hardware gốc. 
    - **Trường hợp The Niche Jargon:** Đoản văn quá ngắn nhưng chứa thuật ngữ ngách *"wait states"*. LSTM mất phương hướng đoán thành `baseball`. DistilBERT đoán nhầm sang `sci.crypt` vì lầm tưởng thuật toán.
    - **Nhưng sự vượt trội của Transfomer (Sự nhầm lẫn từ viết tắt đa nghĩa):** LSTM thấy từ *"models", "LE SE LSE"* liền vội xếp nhãn phần cứng Mac (`mac.hardware` - Macintosh SE). DistilBERT nhờ được Pre-train Wikipedia đã nhận diện cụm *"88-89 bonnevilles"* là siêu xế Pontiac Bonneville nên phân lớp chính xác cực độ vào nhóm ô tô `rec.autos`!

- **Cạm Bẫy Đa Giác Quan Multimodal (Ảnh Lừa Tổ Hợp Chữ):** Có những lúc Não Văn Bản và Não Hình Ảnh của mô hình CLIP cãi vã nhau chan chát. Các Poster Thể Loại Hài Hước (Comedy) có phong cách thiết kế Đâm Chém Troll cực ngầu (Kinh dị - Thriller). Lúc này thị giác máy tính gào thét báo kết quả Thriller dẫn tới Sự cố Chập Mạch Véc-Tơ Dung Hợp (Fused Features Noise), đánh sập cả hệ thống Few-Shot Probing. Mảng Multimodal bị nhiễu do chính cấu trúc mâu thuẫn sinh học!
