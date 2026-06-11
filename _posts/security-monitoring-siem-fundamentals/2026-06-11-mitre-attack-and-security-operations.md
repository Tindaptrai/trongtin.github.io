---
layout: post
title: "MITRE ATT&CK và Security Operations"
date: 2026-06-11 02:00:00 +0700
categories: [SOC, MITRE ATTACK]
tags: [soc, mitre-attack, ttp, detection-engineering, threat-intelligence, red-team, blue-team, soc-maturity, gap-analysis]
description: "Ghi chú tổng hợp về MITRE ATT&CK trong Security Operations: định nghĩa, tactic, technique, TTP và các use case như detection, response, gap analysis, threat intelligence, red teaming và SOC maturity assessment."
toc: true
---

# MITRE ATT&CK và Security Operations

**MITRE ATT&CK** là một framework rất quan trọng trong SOC và Blue Team.

Framework này giúp security team hiểu rõ attacker thường làm gì, dùng kỹ thuật nào, mục tiêu của từng hành vi là gì và nên phát hiện/phản ứng ra sao.

Nói ngắn gọn:

```text
MITRE ATT&CK = bản đồ hành vi attacker.
```

---

## 1. MITRE ATT&CK là gì?

**MITRE ATT&CK** là viết tắt của:

```text
Adversarial Tactics, Techniques, and Common Knowledge
```

Đây là một knowledge base mô tả các chiến thuật, kỹ thuật và quy trình mà threat actor sử dụng trong các cuộc tấn công mạng.

MITRE ATT&CK giúp các chuyên gia an ninh mạng:

- Hiểu hành vi attacker.
- Phân loại kỹ thuật tấn công.
- Xây dựng detection rule.
- Mapping alert với tactic/technique.
- Đánh giá coverage phòng thủ.
- Hỗ trợ threat hunting.
- Hỗ trợ incident response.
- Hỗ trợ red team và purple team exercise.

---

## 2. Tactic, Technique và Procedure là gì?

MITRE ATT&CK thường được hiểu thông qua ba khái niệm chính:

```text
Tactic
Technique
Procedure
```

---

## 2.1. Tactic

**Tactic** là mục tiêu cấp cao của attacker.

Nó trả lời câu hỏi:

```text
Attacker muốn đạt được điều gì?
```

Ví dụ tactic:

- Reconnaissance.
- Initial Access.
- Execution.
- Persistence.
- Privilege Escalation.
- Defense Evasion.
- Credential Access.
- Discovery.
- Lateral Movement.
- Command and Control.
- Exfiltration.
- Impact.

Ví dụ:

```text
Tactic: Credential Access
Mục tiêu: lấy credential từ hệ thống.
```

---

## 2.2. Technique

**Technique** là cách attacker thực hiện để đạt được tactic.

Nó trả lời câu hỏi:

```text
Attacker làm bằng cách nào?
```

Ví dụ:

| Tactic | Technique | ID |
|---|---|---|
| Execution | Command and Scripting Interpreter: PowerShell | T1059.001 |
| Credential Access | OS Credential Dumping | T1003 |
| Lateral Movement | Remote Services | T1021 |
| Command and Control | Ingress Tool Transfer | T1105 |
| Exfiltration | Exfiltration Over Web Service | T1567 |

Ví dụ:

```text
Tactic: Execution
Technique: PowerShell
ID: T1059.001
```

---

## 2.3. Procedure

**Procedure** là cách triển khai cụ thể của attacker trong thực tế.

Nó trả lời câu hỏi:

```text
Attacker cụ thể đã dùng tool, command hoặc workflow nào?
```

Ví dụ:

```text
Technique: PowerShell
Procedure: attacker chạy powershell.exe -enc để thực thi command bị mã hóa base64.
```

Hoặc:

```text
Technique: OS Credential Dumping
Procedure: attacker dùng Mimikatz để dump credential từ LSASS.
```

---

## 3. TTP là gì?

**TTP** là viết tắt của:

```text
Tactics, Techniques and Procedures
```

TTP mô tả đầy đủ hành vi của attacker ở nhiều mức độ.

| Thành phần | Ý nghĩa |
|---|---|
| Tactic | Mục tiêu attacker muốn đạt được |
| Technique | Cách attacker đạt mục tiêu |
| Procedure | Cách triển khai cụ thể trong thực tế |

Ví dụ:

```text
Tactic: Credential Access
Technique: OS Credential Dumping: LSASS Memory
Procedure: Dùng Mimikatz dump LSASS
```

---

# 4. MITRE ATT&CK Matrix

MITRE ATT&CK được tổ chức theo dạng ma trận.

Mỗi ma trận gồm:

