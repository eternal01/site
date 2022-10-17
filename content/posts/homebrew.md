---
title: "Homebrew"
date: 2022-10-16T08:55:14Z
---
## **Homebrew核心概念**
|词汇|含义|
|:---|:---|
|formula(e)|安装包的描述文件，formulae 为复数|
|cellar|安装好后所在的目录|
|keg|具体某个包所在的目录，keg 是 cellar 的子目录|
|bottle|预先编译好的包，不需要现场下载编译源码，速度会快很多；官方库中的包大多都是通过 bottle 方式安装|
|tap|下载源，可以类比于 Linux 下的包管理器 repository|
|cask|安装 macOS native 应用的扩展，你也可以理解为有图形化界面的应用|
|bundle|描述 Homebrew 依赖的扩展|

## **Homebrew安装**
使用终端执行命令
```zsh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

    若以上安装失败，并提醒：

    Failed to connect to http://raw.githubusercontent.com port 443: Connection refused.

    则可以尝试使用国内源进行安装，详情请看 Gitee / CunKai / HomebrewCN。（或者科学上网）


查看安装是否成功
```zsh
brew --version
```

## **Homebrew切换源**

### **Homebrew**
切换中科大源
```zsh
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
brew update
```
    若用户设置了环境变量 HOMEBREW_BREW_GIT_REMOTE，则每次运行 brew update 时将会自动设置远程。 推荐用户将环境变量 HOMEBREW_BREW_GIT_REMOTE 加入 shell 的 profile 设置中。

```zsh
# 对于 bash 用户
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> ~/.bash_profile
# 对于 zsh 用户
echo 'export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"' >> ~/.zshrc
```
重置为官方地址
```zsh
unset HOMEBREW_BREW_GIT_REMOTE
git -C "$(brew --repo)" remote set-url origin https://github.com/Homebrew/brew
```
    重置回默认远程后，用户应该删除 shell 的 profile 设置中的环境变量 HOMEBREW_BREW_GIT_REMOTE 以免运行 brew update 时远程再次被更换。

    若之前使用的 git config url.<URL>.insteadOf URL 的方式设置的镜像，请手动删除 config 文件（一般为 ~/.gitconfig 或仓库目录下的 .git/config）中的对应字段。

使用中科大源安装Homebrew
首先在命令行运行如下几条命令设置环境变量：
```zsh
export HOMEBREW_BREW_GIT_REMOTE="https://mirrors.ustc.edu.cn/brew.git"
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
```
之后在命令行运行 Homebrew 安装脚本：
```zsh
/bin/bash -c "$(curl -fsSL https://github.com/Homebrew/install/raw/HEAD/install.sh)"
```
### **Homebrew-core**
替换中科大源
```zsh
export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"
brew update
```

    若用户设置了环境变量 HOMEBREW_CORE_GIT_REMOTE，则每次运行 brew update 时将会自动设置远程。 推荐用户将环境变量 HOMEBREW_CORE_GIT_REMOTE 加入 shell 的 profile 设置中。
```zsh
# 对于 bash 用户
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> ~/.bash_profile

# 对于 zsh 用户
echo 'export HOMEBREW_CORE_GIT_REMOTE="https://mirrors.ustc.edu.cn/homebrew-core.git"' >> ~/.zshrc
```
重置为官方地址：
```zsh
unset HOMEBREW_CORE_GIT_REMOTE
brew tap --custom-remote homebrew/core https://github.com/Homebrew/homebrew-core
```

    重置回默认远程后，用户应该删除 shell 的 profile 设置中的环境变量 HOMEBREW_CORE_GIT_REMOTE 以免运行 brew update 时远程再次被更换。

### **Homebrew-bottles**
替换为中科大源
```zsh
export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"
```

```zsh
# 对于 bash 用户
echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"' >> ~/.bash_profile

# 对于 zsh 用户
echo 'export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.ustc.edu.cn/homebrew-bottles"' >> ~/.zshrc
```

### **Homebrew-cask**
替换为中科大源
```zsh
brew tap --custom-remote --force-auto-update homebrew/cask https://mirrors.ustc.edu.cn/homebrew-cask.git
```
重置为官方地址
```zsh
brew tap --custom-remote --force-auto-update homebrew/cask https://github.com/Homebrew/homebrew-cask
```
### **Homebrew-cask-version**
替换为中科大源
```zsh
brew tap --custom-remote --force-auto-update homebrew/cask-versions https://mirrors.ustc.edu.cn/homebrew-cask-versions.git
```
重置为官方地址
```zsh
brew tap --custom-remote --force-auto-update homebrew/cask-versions https://github.com/Homebrew/homebrew-cask-versions
```

## **Homebrew常用命令**
查找
```zsh
brew search <package>
```
安装
```zsh
brew install <package>
brew install <package> -v   #显示详细安装信息
```
包信息
```zsh
brew info
brew info <package>
```
卸载
```zsh
brew uninstall <package>
brew uninstall --force <package>    #强制卸载包
```
重装
```zsh
brew reinstall <package>
```
列表
```zsh
brew list --version     #安装的所有包
brew list --formulae    #安装的formule包
brew list --cask    #安装的cask包
```
更新
```zsh
brew update
brew update <package>
```
可更新包列表
```zsh
brew outdated
```
升级
```zsh
brew upgrade
brew upgrade <package>
```
清理旧版本
```zsh
brew cleanup
brew cleanup <package>
```
锁定包版本
```zsh
brew pin <package>
```
取消锁定包版本
```zsh
brew unpin <package>
```
查看已安装的包依赖
```zsh
brew deps --installed --tree
```
## **Homebrew安装本地包**
1. 执行以下命令获取包名（哈希值）
```zsh
brew install <package> -v
```
2. 通过其他方式下载安装包至本地
3. 执行以下命令，获取brew安装目录
```zsh
brew --cask
```
4. 将下载文件迁移至brew下载目录并重命名
5. 再次执行以下命令，因brew从本地找到文件，遂不再下载，直接执行安装
```zsh
brew install <package> -v
```