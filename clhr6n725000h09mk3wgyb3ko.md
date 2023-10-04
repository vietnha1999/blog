---
title: "Trình gỡ lỗi trong Linux hoạt động như thế nào - Phần 1"
datePublished: Wed May 17 2023 04:08:12 GMT+0000 (Coordinated Universal Time)
cuid: clhr6n725000h09mk3wgyb3ko
slug: how-debuggers-work
canonical: https://eli.thegreenplace.net/2011/01/23/how-debuggers-work-part-1
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1696403093867/7b255503-f2b8-4f33-83af-4cf500ffa417.png
tags: debugging

---

# Trong phần này

Tôi sẽ trình bày thành phần chính của việc triển khai trình gỡ lỗi trên Linux: `ptrace` system call. Tất cả mã trong bài viết này được phát triển trên Ubuntu 32-bit.

# Động lực

Trình gỡ lỗi có thể:

* Đi từng bước trong mã
    
* Đặt các điểm dừng và chạy tới chúng
    
* Kiểm tra giá trị của các biến và dấu vết ngăn xếp
    

Một số trình gỡ lỗi có tính năng nâng cao:

* Thực hiện các biểu thức và gọi các hàm
    
* Thay đổi mã của tiến trình đang chạy và theo dõi sự thay đổi
    

# ptrace

ptrace cho phép một tiến trình:

* Kiểm soát việc thực thi của một tiến trình khác
    
* Đọc và ghi (peek and poke) vào bộ nhớ của một tiến trình khác
    

# Đi từng bước trong mã của một tiến trình