# Union và Union ALL
UNION trong SQL là phép kết hợp kết quả của hai hoặc nhiều SELECT lại với nhau thành một tập kết quả duy nhất.
Nó hoạt động như phép “gộp danh sách” nhưng loại bỏ các dòng trùng nhau.
SELECT Name FROM Students_A
UNION
SELECT Name FROM Students_B;
SQL sẽ trả về:

Tất cả tên từ bảng A

Tất cả tên từ bảng B

Không có dòng trùng nhau (ví dụ trùng tên sẽ chỉ xuất hiện 1 lần)


Union ALL sẽ giữ nguyên tất cả các dòng của cả 2 câu lệnh
Yêu cầu khi dùng UNION
Số cột phải bằng nhau


Kiểu dữ liệu tương ứng phải tương thích