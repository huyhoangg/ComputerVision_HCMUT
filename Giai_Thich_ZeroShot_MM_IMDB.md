# GIẢI THÍCH CHI TIẾT TỪNG TRƯỜNG ĐOẠN (CELL) TRONG FILE `ZeroShot_MM_IMDB.ipynb`

Tài liệu này được biên soạn để giúp sinh viên dễ dàng ôn tập, quay video báo cáo và tự tin trả lời vấn đáp liên quan đến kịch bản Phân loại Đa phương thức (Multimodal Classification) sử dụng mô hình OpenAI CLIP.

---

## PHẦN A: KHỞI TẠO, TẢI DỮ LIỆU & PHÂN TÍCH KHÁM PHÁ (EDA)

**1. Các Cell đầu tiên (Cài đặt & Thư viện):**
*   **Mục đích:** Cài đặt mô hình `clip`, thư viện `kagglehub` để kéo dữ liệu, thư viện vẽ biểu đồ `seaborn`, `matplotlib` và các công cụ toán học cơ bản. 
*   **Giải thích:** Việc sử dụng một mô hình nền tảng (foundation model) cực lớn yêu cầu môi trường PyTorch mạnh và một số thư viện lọc nhiễu văn bản (ftfy, regex).

**2. Cell tải Dữ liệu (Kagglehub):**
*   **Mục đích:** Thực hiện lệnh `kagglehub.dataset_download("javierurea/simplified-mm-imdb")`.
*   **Giải thích:** Tải trực tiếp kho dữ liệu cực lớn ~25.000 mẫu của bộ MM-IMDB đã được tinh gọn (simplified). File gốc chứa Posters (ảnh bìa phim) và Plot (Tóm tắt kịch bản chữ).

**3. Các Cell xử lý nhãn (Labels) và EDA (Exploratory Data Analysis):**
*   **Bar Chart phân phối thể loại:** Duyệt qua 25k mẫu, đếm bằng thuật toán mảng xem có bao nhiêu bộ phim thuộc thể loại Drama, Comedy, Action... Thể hiện rõ tính chất Mất cân bằng lớp (Imbalanced Data) — vốn là lý do tại sao cuối bài mình phải dùng điểm F1 thay vì chỉ Accuracy.
*   **Histogram Độ dài Plot:** Đếm số lượng từ vựng của đoạn tóm tắt mỗi phim.
*   **Seaborn Heatmap:** Vẽ Ma trận Tương quan (Correlation Matrix) để tìm ra các thể loại phim thường xuyên đi chung với nhau (Ví dụ hay gặp: Action hay đi kèm với Sci-Fi & Adventure; Romance thường đi kèm với Drama hoặc Comedy). Bài toán Đa nhãn (Multi-label) rất quan tâm đến sự đồng xuất hiện này.

---

## PHẦN B: CHUẨN BỊ DỮ LIỆU (DATASET & DATALOADER)

**4. Khởi tạo Dataset class `MM_IMDB_Dataset`:**
*   **Cơ chế:** Kế thừa lớp Dataset của PyTorch. Khi truyền 1 Index vào, nó sẽ làm 3 việc: 1. Đọc Ảnh từ ổ cứng -> 2. Đọc Chữ (Tóm tắt) -> 3. Lấy Nhãn (Label) chứa mảng 23 chuẩn [0,1,0,0...].

**5. Định nghĩa Kiến trúc Biến đổi (Transforms) cho CLIP:**
*   **Cơ chế:** `clip_transform` thực hiện Resize ảnh về (224x224), Crop phần trung tâm, chuyển thành Tensor và Chuẩn hóa màu sắc (Normalize). Đây là định dạng bắt buộc mà mạng ViT bên trong CLIP yêu cầu do lúc Pretrain nó đã học định dạng này.

**6. Chia Tập Test (Validation) chứa 20% dữ liệu:**
*   **Hàm `Subset` & `np.random.choice`:** Do dữ liệu đồ sộ nên ta bốc ngẫu nhiên ra 1 phần 5 (khoảng hơn 5000 mẫu) làm tập đánh giá độ chính xác (Test set). Gắn nó vào một cái `DataLoader` để chạy lô (batch_size=32) ra kết quả cho lẹ.

---

## PHẦN C: KHỞI TẠO MÔ HÌNH VÀ PHƯƠNG PHÁP ZERO-SHOT LATE FUSION

**7. Tải Mô hình CLIP ViT-B/32:**
*   **Lệnh `clip.load("ViT-B/32", device=device)`:** Cầu nối kéo não bộ pre-trained cực khủng của OpenAI về RAM. "ViT-B/32" nghĩa là dùng mạng **V**ision **T**ransformer cơ bản, phân ảnh thành các khối vuông (patch) **32x32** pixel. Đặt `model.eval()` để đóng băng trọng số (không cho học bậy, vì đây là Zero-shot).

