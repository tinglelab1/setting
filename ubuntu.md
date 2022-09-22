# setting
## keyboard
```
sudo apt-get update
sudo apt-get install fcitx-hangul
```
설정 > 지역 및 언어 > 설치된 언어 관리 > 입력기 > fcitx
```
reboot
```

## mysql
설치, 시작, 재부팅시 자동시작
```
sudo apt install mysql-server
sudo systemctl start mysql
sudo systemctl enable mysql
```
외부 접속 허용
```
sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
> # bind-address = 127.0.0.1
sudo systemctl restart mysql
```

계정 설정
```
# 따로 설정하지 않았다면, 우분투 비번과 동일
sudo mysql -u root -p
> CREATE DATABASE [db_name];
> CREATE USER 'dalgosim'@'%' IDENTIFIED BY '계정비번';
> GRANT ALL PRIVILEGES ON [db_name].* TO 'dalgosim'@'%';
> FLUSH PRIVILEGES;
```


## other packages
```
sudo apt install -y git nodejs python3-pip npm
```
git 설정
```
git config --global user.email "dalgosim@gmail.com"
git config --global user.name "dalgosim"

ssh-keygen
cat ~/.ssh/id_rsa.pub
```

## jupyter lab
```
pip3 install jupyterlab
```
### 설정
password 암호화
```
from notebook.auth import passwd
my_password = "spam-and-eggs"
hashed_password = passwd(passphrase=my_password, algorithm='sha256')
print(hashed_password)
```
환경설정
```
jupyter-lab --generate-config
> c.ServerApp.allow_origin = '*'
> c.ServerApp.ip = '0.0.0.0'
> c.ServerApp.notebook_dir = '/home/sasim/jupyter'
> c.ServerApp.open_browser = False
> c.ServerApp.password = u'sha256:~~~'
> c.ServerApp.port = 8888
```

### 서비스 등록하기
```
sudo vi /etc/systemd/system/jupyter.service

# 파일 내용
[Unit]
Description=Jupyter Notebook Server

[Service]
User=sasim
ExecStart=/usr/local/bin/jupyter-lab
WorkingDirectory=/home/sasim/jupyter

[Install]
WantedBy=multi-user.target

# 서비스 등록 및 활성화
sudo systemctl daemon-reload 
sudo systemctl enable jupyter.service 
sudo systemctl start jupyter.service

# 서비스 상태 확인
sudo systemctl status jupyter.service
```


## port open
```
sudo ufw allow mysql
sudo ufw allow 8888
sudo ufw allow 9000
```