- Các cột là tactic.
- Các ô trong từng cột là technique.
- Một technique có thể có nhiều sub-technique.

MITRE có nhiều matrix cho các môi trường khác nhau, ví dụ:

- Enterprise.
- Mobile.
- Cloud.
- ICS.

Điều này giúp security team phân tích attacker theo đúng môi trường hoạt động.

---

## 5. Vì sao MITRE ATT&CK quan trọng trong SOC?

Trong SOC, alert thường đến từ nhiều nguồn:

- SIEM.
- EDR.
- IDS/IPS.
- Firewall.
- Email gateway.
- Cloud logs.
- Authentication logs.

Nếu chỉ nhìn alert riêng lẻ, analyst có thể khó hiểu attacker đang cố làm gì.

MITRE ATT&CK giúp chuẩn hóa cách mô tả hành vi.

Ví dụ:

```text
Alert: powershell.exe -enc detected
MITRE: T1059.001 - PowerShell
Tactic: Execution
```

Hoặc:

```text
Alert: LSASS memory access
MITRE: T1003.001 - LSASS Memory
Tactic: Credential Access
```

Nhờ vậy, SOC có thể hiểu alert trong ngữ cảnh attack chain.

---

# 6. MITRE ATT&CK Use Cases trong Security Operations

MITRE ATT&CK không chỉ là tài liệu tham khảo. Nó có rất nhiều use case trong Security Operations.

Các use case chính:

- Detection and Response.
- Security Evaluation and Gap Analysis.
- SOC Maturity Assessment.
- Threat Intelligence.
- Cyber Threat Intelligence Enrichment.
- Behavioral Analytics Development.
- Red Teaming and Penetration Testing.
- Training and Education.

---

## 6.1. Detection and Response

MITRE ATT&CK giúp SOC xây dựng kế hoạch phát hiện và phản ứng dựa trên TTP đã biết của attacker.

Ví dụ:

| Hành vi | MITRE ID | Detection idea |
|---|---|---|
| PowerShell encoded command | T1059.001 | Alert khi command line có `-enc` |
| LSASS dump | T1003.001 | Alert khi process lạ truy cập LSASS |
| RDP lateral movement | T1021.001 | Alert đăng nhập RDP bất thường |
| Web shell | T1505.003 | Alert file script lạ trong web directory |
| Data encryption | T1486 | Alert nhiều file bị rename/encrypt |

MITRE giúp SOC trả lời:

```text
Kỹ thuật này nên phát hiện bằng log nào?
Cần response playbook nào?
Nếu thấy technique này, attacker có thể làm gì tiếp?
```

---

## 6.2. Security Evaluation and Gap Analysis

**Gap Analysis** là đánh giá khoảng trống phòng thủ.

Tổ chức có thể dùng MITRE ATT&CK để kiểm tra:

- Technique nào đã có detection.
- Technique nào chưa có detection.
- Technique nào chỉ có visibility nhưng chưa có alert.
- Technique nào có alert nhưng false positive cao.
- Technique nào cần thêm log source.
- Control nào cần đầu tư thêm.

Ví dụ bảng coverage:

| Technique | Có log? | Có detection? | Ghi chú |
|---|---|---|---|
| T1059.001 PowerShell | Có | Có | Cần tuning false positive |
| T1003.001 LSASS Dump | Có | Có | EDR coverage tốt |
| T1021.002 SMB Admin Shares | Có | Chưa | Cần thêm rule |
| T1567 Exfiltration over Web Service | Một phần | Chưa | Cần proxy/DLP logs |

Gap analysis giúp tổ chức ưu tiên đầu tư vào security control đúng chỗ.

---

## 6.3. SOC Maturity Assessment

MITRE ATT&CK có thể dùng để đánh giá mức độ trưởng thành của SOC.

Một SOC trưởng thành không chỉ có tool, mà còn cần:

- Có log visibility.
- Có detection coverage.
- Có response playbook.
- Có threat hunting use case.
- Có mapping với MITRE.
- Có đo lường hiệu quả detection.
- Có cải thiện liên tục.

Ví dụ mức trưởng thành:

| Mức | Đặc điểm |
|---|---|
| Low | Có alert nhưng không map MITRE |
| Medium | Có một số detection map MITRE |
| High | Có ATT&CK coverage matrix và playbook |
| Advanced | Dùng ATT&CK cho hunting, purple team, gap analysis và metrics |

---

## 6.4. Threat Intelligence

MITRE ATT&CK cung cấp ngôn ngữ chung để mô tả hành vi attacker.

Threat intelligence analyst có thể dùng ATT&CK để mô tả:

