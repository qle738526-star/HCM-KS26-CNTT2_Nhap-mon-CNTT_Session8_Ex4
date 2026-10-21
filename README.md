# HCM-KS26-CNTT2_Nhap-mon-CNTT_Session8_Ex4
# Bài 4: Toán tử chuyển hướng (Redirection > vs >>)

## 1. So sánh `>` và `>>`

| Tiêu chí | `>` (Ghi đè - Overwrite) | `>>` (Ghi nối - Append) |
| :--- | :--- | :--- |
| **Cơ chế** | Xóa sạch toàn bộ nội dung cũ, ghi đè dữ liệu mới. | Thêm dữ liệu mới vào dòng cuối cùng của tệp. |
| **Rủi ro (Bẫy)** | **Rất cao:** Nếu gõ nhầm vào cuối tháng, toàn bộ báo cáo tích lũy 30 ngày sẽ bị xóa sạch không thể khôi phục. | **An toàn:** Giữ nguyên dữ liệu lịch sử, không gây mất mát. |
| **Ngữ cảnh** | Tạo báo cáo mới (Khởi tạo ca sáng). | Theo dõi log phát sinh liên tục (Giám sát 24/7). |
| **Encoding** | Alias của lệnh `Out-File` (Mặc định UTF-16). | Alias của lệnh `Out-File -Append` (Mặc định UTF-16). |

## 2. Giải pháp tối ưu cho ghi nhật ký liên tục

*   **Lựa chọn chốt:** Bắt buộc dùng cơ chế **ghi nối tiếp** để đảm bảo tính toàn vẹn của nhật ký hệ thống phát sinh trong ngày.
*   **Tối ưu Encoding:** Vì `>>` mặc định xuất ra tệp UTF-16 (có thể gây lỗi hiển thị ký tự trên một số công cụ đọc), giải pháp tốt nhất là sử dụng rõ ràng lệnh `Add-Content` hoặc `Out-File -Append` kết hợp cờ ép kiểu mã hóa UTF-8.

**Lệnh đề xuất:**

# Cách 1: Dùng Out-File 
Get-Content server.log | Select-String "ERROR" | Out-File -Append -Encoding utf8 daily_report.txt

# Cách 2: Dùng Add-Content (Chuyên dụng để nối text)
Get-Content server.log | Select-String "ERROR" | Add-Content -Path daily_report.txt -Encoding utf8
