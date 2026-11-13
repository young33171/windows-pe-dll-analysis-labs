# 실습 진행 상태 — 검증 중심 기록

최종 정리일: 2026-11-13  
화면을 바탕으로 한 실습 과정: 2026-10-30

| 영역 | 작업 | 확인 상태 |
|---|---|---|
| Visual Studio | `TextView.exe` Win32/Release 빌드 | 확인 |
| Visual Studio | `myhack3.dll` 빌드 | 확인 |
| 파일 준비 | `TextView_Patch.exe` 별도 복사 | 확인 |
| PEView | `IMAGE_FILE_HEADER`, I386, 5개 섹션 | 확인 |
| PEView | `.rdata` 내 Import Directory/Name Table | 확인 |
| HxD | MZ, PE 시그니처 및 오프셋 검사 | 확인 |
| HxD | 원본과 복사본 비교 메뉴 접근 | 확인 |
| HxD | 실제 파일 비교 결과 | 미확인 |
| PE 패치 | `myhack3.dll` 강제 로딩을 위한 유효 패치 | 미확인 |
| PowerShell | `TextView_Patch.exe` 실행 | 미확인 |
| 파일 결과 | `index.html` 생성 | 미확인 |
| Process Explorer | `myhack3.dll` 로딩 최종 증거 | 미확인 |

## 주요 학습 성과
- PEView의 파일 오프셋과 HxD Hex 뷰를 연결해 헤더를 검증했다.
- `Machine=0x014C`를 통해 실행 파일 아키텍처를 판독했다.
- Import Directory와 Import Name Table의 역할을 구분했다.
- 정적 import와 실제 런타임 DLL 로딩을 구분했다.
- 코드 빌드 결과, 파일 비교 결과, 실제 실행/네트워크 결과를 각각 별도의 증거로 판단하는 검증 프로세스를 확립했다.

## 아직 채워야 할 자료
1. HxD 데이터 비교의 실제 결과 캡처
2. 수정 전후 파일의 SHA-256 해시 및 비교 메모
3. PowerShell 실행 로그
4. `index.html` 생성 여부 및 파일 해시
5. Process Explorer 모듈 로딩 화면

검증되지 않은 결과를 성공으로 기재하지 않는다.
