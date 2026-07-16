Power Shell
===

### 시작 프로그램에 바로 파워쉘 등록하는 방법
1. `test.ps1` 이라는 파일을 시작프로그램에 등록하고 싶음
1. `test.bat` 파일 작성
    ```
    powershell.exe -Noprofile -Noninteractive -Nologo -ExecutionPolicy Bypass "C:\Users\sch\Documents\test.ps1"
    ```
1. `C:\Users\sch\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` 에 해당 배치 파일을 배치함
>배치 실행한 콘솔에 대해 최소화나 기타 설정을 하고 싶다면 바로가기를 만들어서 속성에서 변경할 것

<br>

### 실행정책
```ps1
Set-ExecutionPolicy : Windows PowerShell updated your execution policy successfully, but the setting is overridden by
a policy defined at a more specific scope. Due to the override, your shell will retain its current effective
execution policy of RemoteSigned. Type "Get-ExecutionPolicy -List" to view your execution policy settings. For more
information please see "Get-Help Set-ExecutionPolicy".
At line:1 char:1
+ Set-ExecutionPolicy -ExecutionPolicy Restricted -Scope LocalMachine
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : PermissionDenied: (:) [Set-ExecutionPolicy], SecurityException
    + FullyQualifiedErrorId : ExecutionPolicyOverride,Microsoft.PowerShell.Commands.SetExecutionPolicyCommand
```
파워쉘을 실행시킬 때 이러한 메시지가 발생하는 경우가 있는데 이런 경우 각 Scope에 맞춰서 __[정책을 변경](#실행정책-변경)__ 해줘야함

|Scope|Description|
|-|-|
|MachinePolicy|GPO의 컴퓨터 정책을 통해 설정|
|UserPolicy|GPO의 사용자 정책을 통해 설정|
|Process|현재 실행 중인 파워쉘 세션에만 적용|
|CurrentUser|현재 사용자에 적용|
|LocalMachine |로컬 컴퓨터에 적용|

|ExecutionPolicy|Description|
|-|-|
|Undefined|ExecutionPolicy를 설정하지 않았다는 의미. 기본 정책인 Restricted로 작동|
|Restricted|Windows 10의 ExecutionPolicy 기본 값. 기본적으로 스크립트 파일이 실행되지 않으나 Microsoft에서 만든 일부 스크립트 파일들은 실행이 가능한 경우가 있음|
|Unrestricted|Microsoft에서도 권장하지 않는 옵션으로 서명되지 않은 스크립트를 포함한 모든 스크립트를 실행할 수 있음|
|AllSigned|신뢰할 수 있는 인증기관이 서명한 스크립트만 실행 가능함. 로컬 컴퓨터에서 작성된 스크립트라 하더라도 신뢰할 수 있는 인증기관이 서명하지 않았다면 실행이 불가능|
|Bypass |다른 어플리케이션 내에 파워쉘 스크립트가 내장되거나 별도의 자체 보안 설정을 갖추었을 때 사용함. 차단되거나 별다른 경고 없이 실행 가능|
|RemoteSigned |Microsoft에서 권장하는 옵션으로 Windows Server 2012 R2 이후의 기본 값. 로컬 컴퓨터에서 작성된 모든 스크립트는 실행이 가능하고 인터넷에서 다운로드한 스크립트는 인증기관이 발행한 코드로 서명되어야만 실행이 가능|

<br>

### 실행정책 변경
```ps1
# Scope를 지정하지 않으면 LocalMachine에 적용됨
# MachinePolicy, UserPolicy는 그룹정책GPO로만 변경이 가능함
Set-ExecutionPolicy RemoteSigned

# Scope 지정
Set-ExecutionPolicy -scope CurrentUser RemoteSigned

# 파워쉘 실행시 지정
powershell.exe -ExecutionPolicy Bypass 
```

<br>

### 암호를 별도 파일로 실행하는 방법
1. 암호화된 텍스트 파일로 저장해서 실행
    ```ps1
    # 암호 입력
    read-host -assecurestring | convertfrom-securestring | out-file "C:\password.txt"
    *****

    # 파일 읽기
    $encrypted = Get-Content "C:\password.txt" | ConvertTo-SecureString
    $ptr = [System.Runtime.InteropServices.Marshal]::SecureStringToBSTR($encrypted)
    $userPW = [System.Runtime.InteropServices.Marshal]::PtrToStringAuto($ptr)
    ```

1. 실행할 때마다 직접 입력
    ```ps1
    # 스크립트 맨 위에 추가
    $credential = Get-Credential -UserName "administrator" -Message "비밀번호 입력"
    $userPW = $credential.GetNetworkCredential().Password
    ```

<br>
