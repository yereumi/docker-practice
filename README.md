# 📄 Docker CLI
## ✅ 이미지(Image)
### 최신 버전(latest) 이미지 다운로드
```bash
# docker pull 이미지명
$ docker pull nginx # docker pull nginx:latest와 동일하게 작동
```

### 특정 버전 이미지 다운로드
```bash
# docker pull 이미지명:태그명
$ docker pull nginx:stable-perl
```

### 이미지 조회
```bash
$ docker image ls
```

### 특정 이미지 삭제
```bash
$ docker image rm [이미지 ID 또는 이미지명]
```

### 중지된 컨테이너에서 사용하고 있는 이미지 강제 삭제
```bash
$ docker image rm -f [이미지 ID 또는 이미지명]
```

### 전체 이미지 삭제
```bash
# 컨테이너에서 사용하고 있지 않은 이미지만 전체 삭제
$ docker image rm $(docker images -q)

# 컨테이너에서 사용하고 있는 이미지를 포함해서 전체 이미지 삭제
$ docker image rm -f $(docker images -q)
```

---

## ✅ 컨테이너(Container)
### 컨테이너 생성
```bash
# docker create 이미지명[:태그명]
$ docker create nginx

$ docker ps -a # 모든 컨테이너 조회
```

### 컨테이너 실행
```bash
# docker start 컨테이너명[또는 컨테이너 ID]
$ docker start 컨테이너명[또는 컨테이너 ID]

$ docker ps # 실행중인 컨테이너 조회

# Nginx 컨테이너 중단 후 삭제하기
$ docker ps # 실행 중인 컨테이너 조회
$ docker stop {nginx를 실행시킨 Contnainer ID} # 컨테이너 중단
$ docker rm {nginx를 실행시킨 Contnainer ID} # 컨테이너 삭제
$ docker image rm nginx # Nginx 이미지 삭제
```

### 컨테이너 생성 + 실행
```bash
# docker run 이미지명[:태그명]
$ docker run nginx # 포그라운드에서 실행 (추가적인 명령어 조작을 할 수가 없음)

# Ctrl + C로 종료할 수 있음
```

### 컨테이너를 백그라운드에서 실행시키기
```bash
# docker run -d 이미지명[:태그명]
$ docker run -d nginx

# Nginx 컨테이너 중단 후 삭제하기
$ docker ps # 실행 중인 컨테이너 조회
$ docker stop {nginx를 실행시킨 Contnainer ID} # 컨테이너 중단
$ docker rm {nginx를 실행시킨 Contnainer ID} # 컨테이너 삭제
$ docker image rm nginx # Nginx 이미지 삭제
```

### 컨테이너에 이름 붙여서 생성 및 실행하기
```bash
# docker run -d --name [컨테이너 이름] 이미지명[:태그명]
$ docker run -d --name my-web-server nginx

# Nginx 컨테이너 중단 후 삭제하기
$ docker ps # 실행 중인 컨테이너 조회
$ docker stop {nginx를 실행시킨 Contnainer ID} # 컨테이너 중단
$ docker rm {nginx를 실행시킨 Contnainer ID} # 컨테이너 삭제
$ docker image rm nginx # Nginx 이미지 삭제
```

### 호스트의 포트와 컨테이너의 포트를 연결하기
```bash
# docker run -d -p [호스트 포트]:[컨테이너 포트] 이미지명[:태그명]
$ docker run -d -p 4000:80 nginx
```

### 컨테이너 조회
```bash
$ docker ps # 실행 중인 컨테이너들만 조회

$ docker ps -a # 모든 컨테이너 조회(작동 중인 컨테이너 + 작동을 멈춘 컨테이너)
```

### 컨테이너 중지
```bash
$ docker stop 컨테이너명[또는 컨테이너 ID]
$ docker kill 컨테이너명[또는 컨테이너 ID]
```

###  컨테이너 삭제
```bash
$ docker rm 컨테이너명[또는 컨테이너 ID] # 중지되어 있는 특정 컨테이너 삭제

$ docker rm -f 컨테이너명[또는 컨테이너 ID] # 실행되고 있는 특정 컨테이너 삭제

$ docker rm $(docker ps -qa) # 중지되어 있는 모든 컨테이너 삭제

$ docker rm **-f** $(docker ps -qa) # 실행되고 있는 모든 컨테이너 삭제
```

### 특정 컨테이너의 모든 로그 조회
```bash
# docker logs [컨테이너 ID 또는 컨테이너명]

$ docker run -d nginx
$ docker logs [nginx가 실행되고 있는 컨테이너 ID]
```

### 최근 로그 10줄만 조회
```bash
# dokcer logs --tail [로그 끝부터 표시할 줄 수] [컨테이너 ID 또는 컨테이너명]
$ dokcer logs --tail 10 [컨테이너 ID 또는 컨테이너명]
```

### 기존 로그 조회 + 생성되는 로그를 실시간으로 보고 싶은 경우
```bash
# docker logs -f [컨테이너 ID 또는 컨테이너명]

# Nginx의 컨테이너에 실시간으로 쌓이는 로그 확인하기
$ docker run -d -p 80:80 nginx
$ docker logs -f
```

