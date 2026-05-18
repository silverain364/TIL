# AWC EC2 - 클라우드에 내 서버 띄우기
EC2 하나만 알아도 서버를 인터넷에 올릴 수 있다.

### 개념
EC2(Elastic Compute Cloud)는 AWS에서 제공하는 가상 서버다.<br/>
클릭 몇 번으로 Linux 서버를 만들고, SSH로 접속해서 앱을 올릴 수 있다.<br/>
직접 서버를 사고 설치하는 것보다 훨씬 빠르고, 필요할 때 켜고 끄거나 사양을 바꿀 수 있다.

### 핵심 개념 먼저 짚기
`AMI` : 서버 OS 이미지다. Ubuntu, Amazon Linux 등 골라서 시작한다.<br/>
`인스턴스 타입` : 서버 사양이다. t2.micro, t3.small, c5.xlarge 등.<br/>
`Security Group` : 방화벽 역할이다. 어떤 포트를 열지 설정. 기본은 전부 막혀있다.<br/>
`Elatic IP` : 고정 IP다. 없음녀 재시작할 때마다 IP가 바뀐다.<br/>
`EBS` : EC2에 붙는 디스크. 기본 8GB부터 30GB까지다.

### EC2 띄우고 앱 배포까지 흐름
1. EC2 인스턴스 생성<br/>
2. 키 페어 생성하여 .pem 파일 저장<br/>
3. Security Group에서 22(SSH), 80(HTTP), 443(HTTPS) 포트 열기<br/>
4. ssh -i key.pem ubuntu@{퍼블릭IP}로 접속<br/>
5. Docker 설치 후 앱 pull & 실행

> **Elastic IP는 연결해두지 않으면 과금된다.** 생성만하고 인스턴스에 연결 안 하면 비용이 청구된다. 쓸 거면 바로 붙이고, 안 쓸 거면 해제하자.<br/>
> **Security Group 0.0.0.0/0 주의** 모든 IP 허용은 편하지만 위험하다. SSH(22)는 내 IP만 허용하고, 서비스 포트(80/443)만 전체 오픈하는 게 기본 보안 설정이다.
