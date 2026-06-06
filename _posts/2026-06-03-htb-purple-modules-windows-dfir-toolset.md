---
layout: post
title: "HTB Academy Purple Modules vÃ  Windows DFIR Toolset"
date: 2026-06-03 04:00:00 +0700
categories: [Purple Team, DFIR]
tags: [purple-team, dfir, htb-academy, windows-forensics, velociraptor, sysmon, yara, volatility, blue-team, red-team]
description: "Ghi chÃº tá»•ng há»£p vá» HTB Academy Purple Modules, lá»£i Ã­ch cho Blue Team/Red Team/Purple Team vÃ  bá»™ cÃ´ng cá»¥ Windows DFIR Ä‘Æ°á»£c cÃ i sáºµn."
toc: true
---

# HTB Academy Purple Modules vÃ  Windows DFIR Toolset

BÃ i nÃ y tá»•ng há»£p pháº§n giá»›i thiá»‡u vá» **HTB Academy Purple Modules** vÃ  bá»™ cÃ´ng cá»¥ **Windows DFIR Toolset** Ä‘Æ°á»£c cÃ i sáºµn trong cÃ¡c má»¥c tiÃªu Windows Purple Module.

Purple Module giÃºp ngÆ°á»i há»c nhÃ¬n váº¥n Ä‘á» tá»« cáº£ hai phÃ­a:

- **Red Team**: thá»±c hiá»‡n táº¥n cÃ´ng, mÃ´ phá»ng hÃ nh vi adversary.
- **Blue Team**: Ä‘iá»u tra, thu tháº­p log, phÃ¢n tÃ­ch artifact vÃ  xÃ¢y dá»±ng detection.
- **Purple Team**: káº¿t há»£p hai bÃªn Ä‘á»ƒ cáº£i thiá»‡n nÄƒng lá»±c phÃ²ng thá»§.

---

# 1. Giá»›i thiá»‡u vá» Purple Modules

Trong HTB Academy, **Purple Modules** lÃ  cÃ¡c mÃ´-Ä‘un káº¿t há»£p giá»¯a tÆ° duy táº¥n cÃ´ng vÃ  phÃ²ng thá»§.

CÃ¡c mÃ´-Ä‘un nÃ y giÃºp thu háº¹p khoáº£ng cÃ¡ch giá»¯a:

```text
Offensive Security
        +
Defensive Security
        =
Purple Team Learning
```

Má»¥c tiÃªu lÃ  giÃºp ngÆ°á»i há»c khÃ´ng chá»‰ biáº¿t cÃ¡ch táº¥n cÃ´ng, mÃ  cÃ²n hiá»ƒu:

- Cuá»™c táº¥n cÃ´ng Ä‘á»ƒ láº¡i log gÃ¬.
- Artifact nÃ o Ä‘Æ°á»£c táº¡o ra.
- Network traffic cÃ³ dáº¥u hiá»‡u gÃ¬.
- Memory dump cÃ³ thá»ƒ chá»©a báº±ng chá»©ng gÃ¬.
- CÃ´ng cá»¥ DFIR nÃ o cÃ³ thá»ƒ dÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch.
- Detection rule nÃ o cÃ³ thá»ƒ viáº¿t sau khi quan sÃ¡t hÃ nh vi táº¥n cÃ´ng.

---

# 2. VÃ¬ sao cáº§n thá»±c hiá»‡n pháº§n táº¥n cÃ´ng trÆ°á»›c?

PhÃ¢n tÃ­ch phÃ¡p y cáº§n cÃ³ dá»¯ liá»‡u.

VÃ¬ váº­y, trong Purple Module, pháº§n táº¥n cÃ´ng thÆ°á»ng pháº£i xáº£y ra trÆ°á»›c Ä‘á»ƒ táº¡o ra:

- Log.
- Event.
- Network traffic.
- File artifact.
- Process artifact.
- Memory artifact.
- Registry change.
- Detection telemetry.

Sau khi cÃ³ dá»¯ liá»‡u, ngÆ°á»i há»c má»›i cÃ³ thá»ƒ thá»±c hiá»‡n pháº§n Ä‘iá»u tra vÃ  phÃ¢n tÃ­ch forensic.

VÃ­ dá»¥:

```text
Attack executed
      â†“
Logs generated
      â†“
Traffic captured
      â†“
Memory can be dumped
      â†“
DFIR analysis begins
```

