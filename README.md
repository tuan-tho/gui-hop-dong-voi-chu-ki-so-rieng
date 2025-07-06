Hệ Thống Truyền File Hợp Đồng An Toàn (Đề tài 3)
Giới thiệu
Đây là dự án bài tập lớn môn An Toàn Bảo Mật Thông Tin, tập trung vào việc xây dựng một hệ thống truyền tải file hợp đồng an toàn giữa hai bên: Người gửi (Sender) và Người nhận (Receiver). Dự án hiện thực hóa Đề tài 3: "Gửi hợp đồng với chữ ký số riêng", đảm bảo tính bảo mật, toàn vẹn và xác thực của dữ liệu thông qua việc áp dụng các thuật toán mật mã hiện đại.

Hệ thống cho phép người dùng:

Tạo và quản lý các cặp khóa RSA.

Thiết lập kết nối an toàn giữa Người gửi và Người nhận.

Chia file hợp đồng thành nhiều phần, mã hóa và ký số riêng từng phần trước khi gửi.

Người nhận có khả năng xác minh tính toàn vẹn và xác thực của mỗi phần file, sau đó giải mã và tái tạo lại file gốc.

Cung cấp công cụ xác minh offline để kiểm tra chữ ký số độc lập.

Cấu trúc dự án
Dự án được tổ chức một cách rõ ràng với các thành phần chính sau:

receiver_app.py: Ứng dụng phía Người nhận (Receiver), bao gồm giao diện web và logic xử lý việc nhận, giải mã, xác minh file. Tích hợp chức năng tạo khóa RSA cho người nhận.

sender_app.py: Ứng dụng phía Người gửi (Sender), bao gồm giao diện web và logic xử lý việc kết nối, mã hóa, ký số và gửi file. Tích hợp chức năng tạo khóa RSA cho người gửi.

utils.py: Module chứa các hàm tiện ích mật mã và mạng dùng chung cho cả sender_app.py và receiver_app.py.

verify_tool.py: Công cụ Python độc lập (offline) để xác minh chữ ký số của các phần file.

keys/: Thư mục lưu trữ các cặp khóa RSA (.pem) của cả người gửi và người nhận.

received_files/: Thư mục đích nơi các file đã nhận và giải mã thành công sẽ được lưu trữ.

templates/: Thư mục chứa các tệp mẫu HTML cho giao diện web của ứng dụng.

contract.txt: Tệp dữ liệu mẫu dùng để thử nghiệm chức năng gửi file.

myenv/ (hoặc .venv/): Môi trường ảo Python của dự án.

Kỹ thuật và Thuật toán sử dụng
Hệ thống được xây dựng bằng Python 3.13.5 và sử dụng các công nghệ, thuật toán mật mã sau:

PyCryptodome: Thư viện mật mã chính, cung cấp các hàm cho:

Triple DES (3DES): Thuật toán mã hóa đối xứng, dùng để mã hóa nội dung các phần file. Sử dụng chế độ CBC (Cipher Block Chaining) với IV ngẫu nhiên để tăng cường bảo mật và che giấu mẫu dữ liệu.

RSA 2048-bit: Thuật toán mã hóa bất đối xứng. Được dùng để:

Trao đổi khóa phiên an toàn: Mã hóa khóa phiên Triple DES bằng khóa công khai của người nhận.

Tạo và xác minh chữ ký số: Ký metadata và từng phần file bằng khóa riêng tư của người gửi.

Sử dụng chế độ đệm PKCS#1 v1.5 để đảm bảo an toàn.

SHA-512: Hàm băm mật mã, dùng để:

Kiểm tra tính toàn vẹn: Tính toán giá trị băm của (IV || ciphertext) của từng phần file.

Đầu vào cho chữ ký số: Giá trị băm này được ký bằng RSA.

Chữ ký số (Digital Signature): Kết hợp RSA và SHA-512 để đảm bảo tính xác thực (người gửi là ai) và tính không chối bỏ (người gửi không thể phủ nhận việc đã gửi).