- Threat actor dùng tactic nào.
- Malware thường dùng technique nào.
- Campaign nhắm vào ngành nào.
- TTP nào đang phổ biến.
- TTP nào liên quan đến tổ chức.

Ví dụ:

```text
Threat actor A thường dùng:
- T1566 Phishing
- T1059 PowerShell
- T1003 Credential Dumping
- T1021 Remote Services
```

SOC có thể dùng thông tin này để kiểm tra detection coverage.

---

## 6.5. Cyber Threat Intelligence Enrichment

**CTI Enrichment** là làm giàu threat intelligence bằng context.

MITRE ATT&CK giúp bổ sung context cho IOC.

Ví dụ nếu có IOC:

```text
malicious-domain.example
```

IOC chỉ cho biết một domain đáng ngờ.

Nhưng khi gắn với ATT&CK:

```text
Tactic: Command and Control
Technique: Application Layer Protocol
```

SOC hiểu rõ hơn domain đó liên quan đến hành vi C2.

Điều này giúp decision-making tốt hơn:

- Có nên block domain không?
- Có nên hunt host đã kết nối không?
- Có thể attacker đang ở giai đoạn C2 không?
- Có cần kiểm tra exfiltration không?

---

## 6.6. Behavioral Analytics Development

MITRE ATT&CK có thể dùng để phát triển behavioral analytics.

Thay vì chỉ detect theo IOC như hash hoặc IP, SOC có thể detect theo hành vi.

Ví dụ:

```text
Technique: PowerShell
Behavior:
- powershell.exe chạy từ winword.exe
- command có -enc
- tạo network connection
- tải payload từ Internet
```

Detection theo behavior khó né hơn detection theo IOC.

Ví dụ behavioral analytic:

```text
Parent process = winword.exe
Child process = powershell.exe
Command line contains = -enc or DownloadString
Network connection = external IP
```

Mapping ATT&CK:

```text
Tactic: Execution
Technique: T1059.001 PowerShell
```

---

## 6.7. Red Teaming and Penetration Testing

MITRE ATT&CK giúp Red Team và Pentester mô phỏng kỹ thuật attacker thực tế một cách có hệ thống.

Red Team có thể chọn các technique trong ATT&CK để kiểm tra:

- SOC có phát hiện không.
- EDR có alert không.
- SIEM rule có hoạt động không.
- Response playbook có hiệu quả không.
- Logging có đủ không.

Ví dụ bài test:

```text
Execute T1059.001 PowerShell
Execute T1003.001 LSASS Dump
Execute T1021.001 RDP Lateral Movement
Check if Blue Team detects and responds
```

---

## 6.8. Training and Education

MITRE ATT&CK là tài nguyên rất tốt để đào tạo security professional.

Dùng để học:

- Attacker lifecycle.
- Tactic và technique.
- TTP phổ biến.
- Detection engineering.
- Threat hunting.
- Incident response.
- Red team / Blue team collaboration.
- Security operations maturity.

Ví dụ bài học:

```text
Hôm nay học Credential Access tactic.
Tập trung vào T1003 OS Credential Dumping.
Tìm log source, detection idea và response action.
```

---

# 7. Ví dụ mapping alert với MITRE ATT&CK

| Alert | Tactic | Technique | ID |
|---|---|---|---|
| Word spawn PowerShell | Execution | PowerShell | T1059.001 |
| Mimikatz detected | Credential Access | OS Credential Dumping | T1003 |
| RDP login from unusual host | Lateral Movement | Remote Services: RDP | T1021.001 |
| Suspicious DNS beaconing | Command and Control | Application Layer Protocol: DNS | T1071.004 |
| Large upload to cloud storage | Exfiltration | Exfiltration Over Web Service | T1567 |
| Ransomware file encryption | Impact | Data Encrypted for Impact | T1486 |

---

# 8. MITRE ATT&CK trong Incident Response

Khi xử lý incident, MITRE ATT&CK giúp analyst mô tả attack chain rõ ràng hơn.

Ví dụ:

```text
Initial Access: Phishing - T1566
Execution: PowerShell - T1059.001
Persistence: Registry Run Keys - T1060/T1547.001
Credential Access: LSASS Dump - T1003.001
Lateral Movement: RDP - T1021.001
Command and Control: DNS - T1071.004
Impact: Data Encrypted for Impact - T1486
```

Nhờ mapping này, incident report dễ hiểu hơn cho cả SOC, management và các team kỹ thuật.

---

# 9. MITRE ATT&CK và Detection Engineering

Detection Engineer có thể dùng MITRE ATT&CK để xây dựng detection backlog.

Ví dụ quy trình:

