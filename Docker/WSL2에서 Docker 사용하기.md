# WSL2에서 Docker 사용하기

[comment]: <> (## 목차)

[comment]: <> (1. [WSL/Linux 설치]&#40;#WSL/Linux/Docker-설치&#41;)

[comment]: <> (2. [GitLab 설치]&#40;#Gitlab-설치&#41;)

[comment]: <> (3. [Jenkins 설치]&#40;https://www.notion.so/bad25950c64c4727a5f67f1d144a5678&#41;)

---

### WSL/Linux/Docker 설치

1. Linux용 Windows 하위 시스템 / Virtual Machine 활성화
    - Windows Powershell(**관리자모드로 실행)**에 아래 명령어 실행 후 재시작

    ```powershell
    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
    ```

2. [Linux 커널 업데이트 패키지 다운로드](https://docs.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
    - `This update only applies to machines with the Windows Subsystem for Linux` 메세지 표시시
        - Windows 버전 확인
        - Windows 기능 켜기/끄기 > Linux용 Windows 하위 시스템 기능, 가상 머신 플랫폼 기능 활성화 후 재부팅
3. WSL2를 기본 버전으로 설정
    - Windows Powershell을 **관리자모드로 실행**시킨 후, 아래 명령어 실행

    ```powershell
    wsl --set-default-version 2
    ```

4. [Linux 다운로드](https://www.microsoft.com/ko-kr/p/ubuntu-2004-lts/9n6svws3rx71?activetab=pivot:overviewtab)
    - 설치 후 실행하여 ID/PW 설정
5. Linux 설치 확인
    - Windows Powershell을 **관리자모드로 실행**시킨 후, 아래 명령어 실행

    ```powershell
    wsl -l -v
    ```


6. [Docker Desktop 설치](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
    - 설정 → General → Use WSL2 based engine 옵션 체크
    - Resource → WSL Integration에서 버전 확인
    - Windows Powershell을 **관리자모드로 실행**시킨 후, docker 명령어 사용 가능한지 확인