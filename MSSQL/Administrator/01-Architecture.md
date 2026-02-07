MS SQL Server là 1 CSDL quan hệ (or RDBMS) được phát triển bởi Microsoft, SQL Server phát triển theo kiến trúc client-server, client send request -> SQL Server tiếp nhận, xử lý -> trả về dữ liệu đã được xử lý về client.



MS SQL Server có 3 thành phần chính là Protocol Layer – SNI, Relational Engine, Storage Engine

\# Protocol Layer – SNI(SQL Server Network Interface): lớp giao thức

```

\- Lớp này dùng để quản lý và điều phối cách mà client-server giao tiếp với nhau. Lớp này sẽ làm các công việc như: tiếp nhận yêu cầu từ client, quản lý kết nối, tối ưu hóa truyền dữ liệu, xử lý hỗ trợ các giao thức khác nhau(3 loại bên dưới đây)



\- SQL Server hỗ trợ 3 loại giao thức kết nối:



&nbsp;   Shared Memory: client - SQL Server chạy trên cùng một máy, giao tiếp với nhau bằng shared memory protocol

&nbsp;   TCP/IP: client - SQL Server tương tác với nhau ở mạng LAN, WAN, INTERNET

&nbsp;   Named Pipes: client - SQL Server giao tiếp thông qua Mạng cục bộ (LAN).



\- TDS là giao thức mạng được sử dụng để giao tiếp giữa SQL Server và client. Nó chịu trách nhiệm mã hóa query SQL và kết quả, sắp xếp dữ liệu thành packages và xử lý các tác vụ như thiết lập kết nối, thực hiện truy vấn và chấm dứt kết nối.

```



\# Relational Engine(or Query Processor): Bộ xử lý truy vấn

```

\- Lớp này chứa các thành phần xác định chính xác những gì một truy vấn phải làm và cách thực hiện nó một cách tốt nhất, nó thực hiện các truy vấn của người dùng bằng cách yêu cầu dữ liệu từ storage engine và xử lý các kết quả trả về. Relational Engine có 3 thành phần chính:



* CMD Parser: có nhiệm vụ kiểm tra câu query có đúng ngữ nghĩa và cú pháp không, cuối cùng tạo ra Query Tree(biểu diễn cấu trúc logic của truy vấn SQL dưới dạng tree)
* Optimizer: sử dụng các thuật toán(exhaustive and heuristic algorithms) để tối ưu thời gian truy vấn và tạo chiến lược thực thi(table scan, index usage, join, ..), mục tiêu là tìm ra Query plan tiết kiệm chi phí nhất chứ không phải tốt nhất(tức là sử dụng ít tài nguyên hệ thống nhất như CPU, bộ nhớ, và I/O).
* Query Executor: thực thi các bước Query plan và lấy kết quả quy vấn, nhận data từ Storage Engine và trả kết quả ra Protocol Layer

```



\# Storage engine

Công việc của Storage Engine là lưu trữ dữ liệu trong hệ thống lưu trữ như Disk hay SAN và truy xuất dữ liệu khi cần. Các file dữ liệu trong trong SQL Server lưu trữ dữ liệu dưới dạng data pages có kích thước 8KB, là đơn vị lưu trữ nhỏ nhất, các data pages này được nhóm logic thành các extents, mỗi extent bao gồm 8 data page (tổng cộng 64KB). Extents giúp quản lý việc cấp phát và sử dụng không gian lưu trữ một cách hiệu quả hơn.





\# Data file

Có 3 loại data file trong Storage Engine: Primary file, Secondary file và Log file



```

1. Primary file: Mỗi database sẽ có 1 primary file(đuôi mặc định là .mdf), file này chứa thông tin quan trọng liên quan tới tables, views, triggers, etc.

2\. Secondary file: Mỗi database có thể có nhiều Secondary files(đuôi mặc định là .ndf), lưu trữ dữ liệu người dùng.

3\. Log file: mặc định database sẽ có 1 file log(đuôi mặc định là .ldf) chứa thông tin tất cả các transaction và thay đổi dữ liệu để đảm bảo khả năng phục hồi và tính toàn vẹn dữ liệu, thường sử dụng để khôi phục dữ liệu trong trường hợp xảy ra sự cố.

```



