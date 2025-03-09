# Cybersecurity Incident Report - TCP Logs

## Log Data and My Notes

[Full Log Data](https://docs.google.com/spreadsheets/d/1enpRzrIao3J2Lp2tOI0hmu1Cu7D7CjLGhFAiTiR9J64/edit?gid=218501934#gid=218501934)

---

### Log Details:

| No. | Time      | Source             | Destination        | Protocol | Info                                          |
|-----|-----------|--------------------|--------------------|----------|-----------------------------------------------|
| 47  | 3.144521  | 198.51.100.23      | 192.0.2.1          | TCP      | 42584->443 [SYN] Seq=0 Win-5792 Len=120...    |
| 48  | 3.195755  | 192.0.2.1          | 198.51.100.23      | TCP      | 443->42584 [SYN, ACK] Seq=0 Win-5792 Len=120... |
| 49  | 3.246989  | 198.51.100.23      | 192.0.2.1          | TCP      | 42584->443 [ACK] Seq=1 Win-5792 Len=120...    |
| 50  | 3.298223  | 198.51.100.23      | 192.0.2.1          | HTTP     | GET /sales.html HTTP/1.1                      |
| 51  | 3.349457  | 192.0.2.1          | 198.51.100.23      | HTTP     | HTTP/1.1 200 OK (text/html)                   |
| 52  | 3.390692  | 203.0.113.0        | 192.0.2.1          | TCP      | 54770->443 [SYN] Seq=0 Win=5792 Len=0...      |
| 53  | 3.441926  | 192.0.2.1          | 203.0.113.0        | TCP      | 443->54770 [SYN, ACK] Seq=0 Win-5792 Len=120... |
| 54  | 3.49316   | 203.0.113.0        | 192.0.2.1          | TCP      | 54770->443 [ACK Seq=1 Win=5792 Len=0...       |

---

### Key Observations:

- **IP 198.51.100.23**: Employee
- **IP 192.0.2.1**: Website

### Explanation:

- **TCP (Transport Layer)**:
  - **SYN (Synchronize)**: Employee's request to access the website.
  - **SYN, ACK (Synchronize Acknowledge)**: Web server's response agreeing to the connection.
  - **ACK (Acknowledge)**: Employee's machine acknowledging the permission to connect, concluding the three-way handshake and establishing the connection.
  
- **HTTP (Application Layer)**:
  - **GET /sales.html HTTP/1.1**: The employee is requesting the sales page on the website.

---

### Malicious Activity:

![Malicious Activity Diagram](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Cybersecurity%20Incident%20Report%20-%20TCP%20Logs%20WL/green%20and%20red%20tcp%20log.png)

All in **green** are legitimate SYN requests, while the one in **red** is a malicious attack, indicated by the continuous SYN requests even after the server has acknowledged the connection.

---

### Analysis:

#### Section 1: Identify the Type of Attack
The logs indicate a **DoS (Denial of Service) Attack**, specifically a **SYN Flood Attack**, where a single source (IP: `203.0.113.0`) sends an overwhelming number of SYN packets to the server. These packets simulate legitimate connections and exhaust the server's resources, causing a network timeout.

#### Section 2: Explain How the Attack Causes Malfunctions

- **The Three-Way Handshake**:
  1. **SYN**: The user's system sends a request packet to access the website.
  2. **SYN, ACK**: The server acknowledges the request.
  3. **ACK**: The user's system receives the acknowledgment, and the connection is established.
  
- **The Effect of Malicious SYN Packets**:
  When a malicious actor sends a large number of SYN packets, it consumes server resources (memory and CPU), preventing legitimate users from establishing connections. The server becomes overwhelmed, resulting in timeouts for actual users.

---
### Attack Indicators:
![SYN Flood Attack](https://github.com/WilliamLievesley/My-Cyber-Security-Projects/blob/main/Project%20List/images/Cybersecurity%20Incident%20Report%20-%20TCP%20Logs%20WL/green%20red%20and%20yellow%20tcp%20log.png)


- The continuous **SYN requests** from IP `203.0.113.0` indicate a **SYN Flood Attack**.
- The server begins to show signs of struggle, with the server unable to respond to new incoming requests due to resource exhaustion indicated with the logs in **yellow**.

---

#### Mitigation:

- **Rate-Limiting**: Implement rate limiting to mitigate SYN Flood Attacks.
- **Intrusion Detection Systems (IDS)**: Set up IDS to detect unusual SYN traffic patterns.
- **Firewall Rules**: Configure firewalls to block suspicious IP addresses or excessive SYN packets.

---

### Conclusion:

The **SYN Flood Attack** was successfully identified based on the continuous SYN requests from a single IP address, overwhelming the web server's capacity. Mitigation strategies have been proposed, including the use of firewalls, IDS, and rate limiting to prevent future attacks.

---


