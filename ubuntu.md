# Ubuntu 18.04 configure

## basic software and config

```shell
sudo apt install build-essential git python3-pip zsh curl
```

### pypi source

```shell
mkdir .config/pip
nano .config/pip/pip.conf
```

```ini
[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns
```

### fstab

```shell
ll /dev/disk/by-uuid
# UUID=XXXXXXXXXXXXX	/media/monk/d	ntfs-3g uid=1000,gid=1000,umask=0022	0	0
```

## chrome 

[download](https://www.google.com/chrome/)

## shadowsocks server

### install shadowsocks

```shell
sudo pip3 install shadowsocks
sudo mkdir /etc/shadowsocks
sudo nano /etc/shadowsocks/server.json
```

```json
{
        "server":"ip_address",
        "port_password":{
                "8388":"********"
        },
        "timeout":300,
        "method":"aes-256-cfb"
}
```

### configure service

```shell
sudo nano /etc/systemd/system/shadowsocks.service
```

```ini
[Unit]
Description=Shadowsocks Server Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks/server.json

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl enable shadowsocks.service
sudo systemctl start shadowsocks.service
```

## kcptun server

```shell
sudo nano /etc/systemd/system/kcptun.service
```

```ini
Description=Kcptun Server Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/server_linux_amd64 -l ":39900" -t ":8380" --key=*** --crypt=none --mode=fast3 --mtu=1350 --sndwnd=1024 --rcvwnd=1024 --datashard=10 --parityshard=3 --dscp=0 --nocomp --quiet

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl enable kcptun.service
sudo systemctl start kcptun.service
```

## kcptun client

[download](https://github.com/xtaci/kcptun/releases)

```shell
sudo mkdir /etc/kcptun
sudo nano /etc/kcptun/client.json
{
  "localaddr": ":local_port",
  "remoteaddr": "server:port",
  "key": "******",
  "crypt": "none",
  "mode": "fast3",
  "mtu": 1350,
  "sndwnd": 512,
  "rcvwnd": 1024,
  "datashard": 10,
  "parityshard": 3,
  "dscp": 0,
  "nocomp": true,
  "quiet": true,
  "tcp": false
}
```

```shell
sudo nano /etc/systemd/system/kcptun.service
```

```ini
Description=Kcptun Client Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/client_linux_amd64 -c /etc/kcptun/client.json

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl enable kcptun.service
sudo systemctl start kcptun.service
```

## shadowsocks client

### install shadowsocks

```shell
sudo pip3 install git+https://github.com/shadowsocks/shadowsocks.git@master
sudo mkdir /etc/shadowsocks
sudo nano /etc/shadowsocks/client.json
```

```json
{
        "server":"ip_address",
        "server_port":8388,
        "local_address": "127.0.0.1",
        "local_port":1080,
        "password":"********",
        "timeout":300,
        "method":"aes-256-cfb"
}
```

### fix bugs

```shell
sudo nano /usr/local/lib/python3.6/dist-packages/shadowsocks/crypto/openssl.py
```

replace **EVP_CIPHER_CTX_cleanup** by **EVP_CIPHER_CTX_reset**

### configure service

```shell
sudo nano /etc/systemd/system/shadowsocks.service
```

```ini
[Unit]
Description=Shadowsocks Client Service
After=network.target

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/sslocal -c /etc/shadowsocks/client.json

[Install]
WantedBy=multi-user.target
```

```shell
sudo systemctl enable shadowsocks.service
sudo systemctl start shadowsocks.service
```

## provixy

```shell
sudo apt install provixy
```

modify "/etc/privoxy/config", set 
```ini
forward-socks5t /       127.0.0.1:1080  .
```

```shell
sudo systemctl restart privoxy.service
```

### chrome configure
install [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh-CN)

#### proxy:

sock5 127.0.0.1 1080

#### auto switch

规则列表格式  AutoProxy

规则列表网址  https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt

### git proxy

```shell
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### apt proxy

```shell
nano .apt-proxy.conf
```

``` config
Acquire {
  HTTP::proxy "http://127.0.0.1:8118";
  HTTPS::proxy "http://127.0.0.1:8118";
}
```

```shell
sudo apt -c .apt-proxy.conf command
```

## [oh-my-zsh](https://ohmyz.sh/)

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# ZSH_THEME="agnoster"
```

install [powerline font](https://github.com/powerline/fonts.git)

```shell
git clone https://github.com/powerline/fonts.git
cd fonts
./install.sh
```

## pyenv

```shell
curl https://pyenv.run | zsh
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python3-openssl git
cd ~/.pyenv/plugins/python-build/share/python-build
sed -i "s#https://www.python.org/ftp/python/#https://npm.taobao.org/mirrors/python/#g" `grep 'https://www.python.org/ftp/python/' -rl .`
cd ~
pyenv install 3.7.9
```

## [oracle-jdk](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

## [cuda](https://developer.nvidia.com/cuda-toolkit-archive)

## [tensorflow](https://www.tensorflow.org/install)

## [pycharm](https://www.jetbrains.com/pycharm/download/)

## [PhpStorm](https://www.jetbrains.com/phpstorm/download/)

## [Clion](https://www.jetbrains.com/clion/download/)

## [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)

## [Visual Studio Code](https://code.visualstudio.com/download)

## [hadoop](http://hadoop.apache.org/releases.html)

## [spark](https://spark.apache.org/downloads.html)
