# WK2-Project-Work
# NETWORKWALKS SECURITY ASSESSMENT REPORT

**Project:** Network Reconnaissance and Security Assessment  
**Assessment Domain:** networkwalks.com  
**Assessment Environment:** Kali Linux and Windows  
**Assessment Date:** 11 November 2026  
**Prepared By:** IBRAHIM SORA  
**Programme:** CYBERSECURITY INTERNSHIP WK2 PROJECT

---

# 1. LIABILITY DISCLAIMER

This assessment was conducted for educational and cybersecurity learning purposes within an authorized testing scope.

The activities documented in this report involved information gathering against the specified assessment domain and network discovery within a private local network.

All tools were used for the purpose of understanding network infrastructure, identifying exposed services and interpreting security-related information.

The report documents observations obtained during the assessment. The presence of an open port, a disclosed technology or an identifiable service does not independently establish the existence of a security vulnerability.

The author accepts responsibility for ensuring that future testing activities remain within the boundaries of authorized systems and applicable laws.

---

# 2. INTRODUCTION

## 2.1 Background

Network reconnaissance is an important phase of cybersecurity assessments. It involves gathering information about a target's publicly accessible infrastructure, identifying network services and understanding how systems communicate.

During this practical exercise, I worked with several reconnaissance and network discovery tools to investigate DNS information, inspect HTTP response headers, identify a Web Application Firewall and examine devices connected to a private local network.

The exercise combined external information gathering with internal network discovery to provide practical exposure to different aspects of network security.

## 2.2 Objectives

The primary objectives were:

1. Perform DNS enumeration against networkwalks.com.
2. Identify publicly accessible DNS records.
3. Examine HTTP response headers and identify disclosed technologies.
4. Fingerprint the Web Application Firewall protecting the website.
5. Inspect the local network configuration.
6. Discover active devices within the local subnet.
7. Identify open, closed and filtered TCP ports.
8. Perform service and operating system detection.
9. Interpret the collected information and identify areas requiring further security review.

## 2.3 Scope of Assessment

The exercise covered two distinct environments.

**External reconnaissance**

- Domain: networkwalks.com
- DNS enumeration.
- DNS resolution.
- HTTP response header inspection.
- Web Application Firewall fingerprinting.

**Local network assessment**

- Subnet: 192.168.100.0/24
- Active host discovery.
- TCP port scanning.
- Service identification.
- Operating system fingerprinting.

The external website assessment and private network assessment are documented separately because they involve different targets and testing activities.

---

# 3. TOOLS USED

The following tools were used during the assessment.

| Tool | Purpose |
|---|---|
| Kali Linux | Main penetration testing environment |
| DNSRecon | DNS enumeration and record discovery |
| nslookup | Domain name resolution |
| curl | HTTP response header inspection |
| WAFW00F | Web Application Firewall identification |
| Nmap | Host discovery, port scanning and service detection |
| Zenmap | Graphical interface for Nmap |
| Windows Command Prompt | Local network configuration inspection |

## 3.1 Kali Linux

Kali Linux provided the main environment for carrying out the reconnaissance and network scanning activities.

It contains a collection of tools designed for security testing, information gathering and network analysis.

## 3.2 DNSRecon

DNSRecon was used to enumerate DNS records associated with the assessment domain.

It helped identify nameservers, mail servers, IP addresses, TXT records and service discovery records.

## 3.3 nslookup

The nslookup utility was used to resolve the assessment domain through a DNS resolver.

It provided a simple way to verify the IP address associated with the domain.

## 3.4 curl

The curl command-line utility was used to communicate with the website and retrieve its HTTP response headers.

The `-I` option requests response headers without downloading the complete webpage body.

## 3.5 WAFW00F

WAFW00F was used to identify the Web Application Firewall protecting the assessment website.

It analyzes HTTP responses to determine whether a recognizable firewall technology is present.

## 3.6 Nmap and Zenmap

Nmap was used for host discovery, TCP port scanning, service identification and operating system detection.