Äiá»u nÃ y cÅ©ng Ã¡p dá»¥ng cho **memory dump**. Bá»™ nhá»› chá»‰ cÃ³ Ã½ nghÄ©a phÃ¢n tÃ­ch náº¿u sau khi attack xáº£y ra, target váº«n cÃ²n hoáº¡t Ä‘á»™ng vÃ  dá»¯ liá»‡u váº«n cÃ²n tá»“n táº¡i trong RAM.

---

# 3. LÆ°u Ã½ khi dÃ¹ng Purple Module Targets

Khi spawn target trong HTB Academy, má»¥c tiÃªu cáº§n Ä‘Æ°á»£c giá»¯ hoáº¡t Ä‘á»™ng Ä‘á»§ lÃ¢u Ä‘á»ƒ phá»¥c vá»¥ forensic.

Má»™t sá»‘ lÆ°u Ã½:

- Target pháº£i tiáº¿p tá»¥c cháº¡y sau khi attack xáº£y ra.
- KhÃ´ng táº¯t target quÃ¡ sá»›m.
- CÃ³ thá»ƒ cáº§n kÃ©o dÃ i thá»i gian target.
- Má»™t sá»‘ service cáº§n vÃ i phÃºt Ä‘á»ƒ khá»Ÿi Ä‘á»™ng Ä‘áº§y Ä‘á»§.
- Náº¿u target táº¯t, log/traffic/memory artifact cÃ³ thá»ƒ máº¥t hoáº·c khÃ´ng cÃ²n Ä‘áº§y Ä‘á»§.

NÃ³i ngáº¯n gá»n:

```text
Attack xong chÆ°a nÃªn táº¯t mÃ¡y.
Cáº§n giá»¯ target sá»‘ng Ä‘á»ƒ Ä‘iá»u tra.
```

---

# 4. Cáº¥u trÃºc Purple Module

MÃ´-Ä‘un Ä‘Æ°á»£c chia thÃ nh hai pháº§n chÃ­nh:

```text
Windows Purple Module Targets
Linux Purple Module Targets
```

Má»—i pháº§n cung cáº¥p hÆ°á»›ng dáº«n tham kháº£o Ä‘á»ƒ ngÆ°á»i há»c biáº¿t cÃ¡ch:

- Truy cáº­p target.
- Cáº¥u hÃ¬nh cÆ¡ cháº¿ logging.
- XÃ¡c Ä‘á»‹nh vá»‹ trÃ­ log.
- Thu tháº­p network traffic.
- Táº¡o memory dump.
- TÃ¬m file cáº¥u hÃ¬nh.
- Sá»­ dá»¥ng cÃ´ng cá»¥ DFIR Ä‘Æ°á»£c cÃ i sáºµn.
- PhÃ¢n tÃ­ch artifact sau khai thÃ¡c.

---

# 5. Lá»£i Ã­ch cá»§a Purple Modules

Purple Modules cÃ³ lá»£i cho cáº£ Blue Team, Red Team vÃ  Purple Team.

---

## 5.1. Lá»£i Ã­ch cho Blue Team

Äá»‘i vá»›i Blue Team, Purple Module giÃºp:

- Hiá»ƒu chiáº¿n thuáº­t vÃ  ká»¹ thuáº­t cá»§a Red Team.
- Quan sÃ¡t attack artifact thá»±c táº¿.
- PhÃ¢n tÃ­ch log sau khai thÃ¡c.
- TÃ¬m IOC.
- XÃ¢y dá»±ng detection rule.
- Kiá»ƒm tra giáº£ thuyáº¿t threat hunting.
- PhÃ¡t triá»ƒn incident response playbook.
- Hiá»ƒu cÃ¡ch attacker di chuyá»ƒn vÃ  Ä‘á»ƒ láº¡i dáº¥u váº¿t.

VÃ­ dá»¥ use case:

```text
Blue Team mÃ´ phá»ng táº¥n cÃ´ng
        â†“
Thu tháº­p log vÃ  artifact
        â†“
PhÃ¢n tÃ­ch IOC
        â†“
Viáº¿t detection rule
        â†“
Kiá»ƒm tra láº¡i detection
```

---

## 5.2. Lá»£i Ã­ch cho Red Team

Äá»‘i vá»›i Red Team, Purple Module giÃºp hiá»ƒu rÃµ hÆ¡n cuá»™c táº¥n cÃ´ng cá»§a mÃ¬nh Ä‘á»ƒ láº¡i dáº¥u váº¿t gÃ¬.

