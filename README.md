# WK2-Project-Work
## Project Overview

This project documents a practical cybersecurity exercise involving network reconnaissance, DNS enumeration, HTTP header analysis, Web Application Firewall (WAF) fingerprinting, host discovery and port scanning.

The assessment was conducted using Kali Linux and Windows-based networking tools. The objective was to understand how reconnaissance techniques can be used to collect information about a web domain and identify active devices and exposed services within a private local network.

The project focuses on understanding the information returned by security tools, interpreting the results and documenting security observations responsibly.

## Objectives

- Perform DNS enumeration against the assessment domain.
- Identify publicly available DNS records.
- Examine HTTP response headers.
- Identify the Web Application Firewall protecting the website.
- Perform local network discovery.
- Identify active hosts within the local subnet.
- Discover open, closed and filtered ports.
- Perform service and operating system detection.
- Document observations and provide security recommendations.

## Tools and Technologies

| Tool | Purpose |
|---|---|
| Kali Linux | Security testing environment |
| DNSRecon | DNS enumeration |
| nslookup | DNS resolution |
| curl | HTTP response header inspection |
| WAFW00F | Web Application Firewall fingerprinting |
| Nmap | Network discovery and port scanning |
| Zenmap | Graphical interface for Nmap |
| Windows Command Prompt | Network configuration inspection |

## Assessment Activities

### 1. DNS Enumeration

DNSRecon was used to enumerate DNS records associated with `networkwalks.com`.

The enumeration identified:

- SOA records.
- Nameserver records.
- A records.
- MX records.
- TXT records.
- SRV records.

The domain resolved to `192.232.216.135`.

### 2. DNS Resolution

The `nslookup` utility was used to query the domain through Google's public DNS resolver (`8.8.8.8`).

The returned non-authoritative answer resolved `networkwalks.com` to `192.232.216.135`.

### 3. HTTP Header Analysis

The command `curl -I https://networkwalks.com` was used to retrieve HTTP response headers.

The server returned an HTTP/2 200 response.

The headers disclosed information including:

- Apache server identification.
- WordPress-related REST API endpoints.
- Content type.
- Cookie attributes.
- Referrer policy.
- Cache-related headers.

### 4. WAF Fingerprinting

WAFW00F version 2.4.2 was used to identify the web application's firewall technology.

The tool identified ModSecurity (SpiderLabs) as the Web Application Firewall protecting the website.

The detection required two HTTP requests.

### 5. Local Network Discovery

The local network configuration was inspected using Windows `ipconfig /all`.

The evidence identified the local wireless network as using the `192.168.100.0/24` subnet.

Nmap was subsequently used to identify active hosts.

The scan covered 256 IP addresses and identified three active hosts.

### 6. Port Scanning and Service Detection

Nmap and Zenmap were used to identify open ports and associated services.

The recorded scan results included:

**192.168.100.1**
- TCP 22 – SSH
- TCP 23 – Telnet
- TCP 53 – DNS
- TCP 80 – HTTP

**192.168.100.87**
- TCP 62078 – TCP-wrapped service

**192.168.100.42**
- TCP 135 – Microsoft RPC
- TCP 139 – NetBIOS Session Service
- TCP 445 – Microsoft SMB
- TCP 53 – Filtered DNS

The scan also identified the first host as Linux-based and the third host as Windows-based.

## Key Observations

- DNS enumeration exposed multiple records associated with the domain.
- The HTTP response disclosed server and WordPress-related information.
- ModSecurity (SpiderLabs) was detected as the website's WAF.
- Three active devices were identified within the local subnet.
- The local network scan revealed services requiring appropriate access controls.
- SMB-related services were accessible on the Windows host.
- The router exposed SSH, Telnet, DNS and HTTP ports.

These observations are not automatically confirmed vulnerabilities. Further validation would be required to establish exploitability and security impact.

## Recommendations

1. Restrict administrative services such as SSH and Telnet to trusted management networks.
2. Disable Telnet where it is not required and use secure alternatives.
3. Review router administration interfaces and restrict unnecessary access.
4. Ensure SMB services are accessible only to trusted devices and networks.
5. Maintain updated operating systems and network device firmware.
6. Review DNS records and avoid unnecessary information disclosure.
7. Maintain appropriate HTTP security headers and review cookie configuration.
8. Keep the Web Application Firewall updated and properly configured.
9. Conduct periodic vulnerability assessments and security reviews.

## Repository Contents

- `report/` – Detailed assessment documentation.
- `evidence/` – Screenshots supporting the activities.
- `outputs/` – Original command outputs.
- `README.md` – Project overview and findings.

## Disclaimer

This project is documented for educational and cybersecurity portfolio purposes. All testing must be conducted only against systems for which appropriate authorization has been obtained.

The findings represent observations from the tools and evidence collected during the assessment. They should not be interpreted as confirmation of exploitable vulnerabilities without further validation.

## Learning Outcomes

This exercise strengthened practical understanding of:

- DNS architecture and enumeration.
- HTTP communication and response headers.
- Web Application Firewall identification.
- Network addressing and subnet discovery.
- TCP port scanning.
- Service identification.
- Operating system fingerprinting.
- Security evidence collection and documentation.

**Focus:** Network Reconnaissance | Network Security | Ethical Hacking | Information Gathering
