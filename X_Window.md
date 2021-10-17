# X Window

- LINUX를 기존의 desktop 환경에서 사용하는 GUI 버전으로 사용하는 방법

## 1. Ubuntu

- Ubuntu 18.04 기준

- **Update**

  ```bash
  $ sudo apt-get update
  
  $ sudo apt-get upgrade
  ```

- 설치

  ```bash
  # 풀버전 설치
  $ sudo apt-get installubuntu-desktop
  
  # 최소 설치
  $ sudo apt-get install --no-install-recommends ubuntu-desktop
  ```

- 실행

  ```bash
  $ sudo startx
  
  # 또는
  $ sudo reboot
  ```

## 2. CentOS

## 3. Run Level

| Run Level |      Target       |
| :-------: | :---------------: |
|     0     |  poweroff.target  |
|     1     |   rescue.target   |
|  2, 3, 4  | multi-user.target |
|     5     | graphical.target  |
|     6     |   reboot.target   |

- 현재 시스템의 run level 확인

  ```bash
  $ sudo systemctl get-default
  
  $ runlevel
  ```

- 현재 시스템의 run level 변경

  ```bash
  $ sudo systemctl set-default multi-user.target
  
  $ runlevel 3
  ```

  
