# DHCP

## 1. DHCP là gì?
DHCP (Dynamic Host Configuration Protocol) là giao thức cấu hình máy chủ để cấp IP tự động.

Giao thức được định nghĩa trong [RFC 2131](https://tools.ietf.org/html/rfc2131)

<img src="http://i.imgur.com/RSfFKVh.png">

## 2. Nguyên tắc hoạt động 
<img src="http://i.imgur.com/1scgLHe.png">
 
- **Bước 1:** Client bật lên, gửi Broadcast thông điệp DHCP Discover 
- **Bước 2:** DHCP server gửi lại thông điệp DHCP Offer theo hình thức UDP Broadcast. Thông điệp mang theo IP address được cấp cho client, Lease time, DHCP Server IP
- **Bước 3:** Client gửi DHCP Request đến DHCP Server để hoặc là yêu cầu các thông số từ server, hoặc xác nhận tính đúng đắn của địa chỉ được cấp, hoặc gia hạn hợp đồng trên địa chỉ mạng đặc biệt.
- **Bước 4:** DHCP Server chấp nhận request, xử lý và trả về thông điệp DHCP Ack 

## 3. Các thông điệp DHCP
- *DHCP discover* : Client bắt đầu gia nhập mạng, nó yêu cầu IP từ server bằng cách broadcast thông điệp discover mang địa chỉ MAC và tên máy tính. 
- *DHCP offer* : Server nhận được DHCP discover sẽ trả về gói DHCP offer chứa IP và một vài thông tin như IP server DHCP, subnet mark, default gateway, lease time.
- *DHCP request* : Sau khi nhận được DHCP offer đầu tiên, nó gửi DHCP request về cho DHCP server gửi offer cho nó.
- *DHCP ack* : DHCP server được chọn chấp nhận request từ Client và gửi về 1 gói DHCP ack với những thông số cấu hình.
- *DHCP nak* : Server chỉ ra địa chỉ mà thông báo với client không đúng (do client chuyển sang một mạng con khác hoặc quá thời hạn), server sẽ gửi về gói DHCP nak.
- *DHCP deline* : Nếu DHCP Client chỉ ra địa chỉ mạng đã được sử dụng, client sẽ gửi gói DHCP deline cho server.
- *DHCP release* : Client từ chối địa chỉ mạng và hủy bỏ hợp đồng, nó sẽ gửi cho server gói DHCP release.
- *DHCP inform* : Client hỏi những thông số cấu hình cục bộ, nó gửi gói DHCP inform cho server.

## 4. Cấu trúc gói tin DHCP

<img src="http://i.imgur.com/4Mamx2I.png">

| Tên trường | Mô tả |
|------------|-------|
| Op | **Operation Code** : thông điệp yêu cầu : 1, thông điệp phản hồi : 2 |
| HType | **Hardware Type** : công nghệ mạng được sử dụng (vd '1'= 10Mb Ethernet) |
| HLen | **Hardware Address Length** : chiều dài của địa chỉ phần cứng (vd '6'- 10Mb Ethernet |
| Hops | **Hops** : client đặt bằng 0 |
| XID | **Transaction Identifier** : một số ngẫu nhiên để liên kết các thông điệp yêu cầu với phản hồi | 
| Secs | **Senconds** : Số giây trôi qua từ lúc client gửi yêu cầu |
| Flags | **Flags** : Gồm Broadcast Flag (1 bit) và Reserved (15 bits) |
| CIAddr | **Client IP Address** : client đặt địa chỉ IP của nó vào trường này nếu nó hợp lệ khi BOUND, RENEWING hoặc REBINDING còn không thì đặt 0 |
| YIAddr | **Your IP Address** : Địa chỉ server giao cho client |
| SIAddr | **Server IP Address** : Địa chỉ server mà client nên sử dụng cho các bước tiếp theo |
| CHAddr | **Client Hardware Address** : Địa chỉ phần cứng của client, sử dụng để truyền thông | 
| SName | **Server Name** : server gửi `DHCP offer` hoặc `DHCP ack` có thể đắt tên của nó trong trường này |
| File | **Boot Filename** : client yêu cầu một loại file boot trong `DHCP Discover` |
| Options | **Options** : một vài thông số cho hoạt động của DHCP | 

## 5. Các thuật ngữ liên quan
- *DHCP server* : là những máy chủ quản lý việc cấp phát và cấu hình địa chỉ IP
- *DHCP client* : là những máy trạm thực hiện xin thuê hợp đồng sử dụng địa chỉ IP
- *scope* : phạm vi liên tiếp các IP dành cho một mạng 
- *exclusion scope*: dải địa chỉ trong scope không được cấp phát cho client
- *reservation* : địa chỉ IP dành riêng cho một đối tượng nào đó
- *scope options* : các thông số được cấu hình thêm khi cấp IP động cho Client

## 6. Tham khảo 
Book : Computer Networking A Top-Down Approach 6th-edition - Kurose Ross.

https://tools.ietf.org/html/rfc2131

http://vdo.vn/cong-nghe-thong-tin/cac-khai-niem-co-ban-ve-dhcp.html