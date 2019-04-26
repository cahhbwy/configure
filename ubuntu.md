# Ubuntu configure

## basic software and config
sudo apt install build-essential git python3-pip zsh curl

## chrome 

[download](https://www.google.com/chrome/)

## shadowsocks client

### shell

#### install shadowsocks
```shell
sudo pip3 install shadowsocks
sudo mkdir /etc/shadowsocks
sudo nano /etc/shadowsocks/client.json
```
```json
{
        "server":"ip_address",
        "server_port":port,
        "local_address": "127.0.0.1",
        "local_port":1080,
        "password":"********",
        "timeout":300,
        "method":"aes-256-cfb"
}
```

#### fix bugs
```shell
sudo nano /usr/local/lib/python3.6/dist-packages/shadowsocks/crypto/openssl.py
```
replace EVP_CIPHER_CTX_cleanup by EVP_CIPHER_CTX_reset

#### configure service
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

### chrome configure
install [SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl=zh-CN)

#### proxy:

sock5 127.0.0.1 1080

#### auto switch

规则列表格式  AutoProxy

规则列表网址  https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt

## [oh-my-zsh](https://ohmyz.sh/)
```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
ZSH_THEME="agnoster"
```
install [powerline font](https://github.com/powerline/fonts.git)

```shell
sudo apt-get install fonts-powerline
```

## pyenv
```shell
curl https://pyenv.run | zsh
```

## oracle-jdk-8

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

## oracle-jdk-11

```bash
sudo add-apt-repository ppa:linuxuprising/java
sudo apt-get update
sudo apt-get install oracle-java11-installer
```

## sublime

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

## pycharm

https://www.jetbrains.com/pycharm/download/

## PhpStorm

https://www.jetbrains.com/phpstorm/download/

## Clion

https://www.jetbrains.com/clion/download/

## Intellij Idea IU

https://www.jetbrains.com/idea/download/

## cuda

https://developer.nvidia.com/cuda-toolkit-archive

## cudnn

https://developer.nvidia.com/cudnn

## caffe

http://caffe.berkeleyvision.org/install_apt.html

## hadoop

http://hadoop.apache.org/releases.html

## spark

https://spark.apache.org/downloads.html

## opencv

## netease-cloud-music