```text
Select high-risk threat actor
      ↓
Identify common ATT&CK techniques
      ↓
Check available log sources
      ↓
Build detection logic
      ↓
Test with lab/purple team
      ↓
Tune false positives
      ↓
Document detection coverage
```

Detection rule nên có:

- ATT&CK tactic.
- ATT&CK technique ID.
- Log source.
- Query/rule logic.
- Severity.
- False positive notes.
- Response recommendation.

---

# 10. ATT&CK Coverage Matrix

Một tổ chức có thể tạo ATT&CK coverage matrix để biết mình detect được gì.

Ví dụ:

| Tactic | Technique | Visibility | Detection | Response |
|---|---|---|---|---|
| Execution | T1059.001 PowerShell | Good | Good | Playbook available |
| Persistence | T1547.001 Run Keys | Medium | Partial | Need tuning |
| Credential Access | T1003.001 LSASS | Good | Good | EDR response |
| Lateral Movement | T1021.002 SMB Admin Shares | Medium | Missing | Need rule |
| Exfiltration | T1567 Web Service | Low | Missing | Need proxy/DLP logs |

Mục tiêu không phải detect 100% mọi technique, mà là ưu tiên technique phù hợp với threat model của tổ chức.

---

# 11. Bảng ôn nhanh thuật ngữ

| Thuật ngữ | Nghĩa ngắn gọn |
|---|---|
| MITRE ATT&CK | Framework mô tả hành vi attacker |
| ATT&CK | Adversarial Tactics, Techniques, and Common Knowledge |
| TTP | Tactics, Techniques and Procedures |
| Tactic | Mục tiêu attacker muốn đạt được |
| Technique | Cách attacker đạt mục tiêu |
| Procedure | Cách attacker triển khai cụ thể |
| Matrix | Bảng tactic/technique |
| Enterprise Matrix | Ma trận cho môi trường enterprise |
| Detection Engineering | Xây dựng logic phát hiện |
| Gap Analysis | Phân tích khoảng trống phòng thủ |
| SOC Maturity | Mức độ trưởng thành của SOC |
| Threat Intelligence | Tình báo mối đe dọa |
| CTI Enrichment | Làm giàu threat intelligence |
| Behavioral Analytics | Phân tích hành vi |
| Red Teaming | Mô phỏng attacker thực tế |
| Penetration Testing | Kiểm thử xâm nhập |
| Coverage Matrix | Ma trận kiểm tra detection coverage |

---

# 12. Câu hỏi ôn tập

## 1. MITRE ATT&CK là gì?

MITRE ATT&CK là framework mô tả tactic, technique và procedure mà threat actor sử dụng trong các cuộc tấn công mạng.

## 2. Tactic khác technique như thế nào?

Tactic là mục tiêu attacker muốn đạt được. Technique là cách attacker dùng để đạt mục tiêu đó.

## 3. TTP là gì?

TTP là Tactics, Techniques and Procedures, mô tả mục tiêu, cách làm và cách triển khai cụ thể của attacker.

## 4. MITRE ATT&CK giúp gì cho SOC?

MITRE giúp SOC xây dựng detection, response, threat hunting, gap analysis, incident report và đánh giá maturity.

## 5. Gap Analysis với MITRE ATT&CK là gì?

Là quá trình kiểm tra tổ chức đã có visibility và detection cho technique nào, còn thiếu technique nào và cần cải thiện ở đâu.

## 6. Vì sao MITRE hữu ích cho Red Team?

Vì MITRE cung cấp danh sách kỹ thuật attacker thực tế, giúp Red Team mô phỏng attack có hệ thống và kiểm tra khả năng phòng thủ của tổ chức.

## 7. Behavioral Analytics liên quan gì đến MITRE?

MITRE giúp mapping hành vi attacker thành technique, từ đó SOC có thể xây dựng detection dựa trên behavior thay vì chỉ dựa vào IOC.

---

# Key Takeaway

MITRE ATT&CK là ngôn ngữ chung giữa SOC, Detection Engineering, Threat Intelligence, Incident Response, Red Team và Purple Team.

Framework này giúp security team hiểu attacker đang làm gì, mapping alert vào tactic/technique, đánh giá detection coverage, phát hiện gap và cải thiện năng lực phòng thủ.

Trong Security Operations, MITRE ATT&CK không chỉ là tài liệu tham khảo, mà là công cụ thực tế để xây dựng detection, response, threat hunting, gap analysis và SOC maturity assessment.

---

# References

- MITRE ATT&CK Enterprise Matrix: <https://attack.mitre.org/matrices/enterprise/>
- MITRE ATT&CK Tactics: <https://attack.mitre.org/tactics/enterprise/>
- MITRE ATT&CK Techniques: <https://attack.mitre.org/techniques/enterprise/>
