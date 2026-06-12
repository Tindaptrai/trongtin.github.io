---
layout: post
title: "SIEM Use Case Development"
date: 2026-06-11 03:00:00 +0700
categories: [SOC, SIEM]
tags: [soc, siem, use-case-development, detection-engineering, correlation-rule, sop, ttd, ttr, msbuild, mitre-attack, blue-team]
description: "Ghi chú tổng hợp về SIEM Use Case Development: khái niệm use case, vòng đời phát triển, cách xây dựng rule, SOP, TTD/TTR và ví dụ phát hiện MSBuild bị lạm dụng."
toc: true
---

# SIEM Use Case Development

**SIEM Use Case Development** là quá trình xây dựng các tình huống phát hiện trong SIEM để nhận diện hành vi đáng ngờ hoặc sự cố bảo mật.

Một use case tốt giúp SOC phát hiện đúng vấn đề, giảm false positive, có hướng xử lý rõ ràng và hỗ trợ incident response hiệu quả hơn.

Nói ngắn gọn:

```text
SIEM Use Case = tình huống phát hiện cụ thể trong SIEM.
```

Ví dụ:

- Phát hiện brute force.
- Phát hiện đăng nhập bất thường.
- Phát hiện PowerShell đáng ngờ.
- Phát hiện ransomware outbreak.
- Phát hiện MSBuild bị Office gọi.
- Phát hiện outbound connection từ LoLBin.

---

## 1. SIEM Use Case là gì?

**SIEM Use Case** là một kịch bản cụ thể mô tả cách SIEM nên phát hiện một hành vi bảo mật đáng quan tâm.

Use case có thể đơn giản hoặc phức tạp.

Ví dụ đơn giản:

```text
10 lần đăng nhập thất bại trong 4 phút
      ↓
SIEM correlation
      ↓
Alert: Possible brute force attack
```

Ví dụ phức tạp hơn:

```text
Office spawn PowerShell
      ↓
PowerShell tải file từ Internet
      ↓
Endpoint tạo process lạ
      ↓
Outbound connection đến IP đáng ngờ
      ↓
Alert: Possible phishing payload execution
```

---

## 2. Ví dụ brute force cơ bản

Giả sử user tên **Rob** có 10 lần đăng nhập thất bại liên tiếp.

Các khả năng có thể xảy ra:

- Rob quên mật khẩu.
- Có người đang thử brute force tài khoản Rob.
- Có script hoặc service đang dùng password cũ.
- Có attacker đang thử credential stuffing.

SIEM sẽ nhận các event đăng nhập thất bại, sau đó correlation chúng lại thành một alert.

Ví dụ logic:

```text
IF same user has 10 failed authentication attempts
WITHIN 4 minutes
THEN alert "Possible brute force"
```

SOC analyst sau đó cần triage alert để xác định đây là false positive hay incident thật.

---

# 3. SIEM Use Case Development Lifecycle

Khi xây dựng một SIEM use case, cần đi qua nhiều giai đoạn.

Lifecycle cơ bản:

```text
Requirements
      ↓
Data Points
      ↓
Log Validation
      ↓
Design
      ↓
Implementation
      ↓
Documentation
      ↓
Onboarding
      ↓
Testing
      ↓
Fine Tuning
```

---

## 3.1. Requirements

**Requirements** là giai đoạn hiểu mục tiêu của use case.

Cần trả lời:

- Ta muốn phát hiện hành vi gì?
- Vì sao hành vi này quan trọng?
- Use case này do ai yêu cầu?
- Rủi ro nghiệp vụ là gì?
- Cần alert trong điều kiện nào?
- Use case map với MITRE ATT&CK nào?
- Severity nên là gì?

Ví dụ requirement:

```text
Phát hiện brute force attack khi có 10 lần login fail liên tiếp trong 4 phút.
```

Requirement có thể đến từ khách hàng, SOC analyst, incident responder, threat intelligence, compliance team, detection engineer hoặc kết quả post-incident review.

---

## 3.2. Data Points

**Data Points** là các điểm dữ liệu cần thu thập để phát hiện use case.

Cần xác định:

- Event đến từ đâu?
- Log source nào có dữ liệu cần thiết?
- Field nào cần dùng?
- Hệ thống nào sinh log?
- Có đủ user, timestamp, source, destination không?

Ví dụ brute force cần log từ:

- Windows machines.
- Linux machines.
- VPN.
- Web application.
- OWA/Exchange.
- Cloud identity provider.
- Endpoint/server/application logs.