Flask: Micro-framework Python dùng để xây dựng giao diện người dùng dưới dạng ứng dụng web, giúp người dùng dễ dàng tương tác qua trình duyệt.

Socket: Thư viện chuẩn của Python, được sử dụng để thiết lập kết nối mạng TCP giữa Người gửi và Người nhận, cho phép truyền tải dữ liệu.

Các thư viện Python tiêu chuẩn khác: json, base64, os, datetime, threading, math được sử dụng để xử lý dữ liệu, quản lý file, thời gian và đa luồng.

Giao diện ứng dụng
Hệ thống cung cấp hai giao diện web trực quan cho SenderApp và ReceiverApp, truy cập qua trình duyệt.

Giao diện SenderApp
Tiêu đề: "ỨNG DỤNG NGƯỜI GỬI HỢP ĐỒNG"

Bạn hãy chèn ảnh chụp màn hình giao diện SenderApp tại đây.

Các thành phần chính:

Quản Lý Khóa RSA: Khu vực để tạo và tải lên các tệp khóa RSA của người gửi và khóa công khai của người nhận.

Kết Nối Với Người Nhận: Khu vực nhập địa chỉ IP và cổng của máy nhận để thiết lập kết nối.

Gửi Hợp Đồng: Khu vực để chọn tệp hợp đồng và bắt đầu quá trình mã hóa, ký số và gửi.

Nhật Ký Hoạt Động: Hiển thị chi tiết quá trình hoạt động, bao gồm thông tin về Hash (Base64) và Chữ ký (Base64) của từng phần file đã gửi.

Giao diện ReceiverApp
Tiêu đề: "ỨNG DỤNG NGƯỜI NHẬN HỢP ĐỒNG"

Bạn hãy chèn ảnh chụp màn hình giao diện ReceiverApp tại đây.

Các thành phần chính:

Quản Lý Khóa RSA: Khu vực để tạo và tải lên các tệp khóa RSA của người nhận và khóa công khai của người gửi.

Khởi Động Server: Khu vực để nhập cổng và điều khiển việc bắt đầu/dừng máy chủ nhận.

Nhật Ký Hoạt Động: Hiển thị chi tiết quá trình hoạt động, bao gồm trạng thái kết nối, trao đổi khóa, và quá trình nhận, xác minh, giải mã file.

Công cụ xác minh Offline (verify_tool.py)
Bạn hãy chèn ảnh chụp màn hình giao diện Verify Tool tại đây (nếu có).

Công cụ này chạy trên dòng lệnh, cho phép người dùng xác minh tính toàn vẹn và xác thực của một phần file đã nhận một cách độc lập bằng cách cung cấp đường dẫn đến khóa công khai của người gửi, chuỗi Hash (Base64) và chuỗi Chữ ký (Base64) từ log của người gửi.

Hướng dẫn cài đặt và chạy
Clone repository:

git clone <địa chỉ repository của bạn>
cd <tên thư mục dự án>

Tạo và kích hoạt môi trường ảo:

python -m venv myenv
.\myenv\Scripts\activate # Trên Windows PowerShell
# source myenv/bin/activate # Trên Linux/macOS

Cài đặt các thư viện cần thiết:

pip install Flask pycryptodome

Chạy ứng dụng Người nhận (ReceiverApp):
Mở một cửa sổ terminal/PowerShell mới, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python receiver_app.py

Truy cập http://127.0.0.1:5001 trong trình duyệt.

Chạy ứng dụng Người gửi (SenderApp):
Mở một cửa sổ terminal/PowerShell khác, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python sender_app.py

Truy cập http://127.0.0.1:5000 trong trình duyệt.

Chạy công cụ xác minh Offline (Verify Tool):
Mở một cửa sổ terminal/PowerShell khác, kích hoạt môi trường ảo và chạy:

.\myenv\Scripts\activate
python verify_tool.py

Làm theo hướng dẫn trên màn hình để nhập các thông tin cần thiết.
