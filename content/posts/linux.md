---
title: "Linux"
date: 2022-10-16T10:27:19Z
# bookComments: false
# bookSearchExclude: false
---

## **shell 选择**

查看当前系统使用默认shell
```zsh
echo $SHELL
```

列出所有可用shell
```zsh
cat /etc/shells
```

切换shell为zsh
```zsh
chsh -s /bin/zsh
```

安装oh my zsh
```zsh
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
```
国内镜像安装
```zsh
sh -c "$(curl -fsSL https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh)"
```

卸载oh my zsh
```zsh
uninstall_oh_my_zsh
```

配置oh my zsh插件
```zsh
vim ~/.zshrc
```
    修改plugins=(git z extract)
    z 提供智能跳转，cd加强版
    extract 解压缩插件，提供x别名，x解压文件
    z,extract为自带插件，直接开启即可，不需要另行安装