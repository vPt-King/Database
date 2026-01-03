# Oracle architecture
## Oracle instance
```
Oracle instance bao gồm một cấu trúc bộ nhớ System Global Area (SGA) và các background
processes (tiến trình nền) được sử dụng để quản trị cơ sở dữ liệu. Oracle instance được xác
định qua tham số môi trường ORACLE_SID của hệ điều hành.
```

### System global area
```
SGA là vùng bộ nhớ chia sẻ được sử dụng để lưu trữ dữ liệu và các thông tin điều khiển của
Oracle server. SGA được cấp phát (allocated) trong bộ nhớ của máy tính mà Oracle server
đang hoạt động trên đó. 
Các User kết nối tới Oracle sẽ chia sẻ các dữ liệu có trong SGA, việc mở rộng không gian bộ nhớ cho SGA sẽ làm nâng cao hiệu suất của hệ thống, lưu trữ được nhiều dữ liệu trong hệ thống hơn.
SGA bao gồm một vài cấu trúc bộ nhớ chính
```
#### Share pool
Shared pool là một phần trong SGA và được sử dụng khi thực hiện phân tích câu lệnh (parse
phase). Kích thước của Shared pool được xác định bởi tham số SHARED_POOL_SIZE có trong parameter file (file tham số).

Các thành phần của Shared pool gồm có: Library cache và Data dictionary cache.
Library Cache
Library cache lưu trữ thông tin về các câu lệnh SQL được sử dụng gần nhất bao gồm:
▪ Nội dung của câu lệnh dạng text (văn bản).
▪ Parse tree (cây phân tích) được xây dựng tuỳ thuộc vào câu lệnh.
▪ Execution plan (sơ đồ thực hiện lệnh) gồm các bước thực hiện và tối ưu lệnh.
Do các thông tin trên đã được lưu trữ trong Library cache nên khi thực hiện lại một câu lệnh
truy vấn, trước khi thực hiện câu lệnh, Server process sẽ lấy lại các thông tin đã được phân
tích mà không phải phân tích lại câu lệnh. Do vậy, Library cache có thể giúp nâng cao hiệu
suất thực hiện lệnh.

Data Dictionary Cache
Data dictionary cache là một thành phần của Shared pool lưu trữ thông tin của dictionary
cache được sử dụng gần nhất như các định nghĩa các bảng, định nghĩa các cột, usernames,
passwords, và các privileges (quyền).
Trong giai đoạn phân tích lệnh (parse phase), Server process sẽ tìm các thông tin trong
dictionary cache để xác định các đối tượng trong câu lệnh SQL và để xác định các mức quyền
tương ứng. Trong trường hợp cần thiết, Server process có thể khởi tạo và nạp các thông tin từ
các file dữ liệu.

#### Databse buffer cache
Khi thực hiện một truy vấn, Server process sẽ tìm các blocks cần thiết trong database buffer
cache. Nếu không tìm thấy block trong database buffer cache, Server process mới đọc các
block từ data file và tạo luôn một bản sao của block đó vào trong vùng nhớ đệm (buffer
cache). Như vậy, với các lần truy xuất tới block đó sau này sẽ không cần thiết phải truy xuất
vào datafile nữa.
Database buffer cache là vùng nhớ trong SGA sử dụng để lưu trữ các block dữ liệu được sử
dụng gần nhất. Tương tự như kích thước của blocks dữ liệu được xác định bởi tham số
DB_BLOCK_SIZE, kích thước của vùng đệm trong buffer cache cũng được xác định bởi
tham số DB_BLOCK_BUFFERS.
Oracle server sử dụng giải thuật least recently used (LRU) algorithm để làm tươi lại vùng
nhớ. Theo đó, khi nạp mới một block vào bộ đệm, trong trường hợp bộ đệm đã đầy, Oracle
server sẽ loại bớt block ít được sử dụng nhất ra khỏi bộ đệm để nạp block mới vào bộ đệm.

#### Redo log buffer
Server process ghi lại các thay đổi của một instance vào redo log buffer, đây cũng là một phần
bộ nhớ SGA.
Có một số đặc điểm cần quan tâm của Redo log buffer:
▪ Kích thước được xác định bởi tham số LOG_BUFFER.
▪ Lưu trữ các redo records (bản ghi hồi phục) mỗi khi có thay đổi dữ liệu.
▪ Redo log buffer được sử dụng một cách thường xuyên và các thay đổi bởi một
transaction có thể nằm đan xen với các thay đổi của các transactions khác.
▪ Bộ đệm được tổ chức theo kiểu circular buffer (bộ đệm nối vòng) tức là dữ liệu thay
đổi sẽ tiếp tục được nạp lên đầu sau khi vùng đệm đã được sử dụng hết.

### Backgroud process

Background process (các tiến trình nền) thực hiện các chức năng thay cho lời gọi tiến trình xử
lý tương ứng. Nó điều khiển vào ra, cung cấp các cơ chế xử lý song song nâng cao hiệu quả
và độ tin cậy. Tùy theo từng cấu hình mà Oracle instance có các Background process như:

▪ Database Writer (DBW0): Ghi lại các thay đổi trong data buffer cache ra các file dữ
liệu.
▪ Log Writer (LGWR): Ghi lại các thay đổi được đăng ký trong redo log buffer vào các
redo log files.
▪ System Monitor (SMON): Kiểm tra sự nhất quán trong database.
▪ Process Monitor (PMON): Dọn dẹp lại tài nguyên khi các tiến trình của Oracle gặp lỗi.
▪ Checkpoint Process (CKPT): Cập nhật lại trạng thái của thông tin trong file điều khiển
(control file) và file dữ liệu (datafile) mỗi khi có thay đổi trong buffer cache.

#### Database Writer (DBW0)
```
Server process ghi lại các dữ liệu thay đổi để rollback và dữ liệu của các block trong buffer
cache. Database writer (DBWR) ghi các thông tin được đánh dấu thay đổi từ database buffer
cache lên các data files nhằm đảm bảo luôn có khoảng trống bộ đệm cần thiết cho việc sử
dụng.
Với việc sử dụng này, hiệu suất sử dụng database sẽ được cải thiện do Server processes chỉ
tạo các thay đổi trên buffer cache, DBWR ghi dữ liệu vào các data file cho tới khi:
▪ Số lượng buffers đánh bị dấu đạt tới giá trị ngưỡng.
▪ Tiến trình duyệt tất cả buffer mà vẫn không tìm thấy dữ liệu tương ứng.
▪ Quá thời gian quy định.
```

#### Log writer
```
Log Writer (LGWR) là một trong các background process có trách nhiệm quản lý redo log buffer để ghi lại các thông tin trong Redo log buffer vào Redo log file. Redo log buffer là bộ đệm dữ liệu được tổ chức theo kiểu nối vòng.
LGWR ghi lại dữ liệu một cách tuần tự vào redo log file theo các tình huống sau:
▪ Khi redo log buffer đầy
▪ Khi xảy ra timeout (thông thường là 3 giây)
▪ Trước khi DBWR ghi lại các blocks bị thay đổi trong data buffer cache vào các data
files.
▪ Khi commit một transaction.
```

#### System Monitor (SMON)
```
Tiến trình system monitor (SMON) thực hiện phục hồi các sự cố (crash recovery) ngay tại thời
điểm instance được khởi động (startup), nếu cần thiết. SMON cũng có trách nhiệm dọn dẹp
các temporary segments không còn được sử dụng nữa trong dictionary-managed tablespaces.
SMON khôi phục lại các transactions bị chết mỗi khi xảy ra sự cố. SMON đều đặn thực hiện
kiểm tra và khắc phục các sự cố khi cần.
Trong môi trường Oracle Parallel Server, SMON process của một instance có thể thực hiện
khôi phục instance trong trường hợp instance hay CPU của máy tính đó gặp sự cố.
Crash Instance
```

#### Process Monitor (PMON)
```
Tiến trình process monitor (PMON) thực hiện tiến trình phục hồi mỗi khi có một user process gặp lỗi.PMON có trách nhiệm dọn dẹp database buffer cache và giải phóng tài nguyên mà user process đó sử dụng. Ví dụ, nó thiết lập lại (reset) trạng thái của các bảng đang thực hiện trong transaction, giải phóng các locks trên bảng này, và huỷ bỏ process ID của nó ra khỏi danh sách các active processes.

PMON kiểm tra trạng thái của nơi gửi (dispatcher ) và các server processes, khởi động lại (restarts) mỗi khi xảy ra sự cố. PMON cũng còn thực hiện việc đăng ký các thông tin về instance và dispatcher processes với network listener. Tương tự như SMON, PMON được gọi đến mỗi khi xảy ra sự cố trong hệ thống.
crash session > PMON
```

#### Checkpoint 
```
Cập nhật lại trạng thái của thông tin trong file điều khiển và file dữ liệu mỗi khi có thay đổi
trong buffer cache. Xảy ra checkpoints khi:
▪ Tất cả các dữ liệu trong database buffers đã bị thay đổi tính cho đến thời điểm
checkpointed sẽ được background process DBWRn ghi lên data files.
▪ Background process CKPT cập nhật phần headers của các data files và các control
files.
Checkpoints có thể xảy ra đối với tất cả các data files trong database hoặc cũng có thể xảy ra
với một data files cụ thể.
Checkpoint xảy ra theo các tình huống sau:
▪ Mỗi khi có log switch
▪ Khi một shut down một database với các chế độ trừ chế độ abort
▪ Xảy ra theo như thời gian quy định trong các tham số khởi tạo LOG_CHECKPOINT_INTERVAL và LOG_CHECKPOINT_TIMEOUT
▪ Khi có yêu cầu trực tiếp của quản trị viên
Thông tin về checkpoint được lưu trữ trong Alert file trong trường hợp các tham số khởi tạo
LOG_CHECKPOINTS_TO_ALERT được đặt là TRUE. Và ngược lại với giá trị FALSE.
```
