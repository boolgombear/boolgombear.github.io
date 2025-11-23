---
title: "중첩 가상화 환경에서 커널 디버깅 설정하기 (VMware + Hyper-V)"
date: 2025-11-24 10:00:00 +0900
categories: [System, Debugging]
tags: [nested-virtualization, kernel-debugging, windbg, vmware, hyper-v, bcdedit]
---

## 개요

중첩 가상화 환경(VMware 내부에 Hyper-V)에서 커널 디버깅을 설정하는 방법을 다룹니다. 이 가이드는 L1(VMware 게스트)과 L2(Hyper-V 게스트)에서 네트워크 커널 디버깅을 구성하는 단계를 포함합니다.

## 사전 준비

- **호스트 시스템**: WinDbg가 설치된 Windows 환경
- **L1 게스트**: VMware에서 실행되는 Windows (Hyper-V 역할 설치)
- **L2 게스트**: Hyper-V에서 실행되는 Windows
- **관리자 권한**: 모든 설정은 관리자 권한이 필요합니다

## 1. 네트워크 버스 정보 확인

디버깅 설정을 위해 먼저 네트워크 어댑터의 버스 정보를 확인합니다.

**PowerShell (관리자 권한)**에서 실행:

```powershell
Get-NetAdapterHardwareInfo | Format-Table
```

> 💡 **팁**: 출력된 결과에서 `Bus`, `Device`, `Function` 정보를 확인하세요. 이 값은 `busparams` 설정에 사용됩니다.

## 2. L1 (VMware 게스트) 설정

### 커널 및 하이퍼바이저 디버깅 설정

**CMD (관리자 권한)**에서 실행:

```cmd
bcdedit /debug on
bcdedit /dbgsettings net hostip:192.168.0.96 port:50020 key:1.2.3.4
bcdedit /set {dbgsettings} busparams 3.0.0
bcdedit /hypervisorsettings net hostip:192.168.0.96 port:50021 busparams:4.0.0 key:1.2.3.4
bcdedit /set hypervisordebug on
bcdedit /set hypervisorlaunchtype auto
```

### 설정 확인

```cmd
bcdedit /dbgsettings
bcdedit /hypervisorsettings
```

> ⚠️ **주의사항**:
> - `hostip`: 호스트(디버거가 실행될 시스템)의 IP 주소로 변경
> - `key`: 원하는 임의의 키 값으로 설정 (예: 1.2.3.4)
> - `busparams`: `Get-NetAdapterHardwareInfo` 결과에 맞게 변경 (예: 3.0.0)

### WinDbg 연결 명령어

호스트 시스템에서 WinDbg를 실행하여 연결:

```powershell
# 커널 디버깅 연결
start windbgx -k net:port=50020,key=1.2.3.4

# 하이퍼바이저 디버깅 연결
start windbgx -k net:port=50021,key=1.2.3.4
```

> 💡 **참고**: `port`와 `key`는 위에서 설정한 값과 동일하게 사용해야 합니다.

## 3. L2 (Hyper-V 게스트) 설정

### Secure Boot 비활성화

Hyper-V 가상 머신의 설정을 변경하기 전에 **Secure Boot**를 비활성화해야 합니다.

1. Hyper-V 관리자에서 가상 머신 선택
2. 마우스 오른쪽 클릭 → **설정(Settings)**
3. **보안(Security)** → **Secure Boot** 체크 해제
4. 적용 및 확인

### 커널 디버깅 설정

L2 게스트 내부에서 **CMD (관리자 권한)** 실행:

```cmd
bcdedit /debug on
bcdedit /dbgsettings net hostip:192.168.0.96 port:50020 key:1.2.3.4
bcdedit /set {dbgsettings} busparams 3.0.0
```

> ℹ️ **참고**: 
> - L2에서는 **커널 디버깅만** 설정합니다 (하이퍼바이저 디버깅 불필요)
> - `hostip`는 L1 게스트의 IP 주소로 설정해야 합니다

## 설정 매개변수 설명

| 매개변수 | 설명 | 예시 |
|---------|------|------|
| `hostip` | 디버거가 실행되는 시스템의 IP 주소 | 192.168.0.96 |
| `port` | 디버깅에 사용할 네트워크 포트 | 50020, 50021 |
| `key` | 디버거와 디버기 간의 암호화 키 | 1.2.3.4 |
| `busparams` | 네트워크 어댑터의 버스 위치 | 3.0.0 (Bus.Device.Function) |

## 재부팅 및 연결 확인

모든 설정이 완료되면:

1. L2 게스트 재부팅
2. L1 게스트 재부팅
3. 호스트에서 WinDbg 실행 및 연결 대기
4. 게스트 부팅 시 자동으로 디버거에 연결됨

## 문제 해결

### 연결이 안 될 때

```cmd
# 방화벽 확인 (호스트 및 L1)
netsh advfirewall firewall add rule name="WinDbg" dir=in action=allow protocol=TCP localport=50020-50021

# 설정 초기화 (필요시)
bcdedit /deletevalue debug
bcdedit /deletevalue dbgsettings
```

### busparams 찾기

```powershell
Get-NetAdapterHardwareInfo | Select-Object Name, Bus, Device, Function
```

형식: `Bus.Device.Function` (예: Bus=3, Device=0, Function=0 → 3.0.0)

## 참고 자료

- [Microsoft Docs - Setting Up Kernel-Mode Debugging over a Network Cable](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/setting-up-a-network-debugging-connection)
- [WinDbg Documentation](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/)
- [BCDEdit Command-Line Options](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/bcdedit--set)