---
layout: post
title: "SIEM Visualization Example 2: Failed Logon Attempts Disabled Users"
date: 2026-06-12 22:29:00 +0700
categories: [SOC, SIEM]
tags: [soc, siem, elastic-stack, kibana, kql, visualization, failed-logon, disabled-user, windows-event, blue-team]
description: "Ghi chú hướng dẫn tạo visualization trong Kibana để theo dõi failed logon attempts against disabled users bằng event.code 4625 và SubStatus 0xC0000072."
toc: true
---

# SIEM Visualization Example 2: Failed Logon Attempts Disabled Users

Trong bài này, ta tạo một visualization trong Kibana để theo dõi các lần **failed logon attempts** nhắm vào **disabled users**.

Mục tiêu là tạo một bảng hiển thị:

- Disabled user bị thử đăng nhập.
- Máy phát sinh sự kiện đăng nhập thất bại.
- Số lần sự kiện xảy ra trong khoảng thời gian đang chọn.

---

## 1. Vì sao cần theo dõi disabled users?

Một tài khoản đã bị disable thì không thể đăng nhập thành công, kể cả khi attacker nhập đúng mật khẩu.

Tuy nhiên, nếu có nhiều lần đăng nhập thất bại vào disabled account, đây có thể là dấu hiệu đáng ngờ.

Ví dụ:

- Attacker đang thử credential cũ.
- Credential của tài khoản disabled đã bị lộ.
- Script hoặc service vẫn dùng tài khoản cũ.
- Có brute force hoặc password spraying.
- Có activity bất thường trong môi trường Windows.

Nói ngắn gọn:

```text
Disabled user không thể login thành công,
nhưng failed logon vào disabled user vẫn là tín hiệu cần điều tra.
```

---

## 2. Windows Event ID liên quan

Trong Windows, failed logon thường được ghi nhận bằng event:

```text
Event ID: 4625
```

Trong Elastic/Winlogbeat, event này thường có field:

```text
event.code: 4625
```

Đây là event dùng để phát hiện các lần đăng nhập thất bại.

---

## 3. SubStatus 0xC0000072 là gì?

Khi đăng nhập thất bại, Windows có thể ghi thêm field **SubStatus** để mô tả lý do thất bại.

Với disabled user, SubStatus thường là:

```text
0xC0000072
```

Ý nghĩa:

```text
The account is currently disabled.
```

Trong Elastic/Winlogbeat, field thường dùng là:

```text
winlog.event_data.SubStatus
```

Vì vậy, để tìm failed logon attempts against disabled users, ta cần lọc:

```text
event.code: 4625
AND
winlog.event_data.SubStatus: 0xC0000072
```

---

# 4. Mục tiêu của visualization

Visualization cần trả lời ba câu hỏi:

| Câu hỏi | Field dùng |
|---|---|
| Disabled user nào bị thử đăng nhập? | `user.name.keyword` |
| Sự kiện xảy ra trên máy nào? | `host.hostname.keyword` |
| Xảy ra bao nhiêu lần? | `Count` |

Kết quả mong muốn là bảng gồm 3 cột:

```text
User | Hostname | Count
```

---

# 5. Các bước tạo visualization trong Kibana

## Bước 1: Spawn target

Ở cuối section trong HTB Academy, bấm:

```text
Click here to spawn the target system!
```

Sau khi target được spawn, đợi khoảng:

```text
3 - 5 phút
```

Lý do là Kibana và Elasticsearch cần thời gian để khởi động đầy đủ.

---

## Bước 2: Truy cập Kibana

Truy cập:

```text
http://[Target IP]:5601
```

Sau đó:

```text
Side navigation toggle → Dashboard
```

Một dashboard có sẵn sẽ xuất hiện.

Ví dụ dashboard:

```text
SOC-Alerts
```

---

## Bước 3: Vào chế độ edit dashboard

Bấm icon:

```text
Pencil / Edit
```

Sau đó bấm:

```text
Create visualization
```

Kibana sẽ mở cửa sổ tạo visualization mới.

---

# 6. Chọn data set / index pattern

Trong phần index pattern, chọn:

```text
windows*
```

Ý nghĩa:

```text
Chỉ dùng dữ liệu Windows logs để tạo visualization.
```

Trong môi trường SIEM, dữ liệu từ nhiều nguồn thường được tách thành nhiều index như:

- `windows*`
- `linux*`
- `network*`
- `firewall*`
- `endpoint*`

Vì bài này dùng Windows failed logon event nên chọn:

