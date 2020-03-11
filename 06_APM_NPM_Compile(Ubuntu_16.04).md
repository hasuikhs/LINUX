# 06_APM/NPM_Compile(Ubuntu_16.04) 설치

- 설치하기에 앞서 apt 패키지를 업데이트 및 업그레이드

  ```bash
  $ sudo apt update && sudo apt upgrade
  
  $ reboot
  ```

- 필수 요소 설치

  ```bash
  $ sudo apt-get install gcc g++ zlibc zlib1g zlib1g-dev libssl-dev openssl libxml2-dev ncurses-dev make
  ```

## 1. Apache 설치

### 1.1 관련 모듈 다운로드 및 설치

- apr 다운로드

  ```bash
  $ wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz
  
  $ tar xvfz apr-1.5.2.tar.gz
  
  $ rm -rf apr-1.5.2.tar.gz
  
  $ cd apr-1.5.2
  
  $ ./configure --prefix=/usr/local/apr
  
  $ make -j 2
  
  $ sudo make install
  ```

- apr-util 다운로드

  ```bash
  $ wget http://archive.apache.org/dist/apr/apr-util-1.5.4.tar.gz
  
  $ tar xvfz apr-util-1.5.4.tar.gz
  
  $ rm -rf apr-util-1.5.4.tar.gz
  
  $ cd apr-util-1.5.4
  
  $ ./configure --prefix=/usr/local/aprutil --with-apr=/usr/local/apr
  
  $ make -j 2
  
  $ sudo make install
  ```

- pcre 다운로드

  ```bash
  $ wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz
  
  $ tar xvfz pcre-8.37.tar.gz
  
  $ rm -rf pcre-8.37.tar.gz
  
  $ cd pcre-8.37
  
  $ ./configure --prefix=/usr/local/pcre
  
  $ make -j 2
  
  $ sudo make install
  ```

### 1.2 Apache 설치

```bash
$ cd ~

$ wget wget http://archive.apache.org/dist/httpd/httpd-2.4.23.tar.gz

$ tar xvfz httpd-2.4.23.tar.gz

$ rm -rf httpd-2.4.23.tar.gz

$ cd httpd-2.4.23

$ ./configure --prefix=/usr/local/apache2 \
	--with-apr=/usr/local/apr \
	--with-apr-util=/usr/local/aprutil \
	--with-pcre=/usr/local/pcre \
	--enable-module=so \
	--enable-so \
	--with-mpm=worker \
	--enable-cache
	
$ make -j 2

$ sudo make install
```

### 1.3 Apache 설정

- vi 애디터로 수정 및 추가

  ```bash
  $ sudo vi /usr/local/apache2/conf/httpd.conf
  ```

  - `ServerName localhost` 수정 및 주석 해제

  - `<IfModule dir_module>` 찾아서 추가

    ```bash
    DirectoryIndex index.html index.htm index.php
    ```

  - AddType 찾아 확인 및 추가

    ```bash
    AddType application/x-compress .Z
    AddType application/x-gzip .gz .tgz
    AddType application/x-httpd-php .php .htm .html .inc .php4 .php3
    AddType application/x-httpd-php-source .phps
    ```

- httpd.conf

  ```bash
  $ sudo cp /usr/local/apache2/bin/apachectl /etc/init.d/httpd
  
  $ sudo /etc/init.d/httpd configtest
  
  $ sudo /etc/init.d/httpd start
  ```

## 2. MySQL 설치

### 2.1 cmake 설치

```bash
$ sudo apt-get install cmake
```

### 2.2 MySQL 다운로드

```bash
$ cd ~

$ wget https://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.14.tar.gz

$ tar xvfz mysql-5.5.14.tar.gz

$ rm -rf mysql-5.5.14.tar.gz

$ cd mysql-5.5.14
```

### 2.3 MySQL cmake 컴파일

```bash
$ cmake \
	-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
	-DMYSQL_DATADIR=/usr/local/mysql/data \
	-DMYSQL_UNIX_ADDR=/tmp/mysql.sock \
	-DSYSCONFDIR=/etc \
	-DDEFAULT_CHARSET=utf8 \
	-DMYSQL_TCP_PORT=3306 \
	-DWITH_EXTRA_CHARSETS=all \
	-DDFAULT_COLLATION=utf8_general_ci \
	-DDOWNLOAD_BOOST=1 \
	-DWITH_BOOST=/usr/include/boost \
	-DWITH_INNOBASE_STORAGE_ENGINE=1 \
	-DWITH_PERFSCHEMA_STORAGE_ENGINE=1 \
	-DENABLED_LOCAL_INFILE=1
	
$ make -j 2

$ sudo make install
```

### 2.4 리눅스 계정 생성

```bash
$ sudo groupadd mysql

$ sudo useradd -g mysql mysql
```

### 2.5 MySQL 그룹/계정 권한 주기

```bash
$ sudo chown root.mysql -R /usr/local/mysql

$ sudo chown root.mysql -R /usr/local/mysql/data
```

### 2.6 환경설정 기본 파일 복사 및 수정

```bash
$ cd /usr/local/mysql/support-files

$ sudo cp my-huge.cnf /etc/my.cnf
```

- vi 애디터로 파일 열고 [mysqld] 부분에 추가

  ```bash
  $ sudo vi /etc/my.cnf
  ```

  ```
  [mysqld]
  character-set-server = utf8
  collation-server = utf8_general_ci
  character-set-client-handshake = false
  ```

### 2.7 DataBase 생성

```bash
$ cd /usr/local/mysql

$ sudo ./scripts/mysql_install_db --user=mysql --datadir=/usr/local/mysql/data
```

