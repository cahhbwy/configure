# 开发环境配置

## 终端

1. [zsh](https://ohmyz.sh/)

```shell
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

2. [p10k主题](https://github.com/romkatv/powerlevel10k)

```shell
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set `ZSH_THEME="powerlevel10k/powerlevel10k"` in ~/.zshrc.

## pyenv

1. 安装pyenv
```shell
curl https://pyenv.run | zsh
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```

2. 配置编译环境
[suggested-build-environment](https://github.com/pyenv/pyenv/wiki#suggested-build-environment)
```
# macos
brew install openssl readline sqlite3 xz zlib tcl-tk
# ubuntu
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

3. 【可选】改用淘宝源
```shell
# linux
sed -i "s#https://www.python.org/ftp/python/#https://npm.taobao.org/mirrors/python/#g" `grep 'https://www.python.org/ftp/python/' -rl ~/.pyenv/plugins/python-build/share/python-build`
# macos
sed -i "" "s#https://www.python.org/ftp/python/#https://npm.taobao.org/mirrors/python/#g" `grep 'https://www.python.org/ftp/python/' -rl ~/.pyenv/plugins/python-build/share/python-build`
```

4. 【Tips】可将下载好的Python源码放在`~/.pyenv/cache/`文件夹下，省去了下载过程。

5. 【可选】pip使用清华源

```shell
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

# 其他常用软件

* [Visual Studio Code](https://code.visualstudio.com/download)
* [pycharm](https://www.jetbrains.com/pycharm/download/)
* [PhpStorm](https://www.jetbrains.com/phpstorm/download/)
* [Clion](https://www.jetbrains.com/clion/download/)
* [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
* [JETBRA](https://3.jetbra.in/) JetBrains系列激活工具
* [oracle-jdk](https://www.oracle.com/technetwork/java/javase/downloads/index.html)
* [cuda](https://developer.nvidia.com/cuda-toolkit-archive)
* [hadoop](http://hadoop.apache.org/releases.html)
* [spark](https://spark.apache.org/downloads.html)
* [tensorflow](https://www.tensorflow.org/install)
