# Readmine 설치 on Docker


## install 

 - docker-compose.yml 

```yml

version: '3.8'

services:
  redmine:
    image: redmine:5.1  # Redmine 최신 LTS 버전
    container_name: redmine
    restart: always
    ports:
      - "3000:3000"   # 호스트 3000 → 컨테이너 3000
    environment:
      REDMINE_DB_POSTGRES: db
      REDMINE_DB_DATABASE: redmine
      REDMINE_DB_USERNAME: redmine
      REDMINE_DB_PASSWORD: redminepass
    depends_on:
      - db
    volumes:
      - redmine_data:/usr/src/redmine/files  # 첨부파일 저장소

  db:
    image: postgres:14
    container_name: redmine-db
    restart: always
    environment:
      POSTGRES_DB: redmine
      POSTGRES_USER: redmine
      POSTGRES_PASSWORD: redminepass
    volumes:
      - postgres_data:/var/lib/postgresql/data

volumes:
  redmine_data:
  postgres_data:
```

## run

  > $ docker compose run -d
  
  |구분 |value |비고 |
  |---|---|---|
  | 주소| http://localhost:3000 | |
  | ID | admin | |
  | Pw | admin |로그인 후 반드시 비밀번호 변경 |
  | Pw2 | K~~!@ ||



## 싫행 옵션

```shell
# 로그 확인
$ docker compose logs -f redmine

# 컨테이너 중지
$ docker compose down

# 볼륨까지 삭제 (데이터 초기화)
$ docker compose down -v
```
# readme-docker
