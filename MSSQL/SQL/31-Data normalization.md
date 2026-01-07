# Data normalization
Data normalization (chuẩn hoá dữ liệu) trong SQL Server (và mọi hệ CSDL quan hệ) là:

Quá trình thiết kế CSDL sao cho dữ liệu không bị trùng lặp, nhất quán và dễ bảo trì, bằng cách chia bảng theo các quy tắc chuẩn hoá.

# 🎯 Mục tiêu của Data Normalization

❌ Giảm dữ liệu trùng lặp

✅ Đảm bảo toàn vẹn dữ liệu

🔄 Dễ INSERT / UPDATE / DELETE

🛠️ Dễ bảo trì & mở rộng

## 1 First normal form (1NF)
Điều kiện:

Mỗi cột chứa giá trị nguyên tử

Không có cột lặp

❌ Sai 1NF
`Products = "Laptop, Mouse"`

✅ Đúng 1NF

Tách bảng:

Orders
| OrderID | CustomerID |

OrderDetails
| OrderID | Product |

# 2️⃣ Second Normal Form (2NF)

Điều kiện:

Đã đạt 1NF

Các cột không khoá phải phụ thuộc hoàn toàn vào khoá chính

❌ Sai 2NF
`OrderID, ProductID → ProductName`


ProductName chỉ phụ thuộc ProductID

✅ Chuẩn hoá
```
Products
| ProductID | ProductName |
```