\# Access Method

```

Đóng vai trò làm giao diện giữa query executor và Buffer Manager/Transaction Manager, nó xác định xem truy vấn là Select Statement (DDL) hay Non- Select Statement (DDL \& DML) và không thực thi.



\- Nếu truy vấn là câu lệnh DDL, câu lệnh select, truy vấn sẽ được chuyển đến Buffer Manager để xử lý thêm.



\- Nếu truy vấn nếu câu lệnh DDL, NON-SELECT, truy vấn sẽ được chuyển đến Transaction Manager. Điều này chủ yếu bao gồm câu lệnh UPDATE.

```



\# Buffer manager

Thành phần này quản lý chức năng cốt lõi cho các module Plan Cache, Data Parsing: Buffer cache \& Data storage, Dirty Page



\## Plan cache

!\[Plan cache](images/plancache.png)

```

Khi một truy vấn được tối ưu hóa, execution plan của nó được lưu trữ trong Plan Cache để sử dụng lại cho các truy vấn tương tự. Nó giúp tăng hiệu suất và giảm thời gian xử lý cho các truy vấn lặp lại.



* Đã tồn tại Query plan: buffer manager sẽ kiểm tra xem execution plan có trong Plan Cache hay không, nếu có thì Plan Cache truy vấn và Data Cache sẽ được sử dụng. Điều này giúp giảm thời gian xử lý do không cần phải tối ưu hóa lại truy vấn.
* Lần đầu Cache Query plan: khi truy vấn được thực thi lần đầu, SQL Server sẽ tối ưu hóa truy vấn và tạo ra một excution plan. Nếu truy vấn phức tạp và cần nhiều tài nguyên để tối ưu hóa, kế hoạch truy vấn sẽ được lưu trữ trong Plan Cache sau lần thực thi đầu tiên(không phải lúc nào cũng lưu trữ excution plan trong Plan Cache mà nó còn phụ thuộc vào nhiều yếu tố: truy vấn đơn giản hoặc thường xuyên thay đổi, tài nguyên hệ thống, cấu hình hệ thống...)

```

\## Data Parsing: Buffer cache \& Data Storage

```

Buffer Manager đóng vai trò quan trọng trong việc cung cấp dữ liệu cần thiết cho bộ thực thi truy vấn (Query Executor). Dưới đây là hai cách tiếp cận khác nhau để xử lý dữ liệu dựa trên việc dữ liệu có sẵn trong bộ đệm hay không:



* Buffer Cache – Soft Parsing: Buffer Manager kiểm tra xem dữ liệu yêu cầu có sẵn trong Buffer Cache hay không, nếu dữ liệu có thì Query Executor sẽ sử dụng dữ liệu này. Việc truy xuất dữ liệu từ Buffer Cache nhanh hơn so với việc truy xuất từ Data Storage(lưu trữ dữ liệu) vì giảm số lượng thao tác I/O (đọc/ghi), giảm số lượng I/O giúp giảm tải cho hệ thống và cải thiện thời gian phản hồi của các câu truy vấn.
* Data Storage – Hard Parsing: Nếu dữ liệu không có trong Buffer Cache, Buffer Manager sẽ tìm kiếm dữ liệu trong Data Storage (lưu trữ dữ liệu trên đĩa) và nó sẽ được lưu trữ trong Buffer Cache để sử dụng trong tương lai.



```



\## Dirty Page

là các data pages trong Buffer Cache(bộ nhớ đệm) đã bị thay đổi nhưng chưa được ghi trở lại disk. Khi một transaction thực hiện thay đổi dữ liệu, các thay đổi này được thực hiện trên các data pages trong cache và được đánh dấu là "dirty".



\# Transaction Manager





