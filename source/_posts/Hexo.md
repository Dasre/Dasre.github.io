---
title: Hexo
date: 2021-03-22 23:08:37
tags:
- JavaScript
- Blog
---

# Github Actions自動部署Hexo Blog到Github Pages

## 前言

在使用Github Actions之前，要部署至Github Pages都是透過`hexo d -g`指令先生成靜態頁面再上傳到Github Pages。也因此在這次整理Blog並更換Blog主題時，決定順便把自動化部屬方面給加上去。

下面會說明在整個過程。

## Hexo安裝與主題安裝

### 安裝Hexo
```shell
// 全域安裝Hexo
$ npm install -g hexo-cli

// Hexo檔案生成
$ hexo init <blog-name>
```

### 安裝Hexo Next主題

```shell
$ cd <blog-name>
$ git clone https://github.com/theme-next/hexo-theme-next themes/next
```
git clone完後，確認根目錄的themes資料夾內有next資料夾，確認完畢後我們可以先行修改<span style="color: red">根目錄</span>的`_config.yml`檔案。

該檔案的有許多設定可以更換，但這不是我們這次的主題，不多做說明。

我們的目標是將Extensions的<span style="color: red">theme</span>改成next。

### Next主題之設定
這邊Next主題下載下來後，有兩種方式可以讓我們部署上去時，CSS這些文件能順利上傳。
1. 採用git submodules
2. 暴力方式，把Next主題先關git的東西全部刪除，但之後無法透過git更新主題。

這邊我們先採用第二種方式，我們需要先切到`themes/next/`資料夾，將`.git`資料夾、`.gitignore`和`README.md`檔案刪除。而上述行為是為了在上傳Github時，可以順利的抓到主題資料。

```shell
$ cd themes/next/
$ rm -rf .git
$ rm -rf .gitignore
$ rm -rf README.md
$ rm -rf docs/AUTHORS.md
```
使用`rm -rf`指令時，需要意會不需檢查強制刪除。

## Github Pages設定
這邊我們需要先在github上建立一個名為`<username>.github.io`的repository，並設定為public。接著就是將Hexo blog的專案設定git。

```shell
$ git init
$ git add .
$ git commit -m "first commit"
$ git branch -M main
$ git remote add origin https://github.com/Dasre/Dasre.github.io.git
$ git push -u origin main
```
這邊我們會將不是deploy的原始檔案放在main這個branch。

## Travis CI設定(舊方法)
Travis CI是Hexo官網提供的一個CI方法，但Travis CI在去年取消免費使用，每個使用者給予10000積分(linus系統部署1分鐘需要10積分，macOS系統部署1分鐘要50積分)，當積分不夠時就需要購買了。

雖然現在不會再使用此方法部署，但還是簡單說明一下Travis CI的方法。

1. 到[Travis](https://www.travis-ci.com/)註冊帳號，使用Github帳號即可。
2. 到[Applications settings](https://github.com/settings/installations)設定使Travis CI可以訪問帳號的倉庫內容。
3. 到Github的使用者Settings > Developer settings > Personal access tokens產生一組token，名稱可以自己定，下方的Select scopes要打開repo的public_repo，產生完畢後把token複製下來。
4. 到Travis確認是否有抓到使用者的專案，沒有的話點Activate，並允許Travis CI在你的Github帳號上。
5. 有抓到專案後，但專案的Settings，在Environment Variables新增參數，這邊Name我們用`GH_TOKEN`，Value則填上步驟3所複製的token，填完後按ADD。
6. 在根目錄新增一檔案`.travis.yml`，並在裡面新增以下內容。
```yml=
os: linux
language: node_js 
node_js:
  - '12'  # 使用 nodejs 12版
branches:
  only:
    - main # 只監控main這個branch
cache:
  directories:
    - node_modules
script: 
  - hexo generate # 產生靜態檔案
deploy: 
  provider: pages
  skip_cleanup: true 
  token: $GH_TOKEN # 步驟5那個token name
  keep_history: true
  on:
    branch: main # hexo文件所在branch
  local_dir: public 
  target_branch: master # 存放靜態檔案生完後的branch
```
7. 在步驟6新增完此檔案並push上去後，Travis的Running部分應該就會看到有在執行，執行完畢確認沒問題後，到專案的Settings，把GitHub Pages的Source Branch改成master，就完成Blog的自動化部屬了。

## GitHub Actions部署

### 建立Key
若要使用Github Actions，其道理和Travis CI很像，就是我們要給予Deploy的功能。

首先我們可以在blog專案的根目錄執行下列code：

```shell
// -t key type
// -b key的bits長度
// -C 註解
// -f 密碼文件的名稱
// -N Key的passphrase

$ ssh-keygen -t rsa -b 4096 -C "<your email>" -f github-deploy-key -N ""
```

關於上述ssh-keygen的option若有興趣了解，可參考[連結](https://www.ssh.com/ssh/keygen/);

執行完上述的code，會產生兩個檔案
1. github-deploy-key: 私鑰
2. github-deploy-key.pub: 公鑰

記得，這兩個檔案要加入`.gitignore`，不然公開大家都可以針對你的專案做修改。

### Github加入Key

1. 到github專案的Settings -> Secrets -> New repository secret
    * Name自己設定就好，但會影響到後續Github Actions的名稱，這邊輸入`HEXO_DEPLOY`
    * Value則輸入私鑰github-deploy-key的內容
2. 到github專案的Settings -> Deploy keys -> Add deploy key
    * Name一樣自己定，這邊輸入`HEXO_DEPLOY_PUB`
    * Key則輸入公鑰github-deploy-key.pub的內容
    * 注意下方的`Allow write access`要記得打勾

### 撰寫Github Actions
在不是要部署靜態文件的branch建立`.github/workflows/HexoCI.yml`。

HexoCI.yml填入以下內容

```yml
name: CI
on:
  push:
    branches:
      - main
jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout source
        uses: actions/checkout@v2
        with:
          ref: main
      - name: Use Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '14'
      - name: Setup hexo
        env:
          ACTION_DEPLOY: ${{ secrets.HEXO_DEPLOY }}
        run: |
          mkdir -p ~/.ssh/
          echo "$ACTION_DEPLOY" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan github.com >> ~/.ssh/known_hosts
          git config --global user.email "<Email>"
          git config --global user.name "<user name>"
          npm install hexo-cli -g
          npm install hexo-deployer-git --save
          npm install
      - name: Hexo deploy
        run: |
          hexo generate && hexo deploy
```

### Hexo deploy資料配置
到根目錄的`_config.yml`，增加deploy的資訊

這邊的repo要用ssh的方式。
```yml
deploy:
  type: git
  repo: git@github.com:Dasre/Dasre.github.io.git
  branch: master
```

### 確認Github Actions正常執行
在push commit上去後，可以到專案的Actions確認是否正確執行完畢，若正確執行完畢後，可以到專案Settings找到GitHub Pages，並把Source改成Branch為master，儲存後，`<username>.github.io`應該就會是靜態生成的頁面了。




