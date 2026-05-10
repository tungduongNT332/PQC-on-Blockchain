# Thuật toán Falcon

Thư mục này chứa gói demo sử dụng thuật toán **Falcon-512**.

## 📖 Giới thiệu
Falcon là một thuật toán chữ ký điện tử hậu lượng tử dựa trên mạng lưới (Lattice-based) kết hợp với cấu trúc cây NTRU. Điểm mạnh lớn nhất của Falcon là tạo ra **kích thước chữ ký và kích thước khóa công khai rất nhỏ**, đi kèm với tốc độ xác minh cực nhanh. Tuy nhiên, việc tạo khóa (keygen) và ký (sign) lại có phần phức tạp hơn so với Dilithium.

Trong hệ thống Blockchain, do ưu điểm nhỏ gọn, Falcon có lợi thế lớn khi truyền tải qua mạng lưới IPFS hoặc lưu trữ.

**Thông số triển khai:**
- Thuật toán cố định: `falcon_512`

## 🚀 Hướng dẫn chạy Demo

Mở terminal và trỏ vào thư mục hiện tại (`algorithms/falcon`), bạn có thể chạy tuần tự các kịch bản sau:

1. **Chạy giao dịch PQC Hybrid (Bảo mật toàn vẹn):**
   Ký dữ liệu bằng thuật toán Falcon, lưu trữ bằng chứng ngoài chuỗi (off-chain) và cập nhật Blockchain.
   ```bash
   python3 scripts/pqc_demo.py
   ```
   *📌 Lưu ý: Hãy ghi lại ID bản ghi (`record_id`) và mã CID khóa công khai (`public_key_cid`) để tiến hành xác minh ở bước sau.*

2. **Xác minh giao dịch từ đầu đến cuối (End-to-End Verification):**
   Đọc dữ liệu từ Blockchain, tự động tải chữ ký và khóa công khai Falcon từ hệ thống off-chain, tiến hành xác minh tính toàn vẹn.
   ```bash
   python3 scripts/verify_e2e.py --record-id <record_id> --public-key-cid <public_key_cid>
   ```

3. **Chạy kiểm thử tính toàn vẹn (Integrity Cases):**
   Kiểm tra tính an toàn của chữ ký Falcon bằng cách mô phỏng các cuộc tấn công thay đổi nội dung dữ liệu hoặc chữ ký.
   ```bash
   python3 scripts/integrity_cases.py
   ```

4. **Chạy giao dịch mã hóa bảo mật (Confidentiality Demo):**
   Kiểm thử với dữ liệu bị mã hóa bảo mật hoàn toàn trước khi lưu lên on-chain.
   ```bash
   python3 scripts/confidentiality_demo.py
   ```

5. **Chạy Benchmark (Đo lường hiệu năng hệ thống):**
   Đánh giá độ trễ và hiệu suất của hệ thống chạy thuật toán Falcon khi thực hiện liên tiếp nhiều giao dịch.
   ```bash
   python3 scripts/availability_benchmark.py --mode pqc_hybrid --count 5
   python3 scripts/availability_benchmark.py --mode pqc_confidential --count 5
   ```

6. **So sánh kết quả (Compare Benchmark):**
   Tạo báo cáo so sánh Falcon với chuẩn truyền thống. 
   *(Lưu ý: Kết quả baseline ECDSA truyền thống sẽ tự động được lấy từ kết quả benchmark bên thư mục `dilithium`).*
   ```bash
   python3 benchmark/compare.py
   python3 benchmark/compare_availability.py
   ```
