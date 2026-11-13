# Import Table 분석 — PEView

## 목표
실행 파일이 외부 DLL 및 Windows API를 어떤 방식으로 참조하는지 정적 분석한다.

## 분석 경로
`TextView.exe → SECTION .rdata`를 펼쳐 다음 항목을 확인했다.

- `IMPORT Directory Table`
- `IMPORT Name Table`
- `IMPORT Address Table`
- `IMPORT Hints/Names`

## Import Directory에서 직접 확인한 DLL
| DLL | 역할 예시 |
|---|---|
| `KERNEL32.dll` | 파일·프로세스·스레드 관련 기초 API |
| `USER32.dll` | 창과 메시지 등 사용자 인터페이스 |
| `GDI32.dll` | 그래픽 디바이스 인터페이스 |
| `SHELL32.dll` | Windows 셸 연동 |
| `VCRUNTIME140.dll` | Visual C++ 런타임 |
| `api-ms-win-crt-runtime-l1-1-0.dll` | C 런타임 API 집합 |
| `api-ms-win-crt-math-l1-1-0.dll` | 수학 관련 CRT API 집합 |
| `api-ms-win-crt-stdio-l1-1-0.dll` | 표준 입출력 CRT API 집합 |

표에서 일부 `api-ms-` DLL은 Import Directory 화면에 보인 것만 기록했다. 이것이 전체 목록이라는 뜻은 아니다.

## Import Name Table에서 확인한 함수 예
- `CreateFontW`, `GetStockObject` (GDI32)
- `GetLastError`, `GetStartupInfoW`, `ReadFile`, `CreateFileW` (KERNEL32)
- `DragAcceptFiles`, `DragQueryFileW` (SHELL32)
- `SetWindowTextW`, `UpdateWindow`, `LoadIconW`, `RegisterClassExW` (USER32)

## Import 필드 해석
- **Name RVA**: DLL 이름 문자열을 가리키는 상대 가상 주소
- **Import Name Table RVA**: 함수 이름/ordinal 참조 배열 위치
- **Import Address Table RVA**: 로더가 해석한 함수 주소를 담는 배열 위치
- **Time Date Stamp**: 바인딩 등과 관련된 메타데이터

## 중요한 구별
원본 `TextView.exe`의 정적 Import 목록에서 `myhack3.dll`은 확인되지 않았다. 이것은 **정적 Import 항목의 부재**만 의미하며, 해당 DLL이 런타임에 로드되지 않는다고 단정하는 증거는 아니다.
