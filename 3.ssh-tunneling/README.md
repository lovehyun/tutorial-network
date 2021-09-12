# SSH 터널링 및 X11 Forwarding

## X11 Forwarding
- X11-Forwarding 을 위한 서버 설정 (Default: On) 
    ```
    cat /etc/ssh/sshd_config | grep X11

    X11Forwarding yes
    ```
- X11 Windows Client
  - https://sourceforge.net/projects/xming/

- Putty 설정

## SSH 터널링 in Putty
- Putty 설정


## Private Subnet의 서비스 접속
### SSH 접속
- 내 로컬호스트 2222를 통해, 경유지 192.168.56.126 을 통해서, 원격지 10.0.2.11:22 로 접속함.
    ```
    SSH -CNfL 2222:10.0.2.11:22 user1@192.168.56.126
    ssh 127.0.0.1:2222
    ```

### Web/App 서비스 접속
- 내 로컬호스트의 5555를 통해, 경유지를 통해서, 원격지 5000번 서비스로 접속함.
    ```
    SSH -CNfL 5555:10.0.2.11:5000 user1@192.168.56.126
    curl localhost:5555
    ```
