
# 🐳 Docker Compose 파일 정리 

<br>

## 

```yml

version: "3.3"  # 1

services:   # 2 
  web-server:   # 3
    image: nginx    # 4 
    container_name: nginx-dev   # 5
    ports:
      - "7443:80"   # 6
    networks:   # 7
      - bridge
    volumes:    # 8
      - ./nginx/nginx.conf:/etc/nginx/nginx.conf
      - ./nginx/servers:/etc/nginx/conf.d
    command: [ nginx-debug, '-g', 'daemon off;' ]   # 9

```

- 1. version
  - Docker Compose 의 버전을 의미한다.
- 2. services:
  - 



```yaml
  web:
    image: ghcr.io/snaps-corp/snaps-integration-production-web/dev
    container_name: web-dev
    environment:
      TZ: "Asia/Seoul"
    networks:
      - bridge
    env_file:
      - db-dev.env
      - web-dev.env
    ports:
      - "8080:8080"
    depends_on:
      - web-server
      - redis

```


```yml
  batch:
    image: ghcr.io/snaps-corp/snaps-integration-production-batch/dev
    container_name: batch-dev
    environment:
      TZ: "Asia/Seoul"
    networks:
      - bridge
    env_file:
      - db-dev.env
      - batch-dev.env
    ports:
      - "8081:8080"

```


``` yml
  api:
    image: ghcr.io/snaps-corp/snaps-integration-production-api/dev
    container_name: api-dev
    environment:
      TZ: "Asia/Seoul"
    networks:
      - bridge
    env_file:
      - db-dev.env
      - api-dev.env
    ports:
      - "8082:8080"
    depends_on:
      - web-server
```

``` yml

  redis:
    image: redis:6.2.5-alpine
    container_name: redis-dev
    networks:
      - bridge
    environment:
      TZ: "Asia/Seoul"
    volumes:
      - redis:/data
    restart: always
    ports:
      - "6379:6379"
```

``` yml
volumes:
  redis:
  batch:
  web-server:

networks:
  bridge:
    driver: bridge
```



## References

- https://www.daleseo.com/docker-compose-networks/
- 