### 2.8 서버 데몬 구동

```bash
$ sudo ./bin/mysqld_safe &
```

### 2.9 MySQL root 계정 비밀번호 변경

```bash
$ sudo ./bin/mysqladmin -u root password root
```

## 3. PHP 설치

### 3.1 사전 설정

```bash
$ sudo apt-get install -y pkg-config sqlite3 libsqlite3-dev
```

### 3.2 PHP 다운로드 및 설치

```bash
$ wget https://www.php.net/distributions/php-7.4.3.tar.gz

$ tar xvfz php-7.4.3.tar.gz

$ rm -rf php-7.4.3.tar.gz

$ cd php-7.4.3

$ ./configure \
	--prefix=/usr/local/php \
	--with-apxs2=/usr/local/apache2/bin/apxs \
	--enable-mysqlnd \
	--with-mysqli=/usr/local/mysql/bin/mysql_config \
	--with-pdo-mysql=/usr/local/mysql \
	--with-config-file-path=/usr/local/apache2/conf \
	--enable-exif \
	--with-openssl \
	--with-imap-ssl \
	--disable-debug \
	--with-iconv \
	--enable-fpm
	
$ make

$ make test

$ sudo make install
```

### 3.3 Apache와 PHP 연동

```bash
$ sudo cp php.ini-production /usr/local/apache2/conf/php.ini
```

```bash
$ sudo vi /usr/local/apache2/htdocs/index.php
```

```php
<?php
    phpinfo();
?>
```

![image-20200311105113062](06_APM_NPM_Compile(Ubuntu_16.04).assets/image-20200311105113062.png)

## 4. NGINX 설치

### 4.1 NGINX 다운로드

```bash
$ wget https://nginx.org/download/nginx-1.12.0.tar.gz

$ tar xvfz nginx-1.12.0.tar.gz

$ rm -rf nginx-1.12.0.tar.gz
```

### 4.2 PCRE 다운로드

```bash
$ cd nginx-1.12.0

$ wget http://downloads.sourceforge.net/project/pcre/pcre/8.37/pcre-8.37.tar.gz

$ tar xvfz pcre-8.37.tar.gz

$ rm -rf pcre-8.37.tar.gz
```

### 4.3 zlib 다운로드

```bash
$ wget http://zlib.net/zlib-1.2.11.tar.gz

$ tar xvfz zlib-1.2.11.tar.gz

$ rm -rf zlib-1.2.11.tar.gz
```

### 4.4 OpenSSL 다운로드

```bash
$ wget http://www.openssl.org/source/openssl-1.0.2f.tar.gz

$ tar xvfz openssl-1.0.2f.tar.gz

$ rm -rf openssl-1.0.2f.tar.gz
```

### 4.5 NGINX 설치

```bash
$ ./configure \
	--prefix=/usr/local/nginx \
	--with-zlib=./zlib-1.2.11 \
	--with-pcre=./pcre-8.37 \
	--with-openssl=./openssl-1.0.2f \
	--with-http_ssl_module \
	--with-http_stub_status_module
	
$ make

$ sudo make install

$ rm -rf ~/nginx-1.12.0
```

### 4.6 실행권한 설정

```bash
$ cd /usr/local/nginx/sbin

$ sudo chown root nginx

$ sudo chmod +s nginx
```

### 4.7 실행 및 테스트

```bash
$ cd /usr/local/nginx/sbin

$ ./nginx	# 시작

# 포트 오류시
$ sudo vi /usr/local/nginx/conf/nginx.conf	# listen 80 수정

$ ./nginx -s stop
```

### 4.8 NGINX와 PHP 연동

- php-fpm

  ```bash
  $ sudo cp ~/php-7.4.3/php.ini-production /usr/local/lib/php.ini
  
  $ sudo cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf
  
  $ sudo cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf
  ```

- sysv-rc-conf 설정

  ```bash
  $ sudo cp ~/php-7.4.3/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
  
  $ sudo chmod 700 /etc/init.d/php-fpm
  
  $ sudo apt-get install sysv-rc-conf	# ubuntu는 chkconfig가 없다
  
  $ sudo sysv-rc-conf php-fpm on
  ```

- vi 애디터 실행 후 아래 내용 수정

  ```bash
  $ sudo vi /usr/local/nginx/conf/nginx.conf
  ```

  - php 부분 주석 제거 후 수정

    ```bash
    location / {
    	root html;
    	index index.html index.htm index.php;
    }
    
    location ~ \.php$ {
    	root			/usr/local/src/nginx/html;
    	fastcgi_pass	127.0.0.1:9000;
    	fastcgi_index	index.php;
    	fastcgi_param SCRIPT_FILENAME	/usr/local/src/nginx/html$fastcgi_script_name;
    	# /scripts
    	include			fastcgi_params;
    }
    ```

- index.php 작성

  ```bash
  $ sudo vi /usr/local/nginx/html/index.php
  ```

  ```php
  <?php
      phpinfo();
  ?>
  ```

- 유저 및 그룹 추가

  ```bash
  $ useradd nginx
  
  $ groupadd nginx
  ```

- www.conf 수정

  ```bash
  $ sudo vi /usr/loacl/php/etc/php-fpm.d/www.conf
  ```

  ```
  user = nginx
  group = nginx
  ```

- 재시작

  ```bash
  $ ./nginx -s stop
  
  $ ./nginx
  
  $ sudo /etc/init.d/php-fpm start
  ```

  ![image-20200311134602299](06_APM_NPM_Compile(Ubuntu_16.04).assets/image-20200311134602299.png)