Zenmap provided a graphical interface for configuring scans and reviewing their results.

## 3.7 Windows Command Prompt

Windows Command Prompt was used to inspect the local network configuration through the `ipconfig /all` command.

This helped establish the IP addressing information and subnet used during the local network assessment.

---

# 4. ACTIVITIES PERFORMED

## 4.1 DNS Enumeration Using DNSRecon

### Objective

The objective of this activity was to identify DNS records associated with networkwalks.com and understand the information publicly exposed through DNS services.

### Command Executed

```bash
dnsrecon -d networkwalks.com
```

### Procedure

I opened the Kali Linux terminal and executed DNSRecon against the assessment domain.

The tool performed general DNS enumeration and attempted to retrieve different record types associated with the domain.

### Results Obtained

The enumeration returned the following information:

| Record | Result |
|---|---|
| SOA | ns6135.hostgator.com |
| NS | ns6135.hostgator.com |
| NS | ns6136.hostgator.com |
| A | 192.232.216.135 |
| MX | mail.networkwalks.com |
| TXT | SPF policy and Google verification |
| SRV | cPanel autodiscovery services |

The tool also disclosed the reported BIND version:

`9.16.23-RH`

The enumeration returned eight records.

An error was also recorded during the DNSSEC query:

`No answer for DNSSEC query for networkwalks.com`

### Analysis

The results provided information about the domain's DNS infrastructure.

The SOA and NS records identified the authoritative DNS infrastructure, while the MX record provided information about the domain's mail handling.

The TXT records included an SPF policy and a Google site-verification entry.

The SRV records revealed cPanel-related email autodiscovery services.

The DNSSEC query error indicates that the attempted query did not receive an answer. It does not, by itself, prove that DNSSEC is disabled or that the domain is vulnerable.

### Security Implications

Public DNS records are necessary for normal internet operations. However, excessive disclosure of infrastructure details may assist attackers during reconnaissance.

The reported DNS software version is also information that could be used to guide further research.

### Recommendation

The domain administrator should periodically review publicly exposed DNS records and ensure that DNS services and software remain properly maintained.

**Evidence:** `dnsrecon -d networkwalks.com.png`

---

## 4.2 DNS Resolution Using nslookup

### Objective

The objective was to verify the IP address associated with networkwalks.com using an external DNS resolver.

### Command Executed

```bash
nslookup networkwalks.com
```

### Results Obtained

The command returned:

```text
Server: 8.8.8.8
Address: 8.8.8.8#53

Non-authoritative answer:

Name: networkwalks.com
Address: 192.232.216.135
```

### Analysis

The DNS query was sent to Google's public DNS resolver at 8.8.8.8.

The resolver returned a non-authoritative answer associating networkwalks.com with the IPv4 address 192.232.216.135.

A non-authoritative answer means that the response was supplied by a recursive resolver rather than directly by the domain's authoritative nameserver.

The result was consistent with the A record identified during DNSRecon enumeration.

### Security Implications

Identifying the public IP address provides a starting point for understanding the domain's hosting infrastructure.

This information is publicly available and is not a vulnerability by itself.

### Recommendation

Domain administrators should maintain accurate DNS configurations and regularly review their DNS records.

**Evidence:** `nslookup networkwalks.com.png`

---

## 4.3 HTTP Header Analysis Using curl

### Objective

The objective was to inspect the HTTP response headers returned by the website and identify information disclosed by the web server.

### Command Executed

```bash
curl -I https://networkwalks.com
```

### Results Obtained

The server returned:

```text
HTTP/2 200
server: Apache
content-type: text/html; charset=UTF-8
referrer-policy: no-referrer-when-downgrade
x-endurance-cache-level: 0
x-nginx-cache: WordPress
```

Additional headers included WordPress-related REST API links and a cookie with the Secure and HttpOnly attributes.

### Analysis

The HTTP/2 200 response indicated that the server successfully processed the request.

The `server` header identified Apache as the web server.

