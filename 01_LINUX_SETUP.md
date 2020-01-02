# 01_LINUX_SETUP

- 현재 사용하고 있는 OS에서 리눅스 사용을 하기 위해 Virtual Box를 설치하자.
  - Virtual Box는 가상화 환경을 만들어 사용하고 있는 OS 위에서 새로운 OS를 사용 가능하게 한다.

## 1. Virtual Box 설치

- [공식 사이트](http://www.virtualbox.org/)에 접속한 후

  ![image-20200102140323530](01_LINUX_SETUP.assets/image-20200102140323530.png)

- **좌측의 Downloads를 클릭**

  ![image-20200102140522498](01_LINUX_SETUP.assets/image-20200102140522498.png)

- **하단의  VirtualBox older builds를 클릭**

  - **안정성**을 위해서 older virsion을 받는 것이다.

  ![image-20200102140706086](01_LINUX_SETUP.assets/image-20200102140706086.png)

- **VirtualBox 6.0을 클릭**

  ![image-20200102140936809](01_LINUX_SETUP.assets/image-20200102140936809.png)

- Windows에 설치할 것이기 때문에 **Windows hosts**를 클릭해 다운로드 받자.
- 다운로드가 완료됐다면 경로를 찾아가서 설치해주도록 하자.
- 설치 중간에는 **모두 Yes를 눌러줘도 무방**하다.
- 설치가 완료됐다면 이제 리눅스 운영체제인 CentOS와 Ubuntu를 깔기 시작해보자.

## 2. CentOS

- [CentOS 6.10 다운로드](http://mirror.kakao.com/centos/6.10/isos/x86_64/)

  ![image-20200102142201284](01_LINUX_SETUP.assets/image-20200102142201284.png)

- **CentOS-6.10-x86_64-bin-DVD1.iso**를 받자.

- 만약에 가지고 있는 iso파일이 있다면, 있는 것으로 사용하자.

### 2.1 VirtualBox에 가상 환경 추가

![image-20200102143313486](01_LINUX_SETUP.assets/image-20200102143313486.png)

- VirtualBox를 실행하면 다음과 같은 화면을 볼 수 있다.
- **새로 만들기를 클릭하자.**

![image-20200102143603042](01_LINUX_SETUP.assets/image-20200102143603042.png)

![image-20200102143821265](01_LINUX_SETUP.assets/image-20200102143821265.png)

- 위와 같은 화면이 뜬다면 CentOS을 입력하고 다음을 눌러준다.
  - 이름을 입력할 시에 버전 종류를 자동으로 잡아준다.

![image-20200102143854317](01_LINUX_SETUP.assets/image-20200102143854317.png)

- 다음으로 넘어오면 **메모리를 2048**을 입력해주고 넘긴다.

![image-20200102143949712](01_LINUX_SETUP.assets/image-20200102143949712.png)

- **지금 새 가상 하드 디스크 만들기**를 **선택**하고 만들기를 누르고 넘긴다.

![image-20200102144113769](01_LINUX_SETUP.assets/image-20200102144113769.png)

- **VDI를 선택하고 다음**으로 넘긴다.

![image-20200102144226655](01_LINUX_SETUP.assets/image-20200102144226655.png)

- 동적 할당을 선택하고 다음으로 넘긴다.
  - **동적 할당** : 바로 해당 용량이 할당 되는게 아니라 용량이 커질 때마다 크기가 최대 지정 크기까지 커진다.
  - **고정 크기** : 처음부터 해당 용량을 할당하고 사용한다. 속도가 조금 더 빠르지만 큰 차이는 없다.

![image-20200102144608496](01_LINUX_SETUP.assets/image-20200102144608496.png)

- 여기서는 30GB를 입력하고 만들어주었다.

### 2.2 VirtualBox 가상 환경 설정

![image-20200102144816175](01_LINUX_SETUP.assets/image-20200102144816175.png)

- 위의 과정이 완료 됐다면, 처음 화면으로 돌아오는데 이제 설정을 눌러주자.

![image-20200102145037149](01_LINUX_SETUP.assets/image-20200102145037149.png)

- 위 순서대로**(저장소 - 비어있는 컨트롤러 - CD모양)** 눌러주자.
- 그리고 아까 받아두었던 경로를 찾아가서 깔아줄 OS를 클릭해주면 된다.

![image-20200102145417536](01_LINUX_SETUP.assets/image-20200102145417536.png)

- 네트워크 설정은 **어댑터에 브리지**를 선택하고 확인을 누르고 설정을 끝내자.

- 이제 본격적인 설치를 시작해보자.

### 2.3 CentOS 설치

![image-20200102145752021](01_LINUX_SETUP.assets/image-20200102145752021.png)

- 설치할 환경을 선택하고 시작을 눌러주자. 더블 클릭해도 무방하다.

![image-20200102145909714](01_LINUX_SETUP.assets/image-20200102145909714.png)

- <span style="color:red">가상 화면을 벗어나 기존 OS의 마우스를 컨트롤하고 싶다면 **CTRL + ALT**를 눌러주자.</span>
- 위와 같은 화면이 떴다면 **Install or upgrade an existing system**을 선택하고 앤터를 쳐주자.

![image-20200102150051998](01_LINUX_SETUP.assets/image-20200102150051998.png)

- **Skip**을 눌러주고 넘어가자.

![image-20200102150141564](01_LINUX_SETUP.assets/image-20200102150141564.png)

- Next...

![image-20200102150212060](01_LINUX_SETUP.assets/image-20200102150212060.png)

- **한국어** 선택 Next...
- 다음 **키보드 설정도 한국어** 선택 Next...

![image-20200102150320429](01_LINUX_SETUP.assets/image-20200102150320429.png)

- **기본 저장 장치** 선택 다음...

![image-20200102150437034](01_LINUX_SETUP.assets/image-20200102150437034.png)

- **예, 모든 데이터를 삭제합니다**

![image-20200102150521764](01_LINUX_SETUP.assets/image-20200102150521764.png)

- 네트워크 설정 역시 다음...

![image-20200102150554523](01_LINUX_SETUP.assets/image-20200102150554523.png)

- 시간대 설정 또 다음...

![image-20200102150652525](01_LINUX_SETUP.assets/image-20200102150652525.png)

- root 암호를 6자리 이상으로 설정하고 다른 곳에 적어두자.

![image-20200102150931207](01_LINUX_SETUP.assets/image-20200102150931207.png)

- 다음...

![image-20200102151030702](01_LINUX_SETUP.assets/image-20200102151030702.png)

- **디스크에 변경 사항 기록**을 선택

![image-20200102151110661](01_LINUX_SETUP.assets/image-20200102151110661.png)

- 다음...

![image-20200102151830185](01_LINUX_SETUP.assets/image-20200102151830185.png)

- 이렇게 **CentOS 설치가 끝**났다.

### 2.4 CentOS 시작

![image-20200102152039157](01_LINUX_SETUP.assets/image-20200102152039157.png)

- 앞으로...

![image-20200102152119865](01_LINUX_SETUP.assets/image-20200102152119865.png)

- 앞으로...

![image-20200102152142719](01_LINUX_SETUP.assets/image-20200102152142719.png)

- 앞으로... 사용자가 없다고 해도 계속 진행...

![image-20200102152334222](01_LINUX_SETUP.assets/image-20200102152334222.png)

- 날짜 및 시간 설정

- 다음도 모두 예를 눌러주면 재부팅된다.

![image-20200102152757328](01_LINUX_SETUP.assets/image-20200102152757328.png)

- 재부팅 후 root계정으로 로그인하면 위와 같은 화면을 볼 수 있다.
- 이제 CentOS를 사용 할 수 있다.

## 3. Ubuntu

- [Ubuntu 다운로드](http://releases.ubuntu.com/16.04/)

![image-20200102153541517](01_LINUX_SETUP.assets/image-20200102153541517.png)

- **Ubuntu-16.04.6-server-amd64.iso**를 다운로드하자.

### 3.1 VirtualBox에 가상환경 추가

- 2.1과 같음

### 3.2 VirtualBox 가상 환경 설정

- 2.2와 같음

### 3.3 Ubuntu 설치

![image-20200102153834793](01_LINUX_SETUP.assets/image-20200102153834793.png)

- 한국어 선택

![image-20200102153859800](01_LINUX_SETUP.assets/image-20200102153859800.png)

- **우분투 서버 설치**

![image-20200102153932311](01_LINUX_SETUP.assets/image-20200102153932311.png)

- 예...

![image-20200102153954598](01_LINUX_SETUP.assets/image-20200102153954598.png)

- 대한민국 선택 다음...

![image-20200102154038229](01_LINUX_SETUP.assets/image-20200102154038229.png)

- 예...

![image-20200102154128119](01_LINUX_SETUP.assets/image-20200102154128119.png)

- **)**를 입력하고, 처음 글짜 위 사진으로는 y를 누르면 문자들이 뜨는데 맞는지 골라준다. 거의 **아니오**일 것이다.

![image-20200102154351712](01_LINUX_SETUP.assets/image-20200102154351712.png)

- 계속...

![image-20200102154506565](01_LINUX_SETUP.assets/image-20200102154506565.png)

- 계속...

![image-20200102154546411](01_LINUX_SETUP.assets/image-20200102154546411.png)

- 이름과 암호 설정 후 계속...

![image-20200102154646591](01_LINUX_SETUP.assets/image-20200102154646591.png)

- 아니요...

![image-20200102154711735](01_LINUX_SETUP.assets/image-20200102154711735.png)

- 시간을 확인하는 알림창이다. 예를 눌러주자.

![image-20200102154812306](01_LINUX_SETUP.assets/image-20200102154812306.png)

- **자동 - 디스크 전체 사용하고 LVM 설정**

![image-20200102154903052](01_LINUX_SETUP.assets/image-20200102154903052.png)

- 다음...

![image-20200102154919212](01_LINUX_SETUP.assets/image-20200102154919212.png)

- 예...

- 용량 설정 다음...

![image-20200102155002322](01_LINUX_SETUP.assets/image-20200102155002322.png)

- 예...

![image-20200102155045575](01_LINUX_SETUP.assets/image-20200102155045575.png)

- 설치 진행...

![image-20200102155132169](01_LINUX_SETUP.assets/image-20200102155132169.png)

- 계속...

![image-20200102155217848](01_LINUX_SETUP.assets/image-20200102155217848.png)

- 자동 업데이트 하지 않음

![image-20200102155257594](01_LINUX_SETUP.assets/image-20200102155257594.png)

- **OpenSSH server 선택**, 선택은 space bar로 해주고 enter...

![image-20200102155543783](01_LINUX_SETUP.assets/image-20200102155543783.png)

- 예...

![image-20200102155617622](01_LINUX_SETUP.assets/image-20200102155617622.png)

- **설치가 완료**되었다.

### 3.4 Ubuntu 시작

![image-20200102155826832](01_LINUX_SETUP.assets/image-20200102155826832.png)

- 아까 만들어준 사용자 계정으로 로그인한다.
- 이제 Ubuntu를 사용할 수 있다.

