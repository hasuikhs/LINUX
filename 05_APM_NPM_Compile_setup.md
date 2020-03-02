# 01 APM/NPM 설치

## 1. CentOS

### 1.1 source를 이용한 APM 설치

#### 1.1.1 Apache 설치

##### 1.1.1.1 기존에 설치된 Apache 웹서버 제거

```bash
$ yum remove -y httpd httpd-*
```

##### 1.1.1.2 빌드 환경 설정

```bash
$ yum install -y make gcc g++ gcc-c++ autoconf automake libtool pkgconfig findutils oepnssl openssl-devel openldap-devel pcre-devel libxml2-devel lua-devel curl curl-devel libcurl-devel flex
```

##### 1.1.1.3 관련 모듈 다운로드 및 설치

- apr 다운로드

  ```bash
  $ cd /usr/local/src
  
  $ wget https://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
  
  $ tar xvfz apr-1.5.2.tar.gz
  
  $ cd apr-1.5.2
  
  $ ./configure --prefix=/usr/local/apr
  
  $ make
  
  $ make install
  ```

- apr-util 다운로드

  ```bash
  $ cd /usr/local/src
  
  $ wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
  
  $ tar xvzf apr-util-1.5.4.tar.gz
  
  $ cd apr-util-1.5.4
  
  $ ./configure --with-apr=/usr/local/apr/
  
  $ make
  
  $ make install
  ```

- pcre 다운로드

  ```bash
  $ cd /usr/local/src
  
  $ wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
  
  $ tar xvfz pcre-8.37.tar.gz
  
  $ cd pcre-8.37
  
  $ ./configure --prefix=/usr/local/pcre
  
  $ make
  
  $ make install
  ```

##### 1.1.1.4 Apache 웹서버 다운로드 및 설치

```bash
$ cd /usr/local/src

$ wget http://archive.apache.org/dist/httpd/httpd-2.4.23.tar.gz

$ tar xvfz httpd-2.4.23.tar.gz

$ mv apr-1.5.2 httpd-2.4.23/srclib/apr

$ mv apr-util-1.5.4 httpd-2.4.23/srclib/apr-util

$ cd httpd-2.4.23

$ ./configure --enable-module=so --enable-mods-shared=most --enable-maintainer-mode --enable-deflate --enable-headers --enable-rewrite --enable-ssl --enable-proxy --enable-proxy-http --enable-proxy-ajp --enable-proxy-balance --with-pcre=/usr/local/pcre --prefix=/usr/local/apache

$ make

$ make install
```

##### 1.1.1.5 Apache 웹서버 서비스 등록 및 실행

- httpd 서비스 파일 만들기

  ```bash
  $ cp /usr/local/apache/bin/apachectl /etc/init.d/httpd
  ```

- vi 애디터로 파일을 열고 내용 추가

  ```bash
  $ vi /etc/init.d/httpd
  ```

  ```
  #! /bin/sh
  # chkconfig: 2345 90 90
  # description: init file for Apache server daemon
  # processname: /usr/local/apache/bin/apachectl
  # config: /usr/local/apache/conf/httpd.conf
  # pidfile: /usr/local/apache/logs/httpd.conf
  ```

- httpd.conf 파일 수정

  ```bash
  $ vi /usr/local/apache/conf/httpd.conf
  ```

  - ServerName 부분을 찾아 주석을 제거하고 사용할 서버명을 입력(여기서는 localhost)
  - `# LoadModule unique_id_module modules/mod_unique_id.so` 추가

- httpd 서비스 시작

  ```bash
  $ service httpd start
  ```

##### 1.1.1.6 방화벽 설정

- vi 애디터로 파일을 열고 #부분 내용 추가(추가할때는 # 제거)

  ```bash
  $ vi /etc/sysconfig/iptables
  ```

  ```
  -A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
  # -A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
  # -A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
  -A INPUT -j REJECT --reject-with icmp-host-prohibited
  ```

- 방화벽 재시작

  ```bash
  $ service iptables restart
  ```

#### 1.1.2 MySQL 설치

##### 1.1.2.1 기존에 설치된 MySQL과 cmake 삭제

```bash
$ yum remove -y mysql* cmake
```

##### 1.1.2.2 빌드 환경 설정

```bash
$ yum install -y zlib zlib-devel cpp perl bison freetype freetype-devel freetype-utils ncurses-devel libtermcap-devel bzip2-devel
```

##### 1.1.2.3 cmake 다운로드 및 설치

