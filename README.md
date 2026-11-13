# Windows PE & DLL Analysis Labs

> Windows Executable Internals · PEView · HxD · DLL Loading · Process Explorer  
> **Hands-on Windows security lab portfolio** | [한국어 문서]

## Overview

Windows 환경에서 PE(Portable Executable) 구조와 DLL 로딩을 학습하고, **PEView의 정적 분석**, **HxD의 16진수 데이터 확인**, **Process Explorer를 통한 동적 검증 절차**를 정리한 실습 포트폴리오입니다.

단순한 도구 사용을 넘어 **파일 오프셋과 PE 헤더 필드의 대응**, **Import Table 구조 분석**, **정적 의존성과 실제 DLL 로딩의 차이**, **관찰한 결과만으로 성공 여부를 판단하는 검증 방법**에 초점을 두었습니다.

### Tech stack

![Windows](https://img.shields.io/badge/Windows-PE%20Internals-0078D4?logo=windows)
![C++](https://img.shields.io/badge/C%2B%2B-Visual%20Studio-00599C?logo=cplusplus)
![Static Analysis](https://img.shields.io/badge/Static%20Analysis-PEView-555)
![Hex Analysis](https://img.shields.io/badge/Hex%20Analysis-HxD-555)
![Dynamic Analysis](https://img.shields.io/badge/Dynamic%20Analysis-Process%20Explorer-555)

## Lab index

| # | 실습 문서 | 핵심 내용 | 기록 상태 |
|---|---|---|---|
| 01 | [PE 구조 분석](docs/01-pe-format.md) | DOS/NT Header, 32-bit architecture, 5 sections | ✅ 확인 |
| 02 | [Import Table 분석](docs/02-import-table.md) | Import Directory, Name Table, DLL/API 목록 | ✅ 확인 |
| 03 | [HxD Hex 분석](docs/03-hxd-analysis.md) | MZ/PE 시그니처, 오프셋, 파일 비교 준비 | 🟡 비교 결과 미확인 |
| 04 | [DLL 로딩 메커니즘](docs/04-dll-loading.md) | AppInit_DLLs, Import Table, 런타임 로딩 구분 | 🟡 로딩 최종 결과 미확인 |
| 05 | [동적 검증](docs/05-process-explorer-verification.md) | PowerShell, Process Explorer, 파일 생성 검증 | ⬜ 실행 증거 대기 |
| 06 | [Troubleshooting](docs/06-troubleshooting.md) | 확장자, 오프셋, PE 필드, 정적/동적 결과 해석 | ✅ 기록 완료 |

[실습 결과 검증표 보기](reports/lab-results.md) · [스크린샷 업로드 가이드](screenshots/README.md)

## Environment & artifacts

- **OS:** Windows 실습 환경
- **IDE:** Microsoft Visual Studio (Release / Win32)
- **Static analysis:** PEView
- **Hex inspection:** HxD
- **Runtime verification tools:** Windows PowerShell, Sysinternals Process Explorer
- **Lab artifacts:** `TextView.cpp`, `TextView.exe`, `TextView_Patch.exe`, `myhack3.cpp`, `myhack3.dll`
- **Example build location:** `C:\Users\student\source\repos\TextView\Release`

> 본 저장소에는 **실습 분석 문서**를 게시하며, 실행 파일·DLL 바이너리를 배포하지 않습니다.

## Key findings — verified from lab screenshots

### 1. PE architecture
- `Machine: IMAGE_FILE_MACHINE_I386 (0x014C)` → **32-bit (x86)**
- `NumberOfSections: 5`
- Sections: `.text`, `.rdata`, `.data`, `.rsrc`, `.reloc`

### 2. PE signature and file offsets
- DOS header begins with `4D 5A` (**MZ**).
- PE signature `50 45 00 00` (**PE\0\0**) observed at file offset **`0xF8`**.
- `IMAGE_FILE_HEADER.Machine` resides at **`0xFC`** for this file.
- The `0x104` file offset corresponds to **TimeDateStamp**, not Machine.

### 3. Static imports
PEView의 `SECTION .rdata → IMPORT Directory Table`에서 다음 항목들을 관찰했습니다.

`KERNEL32.dll`, `USER32.dll`, `GDI32.dll`, `SHELL32.dll`, `VCRUNTIME140.dll` 및 일부 `api-ms-win-crt-*` 모듈.

Import Name Table에서는 `CreateFileW`, `ReadFile`, `GetLastError`, `SetWindowTextW`, `UpdateWindow`, `RegisterClassExW` 등 Windows API를 확인했습니다.

### 4. Verification-first methodology
**A successful build is not proof of successful DLL injection.**

실습 기록상 **파일 빌드, PE 구조 분석, Import Table 조회, HxD 시그니처 확인**은 확인되지만, **수정 실행 파일의 정상 동작, `index.html` 생성, Process Explorer에서 `myhack3.dll` 로딩**을 입증하는 결과 화면은 확보되지 않았습니다. 해당 결과는 의도적으로 **미검증**으로 남겨뒀습니다.

## Suggested evidence workflow

1. 원본·분석 대상 파일을 분리하고 빌드 버전 및 파일 해시를 기록
2. PEView에서 헤더/섹션/Import 정보를 정적 분석
3. HxD로 각 필드의 파일 오프셋 및 시그니처 교차 검증
4. 파일 비교 결과를 별도 캡처해 변경 여부 문서화
5. 격리된 환경에서 실행 결과와 생성 파일 확인
6. Process Explorer에서 실제 로드 모듈을 검증
7. 예상 결과와 실제 결과를 구분해 보고서 작성

## Repository structure

```text
windows-pe-dll-analysis-labs/
├── README.md
├── docs/
│   ├── 01-pe-format.md
│   ├── 02-import-table.md
│   ├── 03-hxd-analysis.md
│   ├── 04-dll-loading.md
│   ├── 05-process-explorer-verification.md
│   └── 06-troubleshooting.md
├── reports/
│   └── lab-results.md
├── screenshots/
│   └── README.md
└── .gitignore
```

## Skills demonstrated

**Reverse Engineering Fundamentals · PE/COFF File Format · Windows API Analysis · Static/Dynamic Analysis Separation · Hex Inspection · Forensic Evidence Validation · Technical Documentation**

## Scope & ethics

학습용 Windows VM 및 직접 생성한 샘플을 대상으로 진행했습니다. 제3자 시스템에 대한 무단 DLL 로딩이나 실행 파일 변조는 다루지 않습니다. 진행 상태는 대화 중 확보된 실습 화면과 확인된 정보에 한해 기록합니다.
