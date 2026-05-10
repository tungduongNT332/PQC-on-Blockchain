# Thuật toán SPHINCS+

Thư mục này chứa gói demo sử dụng thuật toán **SPHINCS+**.

## 📖 Giới thiệu
Khác với Dilithium và Falcon đều dựa trên mạng lưới (Lattice-based), SPHINCS+ là một thuật toán chữ ký điện tử hậu lượng tử hoàn toàn **dựa trên các hàm băm (Hash-based)**. Nhờ thiết kế an toàn này, SPHINCS+ cung cấp mức độ bảo mật vô cùng bảo thủ và đáng tin cậy. 

Tuy nhiên, nhược điểm chí mạng của SPHINCS+ là **kích thước chữ ký rất khổng lồ** và tốc độ ký/xác minh chậm hơn hẳn các thuật toán khác. Việc tích hợp SPHINCS+ trong dự án này nhằm mục đích chứng minh tính hiệu quả của cơ chế lưu trữ hybrid off-chain (IPFS) của hệ thống: Nó có thể giải quyết gọn gàng điểm yếu về kích thước khổng lồ của thuật toán này mà không làm phình to Blockchain.

**Thông số triển khai:**
- Thuật toán cố định: `sphincs_sha2_128f_simple`

## 🚀 Hướng dẫn chạy Demo

Từ thư mục hiện tại (`algorithms/sphincs-plus`), mở terminal và chạy các lệnh sau:

1. **Chạy giao dịch PQC Hybrid (Bảo mật toàn vẹn):**
   Ký dữ liệu bằng thuật toán SPHINCS+ và lưu trữ bằng chứng. Bạn sẽ thấy kích thước chữ ký ở log output lớn hơn rất nhiều so với Falcon hoặc Dilithium.
   ```bash
   python3 scripts/pqc_demo.py
   ```
   *📌 Lưu ý: Hãy ghi lại ID bản ghi (`record_id`) và mã CID khóa công khai (`public_key_cid`).*

2. **Xác minh giao dịch từ đầu đến cuối (End-to-End Verification):**
   Hệ thống sẽ tải chữ ký dung lượng lớn từ IPFS/Off-chain và xác minh.
   ```bash
   python3 scripts/verify_e2e.py --record-id <record_id> --public-key-cid <public_key_cid>
   ```

3. **Chạy kiểm thử tính toàn vẹn (Integrity Cases):**
   ```bash
   python3 scripts/integrity_cases.py
   ```

4. **Chạy giao dịch mã hóa bảo mật (Confidentiality Demo):**
   ```bash
   python3 scripts/confidentiality_demo.py
   ```

5. **Chạy Benchmark (Đo lường hiệu năng hệ thống):**
   ```bash
   python3 scripts/availability_benchmark.py --mode pqc_hybrid --count 5
   python3 scripts/availability_benchmark.py --mode pqc_confidential --count 5
   ```

6. **So sánh kết quả (Compare Benchmark):**
   So sánh hiệu năng tổng thể của SPHINCS+ (lưu ý đặc biệt ở yếu tố tốc độ và dung lượng) với chuẩn gốc.
   *(Lưu ý: Kết quả baseline ECDSA truyền thống sẽ tự động lấy từ thư mục `dilithium`).*
   ```bash
   python3 benchmark/compare.py
   python3 benchmark/compare_availability.py
   ```
