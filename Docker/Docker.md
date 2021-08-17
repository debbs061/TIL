### 도커에 mysql 설치해서 로컬 디비 이용하기

> **로컬 디비 환경 구축을 위해 Docker 를 이용하게 된 계기**

저는 클라우드에 있는 개발 DB 로 작업을 하고 있습니다. 그러다보면 문제점이 생길 수 있는데요.

여러명이 하나의 DB를 바라보고 있으면 내가 고치지도 않은 테이블의 스키마가 바뀌어 있고, 나는 분명 A 테이블에 데이터를 넣었는데, 다른 개발자가 테스트를 위해 A 테이블의 데이터를 전부 날려버리면 난감한 상황이 생기기 시작하는거죠.

그래서 로컬에다가 각자 DB 환경을 구축하려 하는데요. **그렇다면 DB를 각자 로컬에다가 알아서 깔면 되는데, 왜 Docker 를 이용해서 까는걸까요?**

> **도커 구조**

도커를 이용하기 이전에, 도커가 어떤 구조로 이루어져 있는지 봅시다.

도커는 **server-client 구조**를 띄고 있습니다.

하나는 클라이언트로서의 도커이고, 다른 하나는 서버로서의 도커입니다. **실제로 컨테이너를 생성하고 실행하며 이미지를 관리하는 주체는 도커 서버**입니다. 도커 엔진은 외부에서 API 입력을 받아 도커 엔진의 기능을 수행하는데, 도커 프로세스가 실행되어 서버로서 입력을 받을 준비가 된 상태를 도커 데몬이라고 합니다.

