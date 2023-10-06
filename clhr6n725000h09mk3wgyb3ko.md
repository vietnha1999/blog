---
title: "[Linux] Trình gỡ lỗi hoạt động như thế nào - Phần 1"
datePublished: Wed May 17 2023 04:08:12 GMT+0000 (Coordinated Universal Time)
cuid: clhr6n725000h09mk3wgyb3ko
slug: linux-how-debuggers-work
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1696493200003/26861d05-e5a3-49a2-9343-c4d5e5b33643.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1696403093867/7b255503-f2b8-4f33-83af-4cf500ffa417.png
tags: linux, debugging

---

# Nội dung chính

Tôi sẽ trình bày thành phần chính của việc triển khai trình gỡ lỗi trên Linux: `ptrace`. Tất cả mã nguồn trong bài viết này được phát triển trên Ubuntu 32-bit.

# Tính năng của một trình gỡ lỗi

Trình gỡ lỗi có thể:

* Nhảy từng bước trong mã nguồn
    
* Đặt điểm dừng và chạy tới nó
    
* Kiểm tra giá trị của biến và dấu vết ngăn xếp
    

Một số trình gỡ lỗi có tính năng nâng cao:

* Thực hiện biểu thức và gọi hàm
    
* Thay đổi mã của tiến trình đang chạy và theo dõi sự ảnh hưởng
    

# Khả năng của ptrace

ptrace cho phép một tiến trình:

* Kiểm soát việc thực thi của một tiến trình khác
    
* Đọc và ghi (peek and poke) vào bộ nhớ của một tiến trình khác
    

# Nhảy từng bước trong mã nguồn của một tiến trình