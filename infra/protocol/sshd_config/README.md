# SSH 설정 법 
> 1. Match option 목록

---
## 1. Match option 목록
```shell
 #vi ./sshd_conf
 
 ...
 
 Match [조건] [값]
     [옵션]
     
     ...
     
```
- `Match` : ssh 접속시 특정 조건에 따른 설정
  - 조건 : 
    - `User` : 계정에 대한 조건 설정
    - `Group` : 특정 그룹에 대한 조건 설정
    - `Address` : 접근 IP 주소에 대한 조건 설정
    - `LocalAddress` : 수신한 IP 주소 대한 조건 설정
    - `LocalPort` : 수신한 포트 대한 조건 설정
    - `Host` : 수신한 도메인 이름에 대한 조건 설정
  - 옵션 : 
    - `ChrootDirectory` :  격리할 디렉토리, 즉 접속시 루트디렉토리 지정, 사용자는 이 디렉토리 외부로 나갈 수 없음
    - `PasswordAuthentication` :  비밀번호로 로그인 설정 허용여부
    - `ForceCommand` : 특정 명령만 실행할 수 있게 설정, 다른 쉘 접근 명령 불가능
    - `PermitRootLogin` : root 로그인 허용 및 금지 설정
    - `AllowTcpForwarding` : 포트포워딩 허용 여부
    - `X11Forwarding` : X11 GUI 프로그램 포워딩 허용 여부
    - `PermitOpen` : 특정IP포트의 포트 포워딩 제한여부
    - `PermitTTY` : tty 허용여부
    - `AllowAgentForwarding` : ssh인증 에이전트 포워딩 허용여부
    - `MaxSessions` : 최대 동시 연결 세션 개수
    - `MaxAuthTries` : 인증 실패 시도 횟수 제한 개수

---