**8. Text Prompting (Cấu hình nhãn thành câu tự nhiên):**
*   **Chuẩn bị Vector Nhãn:** Sinh ra các câu theo form mẫu `"a movie poster of a {genre} movie"` (Ví dụ: "a movie poster of a Comedy movie"). Dùng CLIP Tokenizer dịch thành Số rồi đưa vào `model.encode_text()` để tạo thành **Ma trận Đặc trưng Nhãn (Label Features)**. Khi hoạt động, cái nào cosin gần nhất với ma trận này thì phim mang thể loại đó.

**9. Khối Evaluate chạy Zero-Shot (Vòng lặp qua Test Loader):**
*   Đây chính là lõi chạy toàn bài của Zero-shot:
    1.  Mỗi batch 32 Hình ảnh -> Đẩy qua `encode_image` -> Chuẩn hoá Vector ra `img_feat`.
    2.  Mỗi batch 32 Cốt truyện -> Đẩy qua `encode_text` -> Chuẩn hoá Vector ra `txt_feat`.
    3.  **LATE FUSION (Dung hợp sau):** Trộn đặc trưng Ảnh và Cốt truyện lại thông qua công thức cộng tuyến tính `fused_feat = alpha * img_feat + (1 - alpha) * txt_feat`. Cái `alpha` quy định xem Hình hay Chữ quan trọng hơn.
    4.  Nhân ma trận `fused_feat` @ `label_features.T` để đo độ trượt Tương đồng Cosine (Cosine Similarity).
    5.  Vì là Multi-label, dùng kẹp hàm Sigmoid để kéo giá trị về đoạn tĩnh [0,1], sau đó ấn định ngưỡng (Threshold 0.5) để quyết định Nhãn nào là số 1, Nhãn nào là số 0.
    6.  Trộn tất cả dự đoán lại và tính ra Micro/Macro F1-Score cuối cùng. Thế là xong Zero-shot!

---

## PHẦN D: FEW-SHOT LEARNING & BIỂU ĐỒ BÁO CÁO (Đã thêm vào)

Phần Zero-shot tuy hay nhưng không có khả năng nắn nót độ lệch của dữ liệu phân phối cục bộ MM-IMDB. Lúc này, Few-shot sinh ra để áp giải quyết triệt để thông qua kỹ thuật gỡ băng gọi là "Linear Probing" (Dò sóng tuyến tính).

**10. Khối Support Set (Tổ chức dữ liệu học Few-shot):**
*   **Cơ chế:** Lọc phần bù của Tập Test 5000 mẫu ở trên để moi ra tập Train hoàn toàn xa lạ. Bốc đúng ngẫu nhiên 200 mẫu nhồi vào `train_loader`. Bốc ÍT MẪU thì mới gọi là FEW-Shot! (Phần văn bản hay ảnh trên yêu cầu bạn dùng vài nghìn mẫu để train, nhưng Zero/Few-shot sinh ra để lật đổ định luật đó).

**11. Khối Tính Đặc trưng Train và Test (Feature Extraction):**
*   Thay vì suy ra luôn lớp Sigmoid Cosine Similarity như trên, ta lấy thẳng tưng cái vector `fused_feat` của Mẫu Đa phương thức (Ảnh lai Chữ) xuất trực tiếp từ cỗ máy CLIP. Đây gọi là "Trích xuất đặc trưng" (Feature Extractor). Trích rút toàn bộ cho cả 200 mẫu Lớp Train và 5000 mẫu Lớp Test.

**12. Khối Logistic Regression đa nhãn (OneVsRest):**
*   **Cơ chế:** Vì CLIP đang đóng băng, ta dùng ngay Thư viện học máy cổ điển (`sklearn`) là **Mô hình Hồi quy Logistic**. Đưa 200 mẫu đặc trưng đa phương thức (Train Vector) mới rút vào hàm `.fit()` học chớp nhoáng (chưa tới 1 giây).
*   Test ngay và luôn cục Hồi quy này trên 5000 mẫu Test Vector ra xác suất rồi quét thử nghiệm Ngưỡng Threshold giống như Zero-shot để kẹp F1 lên đỉnh tối đa.

**13. Cell cuối cùng vẽ Chart Tổng Kết:**
*   Hứng kết quả cực đại của Zero-shot F1 và nhặt kết quả của Few-shot F1 từ bên trên. Ném vào `matplotlib.pyplot` thiết kế hệ trục tọa độ cột kép hiển thị thông số. (So sánh Few-shot sẽ chiến thắng vẻ vang). XONG! 

---
*Lưu ý: Để tránh hiện tượng Out of Memory (OOM) nếu máy tính yếu (laptop), các batch size đều duy trì ở mức 32 ở mọi Loader trên.*
