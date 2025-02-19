# NGINX 로드밸런싱하는 법
>1. nginx.conf 설정
>2. load balancing 패턴 설정

---

## 1. nginx.conf 설정
- http 블록에 `upstream <이름> { .. }` 형식으로 블록을 추가
- 전달할 서버의 `네트워크 도메인` 혹은 `ip+포트` 들을 넣어줌  

```text
    http {
        upstream api_gateway {
            server backend1.example.com;
            server backend2.example.com;
        }

        server {
            listen 80;
            server_name mydomain.com;

            location / {
                proxy_pass http://api_gateway;
                proxy_set_header Host $host;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
            }
        }
    }
```
위처럼 설정하면 nginx가 들어오는 요청을 번갈아 가며 각 서버에 보냄

---

## 2. Load balancing 패턴 설정
- 가중치 패턴 : 도메인/ip+port 뒤에 `weight`키워드와 값을 입력해 비율을 조정, 숫자가 클수록 많이 전달
  ```text
  upstream backend {
      server backend1.example.com weight=3;
      server backend2.example.com weight=1;
  }
  ```
- 커넥션이 적은 곳 위주 패턴  : `least_conn` 키워드를 upstream 블록에 넣어주면 됨

    ```text
    upstream backend {
        least_conn;
        server backend1.example.com;
        server backend2.example.com;
    }
    ```
- IP Hash 패턴 : ip기반으로 특정 서버에 연결됬던 ip는 다른 서버로 전달 되지 않고  
연결했던 ip로만 연결하게하는 패턴으로  upstream 블럭에 `ip_hash` 키워드를 추가해주면 됨 
    ```text
    upstream backend {
        ip_hash;
        server backend1.example.com;
        server backend2.example.com;
    }
    ```
- 서버가 고장났을 경우 대체 패턴 : 메인으로 사용하는 서버가 고장일 때 대체할 서버에 `backup` 키워드를 추가 
```text
    upstream backend {
        server backend1.example.com;
        server backend2.example.com backup;
    }
```