```text
windows*
```

---

# 7. Thêm filter

Ta cần filter đúng hai điều kiện:

```text
event.code: 4625
winlog.event_data.SubStatus: 0xC0000072
```

## Filter 1: Event ID 4625

Field:

```text
event.code
```

Value:

```text
4625
```

Ý nghĩa:

```text
Chỉ lấy các failed logon events.
```

## Filter 2: Disabled user SubStatus

Field:

```text
winlog.event_data.SubStatus
```

Value:

```text
0xC0000072
```

Ý nghĩa:

```text
Chỉ lấy các failed logon do tài khoản bị disabled.
```

---

## KQL tương đương

Nếu viết bằng KQL, có thể dùng:

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072
```

Nếu Kibana phân biệt chữ hoa/thường hoặc value đang ở dạng chữ thường, có thể thử:

```text
event.code:4625 AND winlog.event_data.SubStatus:0xc0000072
```

---

# 8. Kiểm tra field user.name.keyword

Trong phần search field, tìm:

```text
user.
```

Mục tiêu là xác nhận field sau có tồn tại:

```text
user.name.keyword
```

Field này dùng để nhóm kết quả theo username.

Nếu không thấy `user.name.keyword`, có thể kiểm tra các field gần giống:

```text
user.name
related.user.keyword
winlog.event_data.TargetUserName
winlog.event_data.TargetUserName.keyword
```

Tùy dataset, field name có thể khác nhau.

---

# 9. Chọn loại visualization

Ở menu visualization type, chọn:

```text
Table
```

Lý do chọn Table:

- Dễ xem username.
- Dễ xem hostname.
- Dễ xem số lần failed logon.
- Phù hợp với bài toán thống kê theo user và host.

---

# 10. Cấu hình Rows

Bấm vào:

```text
Rows
```

Thêm row đầu tiên:

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

Ý nghĩa:

```text
Hiển thị top user có nhiều failed logon nhất.
```

---

# 11. Cấu hình Metrics

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
Đếm số lượng event thỏa điều kiện filter.
```

Metric này sẽ cho biết số lần failed logon vào disabled user đã xảy ra.

---

# 12. Thêm Rows thứ hai: Hostname

Để biết sự kiện xảy ra trên máy nào, thêm một row nữa:

```text
host.hostname.keyword
```

Cấu hình:

```text
Field: host.hostname.keyword
Rank by: Count of records
Order: Descending
```

Field này đại diện cho máy gửi log hoặc máy phát sinh failed logon event.

Nếu không thấy field này, có thể kiểm tra field thay thế:

```text
host.name.keyword
computer_name.keyword
winlog.computer_name.keyword
```

---

# 13. Kết quả visualization

Sau khi cấu hình xong, bảng sẽ có 3 thông tin chính:

| Thông tin | Ý nghĩa |
|---|---|
| `user.name.keyword` | Disabled user bị thử đăng nhập |
| `host.hostname.keyword` | Máy xảy ra failed logon |
| `Count` | Số lần event xảy ra |

Ví dụ:

| User | Hostname | Count |
|---|---|---|
| anni | WS001 | 1 |

Ý nghĩa:

```text
Tài khoản disabled 'anni' có 1 failed logon attempt trên máy WS001.
```

---

# 14. Save visualization

Sau khi bảng hiển thị đúng, bấm:

```text
Save and return
```

Visualization mới sẽ được thêm vào dashboard.

Nên đặt tên visualization rõ ràng, ví dụ:

```text
Failed Logon Attempts Against Disabled Users
```

Hoặc ngắn hơn:

```text
Disabled Users Failed Logon
```

---

# 15. SOC analyst nên điều tra gì?

Khi visualization hiển thị failed logon vào disabled user, analyst nên kiểm tra:

- User nào bị thử đăng nhập?
- User đó bị disable từ khi nào?
- Host nào phát sinh event?
- Source IP là gì?
- Logon type là gì?
- Có nhiều host cùng thử user này không?
- Có nhiều user disabled bị thử không?
- Có successful logon nào xảy ra sau đó không?
- Có liên quan đến brute force/password spraying không?
- Có phải service cũ đang dùng credential cũ không?

---

## Field nên kiểm tra thêm

Một số field hữu ích:

```text
source.ip
host.hostname
user.name
event.code
winlog.event_data.SubStatus
winlog.event_data.LogonType
winlog.event_data.IpAddress
winlog.event_data.WorkstationName
winlog.event_data.TargetUserName
@timestamp
```

---

# 16. Điều tra false positive