Red Team cÃ³ thá»ƒ:

- Kiá»ƒm tra payload tá»± viáº¿t.
- Quan sÃ¡t log Ä‘Æ°á»£c táº¡o ra.
- Quan sÃ¡t process behavior.
- Xem telemetry tá»« endpoint.
- Hiá»ƒu tool cá»§a mÃ¬nh bá»‹ phÃ¡t hiá»‡n á»Ÿ Ä‘Ã¢u.
- Tinh chá»‰nh ká»¹ thuáº­t Ä‘á»ƒ giáº£m dáº¥u váº¿t.
- Cáº£i thiá»‡n OPSEC.

VÃ­ dá»¥:

```text
Run payload
      â†“
Check logs and telemetry
      â†“
Identify detection points
      â†“
Refine technique
```

---

## 5.3. Lá»£i Ã­ch cho Purple Team

Äá»‘i vá»›i Purple Team, cÃ¡c target nÃ y lÃ  mÃ´i trÆ°á»ng chia sáº» giá»¯a Red Team vÃ  Blue Team.

Cáº£ hai bÃªn cÃ³ thá»ƒ cÃ¹ng:

- MÃ´ phá»ng attack chain.
- Quan sÃ¡t detection.
- Kiá»ƒm tra logging.
- Kiá»ƒm tra alert.
- XÃ¢y dá»±ng detection.
- Tá»‘i Æ°u response playbook.
- Cáº£i thiá»‡n kháº£ nÄƒng phÃ²ng thá»§.

Purple Team khÃ´ng chá»‰ há»i:

```text
CÃ³ táº¥n cÃ´ng Ä‘Æ°á»£c khÃ´ng?
```

MÃ  cÃ²n há»i:

```text
CÃ³ phÃ¡t hiá»‡n Ä‘Æ°á»£c khÃ´ng?
Náº¿u khÃ´ng phÃ¡t hiá»‡n Ä‘Æ°á»£c thÃ¬ thiáº¿u log nÃ o?
Náº¿u phÃ¡t hiá»‡n Ä‘Æ°á»£c thÃ¬ quy trÃ¬nh pháº£n á»©ng cÃ³ tá»‘t khÃ´ng?
```

---

# 6. Purple Module Targets lÃ  háº¡ táº§ng cÃ³ thá»ƒ tÃ¡i sá»­ dá»¥ng

CÃ¡c Purple Module Targets cÃ³ thá»ƒ Ä‘Æ°á»£c dÃ¹ng láº¡i nhÆ° má»™t mÃ´i trÆ°á»ng thá»±c hÃ nh DFIR.

Blue Team cÃ³ thá»ƒ:

- Chuyá»ƒn báº±ng chá»©ng tá»« mÃ¡y bá»‹ compromise khÃ¡c sang target Ä‘á»ƒ phÃ¢n tÃ­ch.
- CÃ i pháº§n má»m dá»… bá»‹ táº¥n cÃ´ng.
- MÃ´ phá»ng táº¥n cÃ´ng.
- PhÃ¢n tÃ­ch artifact Ä‘á»ƒ láº¡i.
- XÃ¡c Ä‘á»‹nh IOC.
- PhÃ¡t triá»ƒn detection rule.
- Kiá»ƒm thá»­ threat hunting hypothesis.
- Thu tháº­p telemetry Ä‘á»ƒ phÃ¢n tÃ­ch hÃ nh vi malware.
- Thiáº¿t káº¿ incident response playbook.

Red Team cÃ³ thá»ƒ:

- Test payload.
- Quan sÃ¡t log.
- Quan sÃ¡t process behavior.
- Kiá»ƒm tra telemetry.
- Äiá»u chá»‰nh ká»¹ thuáº­t Ä‘á»ƒ giáº£m kháº£ nÄƒng bá»‹ phÃ¡t hiá»‡n.

Purple Team cÃ³ thá»ƒ:

- MÃ´ phá»ng Ä‘áº§y Ä‘á»§ attack-detect-response cycle.
- DÃ¹ng target nhÆ° ná»n táº£ng há»c táº­p chung.
- Kiá»ƒm tra kháº£ nÄƒng phÃ²ng thá»§ trong mÃ´i trÆ°á»ng Ä‘Æ°á»£c kiá»ƒm soÃ¡t.

