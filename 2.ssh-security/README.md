# SSH 의 각종 보안 설정
## ID/PW 접속 허용/비허용
- /etc/ssh/sshd_config (기본 허용)
  ```
  cat /etc/ssh/sshd_config | grep -i pass

  #PasswordAuthentication yes
  ```

