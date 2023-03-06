Install
===
>[https://dev.mysql.com/downloads/](https://dev.mysql.com/downloads/)

### 쉘 설치
```sh
cd /usr/local/src
sudo wget https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm
sudo yum localinstall -y mysql80-community-release-el7-7.noarch.rpm
sudo yum install -y mysql-community-client
mysql --version
```

<br>