The response also contained links associated with the WordPress REST API:

```text
https://networkwalks.com/wp-json/
```

The response disclosed a WordPress-related page endpoint and a shortlink.

The cookie attributes were also examined:

- **Secure:** Restricts transmission of the cookie to secure HTTPS connections.
- **HttpOnly:** Prevents ordinary client-side JavaScript from reading the cookie.

The response included a Referrer-Policy header set to:

`no-referrer-when-downgrade`

This policy controls when browser referrer information is sent during navigation.

### Security Implications

HTTP response headers can disclose useful information about the server environment and the technologies used by a website.

The presence of WordPress-related endpoints and server identification information may help an attacker understand the technology stack.

However, technology disclosure does not automatically indicate a vulnerability.

### Recommendation

The website administrator should:

1. Review server information disclosure.
2. Keep WordPress and its components updated.
3. Review the configuration of HTTP security headers.
4. Ensure cookies use appropriate security attributes.
5. Regularly review publicly accessible application endpoints.

**Evidence:** `curl -I networkwalks.com.png`

---

## 4.4 Web Application Firewall Identification Using WAFW00F

### Objective

The objective was to determine whether the assessment website was protected by a Web Application Firewall and identify the technology where possible.

### Command Executed

```bash
wafw00f networkwalks.com
```

### Tool Information

The assessment used WAFW00F version 2.4.2.

### Results Obtained

The tool returned:

```text
[*] Checking https://networkwalks.com

[+] The site https://networkwalks.com
is behind ModSecurity (SpiderLabs) WAF.

[~] Number of requests: 2
```

### Analysis

The result identified ModSecurity (SpiderLabs) as the Web Application Firewall protecting the website.

A Web Application Firewall monitors HTTP traffic and can apply security rules to detect and block suspicious requests targeting web applications.

The identification was achieved after two requests.

### Security Implications

The presence of a WAF represents an additional security control that may help protect the application against malicious web requests.

However, identifying a WAF does not establish that every attack is blocked or that the application itself is secure.

### Recommendation

The website administrator should ensure that:

1. WAF rules are maintained and updated.
2. Security logs are monitored.
3. False positives and false negatives are reviewed.
4. The underlying web application remains patched.
5. WAF protection is treated as an additional layer rather than a replacement for secure application development.

**Evidence:** `wafw00f networkwalks.com.png`

---

## 4.5 Local Network Configuration Inspection

### Objective

The objective was to identify the IP addressing configuration of the Windows machine before performing local network discovery.

### Command Executed

```cmd
ipconfig /all
```

### Results Obtained

The Windows network configuration showed:

- Wireless network adapter: Intel Dual Band Wireless-AC 8265.
- IPv4 address: 192.168.100.42.
- Subnet mask: 255.255.255.0.
- Default gateway: 192.168.100.1.

The subnet mask indicated a /24 network.

### Analysis

The IPv4 address and subnet mask established that the local machine was operating within the:

`192.168.100.0/24`

network.

The default gateway was identified as:

`192.168.100.1`

This information was used to determine the local network range for subsequent discovery activities.

### Security Implications

Understanding the local network configuration is important before conducting network discovery.

It helps establish the correct scope and prevents scanning unrelated network ranges.

### Recommendation

Network administrators should maintain proper network documentation and ensure that network discovery activities remain within authorized address ranges.

**Evidence:** `local IP adrr & LAN subnet.png`

---

## 4.6 Network Discovery Using Nmap

### Objective

The objective was to identify active devices within the local subnet and determine which hosts were reachable.

### Network Range

```text
192.168.100.0/24
```

### Results Obtained

The Nmap report indicated:

```text
Nmap done:
256 IP addresses (3 hosts up) scanned
```

Three active hosts were identified:

| IP Address | Observation |
|---|---|
| 192.168.100.1 | Local network gateway |
| 192.168.100.42 | Windows assessment machine |
| 192.168.100.87 | Additional active device |

### Analysis

