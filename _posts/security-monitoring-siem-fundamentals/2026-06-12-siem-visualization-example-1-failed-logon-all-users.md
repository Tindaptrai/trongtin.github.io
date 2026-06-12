---
layout: post
title: "SIEM Visualization Example 1: Failed Logon Attempts All Users"
date: 2026-06-12 22:40:00 +0700
categories: [SOC, SIEM]
tags: [soc, siem, elastic-stack, kibana, kql, visualization, failed-logon, windows-event, dashboard, blue-team]
description: "Ghi chú hướng dẫn tạo dashboard và visualization trong Kibana để theo dõi failed logon attempts của tất cả user bằng Windows Event ID 4625."
toc: true
---

# SIEM Visualization Example 1: Failed Logon Attempts All Users

Trong bài này, ta tạo một dashboard và visualization trong Kibana để theo dõi các lần **failed logon attempts** của tất cả user.

Mục tiêu là tạo một bảng hiển thị:

- Username phát sinh failed logon.
- Máy ghi nhận sự kiện.
- Logon Type.
- Số lần đăng nhập thất bại.

---

## 1. Dashboard trong SIEM là gì?

Trong SIEM, **dashboard** là nơi chứa nhiều visualization để hiển thị dữ liệu theo cách dễ quan sát.

Dashboard giúp SOC analyst:

- Theo dõi alert.
- Quan sát xu hướng.
- Phân tích sự kiện.
- So sánh dữ liệu.
- Xem nhanh các dấu hiệu bất thường.
- Hỗ trợ triage và investigation.

Ví dụ một dashboard SOC có thể chứa:

- Failed logon attempts.
- Successful logons.
- Top source IP.
- Top usernames.
- Malware alerts.
- Suspicious PowerShell.
- RDP connections.
- Firewall denies.

---

# 2. Mục tiêu của visualization

Visualization này dùng để theo dõi:

```text
Failed Logon Attempts - All Users
```

Trong Windows, failed logon thường tương ứng với:

```text
Event ID: 4625
```

Trong Elastic/Winlogbeat, field thường dùng là:

```text
event.code: 4625
```

Mục tiêu visualization:

| Câu hỏi | Field dùng |
|---|---|
| User nào đăng nhập thất bại? | `user.name.keyword` |
| Máy nào ghi nhận event? | `host.hostname.keyword` |
| Kiểu logon là gì? | `winlog.logon.type.keyword` |
| Xảy ra bao nhiêu lần? | `Count` |

---

# 3. Spawn target và truy cập Kibana

Ở cuối section trong HTB Academy, bấm:

```text
Click here to spawn the target system!
```

Sau khi target được spawn, đợi khoảng:

```text
3 - 5 phút
```

Lý do là Kibana và Elasticsearch cần thời gian để khởi động đầy đủ.

Sau đó truy cập:

```text
http://[Target IP]:5601
```

Điều hướng:

```text
Side navigation toggle → Dashboard
```

---

# 4. Xóa dashboard cũ nếu cần

Nếu dashboard có sẵn như:

```text
SOC-Alerts
```

và bài yêu cầu tạo từ đầu, có thể xóa dashboard cũ.

Sau đó vào lại Dashboard page, Kibana sẽ hiển thị tùy chọn:

```text
Create new dashboard
```

Bấm để tạo dashboard mới.

---

# 5. Tạo visualization đầu tiên

Trong dashboard mới, bấm:

```text
Create visualization
```

Kibana sẽ mở giao diện tạo visualization.

Trước khi cấu hình, cần chỉnh time range.

---

# 6. Chỉnh time range

Bấm vào biểu tượng calendar/time picker.

Chọn khoảng thời gian:

```text
Last 15 years
```

Sau đó bấm:

```text
Apply
```

Lý do: dữ liệu lab có thể nằm ở thời điểm cũ. Nếu để time range mặc định như last 15 minutes hoặc last 24 hours, có thể không thấy event nào.

---

# 7. Chọn index pattern

Trong phần index pattern, chọn:

```text
windows*
```

Ý nghĩa:

```text
Chỉ dùng Windows logs để tạo visualization.
```

Trong môi trường SIEM, log thường được chia thành nhiều index:

- `windows*`
- `linux*`
- `network*`
- `firewall*`
- `endpoint*`

Vì bài này phân tích Windows failed logon event nên chọn:

```text
windows*
```

---

# 8. Thêm filter Event ID 4625

