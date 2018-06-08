# Ubuntu下的一些软件deb源及安装

## oracle-jdk

```bash
sudo add-apt-repository ppa:webupd8team/java #java8
sudo add-apt-repository ppa:linuxuprising/java #java10
sudo apt-get update
sudo apt-get install oracle-java8-installer
```

## shadowsocks-qt5

```bash
sudo add-apt-repository ppa:hzwhuang/ss-qt5
sudo apt update
sudo apt install shadowsocks-qt5
```

## sublime

```bash
wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | sudo apt-key add -
sudo apt-get install apt-transport-https
echo "deb https://download.sublimetext.com/ apt/stable/" | sudo tee /etc/apt/sources.list.d/sublime-text.list
sudo apt-get update
sudo apt-get install sublime-text
```

## asciiquarium

```bash
sudo apt install libcurses-perl
#wget http://search.cpan.org/CPAN/authors/id/K/KB/KBAUCOM/Term-Animation-2.4.tar.gz
tar -xf Term-Animation-2.4.tar.gz
cd Term-Animation-2.4/
perl Makefile.PL
make
make test
sudo make install
cd ..
#wget http://www.robobunny.com/projects/asciiquarium/asciiquarium.tar.gz
tar -xf asciiquarium.tar.gz
sudo cp asciiquarium_1.1/asciiquarium /usr/local/bin/
sudo chmod 0755 /usr/local/bin/asciiquarium
asciiquarium
```

## gnome

```bash
sudo apt-get install gnome-shell
sudo apt-get install ubuntu-gnome-desktop
```

## Anaconda

https://www.anaconda.com/download/#linux

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

