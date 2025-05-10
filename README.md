# README: Thiết Kế Hệ Thống Mạng cho Công ty O-UIT

## 1. Giới thiệu tổng quan
Công ty Outsource O-UIT hiện có hai cơ sở chính tại Thành phố Hồ Chí Minh:

- **Trụ sở chính (Thủ Đức)**: Tọa lạc trong một tòa nhà 5 tầng hiện đại, được trang bị Data Center quy mô lớn để xử lý và lưu trữ dữ liệu cho các dự án quốc tế. Đây là nơi làm việc của các phòng ban quản lý (CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager) và các nhóm chuyên môn cao (developer, tester). Các nhóm này đảm nhiệm các dự án phần mềm và giải pháp công nghệ phức tạp, đáp ứng tiêu chuẩn khắt khe từ đối tác nước ngoài.
- **Chi nhánh (Quận 3)**: Tập trung vào các dự án phục vụ thị trường trong nước. Các nhóm developer và tester tại đây làm việc linh hoạt để đáp ứng yêu cầu đặc thù của khách hàng Việt Nam, đồng thời phối hợp chặt chẽ với trụ sở chính để đảm bảo chất lượng và tiến độ dự án.

## 2. Các thông tin cơ bản về đề tài

### Phân tích yêu cầu khách hàng

#### Trụ sở chính (Thủ Đức)
| **Yêu cầu** | **Nội dung** |
|-------------|--------------|
| **Thiết bị sử dụng** | - Developer và Tester: Chỉ được sử dụng máy bàn công ty, không được dùng laptop cá nhân để truy cập mạng công ty.<br>- CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager: Được sử dụng laptop cá nhân và truy cập Wi-Fi nội bộ qua tài khoản xác thực. |
| **Wi-Fi nội bộ** | - Cung cấp hệ thống Wi-Fi nội bộ với khả năng xác thực người dùng (tài khoản xác thực).<br>- Dành cho CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager. |
| **Wi-Fi công cộng** | - Hệ thống Wi-Fi công cộng dành cho khách hoặc nhu cầu không yêu cầu bảo mật cao.<br>- Kết nối Internet riêng biệt với Wi-Fi nội bộ. |
| **Hệ thống phần cứng cho server ảo** | - Triển khai hệ thống phần cứng để phục vụ việc triển khai ứng dụng trong giai đoạn test.<br>- Đảm bảo hiệu suất cao và khả năng mở rộng cho các yêu cầu công việc tương lai. |
| **Dịch vụ Cloud** | - Sử dụng dịch vụ Cloud để triển khai ứng dụng trong giai đoạn staging, cho phép khách hàng thử nghiệm trước khi ra mắt.<br>- Đảm bảo an toàn và bảo mật trong quá trình triển khai trên Cloud. |
| **Kết nối mạng nội bộ** | - Thiết lập mạng nội bộ để kết nối giữa các phòng ban, nhóm phát triển, tester, máy chủ và ứng dụng nội bộ. |

#### Chi nhánh (Quận 3)
| **Yêu cầu** | **Nội dung** |
|-------------|--------------|
| **Thiết bị sử dụng** | - Developer và Tester: Chỉ được sử dụng máy bàn công ty, không được dùng laptop cá nhân để truy cập mạng công ty. |
| **Kết nối VPN site-to-site** | - Sử dụng VPN site-to-site để triển khai ứng dụng lên Data Center tại trụ sở chính.<br>- Đảm bảo bảo mật cao và kết nối ổn định giữa hai điểm. |
| **Wi-Fi** | - Cung cấp hệ thống Wi-Fi riêng tại chi nhánh với kết nối Internet riêng biệt, phân tách lưu lượng công việc từ máy tính công ty với kết nối công cộng. |
| **Kết nối với Data Center** | - Đảm bảo kết nối ổn định giữa chi nhánh và Data Center tại trụ sở chính để triển khai ứng dụng. |
| **Hệ thống bảo mật** | - Áp dụng các giải pháp bảo mật để đảm bảo an toàn khi kết nối và truy cập giữa chi nhánh và trụ sở chính, đặc biệt với VPN site-to-site. |

### Phân bổ địa chỉ IP

#### Trụ sở chính
| **Subnet** | **Size** | **Address** | **Subnet Mask** | **Broadcast** |
|------------|----------|-------------|-----------------|---------------|
| Guest Network | 50 | 192.168.1.0 | 255.255.255.192 | 192.168.1.63 |
| Developer | 50 | 192.168.1.64 | 255.255.255.192 | 192.168.1.127 |
| Tester | 50 | 192.168.1.128 | 255.255.255.192 | 192.168.1.191 |
| Server ảo – Data Center | 248 | 192.168.1.192 | 255.255.255.0 | 192.168.10.255 |
| Core Switch 1 – Router 1 | 2 | 192.168.2.0 | 255.255.255.252 | 192.168.2.3 |
| Core Switch 1 – Router 2 | 2 | 192.168.2.4 | 255.255.255.252 | 192.168.2.7 |
| Core Switch 2 – Router 1 | 2 | 192.168.2.8 | 255.255.255.252 | 192.168.2.11 |
| Core Switch 2 – Router 2 | 2 | 192.168.2.12 | 255.255.255.252 | 192.168.2.15 |
| CEO | 10 | 192.168.2.16 | 255.255.255.240 | 192.168.2.31 |
| HR | 10 | 192.168.2.32 | 255.255.255.240 | 192.168.2.47 |
| Project Manager | 10 | 192.168.2.48 | 255.255.255.240 | 192.168.2.63 |
| Technical Manager | 10 | 192.168.2.64 | 255.255.255.240 | 192.168.2.79 |
| Business Analyst | 10 | 192.168.2.80 | 255.255.255.240 | 192.168.2.95 |
| IT Manager | 10 | 192.168.2.96 | 255.255.255.240 | 192.168.2.111 |
| Router 1 – RouterDC | 2 | 192.168.2.112 | 255.255.255.252 | 192.168.2.115 |
| Router 2 – RouterDC | 2 | 192.168.2.116 | 255.255.255.252 | 192.168.2.119 |
| VPN | 2 | 192.168.5.0 | 255.255.255.192 | - |