Ta cần filter các failed logon event.

Thêm filter:

```text
Field: event.code
Operator: is
Value: 4625
```

KQL tương đương:

```text
event.code:4625
```

Ý nghĩa:

```text
Chỉ lấy Windows failed logon events.
```

---

# 9. Kiểm tra field user.name.keyword

Trong ô search field, tìm:

```text
user.
```

Mục tiêu là xác nhận field sau có tồn tại:

```text
user.name.keyword
```

Field này dùng để nhóm kết quả theo username.

---

## Vì sao dùng user.name.keyword thay vì user.name?

Khi làm aggregation trong Kibana, nên dùng field dạng:

```text
.keyword
```

Lý do:

- `.keyword` giữ nguyên giá trị chính xác.
- Phù hợp cho grouping/top values.
- Không bị phân tích thành token như text field.
- Cho kết quả aggregation chính xác hơn.

Vì vậy, dùng:

```text
user.name.keyword
```

thay vì:

```text
user.name
```

---

# 10. Chọn visualization type

Ở menu visualization type, chọn:

```text
Table
```

Lý do chọn Table:

- Dễ xem username.
- Dễ xem hostname.
- Dễ xem logon type.
- Dễ xem số lần failed logon.
- Phù hợp với SOC triage.

---

# 11. Cấu hình Rows: Username

Vào phần:

```text
Rows
```

Thêm field:

```text
Top values of user.name.keyword
```

Cấu hình gợi ý:

```text
Field: user.name.keyword
Number of values: 1000
Rank by: Count of records
Order: Descending
```

Lưu ý: ban đầu có thể Kibana hiển thị `Rank by Alphabetical` thay vì `Count of records`. Không sao. Sau khi cấu hình Metrics là Count, tùy chọn Count of records sẽ khả dụng.

---

# 12. Cấu hình Metrics: Count

Vào phần:

```text
Metrics
```

Chọn:

```text
Count
```

Ý nghĩa:

```text
Đếm số lượng failed logon events.
```

Sau khi chọn Count, table sẽ bắt đầu có dữ liệu nếu dataset có event phù hợp.

---

# 13. Thêm Rows: Hostname

Để biết failed logon xảy ra trên máy nào, thêm row thứ hai:

```text
host.hostname.keyword
```

Ý nghĩa:

```text
Máy ghi nhận failed logon event.
```

Cấu hình gợi ý:

```text
Field: host.hostname.keyword
Number of values: 1000
Rank by: Count of records
Order: Descending
```

Nếu không thấy field này, có thể thử các field tương tự:

```text
host.name.keyword
winlog.computer_name.keyword
computer_name.keyword
```

---

# 14. Kết quả ban đầu

Sau khi cấu hình, table sẽ có ba thông tin:

| Thông tin | Ý nghĩa |
|---|---|
| `user.name.keyword` | Username phát sinh failed logon |
| `host.hostname.keyword` | Máy ghi nhận event |
| `Count` | Số lần event xảy ra |

Lưu ý: bảng ban đầu có thể hiển thị cả user và computer accounts. Computer accounts thường kết thúc bằng ký tự `$`.

---

# 15. Save visualization và dashboard

Sau khi visualization hiển thị đúng, bấm:

```text
Save and return
```

Sau đó lưu dashboard bằng nút:

```text
Save
```

Có thể đặt dashboard title:

```text
SOC-Alerts
```

Description gợi ý:

```text
HTB Academy SOC Analyst Job Role Path
```

---

# 16. Refining The Visualization

Sau khi tạo xong visualization cơ bản, SOC Manager có thể yêu cầu cải tiến:

- Đặt tên cột rõ ràng hơn.
- Thêm Logon Type.
- Sort kết quả.
- Loại trừ một số username không cần monitor.
- Loại trừ computer accounts.

---

## 16.1. Vào Edit Lens

Vào Dashboard:

```text
Dashboard → SOC-Alerts
```

Bấm:

```text
Pencil / Edit
```

Ở visualization, bấm:

```text
Gear icon → Edit lens
```

---

# 17. Đổi tên cột Username

Mở cấu hình:

```text
Top values of user.name.keyword
```

Đổi display name thành:

```text
Username
```

Cấu hình gợi ý:

```text
Field: user.name.keyword
Number of values: 1000
Rank by: Alphabetical
Order: Ascending
Display name: Username
```

---

# 18. Đổi tên cột Hostname