Các field cần có:

| Field | Ý nghĩa |
|---|---|
| user.name | User đăng nhập |
| @timestamp | Thời gian event |
| source.ip | IP nguồn |
| destination.ip | IP đích |
| host.name | Máy liên quan |
| event.action | Hành động |
| event.outcome | Kết quả thành công/thất bại |
| application.name | Ứng dụng liên quan |

---

## 3.3. Log Validation

**Log Validation** là giai đoạn xác minh log có đủ và đúng không.

Cần kiểm tra:

- Log có về SIEM không?
- Log có đúng timestamp không?
- Field có được parse đúng không?
- Field có được normalize đúng không?
- Event login success/fail có xuất hiện không?
- Các hệ thống quan trọng có gửi log không?
- Có thiếu log source nào không?

Ví dụ cần kiểm tra các loại authentication:

- Local authentication.
- Web-based authentication.
- Application authentication.
- VPN authentication.
- OWA/Outlook authentication.
- Cloud authentication.

Nếu log thiếu field quan trọng, rule có thể không hoạt động hoặc hoạt động sai.

---

## 3.4. Design and Implementation

Sau khi xác định requirement và validate log, bắt đầu thiết kế rule.

Ba tham số chính cần xác định:

```text
Condition
Aggregation
Priority
```

### Condition

**Condition** là điều kiện để alert được kích hoạt.

Ví dụ:

```text
event.action: authentication_failure
```

hoặc:

```text
event.code:4625
```

### Aggregation

**Aggregation** là cách gom nhiều event lại để tạo alert có ý nghĩa.

Ví dụ:

```text
10 failed login attempts
within 4 minutes
for the same user
from the same source IP
```

Aggregation giúp tránh tạo alert cho từng event nhỏ lẻ.

### Priority

**Priority** là mức độ ưu tiên hoặc severity của alert.

Severity có thể phụ thuộc vào:

- User có quyền cao không.
- Host có critical không.
- Source IP có reputation xấu không.
- Event có liên quan đến MITRE technique nghiêm trọng không.
- Có nhiều host bị ảnh hưởng không.
- Có dấu hiệu lateral movement không.

---

## 3.5. Documentation

Use case cần có tài liệu rõ ràng.

Tài liệu này thường gọi là:

```text
SOP - Standard Operating Procedure
```

SOP giúp analyst biết phải xử lý alert như thế nào.

Một SOP tốt nên có:

- Mục tiêu của alert.
- Điều kiện rule.
- Aggregation logic.
- Severity.
- MITRE ATT&CK mapping.
- Log source.
- Field cần kiểm tra.
- Các bước triage.
- Cách xác định false positive.
- Cách xác định true positive.
- Escalation matrix.
- Team cần liên hệ.
- Incident Response Plan liên quan.

---

## 3.6. Onboarding

**Onboarding** là quá trình đưa use case từ môi trường development/test vào production.

Không nên đưa rule mới vào production ngay nếu chưa test.

Quy trình nên là:

```text
Develop rule
      ↓
Test in dev/staging
      ↓
Check false positives
      ↓
Tune rule
      ↓
Peer review
      ↓
Move to production
      ↓
Monitor performance
```

Mục tiêu là giảm false positive và tránh làm SOC bị ngập alert.

---

## 3.7. Testing

Testing giúp xác minh rule có hoạt động đúng không.

Cần kiểm tra:

- Rule có trigger khi hành vi thật xảy ra không?
- Rule có bỏ sót event không?
- Rule có tạo quá nhiều false positive không?
- Rule có chạy đúng schedule không?
- Alert có đủ context không?
- Analyst có hiểu alert không?
- SOP có đủ rõ không?

Có thể test bằng lab, Atomic Red Team, Purple Team exercise, simulated log, historical data hoặc replay event.

---

## 3.8. Fine Tuning

**Fine Tuning** là quá trình điều chỉnh rule để tăng độ chính xác.

Một số cách tuning:

- Whitelist activity hợp lệ.
- Exclude known benign process.
- Giới hạn theo user group.
- Giới hạn theo host criticality.
- Điều chỉnh threshold.
- Điều chỉnh time window.
- Thêm threat intelligence enrichment.
- Thêm parent process condition.
- Thêm source/destination context.

Fine tuning cần feedback định kỳ từ analyst.

---

# 4. Cách xây dựng SIEM Use Case

Khi xây dựng SIEM use case, nên đi theo các bước sau:

