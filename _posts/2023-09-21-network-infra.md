# Tổng quan thiết kế hệ thống IT doanh nghiệp điển hình, dành cho IT dưới 5 năm kinh nghiệm. ( Phần 1)

Hệ thống mạng được chia làm các phân vùng khác nhau bởi Core Switch Layer 3 License IP Basevà Firewall.

1. Vùng internet và Telecom sử dụng router cisco kết nối FTTH và E1.
2. Vùng WAN kết nối các site chi nhánh ở xa.
3. Vùng LAN kết nối máy tính, máy in, access point.
4. Vùng DMZ chứa các web server, Proxy (quản lý truy cập internet của User nội bộ), Email gateway vv
5. Vùng Server nơi chứa các server quan trọng, yêu cầu độ bảo mật cao hơn như file server, WLC, AD, DHCP DNS, Ứng dụng doanh nghiệp, Antivirus vv.
6. Thiết kế bảo mật :

- Luồng dữ liệu đi từ LAN tới vùng server đi qua core switch , qua firewall , Firewall chỉ mở các cổng cần thiết khi có yêu cầu truy cập đến từng server. Firewall có tính năng IDS/IPS thì càng giúp hệ thống có tính bảo mật tốt hơn.
- Luồng dữ liệu từ LAN đi internet sẽ qua core switch , qua proxy rồi ra internet. Proxy làm nhiệm vụ kiểm soát nội dung truy cập cũng như ngăn chặn lây lan Virus.
- Luồng dữ liệu đi từ LAN, Internet vào vùng DMZ cũng phải đi qua Firewall bảo vệ , giúp chống tấn công từ bên ngoài vào các cùng LAN và Server bên trong.
- Mọi PC , server đều phải cài đặt Antivirus và bản vá lỗi định kỳ hàng tháng.
- Hệ thống quản lý và định danh người dùng, máy trạm Active Directory cho phép áp dụng các chính sách (GPO) cũng như tùy chỉnh các setting của máy trạm 1 cách đồng bộ và nhanh chóng. Hệ thống AD cũng giúp quản trị viên sử dụng để phân quyền truy cập vào các hệ thống khác ( single signed on) .
- Toàn bộ máy trạm và server đều được cài đặt phần mềm antivirus và cập nhật bản vá định kỳ.
- Hệ thống wifi phát sóng 2 SSID , Guest và Staff, mạng Guest chỉ truy cập dc internet ko cần qua Firewall, mạng Staff truy cập được các vùng server và Internet qua proxy.

![Network Infra Topo](/assets/img/network_topo.jpg)
