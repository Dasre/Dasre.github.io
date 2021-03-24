---
title: 一電腦使用多組Github帳號設定
date: 2020-03-23 15:20:12
tags:
- w3HexSchool
---

# 多Github帳號設定
透過Hostname來決定使用哪一組帳號的SSH Key進行溝通。

# SSH生成
首先我們先使用`ssh-keygen`指令生成金鑰
![](1.png)

圖中我們的指令為`ssh-keygen -t rsa -C "email@gamil.com"`
* -t 為加密方法的選擇，我們選擇使用RSA加密
* -C 為註解，會加入SSH的金鑰。可做為金鑰持有者的辨識。

輸入上述指令後，會分別詢問
* 金鑰存放位置和檔案名稱
* 是否設置Passphrase（如有輸入，會須重複輸入一次）

上述均輸入完畢後，會產生id_rsa和id_rsa.pub（這邊以預設檔名說明）
* id_rsa 此為私鑰，也就是要自己保管好的密碼。
* id_rsa.pub 此為公鑰，也就是對外公開的鑰匙，此會作為與本地端私鑰溝通使用。

---

# 將生成的金鑰複製到Github
如果使用linux，可以直接使用ssh-copy-id複製。

mac如要使用ssh-copy-id，需使用Homebrew安裝。

  * mac還可以使用pbcopy來複製，指令`pbcopy < ~/.ssh/id_rsa.pub`。

或是去打開檔案複製都可以。

---

# 設定ssh config方便選擇對應的git倉庫
在金鑰該層目錄，新增一config檔案。

可以使用`nano`或`vim`之類編輯都可以，看個人習慣。

`nano ~/.ssh/config`

新增下列資訊

```
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa

Host github-another
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_rsa_new
```

* Host 後面的github.com或github-another就是你要連接倉庫的簡稱。
* HostName 填HostNmae的domain或IP，因為是連github，所以是github.com。
* User 登入SSH的username，個人習慣統一為git。
* IdentityFile key的路徑。
其他還有Port或ForwardX11等指令。不過我們是連github，所以不需要。

設定完成後
可以下`ssh -T <Host>`檢查。
ex: `ssh -T github-another`。

---

# Clone與push
* Clone: `git clone <host-in-ssh-config>:<username>/<repo>`
* Push: `git remote set-url origin <host-in-ssh-config>:<username>/<repo>`

---

# 補充
以往只有單一一組的github，我們會直接在git的global設定好username和email。

但在有多組的github帳號後，如不想均使用相通的名稱。
需先使用`git config — global — unset user.name `和`git config — global — unset user.email`取消global設定。

再根據Repo來決定User資料
* `git config user.name "userName"`
* `git config user.email "eamil"`



