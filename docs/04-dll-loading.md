# Windows DLL 로딩 메커니즘 분석

## 다룬 실습 주제
이 프로젝트에서 다룬 수업 주제는 다음과 같다.

1. Windows DLL의 생성과 모듈 로딩 확인
2. AppInit_DLLs 관련 레지스트리 설정 및 동작 관찰
3. DLL 로딩 전후 프로세스 모듈 목록 확인
4. PE 구조와 Import Table 사이의 관계 분석
5. 별도 실행 파일 복사본(`TextView_Patch.exe`)을 대상으로 변경·검증 과정 학습

## 주요 도구
- **Visual Studio**: Win32/Release 빌드 및 소스 검토
- **PEView**: DOS/NT 헤더, 섹션, Import Table 정적 분석
- **HxD**: 파일 바이트 검사, 오프셋 이동, 파일 비교
- **Process Explorer**: 프로세스 및 DLL 모듈 확인
- **PowerShell**: 실행 파일 테스트와 생성된 파일 확인

## 관련 파일
- `TextView.cpp`: 실습용 실행 파일 소스
- `myhack3.cpp`: 실습용 DLL 소스
- `TextView.exe`: 빌드된 원본
- `TextView_Patch.exe`: 분석/변경 대상으로 별도 복사
- `myhack3.dll`: Visual Studio 빌드가 이루어진 DLL

## AppInit_DLLs와 Import Table은 별개의 메커니즘
- **Import Table**은 실행 파일이 정적으로 참조하는 모듈과 함수를 나타낸다.
- **AppInit_DLLs**는 Windows의 특정 사용자 인터페이스 관련 DLL 로딩 동작에 관여할 수 있는 레거시 메커니즘이다. OS 설정, 보안 정책, Secure Boot 등에 따라 동작 여부가 달라질 수 있다.
- **런타임 로딩 모듈**은 실제 프로세스가 실행되는 동안 Process Explorer 등에서 확인해야 한다.

## 결과 기록
앞선 대화에서 AppInit_DLLs 관련 레지스트리 설정과 Process Explorer 확인을 시도한 기록이 있다. 하지만 현재 이 저장소에 실행 중 프로세스의 DLL 목록을 확인할 수 있는 최종 증거 이미지는 없다. 따라서 성공/실패를 임의로 판정하지 않는다.

## 윤리 및 실습 범위
격리된 가상 환경과 본인이 생성한 샘플만 사용한다. 외부 시스템/사용자 프로세스에 대한 무단 DLL 로딩, 은닉·지속성 확보 실습으로 확장하지 않는다.