---

# 7. Available Windows DFIR Toolset

Pháº§n nÃ y tá»•ng há»£p cÃ¡c cÃ´ng cá»¥ DFIR Ä‘Æ°á»£c cÃ i sáºµn trÃªn **Windows Purple Module Targets**.

CÃ¡c cÃ´ng cá»¥ nÃ y phá»¥c vá»¥ phÃ¢n tÃ­ch sau khai thÃ¡c, bao gá»“m:

- System monitoring.
- Log analysis.
- Threat detection.
- Traffic capture.
- Memory dumping.
- Memory forensics.
- Telemetry.
- Malware/process/PE analysis.

---

# 8. System Monitoring

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Sysmon | `C:\Windows\Sysmon64.exe` | Ghi log chi tiáº¿t phá»¥c vá»¥ detection vÃ  forensic |

## Sysmon lÃ  gÃ¬?

**Sysmon** lÃ  cÃ´ng cá»¥ cá»§a Microsoft Sysinternals giÃºp ghi láº¡i nhiá»u sá»± kiá»‡n quan trá»ng trÃªn Windows.

Sysmon cÃ³ thá»ƒ ghi:

- Process creation.
- Network connection.
- File creation.
- Registry modification.
- Driver loaded.
- Image loaded.
- Process injection indicators.
- DNS query.

Sysmon ráº¥t há»¯u Ã­ch cho SOC vÃ¬ Windows Event Log máº·c Ä‘á»‹nh Ä‘Ã´i khi khÃ´ng Ä‘á»§ chi tiáº¿t Ä‘á»ƒ Ä‘iá»u tra attack behavior.

---

# 9. Log Analysis

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Eric Zimmerman Tools | `C:\Tools\EZ-Tools` | Bá»™ cÃ´ng cá»¥ forensic Ä‘á»ƒ phÃ¢n tÃ­ch registry hives, event logs vÃ  artifact sá»‘ |

## Eric Zimmerman Tools

**Eric Zimmerman Tools** lÃ  bá»™ cÃ´ng cá»¥ ráº¥t phá»• biáº¿n trong Windows forensic.

Má»™t sá»‘ tool ná»•i báº­t:

- EvtxECmd.
- RECmd.
- MFTECmd.
- AmcacheParser.
- AppCompatCacheParser.
- PECmd.
- Timeline Explorer.

DÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch:

- Windows Event Logs.
- Registry.
- Prefetch.
- Amcache.
- ShimCache.
- MFT.
- Timeline artifact.

---

# 10. Threat Detection & Monitoring

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Yara | `C:\Tools\yara\yara.exe` | Scan file dá»±a trÃªn signature |
| Chainsaw | `C:\Tools\Sigma\chainsaw\chainsaw_x86_64-pc-windows-msvc.exe` | Hunt qua Windows Event Logs |
| Sigma | `C:\Program Files\Python312\Scripts\sigma.exe` | Äá»‹nh dáº¡ng rule chung cho SIEM |
| Zircolite | `C:\Tools\Sigma\zircolite\zircolite_win_x64_2.20.0.exe` | PhÃ¢n tÃ­ch EVTX dá»±a trÃªn Sigma |
| Osquery | `C:\Program Files\osquery\osqueryi.exe` | Query endpoint báº±ng cÃº phÃ¡p giá»‘ng SQL |
| Velociraptor | `C:\Program Files\Velociraptor` | Endpoint monitoring, collection vÃ  response |

---

## Yara

**Yara** lÃ  cÃ´ng cá»¥ dÃ¹ng Ä‘á»ƒ nháº­n diá»‡n file hoáº·c malware dá»±a trÃªn rule/signature.

Yara thÆ°á»ng dÃ¹ng Ä‘á»ƒ:

- TÃ¬m malware theo chuá»—i Ä‘áº·c trÆ°ng.
- Scan folder nghi ngá».
- Hunt file theo pattern.
- Táº¡o IOC dá»±a trÃªn sample Ä‘Ã£ phÃ¢n tÃ­ch.

---

## Chainsaw

**Chainsaw** lÃ  cÃ´ng cá»¥ command-line dÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch vÃ  hunt trong Windows Event Logs.

Chainsaw thÆ°á»ng káº¿t há»£p vá»›i Sigma rule Ä‘á»ƒ phÃ¡t hiá»‡n hÃ nh vi Ä‘Ã¡ng ngá» trong file `.evtx`.