Nmap successfully identified three active hosts within the scanned subnet.

The first host corresponded to the local gateway, while the second corresponded to the Windows machine used during the assessment.

The third host was identified as another active device.

Host discovery is useful because it establishes which devices are responding within a network before more detailed service analysis is performed.

### Security Implications

Unnecessary devices connected to a local network may increase the overall attack surface.

Unknown or unmanaged devices should therefore be identified and reviewed.

### Recommendation

Network administrators should maintain an inventory of connected devices and investigate systems that cannot be identified.

**Evidence:**
- `list of live hosts.png`
- `hosts live in subnet.png`
- `livehost IP(s).png`

---

## 4.7 Port Scanning and Service Identification

### Objective

The objective was to identify open TCP ports and determine the services associated with active devices.

Nmap was used to perform TCP SYN scanning, service detection and operating system fingerprinting.

### Results Obtained

The scan identified the following services.

### Host 1: 192.168.100.1

| Port | State | Service |
|---|---|---|
| 22/tcp | Open | TCP-wrapped |
| 23/tcp | Open | TCP-wrapped |
| 53/tcp | Open | TCP-wrapped |
| 80/tcp | Open | TCP-wrapped |

The device was identified as Huawei-manufactured equipment.

Nmap reported Linux 3.5 as the operating system match.

### Host 2: 192.168.100.87

| Port | State | Service |
|---|---|---|
| 62078/tcp | Open | TCP-wrapped |

Nmap could not determine an exact operating system match for this host.

### Host 3: 192.168.100.42

| Port | State | Service |
|---|---|---|
| 53/tcp | Filtered | DNS |
| 135/tcp | Open | Microsoft RPC |
| 139/tcp | Open | NetBIOS Session Service |
| 445/tcp | Open | Microsoft SMB |

The service detection identified Microsoft Windows-related services.

The scan also identified the NetBIOS computer name and WORKGROUP membership.

### Analysis

The scan provided useful information about the services exposed by the identified devices.

The gateway exposed SSH, Telnet, DNS and HTTP services.

The additional device exposed TCP port 62078, although the service could not be identified beyond the TCP-wrapped classification.

The Windows machine exposed RPC, NetBIOS and SMB-related services.

Port 53 was reported as filtered on the Windows machine, meaning that Nmap could not determine whether the port was open or closed because packet filtering affected the result.

The scan also reported that SMB 3.1.1 message signing was enabled and required.

### Security Implications

The presence of Telnet on the gateway deserves attention because Telnet does not provide the same transport encryption as SSH.

The exposed administrative services should be reviewed to determine whether they are necessary and whether access is appropriately restricted.

The Windows RPC and SMB services should also be reviewed to ensure that they are not unnecessarily accessible to untrusted devices.

The presence of SMB message signing being required is a positive security observation, but it does not independently establish that the SMB configuration is completely secure.

### Recommendations

1. Disable Telnet if it is not required.
2. Prefer SSH for secure remote administration.
3. Restrict access to management interfaces.
4. Review SMB access controls and firewall rules.
5. Keep Windows systems updated.
6. Review RPC exposure and restrict access where appropriate.
7. Investigate unidentified devices and services.
8. Conduct further service-specific security testing where authorization permits.

**Evidence:**
- `list of live hosts.png`
- `hosts live in subnet.png`
- Zenmap scan results.
- Nmap report output.

---

# 5. RISK ANALYSIS AND IMPACT

The following table summarizes the observations that require further review.

| Observation | Potential Impact | Priority |
|---|---|---|
| Telnet accessible on gateway | Potential exposure of unencrypted remote administration | High |
| SSH accessible on gateway | Management interface requires access-control review | Medium |
| HTTP accessible on gateway | Router administration interface should be reviewed | Medium |
| SMB services accessible on Windows host | Potential exposure of file-sharing and network services | Medium |
| RPC accessible on Windows host | Requires appropriate network access restrictions | Medium |
| Server and WordPress information disclosed | May assist technology fingerprinting | Low |
| DNS infrastructure information disclosed | Provides information about domain infrastructure | Low |
| Additional unidentified device | Requires device identification and inventory review | Medium |

