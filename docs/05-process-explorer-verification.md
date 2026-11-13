# Process Explorer / PowerShell 결과 검증 체크리스트

## 목적
정적 PE 분석과 동적 실행 관찰 결과를 별도로 검증해 오탐을 줄인다.

## Process Explorer
1. 격리된 실습 VM에서 대상 프로그램을 실행한다.
2. Process Explorer에서 대상 프로세스를 찾는다.
3. 하단 패널 표시 기능을 활성화한다.
4. 하단 패널 보기 방식에서 **DLLs**를 선택한다.
5. 로딩된 DLL의 이름과 경로를 확인한다.
6. 프로세스 이름, DLL 이름, 경로, 캡처 시각을 기록한다.

> DLL의 파일이 디스크에 존재하는 것과 프로세스에 DLL이 실제로 로드된 것은 다른 사실이다.

## PowerShell에서 확인할 항목
- 실행 경로 및 프로세스 종료 코드
- 프로그램 출력
- 생성/다운로드됐다고 주장하는 파일의 존재 여부
- 파일 크기, 경로, 필요시 해시
- 실패 시 오류 메시지

## 증거 기록 양식
| 검사 항목 | 기대 결과 | 실제 결과 | 상태 |
|---|---|---|---|
| TextView 빌드 | exe 존재 | 빌드 성공 화면 및 파일 존재 확인 | 확인 |
| myhack3 빌드 | dll 존재 | 빌드 성공 화면 및 파일 존재 확인 | 확인 |
| PEView 정적 구조 | PE 및 Import 구조 조회 | I386, 5 sections, Import 구조 확인 | 확인 |
| HxD 헤더 | MZ/PE 시그니처 | 확인 | 확인 |
| HxD 비교 | 두 파일 차이 확인 | 비교 메뉴 진입까지만 확인 | 미검증 |
| TextView_Patch 실행 | 정상 실행 | 실행 결과 없음 | 미검증 |
| index.html | 실제 파일 생성 | 파일 생성 증거 없음 | 미검증 |
| myhack3.dll 로드 | 프로세스 DLL 목록 표시 | 최종 화면 없음 | 미검증 |

## 스크린샷 권장
- `screenshots/01-peview-file-header.png`
- `screenshots/02-peview-import-names.png`
- `screenshots/03-peview-import-directory.png`
- `screenshots/04-hxd-pe-signature.png`
- `screenshots/05-hxd-file-compare.png`
- `screenshots/06-process-explorer-modules.png`

스크린샷 이름은 권장 파일명이며 실제 저장소에 업로드된 파일이 있다는 뜻은 아니다.
