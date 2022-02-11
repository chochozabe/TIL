# WSL2에서 배포환경 구축하기

## 목차

1. [WSL/Linux/Docker 설치](#wsllinuxdocker-설치)

2. [GitLab 설치 및 배포](#gitlab-설치-및-배포)

3. [Jenkins 설치 및 배포](#jenkins-설치-및-배포)

---

### WSL/Linux/Docker 설치

1. Linux용 Windows 하위 시스템 / Virtual Machine 활성화

    - Windows Powershell(**관리자모드로 실행)**에 아래 명령어 실행 후 재시작

   ```shell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

2. [Linux 커널 업데이트 패키지 다운로드](https://docs.microsoft.com/ko-kr/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
    - `This update only applies to machines with the Windows Subsystem for Linux` 메세지 표시시
        - Windows 버전 확인
        - Windows 기능 켜기/끄기 > Linux용 Windows 하위 시스템 기능, 가상 머신 플랫폼 기능 활성화 후 재부팅
3. WSL2를 기본 버전으로 설정

    - Windows Powershell을 **관리자모드로 실행**시킨 후, 아래 명령어 실행

   ```shell
   wsl --set-default-version 2
   ```

4. [Linux 다운로드](https://www.microsoft.com/ko-kr/p/ubuntu-2004-lts/9n6svws3rx71?activetab=pivot:overviewtab)
    - 설치 후 실행하여 ID/PW 설정
5. Linux 설치 확인

    - Windows Powershell을 **관리자모드로 실행**시킨 후, 아래 명령어 실행

   ```shell
   wsl -l -v
   ```

6. [Docker Desktop 설치](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
    - 설정 → General → Use WSL2 based engine 옵션 체크
    - Resource → WSL Integration에서 버전 확인
    - Windows Powershell을 **관리자모드로 실행**시킨 후, docker 명령어 사용 가능한지 확인

<br>

### GitLab 설치 및 배포

```shell
docker pull gitlab/gitlab-ce

docker run -d -p [사용할 포트]:80 --name [컨테이너명] --restart always \
-v [사용할 디렉토리 위치]/config:/etc/gitlab:Z -v [사용할 디렉토리 위치]/logs:/var/log/gitlab:Z \
-v [사용할 디렉토리 위치]/data:/var/opt/gitlab:Z -v [사용할 디렉토리 위치]/config/ssl:/etc/gitlab/ssl:Z \
gitlab/gitlab-ce
```

- gitlab 관리자 계정 초기화
    - gitlab 컨테이너 접속
      ```shell
      > docker exec -it [컨테이너명] /bin/bash
      ```
    - rails console 접속
      ```shell 
      # gitlab-rails console -e production
      ```
    - root 계정 찾기
      ```shell
      > user = User.where(id: 1).first
      ```
    - 비밀번호 변경
      ```shell
        > user.password='변경할비밀번호'
        > user.password_confirmation='변경할비밀번호'
        > user.save!
      ```
      💡 비밀번호가 너무 짧으면 변경되지않으니 주의

<br>

### Jenkins 설치 및 배포

```shell
docker pull jenkins/jenkins:lts

docker run -p [사용할 포트]:8080 -d --restart always --name [컨테이너명 지정] 
 -v [사용할 디렉토리 위치]:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -u root jenkins/jenkins:lts```