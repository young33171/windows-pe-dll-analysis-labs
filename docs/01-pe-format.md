# PE 파일 구조 분석

## 목표
Windows PE(Portable Executable) 실행 파일의 헤더, 섹션, Import 정보를 PEView로 읽고 HxD의 오프셋과 대응한다.

## 분석 대상
- 원본: `TextView.exe`
- 분석용 복사본: `TextView_Patch.exe`
- 빌드 환경: Visual Studio, Windows, Release/Win32
- 화면에 표시된 경로: `C:\Users\student\source\repos\TextView\Release`

## 확인된 구조 (2026-10-30 실습 화면)
| 항목 | 확인된 값 | 의미 |
|---|---|---|
| IMAGE_FILE_HEADER.Machine | `0x014C` / `IMAGE_FILE_MACHINE_I386` | x86 32비트 실행 파일 |
| NumberOfSections | `0x0005` | 섹션 5개 |
| PE signature | `50 45 00 00` at file offset `0xF8` | PE\0\0 |
| Section names | `.text`, `.rdata`, `.data`, `.rsrc`, `.reloc` | 코드, 읽기 전용 데이터, 데이터, 리소스, 재배치 |

## PEView → HxD 교차 검증
1. PEView에서 `IMAGE_NT_HEADERS → IMAGE_FILE_HEADER`를 확인했다.
2. HxD에서 오프셋 이동을 사용해 PE 시그니처 `0xF8`을 확인했다.
3. PEView의 `IMAGE_FILE_HEADER`에서 Machine이 `I386`임을 확인했다.
4. HxD의 `0xFC`는 Machine 필드이고, `0x104`는 TimeDateStamp 필드다. 초기에 둘을 혼동했으나 교정했다.
5. DOS 헤더 `e_lfanew`는 PE 헤더를 가리키는 4바이트 리틀 엔디언 값이며, 파일 오프셋 `0x3C`에 있다.

### 섹션 해석
- `.text`: 실행 코드
- `.rdata`: 상수 데이터 및 해당 파일의 Import 테이블
- `.data`: 쓰기 가능한 초기화 데이터
- `.rsrc`: 리소스
- `.reloc`: 재배치 정보

## 기록 주의
현재 업로드된 화면은 PE 구조 확인을 입증하지만 실행 파일 패치 성공이나 DLL 강제 로드 성공을 입증하지는 않는다.
