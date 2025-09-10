# chicken-combo-deploy
[1시간만에 치킨콤보값으로 배우는 서버 배포] 치킨콤보값으로 스프링부트를 수동배포 -> 도커로 배포 -> Github Action CI/CD 순으로 배포과정을 배웁니다.




- java project를 build하면 .jar파일이 생성됨.
- 단순 서비스 실행 : java -jar demo-0.0.1-SNAPSHOT.jar
- 서비스를 실행해도 작동을 안하는데, AWS에 존재하는 보안그룹 설정을 해줘야함.
- 인스턴스 > 보안 > 보안 그룹 > 인바운드 규칙 편집 > 규칙 추가(포트 범위:8080 , CIDR 블록:0.0.0.0/0)
- http 로 접속해야함.
- 콘솔이 켜져있는 동안만 서버가 기동되어 있음.
- 리눅스의 백그라운드 실행 기능으로 서버를 기동시켜야함.



  
* CIDR 블록:0.0.0.0/0 :: 모든곳에서 요청 허용을 하겠다.
* 리눅스의 백그라운드 실행 명령어
*  - nohup : 터미널이 닫혀도 프로세스가 종료되지 않게 보호
   - & : 쉘 점유 안함
   - nohup.out : 백그라운드 실행된 로그가 저장되는 파일
* 리눅스 실행중인 8080포트를 사용하는 서비스 알려주는 명령어
*  - sudo lsof -i:8080
* 리눅스 실행중인 PID:145601 서비스를 종료하는 명령어
*  - sudo kill -9 145601


* Docker : 서버 배포
* Client : docker pull , docker build , docker run , docker push
* Docker_Host : docker daemon(Containers , images)
* Registry : application image

* Dockerfile : 요리 레시피
* Build : 조리
* Docker Image : 조리된 음식
* Run : 서빙
* Docker Container : 서빙된 음식

* Dockerfile(레시피)을 Docker Engine(요리사)이 Docker Build 명령어를 통해
* Docker Image(조리된 요리)를 만들고 Docker Push 명령어를 통해
* Docker Hub(메뉴판)에 올리고 Docker Pull 명령어를 통해
* EC2에 올리고 Docker Run 명령어를 통해
* Docker Container(서빙된 요리)를 사용자가 사용할 수 있게 만든다.

* Docker 사용 이유 : 어느 OS든 어느 환경에서든 빠르게 사용이 가능하다. 공통된 dockerfile을 사용하기 때문에.

* Dockerfile은 프로젝트에서 Dockerfile 로 만들면 됨.
  # 사용할 재료
  FROM openjdk:latest  # openjdk 최신버전을 사용하겠다.
  
  # 재료를 저장할 위치
  COPY build/libs/neo-0.0.1-SNAPSHOT.jar /app/app.jar  # build한 jar파일을 도커 파일(/app/app.jar)에 저장을 하겠다.
  
  # 재료를 통해 요리를 할 레시피
  ENTRYPOINT ["java", "-jar", "/app/app.jar"]  # java소스로 된 /app/app.jar로 되있는 .jar파일을 실행을 할 것이다.

* Docker Desktop이 실행중이고, Docker engine이 돌아가고 있어야함.
* 도커 빌드 : docker build -t {본인도커허브ID}/{프로젝트이름} .
* 도커 푸시 : docker push {본인도커허브ID}/{푸쉬할프로젝트이름}

