# 대용량 서비스 구축 (분산처리)
참고 : http://nginx.org/en/docs/http/ngx_http_proxy_module.html#proxy_pass

## nginx 를 사용한 Reverse Proxy
- 관련 설정 파일
  1. /etc/nginx/sites-available/default
  2. /etc/nginx/proxy_params
  3. /etc/nginx/nginx.conf

- 1단계:
  ```
  location / {
      proxy_pass http://127.0.0.1:8000;
  }
  ```

- 2단계:
  ```
  location / {
  	  proxy_pass http://10.0.2.11:8000;

      # proxy_set_header X-Real-IP  $remote_addr;
      # proxy_set_header Host $host;
      include /etc/nginx/proxy_params;
  }
  ```

  ```
  proxy_params
      proxy_set_header Host $http_host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
  ```

- 3단계:
  ```
  location / {
      proxy_pass http://10.0.2.11:3000/proxy_1;
  }
  location /user {
      proxy_pass http://10.0.2.11:5000/proxy_2;
  }
  location /service {
      proxy_pass http://10.0.2.12:8000/proxy_3;
  }
  ```

- 4단계:
  http://nginx.org/en/docs/http/load_balancing.html

  ```
  http {
      upstream my-app1 {
          server Ubuntu20VM_1;
          server Ubuntu20VM_2;
          server Ubuntu20VM_3;
      }

      server {
          listen 80;

          location / {
              proxy_pass http://my-app1;
              include /etc/nginx/proxy_params;
          }
      }
  }
  ```

- app.py
  ```
  from flask import Flask, request

  app = Flask(__name__)

  @app.route('/')
  def home():
      print(request.headers)
      print(request.remote_addr)
      return "<H1>Hello From Host1</H1>"

  if __name__ == "__main__":
      app.run(host="10.0.2.15")
  ```
