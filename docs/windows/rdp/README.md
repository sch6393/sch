RDP (Remote Desktop Protocol)
===

### 전제 조건
1. Windows 11 기준으로 __Pro__ 이상이여야 함
    >공식적으로 Home인 경우 원격 접속을 지원하지 않으나 우회하여 설정할 수는 있음. 이하의 __[설정 방법](#윈도우가-home인-경우의-원격-데스크톱-설정-방법)__ 을 참조
1. 원격 컴퓨터가 __원격 접속을 허용__ 해야함
1. 네트워크가 공용이 아닌 __개인__ 네트워크로 설정되어 있어야함

<br>

### 윈도우가 Home인 경우의 원격 데스크톱 설정 방법
1. 아래의 사이트들에서 필요한 파일들을 다운로드
    * [https://github.com/stascorp/rdpwrap/releases](https://github.com/stascorp/rdpwrap/releases)  
    * [https://github.com/Octanium91/rdpwupDownloads/releases](https://github.com/Octanium91/rdpwupDownloads/releases)

1. RDPWrap의 `install.bat` 파일을 관리자 권한으로 실행

1. 정상적으로 설치가 되었다면 RDPWrap의 `RDPConf.exe` 파일을 실행해서 상태를 확인
    * 현 시점에서는 Wrapper state는 `Installed`, Listener state는 `Not listening`, `[not supported]` 로 표시되어야 함

1. rdpwup의 `rdpwup.exe` 파일을 실행해서 `OS version` 값을 넣고 업데이트
    * 설정할 `OS version` 값은 `RDPConf.exe` 에서 표시되는 버전 값과 같아야 함

1. 정상적으로 완료가 되었다면 `RDPConf.exe` 에서 Listener state는 `Listening`, `[fully supported]` 로 표시됨

1. 윈도우 업데이트 등으로 `OS version` 이 달라지면 접속이 되지 않기 때문에 `OS version` 값이 달라졌다면 다시 수동으로 업데이트를 해줘야 함
    * 이걸 자동으로 해주는 프로그램이 있음. 아래 링크 참조
        * [https://github.com/asmtron/rdpwrap/blob/master/binary-download.md](https://github.com/asmtron/rdpwrap/blob/master/binary-download.md)

<br>
