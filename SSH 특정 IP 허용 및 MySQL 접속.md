# SSH 특정 IP 허용 및 MySQL 접속

## 1. 특정 IP만 허용하기

### 1.1 IP 허용하기

```bash
$ vi /etc/hosts.allow
```

- 한 줄

  - 가능하지만 많아지면 관리가 힘듦

  ```bash
  sshd: 허용할 ip 주소, 허용할 ip 주소
  ```

- 여러 줄

  ```bash
  sshd: 허용할 ip 주소
  sshd: 허용할 ip 주소
  ```

### 1.2 IP 접근 불가

```bash
$ vi /etc/hosts.deny
```

- 나머지는 모두 ssh 접근 불가

  ```bash
  sshd: ALL
  ```

### 2. MySQL 접근

- 특정 IP에서만 접근 가능한 MySQL에 접근하려면 다음과 같이 커맨드 입력

  ```bash
  $ mysql -u 아이디 -p -h 접속할 DB 호스트
  
  # 예
  $ mysql -u root -p -h 127.0.0.1
  ```

  





