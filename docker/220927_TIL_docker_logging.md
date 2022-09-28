# Docker Container Logging 

## DockerFile

```docker
FROM adoptopenjdk/openjdk11:alpine-slim
ARG JAR_FILE=${jar파일경로}.jar
COPY ${JAR_FILE} app.jar
VOLUME ["/logs/web"]
ENTRYPOINT ["java", "-jar", "-Dspring.profiles.active=prd" ,"/app.jar"]
```

- VOLUME 옵션에 컨테이너 내부 로그 저장 경로 설정

## Docker Compose

```yaml
version: "3.3"

services:

  web:
    image: ${이미지명}
    container_name: web-dev
    environment:
      TZ: "Asia/Seoul"
    networks:
      - bridge
    env_file:
      - db-dev.env
      - web-dev.env
    volumes: 
      - /webling/log/was/web:/logs/web #로그 저장할 EC2 경로 : 로그자장할 컨테이너 내부 경로
    ports:
      - "8080:8080"
    depends_on:
      - web-server
      - redis
    
    ...

```

- VOLUME 옵션에 마운트 경로 설정

## applicaion.yml

```yaml
logging:
  level:
    sql: info
    web: info
    root: info
  file:
    name: /logs/web/webling_ip_log_web.log

    ....
```

- Logging 레벨 설정

- file.name : 컨테이너 내부 로그 파일 경로 지정

## Container 내부 로깅 저장 경로 확인

[] 

## EC2 외부 로깅 저장 마운트 확인

[]
