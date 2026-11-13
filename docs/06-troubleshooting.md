# 실습 중 문제 해결 기록

## 1. 파일 확장자 표시 차이
Windows 탐색기에서 확장자 표시 옵션에 따라 `TextView.exe`가 `TextView`로 보였다. 파일 형식을 확인해 원본 실행 파일과 `TextView_Patch.exe`를 구분했다.

## 2. HxD 오프셋 읽기 오류
`0x3C` 이동 이후 데이터 변환기의 이진수 `11111000`을 확인했다. 이는 선택된 바이트 `0xF8`을 뜻한다. 실제 PE 시그니처는 해당 빌드에서 **파일 오프셋 `0xF8`**에 나타났고, HxD에서 `50 45 00 00`으로 확인했다.

핵심: 2진수 변환기에 보인 단일 바이트와 PE 헤더 주소를 나타내는 4바이트 필드를 혼동하지 않는다.

## 3. PE FILE HEADER 오프셋 혼동
PEView 화면 기준 `Machine` 필드는 파일 오프셋 `0xFC`이고, `TimeDateStamp`는 `0x104`다. 주소 혼동이 생겼으나 PEView의 명시적 필드 라벨로 교차 검증해 수정했다.

## 4. Import Table 위치
초기에는 `IMAGE_IMPORT_DESCRIPTOR` 항목을 바로 찾으려 했으나, 이 파일의 PEView 화면에서는 `SECTION .rdata`를 펼쳐 `IMPORT Directory Table` 및 `IMPORT Name Table`에 접근했다.

## 5. 성공 판정 기준
- 빌드 성공 ≠ DLL이 대상 프로세스에 로드됨
- 파일 복사 성공 ≠ 패치 성공
- Import Name Table 열람 ≠ 런타임 로딩 증명
- HxD 비교 메뉴 접근 ≠ 두 파일의 차이 판독 완료
- 다운로드 구현 소스 ≠ 실제 다운로드 성공

각 단계는 별도의 증거 화면과 재현 조건으로 기록한다.
