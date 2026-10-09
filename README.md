## 1. Sơ đồ luồng IPO (Input – Process – Output)
Plaintext

INPUT: Dữ liệu file thô hổn độn nằm trong thư mục Downloads của máy chủ 


PROCESS:
- Kiểm tra phần mở rộng (đuôi file) của từng tệp tin trong thư mục Downloads.
- Phân loại theo quy tắc:

  1: Đuôi .png, .jpg -> Di chuyển sang media/

   2: - Đuôi .py        -> Di chuyển sang scripts/

   3: - Đuôi .tmp       -> Xóa bỏ

- Xử lý ngoại lệ Edge Case (trùng tên thư mục).


OUTPUT: 
- Các file ảnh (.png, .jpg) được gom gọn vào  thư mục media/.
- Các file code (.py) được gom gọn vào thư mục scripts/.
- Các file rác tạm (.tmp) bị xóa sạch để giải phóng dung lượng ổ đĩa.


## 2. Giải quyết Trường hợp kiểm thử (Edge Case): Lỗi "Is a directory":
- Vấn đề: Khi Robot thực hiện lệnh di chuyển file (ví dụ mv image.png media/), nếu trong thư mục đích media/ đã tồn tại sẵn một thư mục con có tên trùng hệt tên file (image.png), hệ thống sẽ từ chối thực hiện và bắn ra lỗi Is a directory làm chương trình/kịch bản bị gián đoạn (crash).

- Giải pháp xử lý an toàn cho Robot: Trước khi thực thi lệnh di chuyển, Robot cần dùng câu lệnh điều kiện hoặc cơ chế kiểm tra sự tồn tại (if-else / kiểm tra kiểu tệp) để xác định xem đích đến có phải là thư mục hay không.

Bổ sung cờ ép buộc, đổi tên file tự động trước khi di chuyển, hoặc đặt khối lệnh trong bộ lọc lỗi (try-catch / bắt ngoại lệ) để nếu phát hiện trùng tên, Robot sẽ tự động bỏ qua lượt đó hoặc thêm hậu tố thời gian (_copy.png) thay vì để crash script.

## 3. Danh sách câu lệnh kịch bản cho Robot (Mẫu Bash trên Linux/macOS)

Bước 1: Tạo sẵn cấu trúc thư mục chứa (nếu chưa có)
Bash
mkdir -p media scripts


Bước 2: Gom các file ảnh vào thư mục media/
Bash
mv *.png media/ 2>/dev/null
mv *.jpg media/ 2>/dev/null
(Thêm 2>/dev/null để ẩn đi các thông báo lỗi nếu vô tình không tìm thấy file đuôi nào đó).


Bước 3: Gom các file mã nguồn Python vào thư mục scripts/
Bash
mv *.py scripts/ 2>/dev/null


Bước 4: Xóa sạch các file tạm .tmp để giải phóng dung lượng
Bash
rm -f *.tmp
