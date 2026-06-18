# Báo cáo Lab 16: Cloud AI Environment Setup (Phương án CPU)

## 7.6 Kết quả Benchmark trên r5.2xlarge (Triển khai thực tế trên t3.micro)

| Metric | Kết quả |
|---|---|
| Thời gian load data | 2.6597 giây |
| Thời gian training | 4.3756 giây |
| Best iteration | 0 |
| AUC-ROC | 0.7941 |
| Accuracy | 0.9976 |
| F1-Score | 0.4585 |
| Precision | 0.3742 |
| Recall | 0.5918 |
| Inference latency (1 row) | 0.001668 giây |
| Inference throughput (1000 rows) | 0.004341 giây |

## Báo cáo Đánh giá

**1. Đánh giá thời gian Đào tạo (Training) và Suy luận (Inference):**
- Mặc dù bị giới hạn dùng máy chủ cấu hình thấp (`t3.micro`) để lọt qua chính sách Free Tier, thuật toán LightGBM vẫn chứng minh tốc độ xử lý vượt trội. Thời gian load tập dữ liệu lớn (~284,000 dòng) mất 2.66s, trong khi thời gian huấn luyện 100 vòng lặp (training) diễn ra rất nhanh, chỉ trong 4.38s.
- Tốc độ suy luận (Inference) cực kỳ ấn tượng: Dự đoán 1 mẫu (latency) tốn chưa tới 2 mili-giây (0.0016s), và khả năng xử lý 1000 mẫu cùng lúc chỉ mất vỏn vẹn 4.3 mili-giây (0.0043s). Chỉ số AUC-ROC đạt khoảng 0.7941 chứng minh mô hình hoạt động hiệu quả ổn định.

**2. Lý do sử dụng CPU thay cho GPU:**
- Với các tài khoản AWS mới hoặc đang dùng Free Tier, AWS thường khóa hạn mức sử dụng (Quota) vCPU của các dòng máy chủ GPU (g4dn) ở mức 0 để bảo mật. Việc yêu cầu AWS mở khóa thường tốn nhiều giờ hoặc bị từ chối.
- Ngoài ra, ngay cả dòng CPU cao cấp `r5.2xlarge` cũng không khả dụng trong Free Tier. Việc linh hoạt hạ cấu hình xuống `t3.micro` và đổi sang thuật toán Machine Learning truyền thống (LightGBM) thay vì Deep Learning (LLM) là phương án thay thế tối ưu nhất. Điều này giúp hoàn thành mục tiêu cốt lõi của Lab là thiết lập thành công IaC (Terraform), triển khai mạng VPC bảo mật, và đưa thuật toán AI lên hạ tầng thật một cách trơn tru mà không bị chặn bởi chính sách giá.
