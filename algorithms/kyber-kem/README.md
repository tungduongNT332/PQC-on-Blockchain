# Thuật toán Kyber (ML-KEM)

Thư mục này là một gói demo sử dụng **Kyber (ML-KEM-512)**.

## 📖 Giới thiệu
Không giống như Dilithium, Falcon hay SPHINCS+ đều là các thuật toán chữ ký điện tử (dùng để xác thực tính toàn vẹn), Kyber là một **Cơ chế Đóng gói Khóa (Key Encapsulation Mechanism - KEM)**. 

Mục đích chính của Kyber là **bảo mật tính bí mật của dữ liệu (Confidentiality)** bằng cách chia sẻ một khóa đối xứng bí mật giữa các bên một cách an toàn thông qua một môi trường không an toàn (có nguy cơ bị tấn công bởi máy tính lượng tử).

Trong kịch bản này, Kyber được dùng để đóng gói (encapsulate) một khóa bí mật chung. Khóa bí mật chung này sau đó được dùng để sinh khóa AES nhằm mã hóa toàn bộ dữ liệu (payload) trước khi lưu lên Blockchain, đảm bảo không ai đọc được dữ liệu nếu không có khóa bí mật Kyber.

**Thông số triển khai:**
- Thuật toán cố định: `ml_kem_512`

## 🚀 Hướng dẫn chạy Demo

Vì Kyber được thiết kế chuyên biệt cho trao đổi khóa bảo mật (chứ không phải chữ ký), các kịch bản demo sẽ tập trung duy nhất vào luồng **KEM Confidentiality**.

Từ thư mục hiện tại (`algorithms/kyber-kem`), mở terminal và chạy các lệnh:

1. **Chạy giao dịch ML-KEM Confidentiality:**
   Hệ thống sẽ thực hiện sinh khóa Kyber, đóng gói khóa bí mật, sử dụng khóa chung đó làm khóa AES để mã hóa dữ liệu. Cuối cùng, dữ liệu đã bị mã hóa và bằng chứng mã hóa sẽ được lưu lên Smart Contract.
   ```bash
   python3 scripts/kem_confidentiality_demo.py
   ```

2. **Chạy Benchmark (Đo lường hiệu năng hệ thống):**
   Đánh giá thời gian tạo khóa (keygen), đóng gói khóa (encapsulate) và mở khóa (decapsulate) của Kyber, cũng như thời gian xử lý toàn bộ giao dịch.
   ```bash
   python3 scripts/availability_kem_benchmark.py --mode kyber_kem_confidential --count 5
   ```

3. **So sánh kết quả (Compare Benchmark):**
   Tạo báo cáo so sánh kết quả.
   *(Lưu ý: Các bài so sánh với giao dịch truyền thống sẽ tự động tái sử dụng kết quả baseline từ thư mục `dilithium/benchmark/results`).*
   ```bash
   python3 benchmark/compare_kem.py
   python3 benchmark/compare_availability_kem.py
   ```