1. Hiểu nhu cầu, rủi ro và hệ thống cần monitor.
2. Xác định priority và impact.
3. Map alert với Cyber Kill Chain hoặc MITRE ATT&CK.
4. Xác định Time to Detection.
5. Xác định Time to Response.
6. Tạo SOP cho analyst.
7. Xây dựng quy trình fine tuning.
8. Phát triển Incident Response Plan.
9. Thiết lập SLA và OLA giữa các team.
10. Thiết lập audit process.
11. Tạo documentation về logging status.
12. Tạo knowledge base cho case management.

---

## 4.1. Time to Detection

**TTD** là viết tắt của:

```text
Time to Detection
```

TTD đo thời gian từ lúc hành vi xảy ra đến lúc SOC phát hiện.

Ví dụ:

```text
Attack xảy ra lúc 10:00
SIEM tạo alert lúc 10:05
TTD = 5 phút
```

TTD phụ thuộc vào log ingestion delay, rule execution interval, pipeline processing, correlation logic và SIEM performance.

---

## 4.2. Time to Response

**TTR** là viết tắt của:

```text
Time to Response
```

TTR đo thời gian từ lúc alert được tạo đến lúc team bắt đầu hoặc hoàn tất phản ứng.

Ví dụ:

```text
Alert tạo lúc 10:05
Analyst bắt đầu triage lúc 10:10
Containment lúc 10:25
```

TTR giúp đánh giá hiệu quả của SOC và Incident Response.

---

## 4.3. SLA và OLA

**SLA** là Service Level Agreement.

**OLA** là Operational Level Agreement.

Trong SOC:

- SLA thường mô tả cam kết xử lý alert/incident.
- OLA mô tả phối hợp vận hành giữa các team nội bộ.

Ví dụ:

```text
High severity alert phải được triage trong 15 phút.
Critical incident phải escalation đến IR trong 10 phút.
IT team phải hỗ trợ isolate host trong 30 phút.
```

---

# 5. Example 1: Microsoft Build Engine Started By Office Application

## 5.1. Bối cảnh

**MSBuild** là một phần của Microsoft Build Engine.

Thông thường MSBuild được dùng để build ứng dụng dựa trên file XML project.

Tuy nhiên, attacker có thể lạm dụng MSBuild để thực thi mã độc thông qua project file hoặc configuration file.

Đây là một dạng lạm dụng **LoLBin**.

---

## 5.2. Rủi ro

Hành vi đáng ngờ:

```text
Microsoft Office executable starts MSBuild.exe
```

Ví dụ:

```text
winword.exe → msbuild.exe
excel.exe → msbuild.exe
```

Trong môi trường bình thường, Word hoặc Excel không nên gọi MSBuild.

Nếu có, đây có thể là dấu hiệu phishing document, malicious macro, script payload execution, defense evasion, LoLBin abuse hoặc initial compromise.

---

## 5.3. Use Case Objective

Mục tiêu của use case:

```text
Phát hiện MSBuild được khởi chạy bởi Word hoặc Excel.
```

Detection logic ý tưởng:

```text
process.name: MSBuild.exe
AND process.parent.name: winword.exe OR excel.exe
```

---

## 5.4. Severity

Use case này nên có severity cao vì:

- Office spawn MSBuild là hành vi bất thường.
- Có liên quan đến payload execution.
- Có dấu hiệu LoLBin abuse.
- Có khả năng là phishing hoặc malware execution.
- Hành vi này thường hiếm với user không phải developer.

Khuyến nghị:

```text
Severity: HIGH
```

Tuy nhiên, severity có thể thay đổi tùy môi trường. Nếu tổ chức có nhiều developer dùng MSBuild hợp lệ, cần tuning cẩn thận.

---

## 5.5. MITRE ATT&CK Mapping

Use case này có thể map với:

| Tactic | Technique | ID |
|---|---|---|
| Defense Evasion | Trusted Developer Utilities Proxy Execution | T1127 |
| Defense Evasion | MSBuild | T1127.001 |
| Execution | Execution context | TA0002 |

Mapping chính:

```text
Tactic: Defense Evasion
Technique: Trusted Developer Utilities Proxy Execution
Sub-technique: MSBuild
ID: T1127.001
```

Ngoài ra, vì MSBuild thực thi payload trên endpoint, nó cũng liên quan đến tactic **Execution**.

---

## 5.6. TTD và rule interval

Ví dụ rule chạy mỗi 5 phút:

