# NGINX 설정
> 1. 알아야될 내용
> 2. 주요 명령어
> 3. 특정 설정
---

## 1. 알아야될 내용 
 NGINX 란? 
웹서버 소프트웨어
- 주요기능 
  - 프록시 : 외부 pc 요청을 받아서 다른 서버나 내 로컬 서비스와 연결
  - 로드 밸런싱 :  

설정 파일 경로 : `/etc/nginx/conf.d`
  
NGINX 디렉토리의 `sites-enabled`와 `sites-available` 차이
- `sites-enabled` : 현재 활성화되어 적용되어 있는 설정
- `sites-available` : 적용가능 한 설정


---

## 2. 주요 명령어

- `user` : Nginx 사용자/그룹 설정
- `worker_processes` : 워커 프로세스 개수, `auto` - 자동
- `events` 블록 : 
  -  `worker_connections` : 워커 프로세스 최대 연결 개수
- `http` 블록 : 
  - `include` : 설정 파일을 추가시 사용 
  - `default_type` :  기본 `MIME` 타입 설정
  - `access_log` : 접근 로그 파일 위치 설정
  - `error_log` : 에러 로그 파일 위치 설정
  - `sendfile` : 파일 전송 최적화 설정
  - `gzip` : gzip 압축 여부 설정
  - `gzip_types` : gzip으로 압축할 타입 설정
  - `keepalive_timeout` : 클라이언트 연결 유지 시간 설정
  - `server` 블록
    - `listen` : 클라이언트 요청을 수신할 포트 
      - `ssl` : 추가시 ssl 인증 처리 - `listen 443 ssl;`
    - `server_name` : 수신할 도메인 
    - `root` : 루트 디렉토리 경로 설정 (php나 예전 app들에서 자주 사용)
    - `index` : 기본으로 보여줄 파일명 (php나 예전 app들에서 자주 사용)
    - `ssl_certificate` : ssl 인증 chain 위치
    - `ssl_certificate_key` : ssl 개인키 위치
    - `location (경로)` 블록 : 해당 도메인에서 경로로 설정한 경로와 매칭될 경우에 처리할 로직
      - `root` :  루트 디렉토리
      - `try_files` : 
```properties
# 간략한 전체 설정
user www-data;
worker_processes 768;

# 이벤트 블록
events {
    worker_connections 1024;
}

# HTTP 블록
http {
    include       mime.types;
    default_type  application/octet-stream;

    # 로그 설정
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    # Gzip 압축 설정
    gzip on;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    # 서버 블록 (Virtual Hosts)
    server {
        listen 80;
        server_name yunhwan.com www.yunhwan.com;

        root /var/www/web_app;
        index index.html index.htm;

        # 로케이션 블록 (Location)
        location / {
            try_files $uri $uri/ =404;
        }
    }
    #https 설정
    server {
        server_name yunhwan.com;
        listen 443 ssl; # managed by Certbot
        #ssl 인증서 설정
        ssl_certificate /etc/letsencrypt/live/yunhwan.com/fullchain.pem; # managed by Certbot
        ssl_certificate_key /etc/letsencrypt/live/yunhwan.com/privkey.pem; # managed by Certbot
        include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
        ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
        
        #리버스 프록시 설정
        location /hi {
            # 프록시 전달 경로
            proxy_pass http://localhost:3000;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
            
            # 최신 버전에서 사용하는 추가적인 헤더 설정
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}

```

---

## 2. 주요 명령어

