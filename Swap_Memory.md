# Swap Memory

> - 스왑 메모리를 설정하지 않은 상태에서 메모리가 부족할시 프로세서에서 스왑 메모리를 추가하려고 시도를 무한히 하는 경우 발생
> - 해당 경우 발생시 CPU Load Average가 100까지 치솟아 결국 서버가 죽어버림
> - 스왑 메모리를 추가해줌으로서 

- 스왑 파일 확인

  ```bash
  $ swapon -s
  ```

- 스왑 파일 생성

  ```bash
  $ fallocate -l 4GB /swapfile
  ```

  - 최상위 디렉토리에 원하는 용량만큼 생성 가능
  - 하지만 현재 메모리를 넘는 용량은 추천하지 않음

  ```bash
  $ chmod 600 /swapfile
  ```

  - 파일 시스템에서만 접근 가능하도록 권한 변경

- 스왑 파일 시스템 등록

  ```bash
  $ swapon /swapfile
  ```
  
- 재부팅 후에도 사용가능하도록 등록

  ```bash
  $ vi /etc/fstab
  
  # /swapfile	none	swap	sw	0	0 추가
  ```

- 스왑 메모리 초기화 및 재시작

  ```bash
  $ swapoff -a && swapon -a
  ```

- 스왑 메모리 사용 빈도

  - 사용 빈도 확인

    ```bash
    $ cat /proc/sys/vm/swappiness
    ```

    - 사용 빈도는 0 ~ 100 사이에서 조정 100이 최대이며 적극적으로 사용하겠다는 뜻

  - 사용 빈도 조정

    ```bash
    $ vi /etc/sysctl.conf
    ```

    ```bash
    # 아래 구문 추가
    vm.swappiness = <빈도>
    ```

  - 적용

    ```bash
    $ echo 0 > /proc/sys/vm/swappiness
    ```

    

  