Mở cấu hình:

```text
Top values of host.hostname.keyword
```

Đổi display name thành:

```text
Event logged by
```

Cấu hình gợi ý:

```text
Field: host.hostname.keyword
Number of values: 1000
Rank by: Count of records
Order: Descending
Display name: Event logged by
```

---

# 19. Thêm Logon Type

Thêm một Rows mới với field:

```text
winlog.logon.type.keyword
```

Đặt display name:

```text
Logon Type
```

Cấu hình gợi ý:

```text
Field: winlog.logon.type.keyword
Number of values: 1000
Rank by: Count of records
Order: Descending
Display name: Logon Type
```

Logon Type giúp analyst hiểu kiểu đăng nhập thất bại.

Ví dụ:

| Logon Type | Ý nghĩa thường gặp |
|---|---|
| Interactive | Đăng nhập trực tiếp tại máy |
| Network | Đăng nhập qua mạng |
| RemoteInteractive | RDP |
| Batch | Batch job |
| Service | Service account |

---

# 20. Đổi tên Metrics

Mở cấu hình:

```text
Count of records
```

Đổi display name thành:

```text
# of logins
```

Cấu hình:

```text
Function: Count
Field: Records
Display name: # of logins
Text alignment: Right
```

---

# 21. Sort kết quả

Sort bảng theo cột:

```text
# of logins
```

Theo thứ tự:

```text
Descending
```

Mục tiêu:

```text
User/host nào có nhiều failed logon nhất sẽ hiện lên trên.
```

---

# 22. Loại trừ username không cần monitor

Theo bài lab, cần loại trừ các username:

```text
DESKTOP-DPOESND
WIN-OK9BH1BCKSD
WIN-RMMGJA7T9TC
```

Có thể tạo thêm filter:

```text
Field: user.name.keyword
Operator: is not
Value: DESKTOP-DPOESND
```

Lặp lại với:

```text
WIN-OK9BH1BCKSD
WIN-RMMGJA7T9TC
```

---

# 23. Loại trừ computer accounts

Computer accounts trong Windows thường kết thúc bằng ký tự:

```text
$
```

Để loại trừ computer accounts, dùng KQL:

```text
NOT user.name: *$ AND winlog.channel.keyword: Security
```

Ý nghĩa:

| Điều kiện | Ý nghĩa |
|---|---|
| `NOT user.name: *$` | Loại bỏ account kết thúc bằng `$` |
| `winlog.channel.keyword: Security` | Chỉ lấy log từ Security channel |

Phần:

```text
AND winlog.channel.keyword: Security
```

giúp tránh các log không liên quan bị tính vào visualization.

---

# 24. Visualization sau khi refine

Sau khi refine, table nên có các cột:

| Column | Ý nghĩa |
|---|---|
| Username | User phát sinh failed logon |
| Event logged by | Máy ghi nhận event |
| Logon Type | Kiểu đăng nhập |
| # of logins | Số lần failed logon |

KQL filter chính:

```text
event.code:4625
```

KQL loại computer accounts:

```text
NOT user.name: *$ AND winlog.channel.keyword: Security
```

---

# 25. Đặt title cho visualization

Bấm vào:

```text
No Title
```

Bật:

```text
Show panel title
```

Đặt title gợi ý:

```text
Failed Logon Attempts - All Users
```

Sau đó bấm Save.

Cuối cùng, đừng quên bấm nút:

```text
Save
```

ở góc trên bên phải để lưu dashboard.

---

# 26. SOC analyst nên điều tra gì?

Khi nhìn vào visualization, analyst nên kiểm tra:

- User nào có nhiều failed logon nhất?
- Failed logon xảy ra trên máy nào?
- Logon Type là gì?
- Có source IP bất thường không?
- Có successful logon sau nhiều failed logon không?
- Có nhiều user bị thử từ cùng source không?
- Có pattern password spraying không?
- Có phải user/service nhập sai mật khẩu không?
- Có liên quan đến disabled account không?
- Có tài khoản privileged bị thử không?

---

# 27. Điều tra false positive

Một số nguyên nhân hợp lệ có thể tạo failed logon:

- User quên mật khẩu.
- Service account password expired.
- Scheduled task dùng credential cũ.
- Application pool dùng credential cũ.
- Máy join domain bị lỗi cached credential.
- User đổi mật khẩu nhưng thiết bị cũ vẫn dùng password cũ.
- Script nội bộ dùng credential sai.
- Scanner nội bộ kiểm tra authentication.

