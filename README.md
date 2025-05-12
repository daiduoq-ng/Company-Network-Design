# README: Network System Design for O-UIT Company

## 1. General Overview
O-UIT, an outsourcing company, operates two main facilities in Ho Chi Minh City:

- **Headquarters (Thu Duc)**: Located in a modern 5-story building equipped with a large-scale Data Center for processing and storing data for international projects. This site houses management departments (CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager) and specialized teams (developers, testers). These teams handle complex software projects and technology solutions, meeting stringent standards from international partners.
- **Branch Office (District 3)**: Focuses on projects for the domestic market. Developers and testers at this location work flexibly to meet specific requirements of Vietnamese clients while collaborating closely with the headquarters to ensure project quality and timelines.

## 2. Project Background Information

### Customer Requirements Analysis

#### Headquarters (Thu Duc)
| **Requirement** | **Details** |
|-----------------|-------------|
| **Devices Used** | - Developers and Testers: Restricted to company desktop PCs; personal laptops are not permitted to access the company network.<br>- CEO, HR, Project Manager, Technical Manager, Business Analyst, IT Manager: Permitted to use personal laptops and access internal Wi-Fi via authenticated accounts. |
| **Internal Wi-Fi** | - Provide an internal Wi-Fi system with user authentication (via accounts).<br>- Designated for CEO, HR, Project Manager, Technical Manager, Business Analyst, and IT Manager. |
| **Public Wi-Fi** | - Provide a public Wi-Fi system for guests or low-security needs.<br>- Use a separate Internet connection from the internal Wi-Fi. |
| **Virtual Server Hardware** | - Deploy hardware systems to support application testing phases.<br>- Ensure high performance and scalability for future workload requirements. |
| **Cloud Services** | - Utilize cloud services for deploying applications during the staging phase, allowing clients to test before launch.<br>- Ensure security and data protection during cloud deployment. |
| **Internal Network Connectivity** | - Establish an internal network to connect departments, development teams, testers, servers, and internal applications. |

#### Branch Office (District 3)
| **Requirement** | **Details** |
|-----------------|-------------|
| **Devices Used** | - Developers and Testers: Restricted to company desktop PCs; personal laptops are not permitted to access the company network. |
| **Site-to-Site VPN** | - Implement a site-to-site VPN to deploy applications to the Data Center at the headquarters.<br>- Ensure high security and stable connectivity between the two locations. |
| **Wi-Fi** | - Provide a separate Wi-Fi system at the branch with independent Internet access, isolating work traffic from company PCs and public connections. |
| **Data Center Connectivity** | - Ensure stable connectivity between the branch and the Data Center at the headquarters for application deployment. |
| **Security Systems** | - Implement security solutions to ensure safe connectivity and access between the branch and headquarters, especially for the site-to-site VPN. |

### IP Address Allocation

#### Headquarters (Thu Duc)
| **Subnet** | **Size** | **Address** | **Subnet Mask** | **Broadcast** |
|------------|----------|-------------|-----------------|---------------|
| Guest Network | 50 | 192.168.1.0 | 255.255.255.192 | 192.168.1.63 |
| Developer | 50 | 192.168.1.64 | 255.255.255.192 | 192.168.1.127 |
| Tester | 50 | 192.168.1.128 | 255.255.255.192 | 192.168.1.191 |
| Virtual Server – Data Center | 248 | 192.168.1.192 | 255.255.255.0 | 192.168.10.255 |
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

#### Branch Office (District 3)
| **Subnet** | **Size** | **Address** | **Subnet Mask** | **Broadcast** |
|------------|----------|-------------|-----------------|---------------|
| Developer | 50 | 192.168.4.0/26 | 255.255.255.192 | 192.168.4.63 |
| Tester | 50 | 192.168.4.64/26 | 255.255.255.192 | 192.168.4.127 |
| Core Switch 1 – Router 1 | 2 | 192.168.4.128/30 | 255.255.255.252 | 192.168.4.131 |
| Core Switch 1 – Router 2 | 2 | 192.168.4.132/30 | 255.255.255.252 | 192.168.4.135 |
| Core Switch 2 – Router 1 | 2 | 192.168.4.136/30 | 255.255.255.252 | 192.168.4.139 |
| Core Switch 2 – Router 2 | 2 | 192.168.4.140/30 | 255.255.255.252 | 192.168.4.143 |

## 3. Network Topology Diagram
Below is the network topology diagram illustrating the connectivity structure between the headquarters (Thu Duc) and the branch office (District 3), including components such as routers, switches, the Data Center, and subnets (VLANs) for departments, Wi-Fi, and site-to-site VPN.

![Network Topology Diagram](./Network%20Topology%20Diagram.png)

## 4. Design Objectives
- Build a secure, high-performance, and scalable network system to meet the needs of both facilities.
- Ensure data security, particularly during application deployment via site-to-site VPN and cloud services.
- Clearly segregate network traffic between departments, workgroups, and guest access.
- Support stable connectivity between the branch office and the Data Center at the headquarters.

## 5. Technologies Used
- **Wi-Fi**: Internal Wi-Fi system with user authentication via RADIUS or LDAP. Public Wi-Fi uses a separate VLAN.
- **VPN**: Site-to-site VPN using IPsec or OpenVPN to ensure secure connectivity.
- **Cloud**: Utilize services like AWS, Azure, or Google Cloud with security measures (IAM, Firewall, Encryption).
- **Hardware**: Virtual servers using solutions like VMware or Hyper-V, ensuring performance and scalability.
- **Security**: Firewalls, IDS/IPS, and data encryption to protect connectivity and applications.

## 6. Implementation Plan
1. **Analysis and Design**:
   - Define network structure, VLAN, and IP allocation.
   - Select equipment (routers, switches, access points, servers).
2. **Infrastructure Setup**:
   - Establish the Data Center at the headquarters.
   - Configure Wi-Fi systems, VPN, and internal network connectivity.
3. **Testing**:
   - Test site-to-site VPN connectivity between the two facilities.
   - Ensure stable operation of internal and public Wi-Fi systems.
   - Evaluate virtual server performance and cloud application deployment.
4. **Deployment and Operation**:
   - Train employees on system usage.
   - Conduct regular network monitoring and maintenance.

## 7. Contributions
- For feedback or bug reports, please create an issue on GitHub or contact the development team.

## 8. License
Copyright Group 12 NT113.P11
