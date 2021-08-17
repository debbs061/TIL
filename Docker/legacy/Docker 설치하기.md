# 도커 설치하기 & 컨테이너 접속하기

---

### AWS EC2 를 사용하고 있다고 가정하고, 그 위에 도커를 설치해보겠다. 스프링부트 프로젝트를 이미지화 시켜서 도커로 말아올리는 작업을 할 예정이다. 

1. 도커 설치하기 

```shell
# 로컬 레파지토리를 최신으로 update
sudo apt-get update 

sudo apt-get install docker-io --yes

# 도커 client, server 버전 확인
$ docker version

# 축약형 버전 확인
$ docker -v
```



2. 도커 데몬 실행 

```shell
$ brew services start docker

# 도커 이미지 확인
$ docker search ubuntu
$ docker search mysql

# 도커 이미지 다운받기 (latest 생략 가능)
$ docker pull ubuntu:latest 

# 같은 이름(ubuntu)을 갖고 있지만 태그가 다른 image 를 출력한다.
$ docker ubuntu images 
```



3. 유저 정보에 도커 권한 부여

```shell
# ec2-user 에게 도커를 읽고 쓸 수 있는 권한을 부여한다
sudo setfacl -m user:ec2-user:rw /var/run/docker.sock

# 제대로 접속되는지 확인
docker ps -a
```



4. 도커 컨테이너에 접속하기 

```shell
# docker exec -it 컨테이너 이름 /bin/bash
docker exec -it jira-container /bin/bash

# 도커 컨테이너에 접속되면서, 더이상 ec2 입력창이 아닌 도커 컨테이너 위에서 새로 command 를 입력할 수 있도록 된다.

# 로그 파일 보기
cd log
```



5. 도커 이미지 만들기 (Dockerfile) 및 실행

EC2 위에 Dockerfile 을 만들어보자

```shell
# Dockerfile 생성 (보통 root 디렉토리에 생성)
vi Dockerfile
```



Dockerfile 기본 구조

```bash
# 이미 가상서버에는 tomcat 9 버전과 jre가 깔려있는 상태라고 가정하자.
# 이런 가상서버가 없었다면 tomcat, open-jdk 를 일일이 설치하는 과정을 이 dockerfile 에 넣어야 했을 것이다.

# 기존에 alpine 에 올라가있는 tomcat 관련 이미지를 뜻한다. 
FROM tomcat:9-jre8-alpine 

# server.xml 에 jndi 정보
COPY server.xml /usr/local/tomcat/conf

# 기존 tomcat 내용 삭제
RUN rm -rf /usr/local/tomcat/webapps/ROOT.war
RUN rm -rf /usr/local/tomcat/webapps/ROOT
RUN rm -rf /usr/local/tomcat/webapps/docs
RUN rm -rf /usr/local/tomcat/webapps/examples
RUN rm -rf /usr/local/tomcat/webapps/host-manager
RUN rm -rf /usr/local/tomcat/webapps/manager

# war 파일 복사
COPY ROOT.war /usr/local/tomcat/webapps

# docker container 의 timezone 을 서울로 변경
ENV TZ=Asia/Seoul
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# 포트정보 기입 
EXPOSE 8080
```



6. 도커 이미지 만들기 및 실행

```shell
# 현재 경로에 있는 모든 파일들을 가지고 review/aws 라는 Tag명으로 도커 이미지를 만들겠다. 
~ $ docker build -t review/aws . 
```



7. 도커 이미지 확인하기 및 필요시, 도커 이미지 삭제하기

```bash
# 도커 이미지 확인
docker images

# 도커 이미지 삭제하기 (docker rmi 이미지ID)
docker rmi c553..
```



8. 도커 이미지 내역 확인하기

```shell
# 실제로 해당 docker 안에서 실행 됐던 모든 명령어들 혹은 변경사항을 볼 수 있다. 
# docker image history :IMAGEID
docker image history c553015...
```



9. 도커 실행하기

```shell
# 도커 이미지 실행하기
docker run --rm -d p 8080:8080 review/aws
```



10. 제대로 실행됐는지 ip:port정보 입력해서 사이트 들어가보기 (도커 실행화면 띄우기)

---

#### 참조

* [aws/docker 실전 클라우드 강의](https://fastcampus.co.kr/courses/201520/clips/)