## 5.1 Risk Interpretation

The priorities above are preliminary review priorities, not confirmed vulnerability severity ratings.

The assessment did not demonstrate successful exploitation of any identified service.

### High Priority

The availability of Telnet on the gateway should be reviewed first because Telnet does not encrypt the communication channel.

Its presence does not prove that authentication is weak or that unauthorized access is possible.

### Medium Priority

SSH, HTTP, SMB and RPC services should be reviewed to establish whether they are required and whether access is limited to trusted systems.

### Low Priority

The information disclosed through DNS and HTTP responses may support reconnaissance but does not independently establish an exploitable weakness.

---

# 6. RECOMMENDATIONS

Based on the observations collected, the following improvements are recommended.

## 6.1 Network Device Security

- Disable unnecessary services.
- Disable Telnet where possible.
- Use SSH for secure remote administration.
- Restrict management access to trusted devices.
- Change default credentials where applicable.
- Keep router firmware updated.

## 6.2 Windows Network Security

- Review SMB access permissions.
- Restrict RPC and SMB traffic through firewall rules.
- Maintain current Windows security updates.
- Disable unnecessary network services.
- Review device discovery and sharing settings.

## 6.3 Web Application Security

- Keep WordPress, themes and plugins updated.
- Review exposed server information.
- Maintain appropriate HTTP security headers.
- Use secure cookie configurations.
- Review publicly accessible API endpoints.
- Maintain and monitor WAF security rules.

## 6.4 DNS Security

- Review publicly exposed DNS records.
- Maintain accurate DNS configurations.
- Investigate DNSSEC configuration and the observed query error.
- Review SPF and other email authentication policies.
- Keep DNS server software maintained.

## 6.5 Continuous Security Monitoring

- Conduct periodic network scans.
- Maintain an inventory of network devices.
- Monitor suspicious network activity.
- Review security logs.
- Perform authorized vulnerability assessments.

---

# 7. CONCLUSION

The assessment provided practical experience in network reconnaissance, DNS enumeration, HTTP analysis, Web Application Firewall identification and local network scanning.

Using Kali Linux and Windows networking utilities, I was able to collect information about the assessment domain, inspect HTTP response headers, identify ModSecurity as the detected WAF and resolve the domain to its public IP address.

The local network assessment identified three active hosts and revealed several accessible TCP services, including SSH, Telnet, DNS, HTTP, RPC, NetBIOS and SMB.

One of the most important lessons from this exercise was understanding that reconnaissance is not simply about collecting technical information. It also involves interpreting that information correctly and distinguishing between normal service exposure and genuine security weaknesses.

The exercise strengthened my practical understanding of network addressing, DNS infrastructure, TCP port scanning, service identification and security documentation.

It also reinforced the importance of responsible testing, accurate evidence collection and making recommendations based on observed results rather than assumptions.

---

# 8. EVIDENCES COLLECTED

The following screenshots and outputs support the activities documented in this report.

| Evidence | Description |
|---|---|
| `dnsrecon -d networkwalks.com.png` | DNS enumeration results |
| `nslookup networkwalks.com.png` | Domain resolution results |
| `curl -I networkwalks.com.png` | HTTP response headers |
| `wafw00f networkwalks.com.png` | WAF fingerprinting results |
| `local IP adrr & LAN subnet.png` | Windows IP configuration |
| `list of live hosts.png` | Active host discovery |
| `hosts live in subnet.png` | Hosts identified in the subnet |
| `livehost IP(s).png` | Live host IP information |
| `topology.pdf` | Network topology |
| `Nmap report.txt` | Detailed Nmap scan output |

## Final Statement

This report documents the activities performed and the observations collected during the practical assessment. The findings should be understood within the scope of the available evidence and should not be interpreted as confirmation of vulnerabilities that were not tested.

**End of Report.**