![img](https://cdn-images-1.medium.com/max/1600/1*md3TZOlLRB7j24PvROh46Q.png)

도커가 설치된 호스트에 접속해 docker 명령어를 입력하면 아래와 같은 과정으로 도커가 제어됩니다.

1. 사용자가 `docker ps` 와 같은 도커 명령어를 입력합니다.
2. `/usr/bin/docker` 는 `/var/run/docker.sock` 유닉스 소켓을 사용해 도커 데몬에게 명령어를 전달합니다.
3. 도커 데몬은 이 명령어를 파싱하고 명령어에 해당하는 작업을 수행합니다.
4. 수행 결과를 도커 클라이언트에게 반환하고 사용자에게 결과를 출력합니다.

그럼, 도커 엔진에서 가장 중요한 개념인 ‘이미지' 와 ‘컨테이너' 는 무엇일까요?

**(1) 이미지**

- 이미지는 컨테이너를 생성할 때 필요한 요소이며, 가상 머신을 생성할 때 사용하는 iso 파일과 비슷한 개념입니다.
- 도커 이미지는 우분투, CentOS 등 기본적인 리눅스 운영체제부터 아파치 웹 서버, MySQL 데이터베이스 등의 각종 애플리케이션까지 갖가지 종류가 있습니다.

**(2) 컨테이너**

- 이미지를 가지고 컨테이너를 생성할 수 있는데, 컨테이너를 생성하면 해당 이미지의 목적에 맞는 파일이 들어 있는 파일 시스템을 가진 **독립된 공간**이 만들어집니다.
- 컨테이너는 이미지에서 변경된 사항만 컨테이너 계층에 저장하므로 컨테이너에서 무엇을 하든지 **원래 이미지는 영향을 받지 않습니다**.

> **설치 과정**

- 이 과정은 macOS Big Sur 11.5 에서 진행되었습니다.

OS 에 따라 도커 설치 방법은 조금씩 다릅니다. mac 기준으로 아래 공식 홈페이지에서 다운받을 수 있습니다.

[**Install Docker Desktop on Mac**
*Estimated reading time: 5 minutes Welcome to Docker Desktop for Mac. This page contains information about Docker…*docs.docker.com](https://docs.docker.com/docker-for-mac/install/)

이제 도커 위에 MySQL을 설치해 이미지로 만들고, 팀원 모두와 공유할 수 있도록 docker hub 에 배포해 볼 것입니다.

여기서 주의할 점은, **컨테이너는 한 번 삭제가 되면 컨테이너 계층에 저장돼있던 데이터베이스의 정보도 삭제됩니다**. 고로, 실수로 컨테이너를 삭제하면 데이터를 복구할 수 없게 됩니다.

이를 방지하기 위해 호스트의 특정 경로에 디비 dump 파일을 넣어놓고, 도커 컨테이너로 해당 dump 파일이 마운트 되도록 설정할 것입니다.

도커를 사용하기에 앞서 설치된 도커 엔진의 버전을 확인합니다.

```
$ docker -v
Docker version 20.10.5, build 55c4c88
```

MySQL 5.7.30 컨테이너를 생성합니다.

```
$ docker run -d \
--name local-mysql
-e MYSQL_ROOT_PASSWORD=password \ 
-e MYSQL_DATABASE=tooning \
mysql:5.7.30
```

- **-d** : detached 모드로 컨테이너를 실행합니다. detached 모드는 컨테이너를 백그라운드에서 동작하는 애플리케이션으로써 실행하도록 설정합니다.
- **— name** : 컨테이너의 이름을 설정합니다.
- **-e** : 컨테이너 내부의 환경 변수를 설정합니다.
- **mysql:5.7.30** : 사용할 이미지 명입니다.

컨테이너가 제대로 실행됐는지 확인하기 위해 `docker ps` 명령어로 확인합니다.

```
$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                 NAMES
10feeda4e95f   mysql:5.7.30   "docker-entrypoint.s…"   7 seconds ago   Up 7 seconds   3306/tcp, 33060/tcp   local-mysql
```

이제, MySQL 환경변수 설정을 살짝 변경해보려 합니다.

`docker exec` 명령어를 사용하여 MySQL 컨테이너 내부에서 `/bin/bash` 프로세스를 실행합니다. 컨테이너 내부로 들어가는 작업입니다.

```
$ docker exec -it local-mysql /bin/bash
```

저는 max_allowed_packet 을 늘려주는 작업을 해줄 건데요.

MySQL 의 환경변수 파일이 위치한 `/etc/mysql/mysql.conf.d` 경로로 가서 max_allowed_packet 을 지정해주었습니다.

![img](https://cdn-images-1.medium.com/max/1600/1*w1i_xMT4pfzTMhJD9wQS6g.png)

이제 MySQL 컨테이너 안에서 설정한 내용까지 통째로 이미지로 만들어보겠습니다. 작업한 컨테이너 `local-mysql`을 `mysql:tooning` 이라는 이미지로 생성합니다.

```
$ docker commit local-mysql mysql:tooning
```

**그럼 기존에 도커 허브에서 다운받은** `**mysql:5.7.30**` **이미지와 직접 만든** `**mysql:tooning**` **이미지가 뭐가 다른건지는 어떻게 확인할 수 있을까요?**

`docker inspect` 명령어를 이용하면 컨테이너, 이미지 등 모든 도커 단위의 정보를 확인할 수 있습니다.

```
$ docker inspect mysql:5.7.30
$ docker inspect mysql:tooning
```

다음 그림은 각 이미지에 대한 `inspect` 명령어의 출력 결과 중 Layers 항목만 나타낸 것입니다.

- mysql:5.7.30 이미지의 Layer

![img](https://cdn-images-1.medium.com/max/1600/1*AkSabV09yHRgH0C0F5aoRg.png)

- mysql:tooning 이미지의 Layer

![img](https://cdn-images-1.medium.com/max/1600/1*8N96TjX5BhjrW6RoWws9Rw.png)

기존의 `mysql:5.7.30` 이미지에서 변경된 사항만 새로운 레이어로 저장이 되어 `mysql:tooning` 이미지로 생성된 것을 확인할 수 있습니다.

**즉, MySQL 환경설정 파일에서 max_allowed_packet 을 지정해준 것에 대한 변경사항이 하나의 Layer 로 추가된 것입니다**.

이제 팀원들과 공유할 수 있도록 새로 생성한 이미지를 저장소에 올려볼 건데요. 좀 전에 커밋해둔 `mysql:tooning` 이라는 이름으로는 이미지를 저장소에 올릴 수 없습니다. 이미지를 올리려면 저장소 이름을 앞에 붙여줘야 합니다. `docker tag` 명령어를 사용하면 이미지의 이름을 추가할 수 있습니다.

```
$ docker tag mysql:tooning 저장소이름/mysql:tooning
```

`docker images` 명령어를 이용해 image 가 제대로 생성되었는지 확인해봅시다.

```
$ docker images
REPOSITORY      TAG       IMAGE ID       CREATED         SIZE
저장소이름/mysql   tooning   9fce79844e8f   6 seconds ago   499MB
mysql           5.7.30    9cfcce23593a   13 months ago   448MB
```

push 명령어를 입력해 이미지를 저장소에 올려보겠습니다.

```
$ docker push 저장소이름/mysql:tooning
```

docker hub 저장소로 가보면, 이미지가 제대로 들어온 것을 확인할 수 있습니다.

![img](https://cdn-images-1.medium.com/max/1600/1*0dpGLj8h4zsmA3BTOz8Trw.png)

이제 마지막 단계로, 방금 저장소에 올린 도커 이미지를 다운받아서 로컬에 미리 다운받아 놓은 dump 파일을 컨테이너에서 마운트 하도록 설정해줄 것입니다. 그러려면 특정 경로에 db dump 파일을 미리 만들어 놓아야 합니다. 저는 `/var/db_dump/dev` 위치에 덤프 파일을 만들어 놓았습니다.

![img](https://cdn-images-1.medium.com/max/1600/1*EzWol1Or9qxbEfrd11vEmw.png)

정확한 테스트를 위해, 기존에 로컬에 있던 이미지를 전부 삭제하고 도커에서 pull 명령어를 입력해서 이미지를 다운받습니다.

```
$ docker pull 저장소이름/mysql:tooning
```

다운받은 이미지를 컨테이너로 띄웁니다.

```
$ docker run -d \
--name local-mysql 
-e MYSQL_ROOT_PASSWORD=password \ 
-e MYSQL_DATABASE=tooning \
-v /var/db_dump/dev:/docker-entrypoint-initdb.d
hoyu210/mysql:tooning
```

처음에 mysql:5.7.30 이미지를 컨테이너로 띄웠을 때와는 다르게, 새로 추가된 **-v 옵션**이 있습니다. 호스트의 특정 경로를 컨테이너로 마운트 시키는 옵션입니다.

![img](https://cdn-images-1.medium.com/max/1600/1*XhR4RLqLQ-RWsIGfcYobcw.png)

즉, 호스트의 `/var/db_dump/dev` 경로가 컨테이너의 `/docker-entrypoint-initdb.d` 경로로 마운트 되도록 설정해준 것입니다. dump 파일을 `/var/db_dump/dev` 경로에 넣어준 상태이므로, 컨테이너가 생성되면서 `/docker-entrypoint-initdb.d` 경로에 dump 파일이 마운트 될 것 입니다.

**※참고) 왜** `**/docker-entrypoint-initdb.d**` **경로에 dump 파일을 넣게 지정해줬을까요?**

[**mysql official image**](https://hub.docker.com/_/mysql) 에 보면 새로 생성한 database 가 sql dump 를 실행시키기 위해서는 `/docker-entrypoint-initdb.d` 경로에 dump 파일을 놓으면 된다고 명시되어 있습니다.

![img](https://cdn-images-1.medium.com/max/1600/1*DPHSATXrcWg-oRwesMwGhg.png)

다시 본론으로 돌아가서, 생성된 컨테이너의 `/docker-entrypoint-initdb.d` 경로에 dump 파일이 정말 존재하는 지 확인해 봅시다.

```
root@a4971a014b01:/docker-entrypoint-initdb.d# ls
dev_db.sql
```

위에서 생성해놓은 dev_db.sql 파일이 있는 걸 확인할 수 있습니다.

MySQL 이 data files 를 가지고 있는 경로인 `/var/lib/mysql` 로 가보면 dump 파일이 제대로 import 된 것도 확인할 수 있습니다.

```
root@a4971a014b01:/var/lib/mysql# ls
performance_schema sys tooning 
...
```

이렇게 Docker 를 이용해 mysql 을 설치하고 그걸 배포해서 가지고 있는 dump 파일을 마운트 시키는 작업까지 해보았습니다.

만약 mysql 을 팀원들 각자 로컬에 설치하도록 했다면, mysql 의 환경변수를 설정하는 작업을 각자 직접 해야 될 것입니다.

이 실습에서는 간단하게 max_allowed_packet 이라는 변수만 추가해주었지만, 만약 이보다 더 많은 환경 세팅이 들어가야 한다거나, mysql 뿐 아니라 웹 서버 등 어떠한 개발 환경을 구축해야 하는 경우에는 모두가 같은 환경을 구축하기 위해 같은 작업을 반복 해야 할 것입니다.

하지만 누구 한명이 이미지로 만들어 배포해두면, 각종 라이브러리 설치 등으로 인한 의존성도 걱정할 필요도 없이 이미지를 다운받는 모든 사람이 똑같은 개발 환경을 쉽게 이용할 수 있기 때문에 docker 를 이용하는 게 크나큰 장점이 되겠습니다. :)
