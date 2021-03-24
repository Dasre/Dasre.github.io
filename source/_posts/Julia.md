---
title: Julia安裝教學
date: 2020-05-06 18:31:54
tags:
- w3HexSchool
- Data Science
---

# Julia
有使用過Python的人，大致上都了解Python雖然易學，但執行效率低的問題。而Julia是一個新興語言，它號稱與Python一樣易學，但又有著C的高效能。

這邊會說明Julia的安裝過程。

# Julia安裝
到[The Julia Language](https://julialang.org/)網站，點選上方的Download，選擇要安裝目前最新版本還是LTS版本。

如使用mac，會下載一dmg檔，直接拖曳進Application內即可安裝完成。Windows會下載一.exe檔案，雙擊安裝。

windows和mac安裝完後，建議添加PATH到環境變數。未來要使用terminal啟動Julia較為方便。

# IDE選擇
Julia安裝完後，選擇欲撰寫Julia的IDE。
目前可以使用的IDE很多，Jupyter Notebook、Atom、Visual Studio Code...。

個人自己比較喜歡VSCode，這邊會用VSCode做說明。
打開VSCode的Extension，搜尋Julia並安裝。

安裝完成後，至Settings>Extensions>Julia 找到Julia Executable Path，輸入Julia的Path。
在MAC上，Path原則上是`/Applications/Julia-<version>.app/Contents/Resources/julia/bin/julia`。

設定完成後，如果前面有將Julia路徑新增至環境變數，則可在資料夾路境內執行`julia name.jl`。

但目前Julia與VSCode相容並沒有說很好，所以還是來使用Jupyter Notebook (x

# 補充
## mac環境變數添加Julia Path
我自己的mac是使用zsh，而不是bash。因此在添加環境變數我會編輯zshrc。

* `sudo nano ~/.zshrc`
* 新增 `alias julia='/Applications/Julia-1.4.app/Contents/Resources/julia/bin/julia'`，要使用export也可以。
* Terminal輸入julia，即可使用julia。



