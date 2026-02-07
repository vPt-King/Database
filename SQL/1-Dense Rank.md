\# DENSE\_RANK() 

Là hàm xếp hạng (window function) trong SQL, dùng để đánh số thứ hạng cho các dòng dữ liệu, không bỏ số khi bị trùng.



Nó cực hay dùng trong các bài:



Top-N



Xếp hạng lương



Leaderboard



Báo cáo thống kê



\# Cú pháp DENSE\_RANK()

DENSE\_RANK() OVER (

&nbsp;   PARTITION BY column1, column2   -- (tùy chọn)

&nbsp;   ORDER BY column3 \[ASC|DESC]

)



ORDER BY: bắt buộc



PARTITION BY: chia nhóm (giống GROUP BY nhưng không gộp dòng)

