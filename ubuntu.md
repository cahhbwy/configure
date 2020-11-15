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
sudo nano /etc/shadowsocks/client.json
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
sudo pip3 install shadowsocks
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
sudo apt install proxy
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
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
pyenv install 3.6.8
```

## [oracle-jdk](https://www.oracle.com/technetwork/java/javase/downloads/index.html)

## [cuda-10-0](https://developer.nvidia.com/cuda-toolkit-archive) and [cudnn-7.4.1](https://developer.nvidia.com/rdp/cudnn-archive)

```
# Add NVIDIA package repositories
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo apt-get update
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb
sudo apt install ./nvidia-machine-learning-repo-ubuntu1804_1.0.0-1_amd64.deb
sudo apt-get update

# Install development and runtime libraries
sudo apt-get install cuda-10-0 libcudnn7=7.4.1.5-1+cuda10.0 libcudnn7-dev=7.4.1.5-1+cuda10.0

# Install TensorRT. Requires that libcudnn7 is installed above.
sudo apt-get update
sudo apt-get install nvinfer-runtime-trt-repo-ubuntu1804-5.0.2-ga-cuda10.0
sudo apt-get update
sudo apt-get install libnvinfer5=5.0.2-1+cuda10.0 libnvinfer-dev=5.0.2-1+cuda10.0
```

## [tensorflow-1.13.1](https://www.tensorflow.org/install)

needs **cuda-10-0**, **cudnn-7.4.1**, **nvinfer-5.0**

```shell
pyenv local 3.6.8
pip3 install tensorflow-gpu
```

## [pycharm](https://www.jetbrains.com/pycharm/download/)

## [PhpStorm](https://www.jetbrains.com/phpstorm/download/)

## [Clion](https://www.jetbrains.com/clion/download/)

## [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)

## sublime

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

## [hadoop](http://hadoop.apache.org/releases.html)

## [spark](https://spark.apache.org/downloads.html)