```bash
$ cd /usr/local/src

$ wget https://cmake.org/files/v3.5/cmake-3.5.2.tar.gz

$ tar xvfz cmake-3.5.2.tar.gz

$ cd cmake-3.5.2

$ ./bootstrap

$ make && make install
```

##### 1.1.2.4 MySQL 그룹 및 게정 만들기

```bash
$ groupadd mysql
$ useradd -g mysql mysql
```

##### 1.1.2.5 MySQL 다운로드

```bash
$ cd /usr/local/src

$ wget http://dev.mysql.com/get/Downloads/MySQL-5.6/mysql-5.6.30.tar.gz

$ tar xvfz mysql-5.6.30.tar.gz

$ cd mysql-5.6.30
```

##### 1.1.2.6 MySQL cmake 컴파일

```bash
$ /usr/local/bin/cmake \
	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
	-DDEFAULT_CHARSET=utf8 \
	-DDEFAULT_COLLATION=utf8_general_ci \
	-DWITH_EXTRA_CHARSETS=all \
	-DENABLED_LOCAL_INFILE=1 \
	-DMYSQL_DATADIR=/usr/local/mysql/data \
	-DMYSQL_USER=mysql \
	-DWITH_INNOBASE_STORAGE_ENGINE=1 \
	-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
	-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
	-DWITH_PERFSCHEMA_STORAGE_ENGINE=1 \
	-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \
	-DMYSQL_TCP_PORT=3306
```

```bash
	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \		# mysql 설치할 디렉토리
	-DDEFAULT_CHARSET=utf8 \						# mysql 서버의 문자셋
	-DDEFAULT_COLLATION=utf8_general_ci \			# db의 문자셋
	-DWITH_EXTRA_CHARSETS=all \						# 추가로 지원할 문자셋
	-DENABLED_LOCAL_INFILE=1 \						# local_infile 변수 사용 가능 여부
	-DMYSQL_DATADIR=/usr/local/mysql/data \			# db설치할 디렉토리
	-DMYSQL_USER=mysql \							# mysql 유저를 지정
	-DWITH_INNOBASE_STORAGE_ENGINE=1 \				# 스토리지 엔진, default innodb
	-DWITH_ARCHIVE_STORAGE_ENGINE=1 \				# 스토리지 엔진
	-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \				# 스토리지 엔진
	-DWITH_PERFSCHEMA_STORAGE_ENGINE=1 \			# 스토리지 엔진
	-DMYSQL_UNIX_ADDR=/usr/local/mysql/mysql.sock \	# mysql 소켓 디렉토리
	-DMYSQL_TCP_PORT=3306							# mysql 포트번호, default가 3306
```

##### 1.1.2.7 MySQL 그룹/게정 권한 주기

```bash
$ chown -R (계정명):(그룹명) /usr/local/mysql

$ chown -R mysql:mysql /usr/local/mysql

$ chown -R mysql:mysql /usr/local/mysql/data
```

##### 1.1.2.8 DB 생성

```bash
$ cd /usr/local/mysql

$ ./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data
```

##### 1.1.2.9 MySQL 설정파일 복사 및 데몬 복사

```bash
$ cp support-files/my-default.cnf /etc/my.cnf

$ cp support-files/mysql.server /etc/init.d/mysqld

$ chmod 755 /etc/init.d/mysqld
```

- vi 애디터로 파일 열고 내용 추가

  ```bash
  $ vi /etc/init.d/mysqld
  ```

  ```
  basedir=/usr/local/mysql
  datadir=/usr/local/mysql/data
  ```

##### 1.1.2.10 환경변수 등록 및 MySQL 데몬 실행

```bash
$ cd ~
```

- vi 애디터로 파일을 열고 내용 추가

  ```bash
  $ vi .bash_profile
  ```

  ```
  PATH=$PATH:$HOME/bin:/usr/local/mysql/bin
  ```

```bash
$ source .bash_profile

$ service mysqld start
```

##### 1.1.2.11 MySQL root 계정 비밀번호 변경

```bash
$ mysqladmin -u root password root123
```

##### 1.1.2.12 리눅스 시작시 MySQL 구동되도록 설정

```bash
$ chkconfig --add mysqld

$ chkconfig mysqld on

$ chkconfig --list mysqld
```

[https://moguwai.tistory.com/entry/CentOS%EC%97%90%EC%84%9C-APM-Source-%EC%84%A4%EC%B9%982Mysql-%EC%84%A4%EC%B9%98?category=517733](https://moguwai.tistory.com/entry/CentOS에서-APM-Source-설치2Mysql-설치?category=517733)