```text
Rule interval: 5 minutes
```

Nếu log ingestion pipeline hoạt động tốt, TTD mục tiêu có thể là khoảng 5 phút hoặc hơn một chút.

Cần tính:

```text
TTD = ingestion delay + rule interval + alert processing time
```

---

## 5.7. SOP cho alert MSBuild từ Office

Khi alert xảy ra, analyst nên kiểm tra:

- `process.name`
- `process.parent.name`
- `process.command_line`
- `event.action`
- Máy phát hiện alert.
- User liên quan.
- File Office đã mở.
- User activity trong khoảng ±2 ngày.
- Antivirus/EDR logs.
- Proxy logs.
- Network connections.
- File created sau khi MSBuild chạy.
- Có payload hoặc child process nào khác không.

Các bước triage:

```text
1. Xác định parent process là Word/Excel/Office app nào.
2. Kiểm tra command line của MSBuild.
3. Kiểm tra user và host.
4. Kiểm tra file Office liên quan.
5. Kiểm tra network connection.
6. Kiểm tra EDR/AV alert.
7. Kiểm tra proxy/DNS logs.
8. Liên hệ user nếu cần.
9. Escalate nếu có dấu hiệu malicious.
```

---

## 5.8. Fine Tuning

False positive có thể xảy ra nếu:

- User là developer.
- Office plugin hợp lệ gọi build tool.
- Script nội bộ hợp lệ.
- Build workflow hợp lệ.

Cách tuning:

- Whitelist developer machines.
- Whitelist known parent process nếu hợp lệ.
- Whitelist known project path.
- Exclude signed internal tools.
- Giữ rule nghiêm ngặt với non-developer users.
- Tăng severity nếu host không thuộc developer group.

---

# 6. Example 2: MSBuild Making Network Connections

## 6.1. Bối cảnh

Trong ví dụ thứ hai, vẫn tập trung vào **MSBuild.exe**.

Nhưng lần này use case phát hiện khi:

```text
MSBuild.exe tạo outbound network connection.
```

Hành vi này có thể đáng ngờ vì MSBuild thường không cần kết nối ra Internet trong nhiều môi trường người dùng thông thường.

---

## 6.2. Rủi ro

Hành vi đáng ngờ:

```text
MSBuild.exe → outbound connection → remote IP
```

Có thể là payload tải thêm stage, C2 communication, data retrieval, LoLBin abuse, defense evasion hoặc malicious project execution.

---

## 6.3. Severity

Use case này thường có severity **MEDIUM** thay vì HIGH.

Lý do:

- MSBuild có thể kết nối đến IP hợp lệ.
- Có thể là Microsoft update hoặc developer workflow.
- False positive có thể cao hơn.
- Cần threat intelligence để đánh giá IP.

Khuyến nghị:

```text
Severity: MEDIUM
```

Nếu remote IP có reputation xấu hoặc host không phải developer machine, có thể tăng severity.

---

## 6.4. MITRE ATT&CK Mapping

Mapping chính:

| Tactic | Technique | ID |
|---|---|---|
| Execution | MSBuild execution context | TA0002 |
| Defense Evasion | Trusted Developer Utilities Proxy Execution: MSBuild | T1127.001 |
| Command and Control | Application Layer Protocol / Web Protocols | T1071 context nếu có C2 |

Tùy bằng chứng, có thể map thêm C2 nếu traffic đến infrastructure độc hại.

---

## 6.5. SOP cho alert MSBuild network connection

Analyst nên kiểm tra:

- `process.name`
- `process.command_line`
- `process.parent.name`
- `event.action`
- Destination IP.
- Destination port.
- Destination domain.
- IP reputation.
- Threat intelligence match.
- User liên quan.
- Host liên quan.
- Host có phải developer machine không.
- Proxy logs.
- DNS logs.
- EDR process tree.
- File hoặc project được MSBuild xử lý.

Các bước triage:

```text
1. Xác định MSBuild được chạy bởi process nào.
2. Kiểm tra command line.
3. Kiểm tra destination IP/domain.
4. Kiểm tra reputation của IP/domain.
5. Kiểm tra host có thuộc developer group không.
6. Kiểm tra network connection trước/sau alert.
7. Kiểm tra file được MSBuild xử lý.
8. Kiểm tra các child process hoặc file created.
9. Escalate nếu IP/domain độc hại hoặc có dấu hiệu payload.
```

---

## 6.6. Fine Tuning

Cách giảm false positive:

- Whitelist Microsoft IP/domain hợp lệ.
- Whitelist developer build servers.
- Whitelist internal repository.
- Exclude known CI/CD workflows.
- Kết hợp với threat intelligence.
- Chỉ alert nếu parent process đáng ngờ.
- Tăng severity nếu destination là rare external IP.
- Tăng severity nếu host không thuộc developer OU/group.

---

# 7. Mẫu tài liệu cho một SIEM Use Case

Một use case nên được document theo mẫu:

```text
Use Case Name:
Objective:
Risk:
Log Sources:
Required Fields:
Detection Logic:
Aggregation:
Severity:
MITRE Mapping:
TTD Target:
TTR Target:
SOP:
False Positive Scenarios:
Tuning Notes:
Escalation Path:
Incident Response Plan:
Owner:
Review Frequency:
```

---

## 7.1. Ví dụ mẫu: Brute Force

```text
Use Case Name: Multiple Failed Login Attempts
Objective: Detect possible brute force attack.
Condition: 10 failed logins within 4 minutes.
Aggregation: Same user or same source IP.
Severity: Medium.
MITRE: T1110 - Brute Force.
TTD Target: 5 minutes.
TTR Target: 15 minutes.
SOP: Check source IP, user, asset, success login after failures.
False Positives: User forgot password, service account password expired.
Escalation: Escalate if privileged account or successful login after failures.
```

---

# 8. Bảng ôn nhanh thuật ngữ

| Thuật ngữ | Nghĩa ngắn gọn |
|---|---|
| SIEM Use Case | Tình huống phát hiện cụ thể trong SIEM |
| Correlation Rule | Rule tương quan nhiều event |
| Requirements | Yêu cầu phát hiện |
| Data Points | Điểm dữ liệu/log cần thu thập |
| Log Validation | Xác minh log đủ và đúng |
| Condition | Điều kiện kích hoạt alert |
| Aggregation | Cách gom event |
| Priority | Mức độ ưu tiên |
| Severity | Mức độ nghiêm trọng |
| SOP | Quy trình thao tác chuẩn cho analyst |
| Onboarding | Đưa rule vào vận hành |
| Fine Tuning | Tinh chỉnh rule để giảm false positive |
| TTD | Time to Detection |
| TTR | Time to Response |
| SLA | Service Level Agreement |
| OLA | Operational Level Agreement |
| IRP | Incident Response Plan |
| LoLBin | Công cụ hợp pháp bị attacker lạm dụng |
| MSBuild | Microsoft Build Engine |
| False Positive | Cảnh báo sai |
| MITRE Mapping | Gắn alert với tactic/technique MITRE |

---

# 9. Câu hỏi ôn tập

## 1. SIEM use case là gì?

SIEM use case là một kịch bản phát hiện cụ thể được thiết kế để SIEM nhận diện hành vi đáng ngờ hoặc sự cố bảo mật.

## 2. Vì sao cần log validation?

Vì nếu log thiếu field quan trọng hoặc không được parse đúng, rule có thể không hoạt động hoặc tạo alert sai.

## 3. Ba tham số quan trọng khi thiết kế rule là gì?

Ba tham số chính là condition, aggregation và priority.

## 4. SOP dùng để làm gì?

SOP hướng dẫn analyst cách xử lý alert, kiểm tra thông tin nào, xác định false positive/true positive và escalation ra sao.

## 5. TTD là gì?

TTD là Time to Detection, thời gian từ lúc hành vi xảy ra đến khi SOC phát hiện.

## 6. Vì sao MSBuild bị Office gọi là đáng ngờ?

Vì Word/Excel thường không nên khởi chạy MSBuild. Hành vi này có thể cho thấy macro hoặc payload đang lạm dụng MSBuild để thực thi mã độc.

## 7. Vì sao MSBuild making network connections thường có severity medium?

Vì MSBuild có thể kết nối đến IP hợp lệ trong một số trường hợp, nên cần threat intelligence và context để giảm false positive.

---

# Key Takeaway

SIEM use case development là nền tảng của Detection Engineering.

Một use case tốt không chỉ có rule, mà còn cần requirement rõ ràng, log source đầy đủ, validation kỹ, detection logic hợp lý, MITRE mapping, SOP, TTD/TTR, IRP và quy trình fine tuning.

Khi được xây dựng đúng cách, SIEM use case giúp SOC phát hiện threat hiệu quả hơn, giảm false positive và phản ứng nhanh hơn với incident thật.