#### Chi nhánh (Quận 3)
| **Subnet** | **Size** | **Address** | **Subnet Mask** | **Broadcast** |
|------------|----------|-------------|-----------------|---------------|
| Developer | 50 | 192.168.4.0/26 | 255.255.255.192 | 192.168.4.63 |
| Tester | 50 | 192.168.4.64/26 | 255.255.255.192 | 192.168.4.127 |
| Core Switch 1 – Router 1 | 2 | 192.168.4.128/30 | 255.255.255.252 | 192.168.4.131 |
| Core Switch 1 – Router 2 | 2 | 192.168.4.132/30 | 255.255.255.252 | 192.168.4.135 |
| Core Switch 2 – Router 1 | 2 | 192.168.4.136/30 | 255.255.255.252 | 192.168.4.139 |
| Core Switch 2 – Router 2 | 2 | 192.168.4.140/30 | 255.255.255.252 | 192.168.4.143 |

## 3. Sơ đồ topo mạng
Dưới đây là sơ đồ topo mạng mô tả cấu trúc kết nối giữa trụ sở chính (Thủ Đức) và chi nhánh (Quận 3), bao gồm các thành phần như router, switch, Data Center, và các mạng con (VLAN) cho các phòng ban, Wi-Fi, và VPN site-to-site.

![Network Topology Diagram](./images/network-topology.png)

**Lưu ý**: Đảm bảo rằng file hình ảnh `network-topology.png` đã được tải lên thư mục `/images` trong repository GitHub của bạn. Nếu bạn cần hỗ trợ tạo sơ đồ topo mạng, bạn có thể sử dụng các công cụ như Cisco Packet Tracer hoặc Draw.io, sau đó xuất file ảnh và tải lên.

## 4. Mục tiêu thiết kế
- Xây dựng hệ thống mạng an toàn, hiệu suất cao và khả năng mở rộng để đáp ứng nhu cầu của cả hai cơ sở.
- Đảm bảo bảo mật dữ liệu, đặc biệt khi triển khai ứng dụng qua VPN site-to-site và dịch vụ Cloud.
- Phân tách rõ ràng lưu lượng mạng giữa các phòng ban, nhóm công việc và khách truy cập.
- Hỗ trợ kết nối ổn định giữa chi nhánh và Data Center tại trụ sở chính.

## 5. Công nghệ sử dụng
- **Wi-Fi**: Hệ thống Wi-Fi nội bộ sử dụng xác thực qua tài khoản (RADIUS hoặc LDAP). Wi-Fi công cộng sử dụng VLAN riêng.
- **VPN**: Kết nối site-to-site sử dụng IPsec hoặc OpenVPN để đảm bảo bảo mật.
- **Cloud**: Sử dụng các dịch vụ như AWS, Azure hoặc Google Cloud với các biện pháp bảo mật (IAM, Firewall, Encryption).
- **Phần cứng**: Server ảo sử dụng các giải pháp như VMware hoặc Hyper-V, đảm bảo hiệu suất và khả năng mở rộng.
- **Bảo mật**: Tường lửa, IDS/IPS, và mã hóa dữ liệu để bảo vệ kết nối và ứng dụng.

## 6. Kế hoạch triển khai
1. **Phân tích và thiết kế**:
   - Xác định cấu trúc mạng, phân bổ VLAN và IP.
   - Lựa chọn thiết bị (router, switch, access point, server).
2. **Cài đặt hạ tầng**:
   - Thiết lập Data Center tại trụ sở chính.
   - Cài đặt hệ thống Wi-Fi, VPN và kết nối mạng nội bộ.
3. **Kiểm thử**:
   - Kiểm tra kết nối VPN site-to-site giữa hai cơ sở.
   - Đảm bảo Wi-Fi nội bộ và công cộng hoạt động ổn định.
   - Test hiệu suất server ảo và triển khai ứng dụng trên Cloud.
4. **Triển khai và vận hành**:
   - Đào tạo nhân viên sử dụng hệ thống.
   - Giám sát và bảo trì mạng định kỳ.

## 7. Đóng góp
- Mọi ý kiến đóng góp hoặc báo cáo lỗi, vui lòng tạo issue trên GitHub hoặc liên hệ nhóm phát triển.

## 8. Giấy phép
Dự án được phát triển dưới giấy phép [MIT License](LICENSE).
