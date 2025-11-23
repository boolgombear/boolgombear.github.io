---
title: "Windows 필터 드라이버 설치 및 문제 해결"
date: 2025-11-21 10:00:00 +0900
categories: [System, Driver]
tags: [windows, driver, filter-driver, registry, troubleshooting]
---

## 개요

Windows에서 VMBUS 디바이스에 필터 드라이버(BlogDriver)를 설치하고 문제를 해결하는 방법을 다룹니다.

## 1. UpperFilters 제거

**PowerShell (관리자 권한)**에서 실행:

```powershell

# 2. 서비스 삭제
sc delete BlogDriver

# 3. 재부팅
shutdown /r /t 0

# 4. 재부팅 후 처음부터 설치
Copy-Item "C:\Users\user\Desktop\BlogDriver.sys" "C:\Windows\System32\drivers\BlogDriver.sys" -Force
pnputil /add-driver C:\Users\user\Desktop\BlogDriver.inf /install
# UpperFilters 설정
$devicePath = "HKLM:\SYSTEM\CurrentControlSet\Enum\VMBUS\{f8615163-df3e-46c5-913f-f2d2f965ed0e}\{6d936397-af30-4df3-8f97-112457600670}"
New-ItemProperty -Path $devicePath -Name "UpperFilters" -Value @("BlogDriver") -PropertyType MultiString -Force
sc config BlogDriver start= system
sc config BlogDriver group= NDIS
shutdown /r /t 0


# 1. 현재 UpperFilters 확인
Get-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Enum\VMBUS\{f8615163-df3e-46c5-913f-f2d2f965ed0e}\{6d936397-af30-4df3-8f97-112457600670}" -Name UpperFilters -ErrorAction SilentlyContinue

# 2. UpperFilters 수동 추가
$devicePath = "HKLM:\SYSTEM\CurrentControlSet\Enum\VMBUS\{f8615163-df3e-46c5-913f-f2d2f965ed0e}\{6d936397-af30-4df3-8f97-112457600670}"
New-ItemProperty -Path $devicePath -Name "UpperFilters" -Value @("BlogDriver") -PropertyType MultiString -Force

# 3. 확인
Get-ItemProperty -Path $devicePath -Name UpperFilters

# 4. 디바이스 재시작 (네트워크 어댑터 비활성화/활성화)
Disable-NetAdapter -Name "Ethernet*" -Confirm:$false
Start-Sleep -Seconds 2
Enable-NetAdapter -Name "Ethernet*" -Confirm:$false

# 또는 시스템 재부팅
shutdown /r /t 0