```

trong SQL Server đóng vai trò quan trọng trong việc quản lý transaction. Dưới đây là các chức năng chính của Transaction Manager:



* Quản lý transaction Không phải SELECT: Transaction Manager xử lý các transaction liên quan tới INSERT, UPDATE, DELETE. Những giao dịch này có thể thay đổi dữ liệu trong cơ sở dữ liệu và cần được quản lý cẩn thận để đảm bảo tính nhất quán và toàn vẹn dữ liệu.
* Sử dụng Log Manager: Transaction Manager sử dụng Log Manager để ghi lại các thay đổi dữ liệu trong transaction log, nó hỗ trợ khả năng recovery and rollback transaction, đảm bảo tính toàn vẹn của dữ liệu ngay cả khi có lỗi hoặc sự cố.
* Sử dụng Lock Manager: giúp quản lý việc khóa các bản ghi hoặc các đối tượng dữ liệu khác để đảm bảo rằng nhiều transaction không can thiệp vào nhau và gây ra các vấn đề về tính nhất quán dữ liệu. Nó cũng giúp ngăn chặn deadlock, nơi hai hoặc nhiều transaction chờ nhau release lock, dẫn đến tình trạng không thể tiếp tục.
* Checkpoint và Write-Ahead Logging(Cơ chế ghi trước): Transaction Manager sử dụng cơ chế Write-Ahead Logging (WAL) để đảm bảo rằng các thay đổi dữ liệu được ghi vào transaction log trước khi chúng được ghi vào disk. Điều này giúp bảo vệ dữ liệu khỏi mất mát trong trường hợp hệ thống gặp sự cố. Checkpoint là quy trình này chạy và đánh dấu tất cả các data pages từ Dirty Pages vào Disk, tần suất khoảng 1 lần/phút.
* Lazy Writers: Lazy Writers là background processes giúp ghi các dirty page từ cache vào disk để giải phóng bộ nhớ cache và duy trì hiệu suất hệ thống. Chúng giúp giảm thiểu số lượng các dirty page và cải thiện hiệu suất tổng thể.

```



\# The Components of SQL Server Architecture(các thành phần trong kiến trúc SQL Server)

```

Database Engine: Thành phần này chịu trách nhiệm lưu trữ, bảo mật dữ liệu và xử lý giao dịch nhanh chóng.



SQL Server: Dịch vụ này khởi động, dừng, tạm dừng và tiếp tục các phiên bản Microsoft SQL Server (chúng ta sẽ đề cập sau). Tên thực thi là sqlservr.exe.



SQL Server Agent: Tác nhân này đảm nhận vai trò của trình lập lịch tác vụ và kích hoạt bằng bất kỳ sự kiện nào hoặc theo yêu cầu. Tên thực thi là sqlagent.exe.



SQL Server Browser: Trình duyệt này lắng nghe các yêu cầu đến và kết nối chúng với phiên bản SQL Server cần thiết. Tên thực thi là sqlbrowser.exe.



SQL Server Full-Text Search: Tìm kiếm này cho phép người dùng chạy các truy vấn toàn văn đối với dữ liệu ký tự nằm trong Bảng SQL. Tên thực thi là fdlauncher.exe.



SQL Server VSS Writer: Thành phần này cho phép sao lưu và khôi phục tệp dữ liệu khi SQL Server không chạy. Tên thực thi là sqlwriter.exe.



SQL Server Analysis Services (SSAS): Dịch vụ này cung cấp các chức năng phân tích dữ liệu, khai thác dữ liệu và Học máy. SQL Server được tích hợp với các ngôn ngữ lập trình R và Python cho mục đích phân tích nâng cao. Tên thực thi là msmdsrv.exe.



SQL Server Reporting Services (SSRS): Dịch vụ này cung cấp các tính năng báo cáo và khả năng ra quyết định, bao gồm tích hợp Hadoop. Tên thực thi là ReportingServicesService.exe.



SQL Server Integration Services (SSIS): Cuối cùng, dịch vụ này cung cấp khả năng trích xuất-biến đổi và tải các loại dữ liệu khác nhau giữa các nguồn. Tóm lại, nó chuyển đổi thông tin thô thành thông tin hữu ích. Tên thực thi là MsDtsSrvr.exe.

```