Không phải mọi failed logon vào disabled user đều là attack.

Một số false positive có thể là:

- Service account đã disable nhưng service vẫn chạy.
- Scheduled task dùng credential cũ.
- Application pool dùng tài khoản cũ.
- Người dùng nhập nhầm tài khoản.
- Script cũ vẫn dùng username/password cũ.
- Máy cached credential cũ.
- Scanner hoặc health check nội bộ.

SOC analyst cần phân biệt false positive và true positive bằng context.

---

# 17. Khi nào cần escalation?

Nên escalation nếu có dấu hiệu:

- Nhiều failed logon vào nhiều disabled users.
- Một source IP thử nhiều tài khoản.
- Có pattern password spraying.
- Có successful login sau failed attempts.
- Source IP đến từ máy lạ.
- Host liên quan là server quan trọng.
- User bị thử từng là privileged account.
- Có event khác đi kèm như PowerShell, RDP, SMB.
- Có dấu hiệu lateral movement.

---

# 18. Mapping MITRE ATT&CK

Use case này có thể map với:

| Tactic | Technique | ID |
|---|---|---|
| Credential Access | Brute Force | T1110 |
| Credential Access | Password Guessing | T1110.001 |
| Credential Access | Password Spraying | T1110.003 |
| Initial Access | Valid Accounts | T1078 |

Tùy context, nếu attacker dùng credential thật hoặc thử tài khoản hợp lệ, có thể liên quan đến **Valid Accounts**.

---

# 19. KQL mẫu cho điều tra

## Failed logon vào disabled users

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072
```

## Theo user cụ thể

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND user.name:"anni"
```

## Theo host cụ thể

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND host.hostname:"WS001"
```

## Theo source IP nếu có field

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072 AND source.ip:10.10.10.5
```

## Tìm mọi failed logon Windows

```text
event.code:4625
```

---

# 20. Bảng ôn nhanh thuật ngữ

| Thuật ngữ | Nghĩa ngắn gọn |
|---|---|
| Visualization | Biểu đồ/bảng trực quan hóa dữ liệu |
| Dashboard | Bảng tổng hợp nhiều visualization |
| Failed Logon | Đăng nhập thất bại |
| Disabled User | Tài khoản đã bị vô hiệu hóa |
| Event ID 4625 | Windows failed logon event |
| SubStatus | Mã lý do đăng nhập thất bại |
| 0xC0000072 | Tài khoản đang bị disabled |
| KQL | Kibana Query Language |
| Index Pattern | Mẫu index dữ liệu trong Kibana |
| windows* | Index chứa Windows logs |
| Rows | Trường dùng để nhóm dữ liệu trong table |
| Metrics | Số liệu dùng để tính toán |
| Count | Đếm số lượng event |
| user.name.keyword | Field username dạng keyword |
| host.hostname.keyword | Field hostname dạng keyword |
| False Positive | Cảnh báo sai |
| Escalation | Chuyển case lên mức xử lý cao hơn |

---

# 21. Câu hỏi ôn tập

## 1. Visualization này dùng để làm gì?

Dùng để theo dõi các failed logon attempts nhắm vào disabled users.

## 2. Event ID nào dùng cho failed logon trên Windows?

Event ID thường dùng là:

```text
4625
```

## 3. SubStatus nào cho biết tài khoản bị disabled?

SubStatus là:

```text
0xC0000072
```

## 4. KQL chính của use case này là gì?

```text
event.code:4625 AND winlog.event_data.SubStatus:0xC0000072
```

## 5. Vì sao chọn visualization type là Table?

Vì table giúp hiển thị rõ user, hostname và số lượng event.

## 6. Field nào dùng để nhóm theo user?

```text
user.name.keyword
```

## 7. Field nào dùng để nhóm theo máy?

```text
host.hostname.keyword
```

---

# Key Takeaway

Visualization **Failed Logon Attempts Against Disabled Users** giúp SOC nhanh chóng nhận biết các nỗ lực đăng nhập vào tài khoản đã bị vô hiệu hóa.

Dù disabled user không thể login thành công, các failed logon dạng này vẫn có giá trị điều tra vì có thể liên quan đến brute force, password spraying, credential reuse hoặc service/script cũ đang dùng credential không còn hợp lệ.

Trong Kibana, ta có thể dùng filter `event.code:4625` kết hợp `winlog.event_data.SubStatus:0xC0000072`, sau đó tạo table theo `user.name.keyword`, `host.hostname.keyword` và metric `Count`.