---

## Sigma

**Sigma** lÃ  Ä‘á»‹nh dáº¡ng rule chung cho detection.

CÃ³ thá»ƒ hiá»ƒu Sigma giá»‘ng nhÆ°:

```text
YARA for logs
```

Sigma rule cÃ³ thá»ƒ Ä‘Æ°á»£c chuyá»ƒn Ä‘á»•i sang nhiá»u SIEM khÃ¡c nhau nhÆ° Splunk, Elastic, Sentinel, QRadar.

---

## Zircolite

**Zircolite** dÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch Windows EVTX log dá»±a trÃªn Sigma rule.

NÃ³ há»¯u Ã­ch khi analyst cÃ³ file event log offline vÃ  muá»‘n nhanh chÃ³ng tÃ¬m hÃ nh vi Ä‘Ã¡ng ngá».

---

## Osquery

**Osquery** cho phÃ©p query endpoint báº±ng cÃº phÃ¡p giá»‘ng SQL.

VÃ­ dá»¥:

```sql
SELECT name, path, pid FROM processes;
```

DÃ¹ng Ä‘á»ƒ kiá»ƒm tra:

- Process.
- Service.
- User.
- Network connection.
- Scheduled task.
- Installed software.

---

## Velociraptor

**Velociraptor** lÃ  ná»n táº£ng endpoint monitoring, collection vÃ  response.

Truy cáº­p Velociraptor:

```text
https://<Target_IP>:8889
```

ThÃ´ng tin Ä‘Äƒng nháº­p:

```text
Username: admin
Password: P3n#31337@LOG
```

LÆ°u Ã½: sau khi spawn Windows Purple target, nÃªn chá» Ã­t nháº¥t **5 phÃºt** Ä‘á»ƒ cÃ¡c service khá»Ÿi Ä‘á»™ng Ä‘áº§y Ä‘á»§, bao gá»“m Velociraptor.

---

# 11. Traffic Capturing

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Wireshark | `C:\Program Files\Wireshark` | Báº¯t vÃ  phÃ¢n tÃ­ch gÃ³i tin máº¡ng |

## Wireshark

**Wireshark** dÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch traffic máº¡ng.

CÃ³ thá»ƒ dÃ¹ng Ä‘á»ƒ kiá»ƒm tra:

- DNS query.
- HTTP request.
- TLS connection.
- SMB traffic.
- C2 traffic.
- File transfer.
- Port scanning.
- Suspicious beaconing.

---

# 12. Memory Dumping

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| DumpIt | `C:\Tools\Memory-Dump\DumpIt.exe` | Táº¡o memory dump phá»¥c vá»¥ memory forensic |
| WinPmem | `C:\Tools\Memory-Dump\winpmem_mini_x64_rc2.exe` | Táº¡o memory dump phá»¥c vá»¥ memory forensic |

## Memory Dump lÃ  gÃ¬?

**Memory dump** lÃ  báº£n chá»¥p ná»™i dung RAM táº¡i má»™t thá»i Ä‘iá»ƒm.

Memory dump cÃ³ thá»ƒ chá»©a:

- Process Ä‘ang cháº¡y.
- Malware cháº¡y trong memory.
- Network connection.
- Credential.
- Command history.
- Injected code.
- Encryption key.
- Token/session.

---

# 13. Memory Forensics

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Volatility v2 | `C:\Tools\Volatility2` | PhÃ¢n tÃ­ch memory dump |
| Volatility v3 | `C:\Tools\volatility3` | PhÃ¢n tÃ­ch memory dump |

## Volatility

**Volatility** lÃ  cÃ´ng cá»¥ memory forensic phá»• biáº¿n.

DÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch:

- Process list.
- Network connection.
- DLL loaded.
- Command line.
- Registry in memory.
- Malware injection.
- Hidden process.
- Credential artifact.

---

# 14. Additional Telemetry

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| SilkETW | `C:\Tools\SilkETW\SilkETW\SilkETW.exe` | C# wrapper cho ETW |
| SealighterTI | `C:\Tools\SealighterTI.exe` | Cháº¡y Microsoft-Windows-Threat-Intelligence khÃ´ng cáº§n driver |
| AMSI-Monitoring-Script | `C:\Tools\AMSIScript\AMSIScriptContentRetrieval.ps1` | TrÃ­ch xuáº¥t ná»™i dung script qua AMSI ETW provider |
| JonMon | `C:\Tools\JonMon` | Bá»™ sensor telemetry mÃ£ nguá»“n má»Ÿ |
| Fibratus | `C:\Program Files\Fibratus` | Detection theo behavior vÃ  YARA memory scanner |

