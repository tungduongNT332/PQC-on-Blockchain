# Thuật toán Dilithium (ML-DSA)

Thư mục này chứa gói demo sử dụng thuật toán **Dilithium (ML-DSA-44)**.

## 📖 Giới thiệu
Dilithium là một thuật toán chữ ký điện tử hậu lượng tử dựa trên mạng lưới (Lattice-based). Thuật toán này được đánh giá rất cao về sự cân bằng giữa tốc độ ký/xác minh, kích thước khóa và kích thước chữ ký. Trong dự án này, thuật toán Dilithium được sử dụng làm **chuẩn gốc (baseline)** để so sánh hiệu năng với các thuật toán khác.

**Thông số triển khai:**
- Thuật toán cố định: `ml_dsa_44`

## 🚀 Hướng dẫn chạy Demo

Mở terminal và trỏ vào thư mục hiện tại (`algorithms/dilithium`), bạn có thể chạy tuần tự các kịch bản sau:

1. **Chạy giao dịch ECDSA Truyền thống (Traditional):**
   Thực hiện một giao dịch mẫu chỉ dùng hệ mật truyền thống (không dùng PQC) để làm điểm chuẩn đối chiếu.
   ```bash
   python3 scripts/traditional_demo.py
   ```

2. **Chạy giao dịch PQC Hybrid (Bảo mật toàn vẹn):**
   Ký dữ liệu bằng thuật toán Dilithium, lưu chữ ký/khóa vào lưu trữ ngoài chuỗi (off-chain), và lưu thông tin bằng chứng (hash, CID) lên Smart Contract.
   ```bash
   python3 scripts/pqc_demo.py
   ```
   *📌 Lưu ý: Hãy ghi lại ID bản ghi (`record_id`) và mã CID khóa công khai (`public_key_cid`) ở output màn hình để chạy bước xác minh phía dưới.*

3. **Xác minh giao dịch từ đầu đến cuối (End-to-End Verification):**
   Đóng vai trò là một người xác minh: Đọc dữ liệu từ Blockchain, tải chữ ký từ hệ thống off-chain và tiến hành xác minh PQC.
   ```bash
   python3 scripts/verify_e2e.py --record-id <record_id> --public-key-cid <public_key_cid>
   ```
   *Thay `<record_id>` và `<public_key_cid>` bằng giá trị bạn vừa nhận được ở bước 2.*

4. **Chạy kiểm thử tính toàn vẹn (Integrity Cases):**
   Kiểm tra khả năng phát hiện của hệ thống khi dữ liệu hoặc chữ ký bị kẻ gian cố ý giả mạo.
   ```bash
   python3 scripts/integrity_cases.py
   ```

5. **Chạy giao dịch kết hợp mã hóa bảo mật (Confidentiality Demo):**
   Dữ liệu được mã hóa cục bộ bằng khóa AES trước khi đưa lên Blockchain, đảm bảo tính bảo mật của nội dung.
   ```bash
   python3 scripts/confidentiality_demo.py
   ```

6. **Chạy Benchmark (Đo lường hiệu năng hệ thống):**
   Chạy nhiều giao dịch liên tiếp (mặc định 5 giao dịch) để đánh giá tốc độ, độ trễ và sự ổn định của cả 3 mô hình.
   ```bash
   python3 scripts/availability_benchmark.py --mode traditional --count 5
   python3 scripts/availability_benchmark.py --mode pqc_hybrid --count 5
   python3 scripts/availability_benchmark.py --mode pqc_confidential --count 5
   ```

7. **So sánh kết quả (Compare Benchmark):**
   Tổng hợp và so sánh kết quả benchmark giữa các phương pháp. Kết quả phân tích sẽ được in ra.
   ```bash
   python3 benchmark/compare.py
   python3 benchmark/compare_availability.py
   ```

---
*Ghi chú: Mặc định kịch bản sẽ lưu file tại thư mục `../../shared/offchain_store/`. Nếu biến `IPFS_API_URL` được cấu hình trong `.env`, hệ thống sẽ sử dụng một node IPFS thật thay vì lưu trữ cục bộ.*
