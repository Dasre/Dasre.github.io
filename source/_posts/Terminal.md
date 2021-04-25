---
title: Terminal
date: 2019-11-04 22:07:06
tags:
---

# VS code內使用Z Shell進行開發
在因工作的關係，最近需要使用Windows OS進行開發，然而在Windows內，並不能使用Z shell，也因此讓我在尋找是否有可以在Windows內使用Z Shell的方法。

在Windows OS裡面，我們都知道能使用的shell只有CMD和Power Shell，前者因時間久遠，許多的功能並不齊全，而後者雖然功能強大，但許多語法與我在macOS使用Z Shell的語法不同，造成使用體驗並不良好。

最近在Will 保哥的blog的文章發現[WSL](https://blog.miniasp.com/post/2019/02/01/Useful-tool-WSL-Windows-Subsystem-for-Linux)這套工具，因此有了這次的實作。以下會簡單說明如何在VS code透過WSL將Terminal改成Z Shell。關於WSL的安裝方法請參考上述的連結。

-----

### 關於WSL Linux系統選擇
我個人是選擇Ubuntu 16.04，主要是以往在Linux環境下進行開發時就是使用此OS，至於像Ubuntu 18.04我想語法也不會有太大的差別。

-----

### 安裝Z Shell和oh-my-zsh
在WSL安裝完畢且設定完帳號密碼後，完成後會是Ubuntu的Bash介面，我們要在此介面下安裝Z Shell和oh-my-zsh。

#### 在安裝之前請記得先執行以下code進行更新
``` shell
$ sudo apt update && sudo apt upgrade
```

#### 安裝zsh、切換zsh
``` shell
//安裝Z Shell
$ sudo apt-get install zsh

//切換bash -> zsh
$ which zsh
(/usr/bin/zsh)
$ chsh -s /usr/bin/zsh 
```

#### 安裝oh-my-zsh
``` shell
//安裝oh-my-zsh
$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

//更改theme（可自行選擇）
//要使用vim nano哪種方法編輯都可以
$ vim ~/.zshrc
//在.zshrc文件中找到 ZSH_THEME="..."並更改要使用之主題
//我自己是使用astro(主題請自行google取得)
ZSH_THEME="astro"
```
* 在.zshrc文件中同時可修改字體大小、字體等設定，同時一些highlight的套件也須透過.zshrc去設定
-------


### 在VS code使用Z Shell

至settings.json設定，並加入下列指令
``` shell
"terminal.integrated.shell.windows": "C:\\Windows\\System32\\bash.exe",
    "terminal.integrated.shellArgs.windows": [
        "-c",
        "zsh"
    ],
```
如出現字元無法呈現或框框等圖案，或許是字體上出問題，請加入下列指令。
``` shell
"terminal.integrated.fontFamily": "'Ubuntu Mono derivative Powerline'"
```

------
### 最終成果
{% asset_img VScode.png %}


