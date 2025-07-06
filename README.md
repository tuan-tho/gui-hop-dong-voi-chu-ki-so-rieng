![image](https://github.com/user-attachments/assets/abf5fc26-76fb-41c3-be75-55f3cb096f12)🚀 Hệ Thống Truyền File Hợp Đồng An Toàn


**✨ Giới thiệu dự án**


Đây là dự án bài tập lớn môn An Toàn Bảo Mật Thông Tin, tập trung xây dựng một hệ thống truyền tải file hợp đồng an toàn giữa Người gửi (Sender) và Người nhận (Receiver).

Dự án hiện thực hóa Đề tài 3: **Gửi hợp đồng với chữ ký số riêng** đảm bảo các yếu tố cốt lõi:

🔒 Bảo mật (Confidentiality): Giữ bí mật tuyệt đối nội dung file.

✅ Toàn vẹn (Integrity): Đảm bảo file không bị thay đổi trên đường truyền.

🤝 Xác thực (Authentication): Xác nhận file đến từ đúng người gửi.

**Hệ thống của chúng tôi cho phép bạn**

🔑 Tạo và quản lý các cặp khóa RSA một cách dễ dàng.

🔗 Thiết lập kết nối an toàn giữa Người gửi và Người nhận.

✂️ Chia nhỏ file hợp đồng, sao đó mã hóa và ký số riêng từng phần trước khi gửi đi.

🔍 Người nhận có thể xác minh tính toàn vẹn và xác thực của từng phần file, giải mã và tái tạo file gốc.

🛠️ Cung cấp công cụ xác minh offline tiện lợi để kiểm tra chữ ký số độc lập.

**🏗️ Cấu trúc dự án**


Dự án được tổ chức gọn gàng và logic với các thành phần chính:

**receiver_app.py:** Ứng dụng Người nhận (Receiver).

🌐 Giao diện web và logic xử lý nhận, giải mã, xác minh file.

🔑 Tích hợp chức năng tạo khóa RSA cho người nhận.

**sender_app.py:** Ứng dụng Người gửi (Sender).

🌐 Giao diện web và logic xử lý kết nối, mã hóa, ký số và gửi file.

🔑 Tích hợp chức năng tạo khóa RSA cho người gửi.

**utils.py:** Module chứa các hàm tiện ích mật mã và mạng dùng chung.

**verify_tool.py:** Công cụ Python độc lập (offline) để xác minh chữ ký số.

**keys/:** Thư mục lưu trữ các cặp khóa RSA (.pem).

**received_files/:** Nơi lưu trữ các file đã nhận và giải mã thành công.

**templates/:** Chứa các tệp mẫu HTML cho giao diện web.

**contract.txt:** Tệp dữ liệu mẫu dùng để thử nghiệm.

**myenv/ (hoặc .venv/):** Môi trường ảo Python của dự án.

**🛠️ Kỹ thuật và Thuật toán sử dụng**


Hệ thống được xây dựng bằng Python 3.13.5 và áp dụng các công nghệ, thuật toán mật mã tiên tiến:

**PyCryptodome:** Thư viện mật mã chủ chốt:

**Triple DES (3DES):** Thuật toán mã hóa đối xứng, dùng để mã hóa nội dung file.

Sử dụng chế độ CBC (Cipher Block Chaining) với IV ngẫu nhiên để tăng cường bảo mật và che giấu mẫu dữ liệu.

**RSA 2048-bit:** Thuật toán mã hóa bất đối xứng.

Trao đổi khóa phiên an toàn: Mã hóa khóa phiên 3DES bằng khóa công khai của người nhận.

Tạo và xác minh chữ ký số: Ký metadata và từng phần file bằng khóa riêng tư của người gửi.

Sử dụng chế độ đệm PKCS#1 v1.5 để đảm bảo an toàn.

**SHA-512:** Hàm băm mật mã mạnh mẽ, dùng để:

Kiểm tra tính toàn vẹn: Tạo giá trị băm của (IV || ciphertext) của từng phần file.

Đầu vào cho chữ ký số: Giá trị băm này được ký bằng RSA.

**Chữ ký số (Digital Signature):** Sự kết hợp giữa RSA và SHA-512, đảm bảo tính xác thực và không chối bỏ.

**Flask:** Micro-framework Python, tạo giao diện người dùng dạng web thân thiện.

**Socket:** Thư viện chuẩn Python, thiết lập kết nối mạng TCP giữa hai bên.

**Các thư viện Python tiêu chuẩn khác:** json, base64, os, datetime, threading, math hỗ trợ xử lý dữ liệu, quản lý file, thời gian và đa luồng.

**🖥️ Giao diện ứng dụng**
Hệ thống cung cấp hai giao diện web trực quan cho SenderApp và ReceiverApp, dễ dàng truy cập qua trình duyệt.

Giao diện SenderApp
Tiêu đề: "ỨNG DỤNG NGƯỜI GỬI HỢP ĐỒNG"

Hãy chèn ảnh chụp màn hình giao diện SenderApp tại đây.

Các thành phần chính:

🔑 Quản Lý Khóa RSA: Tạo và tải khóa RSA (riêng tư của người gửi, công khai của người nhận).

🔗 Kết Nối Với Người Nhận: Nhập địa chỉ và cổng để thiết lập kết nối.

✉️ Gửi Hợp Đồng: Chọn file và bắt đầu quá trình mã hóa, ký số, gửi.

📜 Nhật Ký Hoạt Động: Hiển thị chi tiết quá trình, bao gồm Hash (Base64) và Chữ ký (Base64) của từng phần file.

Giao diện ReceiverApp
Tiêu đề: "ỨNG DỤNG NGƯỜI NHẬN HỢP ĐỒNG"

Hãy chèn ảnh chụp màn hình giao diện ReceiverApp tại đây.

Các thành phần chính:

🔑 Quản Lý Khóa RSA: Tạo và tải khóa RSA (riêng tư của người nhận, công khai của người gửi).

▶️ Khởi Động Server: Nhập cổng và điều khiển việc bắt đầu/dừng máy chủ nhận.

📜 Nhật Ký Hoạt Động: Hiển thị chi tiết quá trình nhận, xác minh, giải mã file.

Công cụ xác minh Offline (verify_tool.py)
Hãy chèn ảnh chụp màn hình giao diện Verify Tool tại đây (nếu có).

Công cụ dòng lệnh này cho phép xác minh tính toàn vẹn và xác thực của một phần file đã nhận một cách độc lập, bằng cách cung cấp khóa công khai của người gửi, Hash (Base64) và Chữ ký (Base64) từ log.

🚀 **Hướng dẫn cài đặt và chạy**
**Clone repository**

git clone <địa chỉ repository của bạn>
cd <tên thư mục dự án>


**Tạo và kích hoạt môi trường ảo**

python -m venv myenv
.\myenv\Scripts\activate # Trên Windows PowerShell # source myenv/bin/activate # Trên Linux/macOS


**Cài đặt các thư viện cần thiết**

pip install Flask pycryptodome


**Chạy ứng dụng Người nhận (ReceiverApp)**
Mở một cửa sổ terminal/PowerShell mới, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python receiver_app.py


Truy cập http://127.0.0.1:5001 trong trình duyệt.

**Chạy ứng dụng Người gửi (SenderApp)**
Mở một cửa sổ terminal/PowerShell khác, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python sender_app.py


Truy cập http://127.0.0.1:5000 trong trình duyệt.

**Chạy công cụ xác minh Offline (Verify Tool)**
Mở một cửa sổ terminal/PowerShell khác, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python verify_tool.py
