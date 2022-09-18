# Docker에서 Cubrid 설치하기

---

### 1-1. 설치하기 (이미지 생성)

##### (1) Docker에서 Docerfile 만들기 (3개파일 - Dockerfile, docker-compose-stack-ha.yml, docker-entrypoint.sh)

```docker
1. https://github.com/CUBRID/cubrid-docker/tree/master/10.2 에서 3개 파일은 복사한다.
2. Notepad++로 편집한다. 예) 설치파일 다운로드 경로 수정 및 디버깅한다.
3. 3개 파일은 "c:\사용자\사용자계정\"에 넣는다. (Powershell 오픈 위치)
```

##### (2) 이미지 빌드하기

```docker
docker build --tag cubrid:10.2l .
- 이미지 빌드 시 꼭 끝에 .(온점)을 찍어야 한다.
- Dockerfile 디버깅하면서 맞춘다.
  > 권한문제, 경로문제, Kubernetes 관련 인증문제 등
※ https://github.com/CUBRID/cubrid-docker - 도커제공 파일에 대한 설치 방법 설명
※ https://www.cubrid.com/blog/3820603 - Docker, Kubernetes 환경
xxx 이하 미확인 작업예정 xxx
```

##### (3) 컨테이너 생성하기

```docker
docker run -d --name cubrid_temp -e "CUBRID_DB=db" cubrid:10.2l
-생성하자마자 exited 되어 이상 설치 못함
```

##### (4) CUBRID Command Line Client(csql)에서 CUBRID 접속

```docker
docker exec -it --user cubrid cubrid_temp csql db
```

##### (5) 브로커 및 DB 서버용 시작 컨테이너

```docker
docker run -d -e 'CUBRID_COMPONENTS=svr_db' --name svr_cont_cubrid_temp cubrid:10.2l
docker run -d -e 'CUBRID_COMPONENTS=brkr_db' -e 'CUBRID_DB_HOST=svr_cubrid' -e 'CUBRID_DB=db' --name brkr_cont_db --link cubrid_server:svr_cont_db cubrid:10.2l
```

##### (6) Docker Compose를 사용하여 브로커 및 DB 서버용 컨테이너 시작

```docker
docker-compose -p project-name -f tag/docker-compose.yml up
```

##### (7) CUBRID HA 구성요소 시작(마스터 DB 및 슬레이브 DB 서버)

```docker
docker network create --driver bridge cubrid_ha_net
docker run -d --net=cubrid_ha_net -e 'CUBRID_COMPONENTS=HA' -e 'CUBRID_DB_HOST=master-container-name:slave-container-name' -e 'CUBRID_DB=dbname' --hostname master-container-name --name master-container-name cubrid:tag
docker run -d --net=cubrid_ha_net -e 'CUBRID_COMPONENTS=HA' -e 'CUBRID_DB_HOST=master-container-name:slave-container-name' -e 'CUBRID_DB=dbname' --hostname slave-container-name --name slave-container-name cubrid:tag
docker exec -it master-container-name gosu cubrid cubrid hb status
```

##### (8) Docker Compose를 사용하여 CUBRID HA 구성 요소(브로커, 마스터 DB 및 슬레이브 DB 서버) 시작

```docker
docker-compose -p ha-project-name -f tag/docker-compose-ha.yml up
docker-compose -p ha-project-name -f tag/docker-compose-ha.yml down -v
```

##### (9) Docker Compose를 사용하여 CUBRID HA 구성 요소(브로커가 있는 마스터 DB 및 브로커가 있는 슬레이브 DB 서버) 시작

```docker
docker-compose -p ha2-project-name -f tag/docker-compose-ha2.yml up
```

### 1-2. 설치하기 (pull 이미지)

##### (1) Docker에서 이미지 받아오기

```docker
docker pull cubrid/10.2
```

##### (2) 이미지 빌드하기

```docker
docker build --tag cubrid:10.2 .
-에러 : failed to solve with frontend dockerfile.v0: failed to create LLB definition: dockerfile parse error line 19: unknown instruction: CHOWN
->Troubleshoote > reset
```

##### (3) 컨테이너 생성하기

```docker
docker run -d -p 33000:33000 --name cubrid_temp -e "CUBRID_DB=db" cubrid:10.2l
생성하자마자 exited 되어 이상 설치 못함 ->
xxx 이하 미확인 작업예정 xxx
```

##### 설치방법 1의 (4) ~(9)와 동일

```docker
(4) ~ (9) 동일
```


### 2. Cubrid Manager 설치하기

##### (1) 이미지 생성 방법으로 설치

```docker
작업예정.
```





