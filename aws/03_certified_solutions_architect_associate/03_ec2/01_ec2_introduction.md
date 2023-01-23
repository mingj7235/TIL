# EC2

## 개요

- AWS 클라우드 컴퓨팅 서비스 = 클라우드 가상 서버 (Virtual Machine)

- EC2 클라우드 가상 서버를 `'인스턴스'` 라고 칭한다. 

<br>

## 원격 접속 - SSH 연결 (Linux 인스턴스)

- SSH 프로토콜을 이용해 `Linux 인스턴스`에 원격으로 연결 및 파일 전송 가능

- SSH 은 보안을 통해 원격으로 접속하기 위한 방식을 의미한다. 

    - Secure Shell Protocol

    - `TCP Port 22` 가 열려있어야 접속이 가능하다. 

- 아이디, 패스워드 방식이 아닌 Public Key 와 Private Key 를 이용해 접속

<br>

## 원격 접속 - RDP 연결

- RDP 프로토콜을 이용해 `Windows 인스턴스`에 원격으로 연결 및 파일 전송 가능

- RDP 는 Windows OS 를 원격으로 접속하기 위한 방식

    - Remote Desktop Protocol

    - `TCP Port 3389` 가 열려있어야 접속이 가능

- 아이디, 패스워드를 이용해 접속

- 윈도우의 `원격 데스크톱 연결 프로그램`을 사용하여 원격 접속한다.

<br>

## 원격 접속 - Instance Connect 연결 

- 웹 브라우저를 이용해 EC2 인스턴스에 연결 (Linux 인스턴스만 연결 가능)

- SSH 프로토콜을 사용하여 일회용 SSH 퍼블릭키를 인스턴스 메타데이터에 업로드하여 EC2 연결

- SSH 프로토콜을 사용하기 22번 포트가 오픈 되어 있어야 연결가능

- PowerShell, Putty 등을 이용한 SSH 연결처럼 프라이빗 키를 다운받을 필요 없다.