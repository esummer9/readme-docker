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
      - ./config/configuration.yml:/usr/src/redmine/config/configuration.yml

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
  | ID | esummer | |
  | Pw | esummer |로그인 후 반드시 비밀번호 변경 |
  | Pw2 | K~~!@ ||



## 싫행 옵션

```shell
# 서비스 시작 
$ docker compose up -d

# 컨테이너를 시작하기 전에 이미지를 강제로 리빌드. 소스코드를 업데이트시 특히 유용
$ docker compose up -d --build

# 설정 변경 여부와 관계없이 모든 컨테이너를 강제로 다시 생성
$ docker compose up -d --force-recreate

# 전체 스택을 다시 시작하지 않고 특정 컨테이너(서비스)만 다시 시작
$ docker compose up -d <service_name>

# 컨테이너 재시작
$ docker-compose restart redmine

# 컨테이너 중지
$ docker compose down

# 볼륨까지 삭제 (데이터 초기화)
$ docker compose down -v

# 미사용 볼륨이미지 삭제 
$ docker image prune -a

# 로그 확인
$ docker compose logs -f redmine

```

## 백업 

```shell
$ mkdir backup

$ docker-compose start 

$ docker-compose stop redmine

# files 디렉토리 백업
docker exec redmine tar czf - /usr/src/redmine/files > backup/redmine_files_backup-$(date +%F).tar.gz

# 플러그인, 테마도 함께 백업 추천
docker exec redmine tar czf - /usr/src/redmine/plugins > backup/redmine_plugins_backup-$(date +%F).tar.gz
docker exec redmine tar czf - /usr/src/redmine/public/themes > backup/redmine_themes_backup-$(date +%F).tar.gz
```


## 이메일 테스트


```shell
curl -v --url "smtps://smtp.naver.com:465" \
     --ssl-reqd \
     --mail-from "appindex@naver.com" \
     --mail-rcpt "dhlee@ecodoobiz.com" \
     --upload-file mail.txt \
     --user "appindex@naver.com:6XTCLM4UH9UC"
```

```shell
curl -v --url "smtps://smtp.naver.com:465" \
     --ssl-reqd \
     --mail-from "appindex@naver.com" \
     --mail-rcpt "dhlee@ecodoobiz.com" \
     --upload-file mail.txt \
     --user "appindex@naver.com:YPG5QDJLNY1X"
```