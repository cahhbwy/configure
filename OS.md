# win10、Ubuntu 蓝牙共用

1. win10下连接

2. Ubuntu下连接

3. cat /var/lib/bluetooth/电脑蓝牙Mac地址/设备蓝牙Mac地址/info

4. 记录[linkKey]下Key值

5. 使用PSTools，cmd=PsExec.exe -s -i regedit，找到[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\services\BTHPORT\Parameters\Keys\]，写入Key值

# win10、Ubuntu 系统时间

Ubuntu下执行 sudo timedatectl set-local-rtc 1

# win 10 状态栏时间显示秒

[HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced]
"ShowSecondsInSystemClock"=dword:00000001

# ohmyzsh

修复putty连接时小键盘失灵（查看绑定编码方式ctrl+v之后输入要查看的按键）

```zsh
# Home
bindkey '\e[1~' beginning-of-line
# End
bindkey '\e[4~' end-of-line

# Keypad
# 0 . Enter
bindkey -s "^[Op" "0"
bindkey -s "^[On" "."
bindkey -s "^[OM" "^M"
# 1 2 3
bindkey -s "^[Oq" "1"
bindkey -s "^[Or" "2"
bindkey -s "^[Os" "3"
# 4 5 6
bindkey -s "^[Ot" "4"
bindkey -s "^[Ou" "5"
bindkey -s "^[Ov" "6"
# 7 8 9
bindkey -s "^[Ow" "7"
bindkey -s "^[Ox" "8"
bindkey -s "^[Oy" "9"
# + -  * /
bindkey -s "^[Ol" "+"
bindkey -s "^[OS" "-"
bindkey -s "^[OR" "*"
bindkey -s "^[OQ" "/"
```

# SwitchOmega

规则列表 https://raw.githubusercontent.com/gfwlist/gfwlist/master/gfwlist.txt
