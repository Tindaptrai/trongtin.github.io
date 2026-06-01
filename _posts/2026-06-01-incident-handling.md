---
layout: post
title: "Incident Handling"
date: 2026-06-01 00:00:00 +0700
categories: [SOC, Incident Response]
tags: [soc, dfir, incident-handling, blue-team]
description: "Ghi chú về quy trình Incident Handling trong SOC: preparation, identification, containment, eradication, recovery và lessons learned."
toc: true
---

# Incident Handling

## Tổng quan

Incident Handling là quy trình giúp SOC analyst chuyển từ cảnh báo rời rạc sang hành động có kiểm soát. Mục tiêu không chỉ là đóng alert, mà là hiểu chuyện gì đã xảy ra, phạm vi ảnh hưởng ở đâu, cần khắc phục như thế nào và bài học nào cần đưa vào playbook.

```text
Preparation
Identification
Containment
Eradication and Recovery
Lessons Learned
```

| Giai đoạn | Mục tiêu | Output |
| --- | --- | --- |
| Preparation | Chuẩn bị người, quy trình và log | Playbook, asset list, contact list |
| Identification | Xác định alert có phải incident không | Initial triage notes |
| Containment | Giảm phạm vi ảnh hưởng | Isolated host, disabled account, block rule |
| Eradication and Recovery | Loại bỏ nguyên nhân và khôi phục dịch vụ | Clean host, patched system, monitoring plan |
| Lessons Learned | Rút kinh nghiệm sau sự cố | Timeline, root cause, detection improvement |

## 1. Preparation

Preparation là giai đoạn chuẩn bị trước khi có sự cố. Một SOC không nên chờ đến khi incident xảy ra mới tìm log, quyền truy cập hoặc người chịu trách nhiệm.

- Chuẩn bị playbook cho phishing, malware, brute force, credential compromise và data exfiltration.
- Đảm bảo log từ endpoint, firewall, proxy, DNS, VPN, identity provider và cloud được thu thập đầy đủ.
- Kiểm tra quyền truy cập SIEM, EDR, ticketing, case management và kênh liên lạc nội bộ.
- Xác định owner của từng hệ thống quan trọng.
- Lập danh sách asset, business impact và mức ưu tiên xử lý.

## 2. Identification

Identification là giai đoạn xác định alert có thật sự là incident hay không. Cần kiểm tra nguồn phát hiện, độ tin cậy, tài sản liên quan và bằng chứng từ nhiều nguồn log.

- Host hoặc user nào bị ảnh hưởng?
- Dấu hiệu ban đầu là gì?
- Có bằng chứng lặp lại trên nguồn log khác không?
- Đây là true positive, false positive hay cần theo dõi thêm?
- Phạm vi ban đầu là một máy, một user hay nhiều hệ thống?

Ví dụ ghi chú triage ngắn:

```text
Alert: Suspicious PowerShell execution
User: corp\user01
Host: WIN-WS-014
Evidence: EDR process tree, Windows Event ID 4688, proxy log
Initial verdict: Needs containment
```

## 3. Containment

Containment là cô lập hoặc chặn hành vi độc hại để giảm phạm vi ảnh hưởng. Cần cân bằng giữa giảm thiểu rủi ro và bảo toàn bằng chứng phục vụ DFIR.

- Cô lập endpoint qua EDR.
- Disable hoặc reset tài khoản nghi ngờ bị compromise.
- Chặn domain, IP hoặc hash độc hại.
- Thu thập memory hoặc triage artifact trước khi xóa file nếu cần điều tra.
- Tạm thời block rule firewall hoặc proxy theo IOC đã xác thực.

## 4. Eradication and Recovery

Eradication là loại bỏ nguyên nhân gốc. Recovery là đưa hệ thống trở lại trạng thái an toàn và theo dõi để chắc chắn sự cố không tái diễn.

- Xóa persistence, malware, scheduled task hoặc service độc hại.
- Vá lỗ hổng bị khai thác.
- Thu hồi credential bị lộ.
- Kiểm tra lại cấu hình firewall, VPN, RDP và quyền tài khoản.
- Restore từ backup sạch nếu hệ thống bị phá hoại.
- Tăng mức giám sát sau khi khôi phục.

## 5. Lessons Learned

Lessons Learned giúp biến một incident thành cải tiến phòng thủ. Mỗi incident nên kết thúc bằng ghi chú ngắn, dễ đọc lại và có hành động cụ thể.

- Timeline sự kiện.
- Root cause.
- Tác động.
- Hành động đã thực hiện.
- Detection rule cần bổ sung.
- Hardening cần triển khai.
- Playbook cần cập nhật.

## Quick Reference

| Câu hỏi | Gợi ý kiểm tra |
| --- | --- |
| Ai bị ảnh hưởng? | User, host, group, service account |
| Điều gì xảy ra? | Process, network connection, file change, login event |
| Bắt đầu khi nào? | First seen timestamp trong SIEM/EDR |
| Lan rộng đến đâu? | Asset scope, user scope, subnet scope |
| Đã chặn chưa? | Isolation, disable account, block IOC |
| Cần cải thiện gì? | Detection, logging, hardening, training |

## Key Takeaway

Incident Handling không chỉ là phản ứng khi bị tấn công. Đây là một vòng đời đầy đủ từ chuẩn bị, phát hiện, cô lập, loại bỏ, phục hồi đến cải thiện liên tục. Một ticket tốt phải giúp người khác đọc lại được timeline, bằng chứng, quyết định xử lý và bài học phòng thủ.
