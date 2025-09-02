# IPv4 Access Control Lists (ACLs) Lab

## Tổng quan
Thư mục này chứa các lab và ví dụ về **Access Control Lists (ACLs) cho IPv4**.  
ACL được sử dụng trong mạng để **lọc gói tin** dựa trên địa chỉ IP và các thông số khác.  
Thư mục bao gồm cả **Standard ACL** và **Extended ACL**, kèm giải thích, ví dụ minh họa và bảng so sánh trực quan.

---

## 1. Standard IPv4 ACL

### Định nghĩa
- **Standard ACL** chỉ lọc traffic dựa trên **địa chỉ IP nguồn**.  
- Loại ACL này đơn giản nhưng ít linh hoạt hơn **Extended ACL**.

### Các loại Standard ACL
1. **ACL đánh số (Numbered ACL)**
   - Sử dụng **số** để xác định ACL.
   - Ví dụ:
     ```bash
     access-list 10 permit 192.168.1.0 0.0.0.255
     ```
   - Khoảng số phổ biến:
     - `1–99`: Standard IPv4 ACL
     - `1300–1999`: Standard ACL nâng cao (Cisco)

2. **ACL đặt tên (Named ACL)**
   - Sử dụng **tên** thay vì số.
   - Ví dụ:
     ```bash
     ip access-list standard BLOCK_SALES
       permit 192.168.1.0 0.0.0.255
     ```
   - Dễ nhớ và quản lý hơn ACL đánh số.
   - Có thể chỉnh sửa trực tiếp mà không cần xóa và tạo lại.

### Lưu ý
- Chỉ xét **IP nguồn**, không xét IP đích hay giao thức.  
- Nên áp dụng **gần với đích** để tránh vô tình chặn traffic hợp lệ.

---

## 2. Extended IPv4 ACL

### Định nghĩa
- **Extended ACL** lọc traffic dựa trên nhiều tiêu chí:
  - IP nguồn
  - IP đích
  - Giao thức (TCP, UDP, ICMP)
  - Cổng nguồn hoặc cổng đích

### Ví dụ
```bash
access-list 100 permit tcp 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255 eq 80
```
- Cho phép traffic TCP từ mạng `192.168.1.0/24` tới mạng `10.0.0.0/24` trên cổng 80 (HTTP).

### Lưu ý
- Extended ACL **linh hoạt hơn** vì kiểm soát chi tiết loại traffic.  
- Nên áp dụng **gần với nguồn** để tối ưu hiệu suất và bảo mật.

---

## 3. Bảng so sánh Standard vs Extended ACL


| Tiêu chí                 | Standard ACL                                          | Extended ACL                                               |
|---------------------------|------------------------------------------------------|-----------------------------------------------------------|
| Kiểm soát dựa trên        | Chỉ IP nguồn                                        | IP nguồn, IP đích, giao thức, port                        |
| Linh hoạt                 | Thấp                                                | Cao                                                       |
| Vị trí áp dụng tốt nhất   | Gần đích                                            | Gần nguồn                                                 |
| Ví dụ                     | access-list 10 permit 192.168.1.0 0.0.0.255        | access-list 100 permit tcp 192.168.1.0 0.0.0.255 10.0.0.0 0.0.0.255 eq 80 |
| Mục đích sử dụng          | Chặn hoặc cho phép mạng đơn giản                    | Kiểm soát chi tiết theo dịch vụ và cổng                  |

---

## 4. Tham khảo
- [Cisco ACL Documentation](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/sec_data_acl/configuration/15-s/sec-data-acl-15-s-book.html)