---

# 28. Khi nào cần escalation?

Nên escalation nếu có dấu hiệu:

- Nhiều failed logon từ cùng source IP.
- Một source IP thử nhiều user.
- Một user bị thử trên nhiều host.
- Có successful login sau chuỗi failed logon.
- User bị thử là admin hoặc privileged account.
- Logon Type là RDP/RemoteInteractive đáng ngờ.
- Source host không thuộc hệ thống hợp lệ.
- Sự kiện xảy ra ngoài giờ làm việc.
- Có alert EDR/SIEM khác đi kèm.

---

# 29. MITRE ATT&CK Mapping

Use case failed logon có thể liên quan đến:

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | T1110 |
| Credential Access | Password Guessing | T1110.001 |
| Credential Access | Password Spraying | T1110.003 |
| Initial Access | Valid Accounts | T1078 |

Nếu attacker thử nhiều user với một password, khả năng cao là password spraying.

Nếu attacker thử nhiều password trên một user, khả năng cao là password guessing hoặc brute force.

---

# 30. KQL mẫu

## Failed logon all users

```text
event.code:4625
```

## Failed logon và chỉ Security channel

```text
event.code:4625 AND winlog.channel.keyword: Security
```

## Loại computer accounts

```text
event.code:4625 AND NOT user.name: *$ AND winlog.channel.keyword: Security
```

## Loại username cụ thể

```text
event.code:4625 AND NOT user.name.keyword: "DESKTOP-DPOESND"
```

## Theo user cụ thể

```text
event.code:4625 AND user.name:"rob"
```

## Theo host cụ thể

```text
event.code:4625 AND host.hostname:"WS001"
```

---

# 31. Bảng ôn nhanh thuật ngữ

| Thuật ngữ | Nghĩa ngắn gọn |
|---|---|
| Dashboard | Nơi chứa nhiều visualization |
| Visualization | Biểu đồ/bảng trực quan hóa dữ liệu |
| Failed Logon | Đăng nhập thất bại |
| Event ID 4625 | Windows event cho failed logon |
| Index Pattern | Mẫu index dữ liệu trong Kibana |
| windows* | Index chứa Windows logs |
| user.name.keyword | Username dạng keyword dùng cho aggregation |
| host.hostname.keyword | Hostname dạng keyword |
| winlog.logon.type.keyword | Kiểu logon |
| Metrics | Số liệu tính toán |
| Count | Đếm số event |
| Rows | Trường dùng để nhóm dữ liệu |
| KQL | Kibana Query Language |
| Computer Account | Tài khoản máy, thường kết thúc bằng `$` |
| Security Channel | Windows Security Event Log |
| False Positive | Cảnh báo sai |
| Escalation | Chuyển case lên mức xử lý cao hơn |

---

# 32. Câu hỏi ôn tập

## 1. Visualization này dùng để làm gì?

Dùng để theo dõi failed logon attempts của tất cả user trong Windows logs.

## 2. Event ID nào đại diện cho failed logon?

```text
4625
```

## 3. Vì sao dùng `user.name.keyword`?

Vì field `.keyword` phù hợp cho aggregation và top values trong Kibana.

## 4. Field nào dùng để hiển thị máy ghi nhận event?

```text
host.hostname.keyword
```

## 5. Field nào dùng để hiển thị Logon Type?

```text
winlog.logon.type.keyword
```

## 6. KQL nào dùng để loại computer accounts?

```text
NOT user.name: *$ AND winlog.channel.keyword: Security
```

## 7. Vì sao cần set time range là Last 15 years?

Vì dữ liệu lab có thể nằm ở thời điểm cũ. Nếu để time range mặc định, dashboard có thể không hiển thị dữ liệu.

---

# Key Takeaway

Visualization **Failed Logon Attempts - All Users** giúp SOC analyst quan sát nhanh các tài khoản có nhiều lần đăng nhập thất bại.

Khi được refine đúng cách, bảng nên hiển thị Username, Event logged by, Logon Type và số lần failed logon. Các filter như `event.code:4625`, loại computer accounts bằng `NOT user.name: *$`, và giới hạn `winlog.channel.keyword: Security` giúp visualization chính xác hơn.

Đây là một use case cơ bản nhưng rất quan trọng để phát hiện brute force, password guessing, password spraying hoặc credential misuse trong môi trường Windows.
