# Deterministic function
Deterministic Function là gì?
Một hàm hoặc biểu thức được gọi là deterministic nếu cùng một input → luôn trả về cùng một output, bất kể thời điểm hay môi trường thực thi.

Ví dụ hàm deterministic trong SQL Server:
ABS( )
UPPER( )
LOWER( )
ROUND( ) (trong đa số trường hợp)
CONVERT giữa kiểu dữ liệu cố định
LEFT(), RIGHT()
Tính chất:
Nếu bạn gọi ABS(-5) 100 lần, nó đều trả về 5 → deterministic.

# Non-Deterministic Function là gì?
Một hàm hoặc biểu thức được gọi là non-deterministic nếu cùng một input, nhưng output có thể thay đổi tùy thời điểm hoặc môi trường.
Các hàm non-deterministic phổ biến:
GETDATE() → mỗi lần trả về thời gian khác nhau
NEWID() → tạo GUID ngẫu nhiên
RAND() → số ngẫu nhiên (không cố định)
CURRENT_TIMESTAMP
DATEADD khi áp dụng vào GETDATE()
@@SPID, @@ROWCOUNT
Tính chất:
GETDATE() hôm nay và 5 phút nữa luôn khác nhau → non-deterministic.

Tại sao SQL Server quan tâm đến Deterministic vs Non-Deterministic?

Vì SQL Server cần biết điều này để:

1️⃣ Index được hay không

Hàm non-deterministic không được phép dùng trong indexed view hoặc computed column có index.

Ví dụ:

❌ Không thể tạo index lên computed column:

ALTER TABLE Users
ADD CreatedDate AS GETDATE();


Vì GETDATE() là non-deterministic.

Tối ưu hóa query (query optimizer)

Hàm deterministic có thể được SQL Server tính toán trước và cache kết quả.


Loại	Đặc điểm	Ví dụ
Deterministic	input giống → output giống	ABS(), UPPER(), ROUND()
Non-Deterministic	output luôn thay đổi	GETDATE(), RAND(), NEWID()