# PQC on BlockChain: Đánh giá và Triển khai Thuật toán Hậu Lượng Tử trên Blockchain

Dự án này là một bộ công cụ hoàn chỉnh để thử nghiệm, đánh giá và so sánh các thuật toán Mật mã Hậu Lượng Tử (Post-Quantum Cryptography - PQC) trên nền tảng Blockchain (Ethereum/EVM).

Mục tiêu chính là giải quyết vấn đề kích thước của khóa và chữ ký PQC quá lớn để có thể lưu trữ trực tiếp trên chuỗi (on-chain), thông qua một kiến trúc lưu trữ lai (hybrid storage) kết hợp lưu trữ ngoài chuỗi (off-chain, ví dụ như IPFS hoặc local storage) và xác minh trên chuỗi.

## 🌟 Tổng quan Kiến trúc

Dự án sử dụng mô hình Hybrid Proof Flow (Lưu trữ ngoài chuỗi - Xác thực toàn vẹn trên chuỗi):
1. **Người gửi (Sender):** Ký dữ liệu bằng thuật toán PQC (hoặc tạo khóa KEM đối với Kyber).
2. **Lưu trữ ngoài chuỗi (Off-chain Storage):** Khóa công khai (Public Key) và Chữ ký PQC (Signature) được lưu vào lưu trữ ngoài chuỗi (hệ thống tệp cục bộ hoặc IPFS).
3. **Lưu trữ trên chuỗi (On-chain Storage):** Người gửi tạo một giao dịch Smart Contract (sử dụng chữ ký ECDSA truyền thống) để lưu trữ mã băm (Hash) của dữ liệu, `ipfsCid` của chữ ký PQC, và thuật toán được sử dụng.
4. **Người xác minh (Verifier):** Lấy dữ liệu bản ghi từ Smart Contract, tải chữ ký và khóa công khai từ IPFS/Off-chain bằng CID, tiến hành kiểm tra tính toàn vẹn và xác minh bằng chữ ký PQC.

## 📁 Cấu trúc Dự án

Dự án được chia làm 2 thành phần chính:

- `shared/`: Chứa các thành phần dùng chung cho toàn bộ dự án.
  - `contracts/`: Smart Contract `Demo.sol` và ABI.
  - `python/`: Các file Python dùng chung như `common.py` (kết nối Web3) và `offchain_storage.py` (xử lý lưu trữ IPFS/Local).
  - `offchain_store/`: Thư mục lưu trữ các tệp tin khóa và chữ ký khi chạy ở chế độ mô phỏng local.
  
- `algorithms/`: Chứa các thuật toán PQC đã được triển khai thành các gói demo riêng biệt:
  - `dilithium/`: Thuật toán chữ ký điện tử ML-DSA (Tính cân bằng cao). Dùng làm baseline chuẩn.
  - `falcon/`: Thuật toán chữ ký điện tử Falcon (Tối ưu kích thước chữ ký).
  - `sphincs-plus/`: Thuật toán chữ ký điện tử SPHINCS+ (Bảo mật cao nhưng kích thước chữ ký lớn).
  - `kyber-kem/`: Thuật toán đóng gói khóa ML-KEM (Tập trung vào tính bí mật và trao đổi khóa).

## ⚙️ Cài đặt Môi trường

1. **Yêu cầu hệ thống:** Python 3.8+
2. **Cài đặt các thư viện cần thiết:**
   ```bash
   pip install -r shared/requirements.txt
   ```
3. **Cấu hình biến môi trường:**
   Tạo tệp `.env` tại thư mục gốc của dự án (`PQC-on-BlockChain/.env`) từ tệp mẫu:
   ```bash
   cp .env.example .env
   ```
   **Các biến quan trọng cần cấu hình trong `.env`:**
   - `RPC_URL`: Địa chỉ RPC của mạng Blockchain (VD: Sepolia RPC).
   - `PRIVATE_KEY`: Khóa bí mật ví Ethereum của bạn (Khuyến cáo: Chỉ dùng ví testnet, KHÔNG dùng ví mainnet có tài sản thật).
   - `CONTRACT_ADDRESS`: Địa chỉ Smart Contract `Demo.sol` sau khi đã deploy thành công lên mạng.
   - `IPFS_API_URL` (Tùy chọn): Nếu muốn chạy với node IPFS thật (VD: `/ip4/127.0.0.1/tcp/5001/http`).

*(Lưu ý: Mọi thư mục thuật toán sẽ tự động đọc cấu hình từ tệp `.env` dùng chung này. Nếu cần cấu hình thay thế riêng biệt, bạn có thể tạo tệp `.env` trực tiếp bên trong từng thư mục thuật toán).*

## 🚀 Cách chạy

Vui lòng chuyển hướng vào từng thư mục thuật toán bên trong `algorithms/` và đọc file `README.md` tương ứng ở trong đó. Mỗi thư mục có hướng dẫn chi tiết về các câu lệnh để chạy demo giao dịch và benchmark cho thuật toán đó.