LÆ°u Ã½: má»™t sá»‘ tool nhÆ° **Fibratus** cÃ³ thá»ƒ Ä‘Æ°á»£c thÃªm sau khi module phÃ¡t hÃ nh vÃ  khÃ´ng cÃ³ sáºµn trong target cá»§a module nÃ y.

---

## ETW lÃ  gÃ¬?

**ETW** lÃ  viáº¿t táº¯t cá»§a **Event Tracing for Windows**.

ETW lÃ  cÆ¡ cháº¿ ghi nháº­n telemetry sÃ¢u trong Windows.

NÃ³ cÃ³ thá»ƒ cung cáº¥p thÃ´ng tin vá»:

- Process.
- Thread.
- File.
- Registry.
- Network.
- PowerShell.
- AMSI.
- Threat intelligence events.

---

## AMSI Monitoring Script

AMSI Monitoring Script giÃºp trÃ­ch xuáº¥t ná»™i dung script thÃ´ng qua AMSI ETW provider.

DÃ¹ng Ä‘á»ƒ quan sÃ¡t script trÆ°á»›c khi nÃ³ Ä‘Æ°á»£c thá»±c thi, Ä‘áº·c biá»‡t há»¯u Ã­ch khi phÃ¢n tÃ­ch:

- PowerShell.
- Script obfuscation.
- Malicious macro.
- Script-based payload.

---

# 15. Adversary Simulation

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| Atomic Red Team | `C:\AtomicRedTeam` | Bá»™ test mÃ´ phá»ng ká»¹ thuáº­t MITRE ATT&CK |

LÆ°u Ã½: Atomic Red Team Ä‘Æ°á»£c thÃªm sau khi module phÃ¡t hÃ nh vÃ  cÃ³ thá»ƒ khÃ´ng cÃ³ sáºµn trong target cá»§a module nÃ y.

## Atomic Red Team

**Atomic Red Team** cung cáº¥p cÃ¡c test nhá» Ä‘á»ƒ mÃ´ phá»ng ká»¹ thuáº­t MITRE ATT&CK.

VÃ­ dá»¥:

- MÃ´ phá»ng PowerShell execution.
- MÃ´ phá»ng credential dumping.
- MÃ´ phá»ng persistence.
- MÃ´ phá»ng lateral movement.

DÃ¹ng Ä‘á»ƒ kiá»ƒm tra detection rule vÃ  kháº£ nÄƒng log/alert.

---

# 16. Malware, Process vÃ  PE Analysis

| Tool | Path | Má»¥c Ä‘Ã­ch |
|---|---|---|
| CFF Explorer | `C:\Tools\CFF-Explorer\CFF Explorer.exe` | Xem vÃ  chá»‰nh sá»­a PE file |
| Ghidra | `C:\Tools\Ghidra\ghidraRun.bat` | Reverse engineering framework |
| x64dbg | `C:\Tools\x64dbg` | Debugger x64/x32 cho Windows |
| SpeakEasy | `C:\Tools\speakeasy` | Emulator cho Windows malware |
| SysInternalsSuite | `C:\Tools\SysinternalsSuite` | Bá»™ cÃ´ng cá»¥ troubleshooting cá»§a Sysinternals |
| Get-InjectedThread | `C:\Tools\Get-InjectedThread.ps1` | TÃ¬m thread Ä‘Æ°á»£c táº¡o do code injection |
| Hollows Hunter | `C:\Tools\hollows_hunter64.exe` | Scan process Ä‘á»ƒ phÃ¡t hiá»‡n implant |
| Moneta | `C:\Tools\Moneta64.exe` | Live usermode memory analysis |
| PE-Sieve | `C:\Tools\pe-sieve64.exe` | PhÃ¡t hiá»‡n malware trong process vÃ  dump material Ä‘Ã¡ng ngá» |
| API Monitor | `C:\Tools\API Monitor` | Theo dÃµi API call |
| PE-Bear | `C:\Tools\PE-bear` | Reversing tool cho PE file |
| Process Hacker | `C:\Tools\ProcessHacker` | Theo dÃµi process, debug vÃ  phÃ¡t hiá»‡n malware |
| ProcMonX | `C:\Tools\ProcMonX.exe` | Process Monitor má»Ÿ rá»™ng dá»±a trÃªn ETW |
| Frida | `C:\Program Files\Python312\Scripts\frida.exe` | Dynamic instrumentation toolkit |
| LitterBox | `C:\Tools\LitterBox\litterbox.py` | Malware sandbox Ä‘á»ƒ test payload behavior |

