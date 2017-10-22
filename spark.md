# spark

## 需要hadoop，见[hadoop安装](hadoop.md)

## 需要scala

1. 可以使用apt源里的scala

```bash
sudo apt install scala
```

2. 可以去[官网](https://www.scala-lang.org/download/)下载最新版本的scala

解压并拷贝到/usr/share/目录下

```bash
sudo cp -r scala-2.12.4 /usr/share/
```

使用update-alternatives安装并配置scala，

```bash
sudo update-alternatives --install /usr/bin/scala scala /usr/share/scala-2.12.4/bin/scala 100
sudo update-alternatives --config scala
# 如果有多个版本的scala，进行选择配置
```

## 安装spark