### 실행 중인 컨테이너 내부에 접속하기
```bash
# docker exec -it 컨테이너명[또는 컨테이너 ID] bash

$ docker run -d nginx
$ docker exec -it [Nginx가 실행되고 있는 컨테이너 ID] bash
$ ls # 컨테이너 내부 파일 조회
$ cd /etc/nginx 
$ cat nginx.conf
```

---

## ✅ 볼륨(Volume)
### 볼륨(Volume)을 사용하는 명령어
```bash
$ docker run -v [호스트의 디렉토리 절대경로]:[컨테이너의 디렉토리 절대경로] [이미지명]:[태그명]
```

### 볼륨(Volume)을 활용해 MySQL 컨테이너 띄우기
```bash
$ cd /Users/yereumi/Documents/Develop
$ mkdir docker-mysql # MySQL 데이터를 저장하고 싶은 폴더 만들기

# docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -v {호스트의 절대경로}/mysql_data:/var/lib/mysql -d mysql
$ docker run -e MYSQL_ROOT_PASSWORD=password123 -p 3306:3306 -v /Users/jaeseong/yereumi/Develop/docker-mysql/mysql_data:/var/lib/mysql -d mysql
```

---

## ✅ 도커파일(Dockerfile)
```dockerfile
# FROM - 베이스 이미지를 생성하는 역할
FROM [이미지명]
FROM [이미지명]:[태그명]

# COPY - 호스트 컴퓨터에 있는 파일을 복사해서 컨테이너로 전달
COPY [호스트 컴퓨터에 있는 복사할 파일의 경로] [컨테이너에서 파일이 위치할 경로]
  
# ENTRYPOINT - 컨테이너가 생성되고 최초로 실행할 때 수행되는 명령어
ENTRYPOINT [명령문...]

# RUN - 이미지 생성 과정에서 명령어를 실행시켜야 할 때 사용
RUN [명령문]

# WORKDIR - 작업 디렉토리를 지정
WORKDIR [작업 디렉토리로 사용할 절대 경로]

# EXPOSE - 컨테이너 내부에서 사용 중인 포트를 문서화하기
EXPOSE [포트 번호]
```

---

## ✅ 도커 컴포즈(Docker Compose)
### compose.yml
```yml
services:
  websever:
    container_name: webserver
    image: nginx
    ports: 
      - 80:80
```

### compose.yml에서 정의한 컨테이너 실행
```bash
$ docker compose up    # 포그라운드에서 실행
$ docker compose up -d # 백그라운드에서 실행
```

### Docker Compose로 실행시킨 컨테이너 확인하기
```bash
# compose.yml에 정의된 컨테이너 중 실행 중인 컨테이너만 보여줌 
$ docker compose ps 

# compose.yml에 정의된 모든 컨테이너를 보여줌
$ docker compose ps -a
```

### Docker Compose 로그 확인하기
```bash
# compose.yml에 정의된 모든 컨테이너의 로그를 모아서 출력
$ docker compose logs
```

### 컨테이너를 실행하기 전에 이미지 재빌드하기
```bash
$ docker compose up --build # 포그라운드에서 실행
$ docker compose up --build -d # 백그라운드에서 실행
```

### 이미지 다운받기 / 업데이트하기
```bash
$ docker compose pull
```

### Docker Compose에서 이용한 컨테이너 종료하기
```bash
$ docker compose down
```

---

## ✅ Spring Boot, MySQL 컨테이너 동시에 띄워보기
### application.yml 작성하기
```yml
spring:
  datasource:
    url: jdbc:mysql://my-db:3306/mydb # localhost가 아니라 mysql의 컨테이너 이름인 my-db로 수정
    username: root
    password: pwd1234
    driver-class-name: com.mysql.cj.jdbc.Driver
```

### Dockerfile 작성하기
```dockerfile
FROM openjdk:17-jdk

COPY build/libs/*SNAPSHOT.jar /app.jar

ENTRYPOINT ["java", "-jar", "/app.jar"]
```

### compose.yml 작성하기
```yml
services:
  my-server:
    build: .
    ports:
      - 8080:8080
		# my-db의 컨테이너가 생성되고 healthy 하다고 판단 될 때, 해당 컨테이너를 생성한다. 
    depends_on:
      my-db:
        condition: service_healthy
  my-db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: pwd1234
      MYSQL_DATABASE: mydb # MySQL 최초 실행 시 mydb라는 데이터베이스를 생성해준다.
    volumes:
      - ./mysql_data:/var/lib/mysql
    ports:
      - 3306:3306
    healthcheck:
      test: [ "CMD", "mysqladmin", "ping" ] # MySQL이 healthy 한 지 판단할 수 있는 명령어
      interval: 5s # 5초 간격으로 체크
      retries: 10 # 10번까지 재시도
```

### Spring Boot 프로젝트 빌드하기
```bash
$ ./gradlew clean build
```

### compose 파일 실행시키기
```bash
$ docker compose up -d **--build**
```

### compose 실행 현황 보기
```bash
$ docker compose ps
$ docker ps
$ docker logs [Container ID]
```