LÆ°u Ã½: má»™t sá»‘ tool nhÆ° **Frida** vÃ  **LitterBox** cÃ³ thá»ƒ Ä‘Æ°á»£c thÃªm sau khi module phÃ¡t hÃ nh vÃ  khÃ´ng cÃ³ sáºµn trong target cá»§a module nÃ y.

---

## PE file lÃ  gÃ¬?

**PE** lÃ  viáº¿t táº¯t cá»§a **Portable Executable**.

ÄÃ¢y lÃ  Ä‘á»‹nh dáº¡ng file thá»±c thi trÃªn Windows, vÃ­ dá»¥:

- `.exe`
- `.dll`
- `.sys`

CÃ´ng cá»¥ nhÆ° CFF Explorer, PE-Bear, Ghidra, x64dbg dÃ¹ng Ä‘á»ƒ phÃ¢n tÃ­ch PE file.

---

## Process Injection lÃ  gÃ¬?

**Process Injection** lÃ  ká»¹ thuáº­t attacker dÃ¹ng Ä‘á»ƒ Ä‘Æ°a code Ä‘á»™c háº¡i vÃ o process há»£p lá»‡.

Má»¥c tiÃªu:

- NÃ© detection.
- Cháº¡y malware dÆ°á»›i process tin cáº­y.
- áº¨n hÃ nh vi Ä‘á»™c háº¡i.
- Bypass má»™t sá»‘ security control.

CÃ¡c tool nhÆ° Get-InjectedThread, PE-Sieve, Moneta, Hollows Hunter cÃ³ thá»ƒ há»— trá»£ phÃ¡t hiá»‡n dáº¥u hiá»‡u injection.

---

# 17. Workflow thá»±c hÃ nh Purple Module

Má»™t workflow Ä‘Æ¡n giáº£n khi thá»±c hÃ nh:

```text
Spawn Target
      â†“
Chá» service khá»Ÿi Ä‘á»™ng Ä‘áº§y Ä‘á»§
      â†“
Thá»±c hiá»‡n pháº§n táº¥n cÃ´ng
      â†“
Giá»¯ target hoáº¡t Ä‘á»™ng
      â†“
Thu tháº­p log/network/memory
      â†“
PhÃ¢n tÃ­ch báº±ng DFIR tools
      â†“
XÃ¡c Ä‘á»‹nh IOC/artifact
      â†“
Map MITRE ATT&CK náº¿u cáº§n
      â†“
Viáº¿t detection rule
      â†“
Kiá»ƒm thá»­ láº¡i detection
```

---

# 18. Báº£ng Ã´n nhanh thuáº­t ngá»¯

| Thuáº­t ngá»¯ | NghÄ©a ngáº¯n gá»n |
|---|---|
| Purple Team | Káº¿t há»£p Red Team vÃ  Blue Team |
| Purple Module | MÃ´-Ä‘un há»c cáº£ táº¥n cÃ´ng vÃ  phÃ²ng thá»§ |
| DFIR | Digital Forensics and Incident Response |
| Artifact | Dáº¥u váº¿t hoáº·c báº±ng chá»©ng Ä‘á»ƒ láº¡i trÃªn há»‡ thá»‘ng |
| Telemetry | Dá»¯ liá»‡u quan sÃ¡t tá»« endpoint/network |
| IOC | Indicator of Compromise |
| Sysmon | CÃ´ng cá»¥ ghi log chi tiáº¿t trÃªn Windows |
| Yara | CÃ´ng cá»¥ scan file theo rule/signature |
| Sigma | Äá»‹nh dáº¡ng rule phÃ¡t hiá»‡n cho log/SIEM |
| Chainsaw | CÃ´ng cá»¥ hunt Windows Event Logs |
| Zircolite | PhÃ¢n tÃ­ch EVTX báº±ng Sigma |
| Osquery | Query endpoint báº±ng SQL-like syntax |
| Velociraptor | Endpoint monitoring, collection vÃ  response |
| Wireshark | CÃ´ng cá»¥ báº¯t vÃ  phÃ¢n tÃ­ch gÃ³i tin |
| Memory Dump | Báº£n chá»¥p RAM |
| Volatility | CÃ´ng cá»¥ phÃ¢n tÃ­ch memory dump |
| ETW | Event Tracing for Windows |
| AMSI | Antimalware Scan Interface |
| PE | Portable Executable |
| Process Injection | Ká»¹ thuáº­t inject code vÃ o process khÃ¡c |
| Atomic Red Team | Bá»™ test mÃ´ phá»ng ká»¹ thuáº­t MITRE ATT&CK |

---

# 19. CÃ¢u há»i Ã´n táº­p

## 1. VÃ¬ sao forensic analysis cáº§n pháº§n táº¥n cÃ´ng xáº£y ra trÆ°á»›c?

VÃ¬ táº¥n cÃ´ng táº¡o ra log, traffic, memory artifact vÃ  cÃ¡c dáº¥u váº¿t cáº§n thiáº¿t Ä‘á»ƒ phÃ¢n tÃ­ch forensic.

## 2. VÃ¬ sao target cáº§n duy trÃ¬ hoáº¡t Ä‘á»™ng sau khi attack?

VÃ¬ nhiá»u báº±ng chá»©ng nhÆ° memory artifact, process, network connection hoáº·c service telemetry cÃ³ thá»ƒ máº¥t náº¿u target bá»‹ táº¯t quÃ¡ sá»›m.

## 3. Purple Module cÃ³ lá»£i gÃ¬ cho Blue Team?

Blue Team cÃ³ thá»ƒ há»c cÃ¡ch attack táº¡o artifact, phÃ¡t triá»ƒn detection rule, kiá»ƒm thá»­ threat hunting hypothesis vÃ  xÃ¢y dá»±ng incident response playbook.

## 4. Purple Module cÃ³ lá»£i gÃ¬ cho Red Team?

Red Team cÃ³ thá»ƒ quan sÃ¡t dáº¥u váº¿t mÃ  payload hoáº·c ká»¹ thuáº­t cá»§a mÃ¬nh Ä‘á»ƒ láº¡i, tá»« Ä‘Ã³ cáº£i thiá»‡n OPSEC vÃ  giáº£m kháº£ nÄƒng bá»‹ phÃ¡t hiá»‡n.

## 5. Velociraptor dÃ¹ng Ä‘á»ƒ lÃ m gÃ¬?

Velociraptor dÃ¹ng cho endpoint monitoring, collection vÃ  response. NÃ³ giÃºp thu tháº­p artifact, Ä‘iá»u tra endpoint vÃ  pháº£n á»©ng trong quÃ¡ trÃ¬nh DFIR.

---

# Key Takeaway

HTB Academy Purple Modules giÃºp ngÆ°á»i há»c káº¿t ná»‘i giá»¯a táº¥n cÃ´ng vÃ  phÃ²ng thá»§.

Thay vÃ¬ chá»‰ há»c cÃ¡ch khai thÃ¡c, ngÆ°á»i há»c cÃ²n cÃ³ thá»ƒ phÃ¢n tÃ­ch cÃ¡c dáº¥u váº¿t mÃ  cuá»™c táº¥n cÃ´ng Ä‘á»ƒ láº¡i, tá»« Ä‘Ã³ hiá»ƒu rÃµ hÆ¡n vá» detection, forensic vÃ  incident response.

Äá»‘i vá»›i Blue Team, Ä‘Ã¢y lÃ  mÃ´i trÆ°á»ng tá»‘t Ä‘á»ƒ luyá»‡n Ä‘iá»u tra vÃ  viáº¿t detection.

Äá»‘i vá»›i Red Team, Ä‘Ã¢y lÃ  mÃ´i trÆ°á»ng tá»‘t Ä‘á»ƒ hiá»ƒu tool vÃ  payload cá»§a mÃ¬nh bá»‹ phÃ¡t hiá»‡n nhÆ° tháº¿ nÃ o.

Äá»‘i vá»›i Purple Team, Ä‘Ã¢y lÃ  ná»n táº£ng giÃºp hai bÃªn phá»‘i há»£p Ä‘á»ƒ cáº£i thiá»‡n nÄƒng lá»±c phÃ²ng thá»§ thá»±c táº¿.
