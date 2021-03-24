---
title: React Native Firebase安裝筆記
date: 2020-07-05 15:30:30
tags:
- react native
---

# React Native Firebase
要在React Native使用Firebase，我們可以使用React Native Firebase這個package，但在此package中有提到此package不能使用Expo CLI，因此我們要使用React Native CLI作為開發模板。詳細安裝方法[參考](https://reactnative.dev/docs/environment-setup)。

在官方文件中有提到，若之前有安裝過react-native-cli這個package，要把它移除。

---
## React Native Firebase安裝

根據自己的習慣使用npm或yarn安裝。

```zsh
# Using npm
npm install --save @react-native-firebase/app

# Using Yarn
yarn add @react-native-firebase/app
```
在這個`@react-native-firebase/app`套件中，並不包含firebase的功能，僅是可以使用firebase，但其他功能的package會根據需求再來安裝。

---

## 根據要開發iOS或Android App進行個別設定

首先我們先到Firebase網站開一新專案，是否要使用GA功能可以自己選擇。專案設立完畢且進入後，在專按名稱下方可選擇新增應用程式
![](1.png)。並分別設定將Firebase綁到iOS和Android。

## iOS 安裝

Firebase安裝步驟第一步，會要我們一定要輸入`軟體包ID`，而這個軟體包ID，就需要我們進入資料夾內尋找。

在我們前面使用React Native CLI作為開發模板時，會產生兩個資料夾，一個名稱為ios，一個名稱為android。
![](2.png)

根據官方說明，我們要先開啟`/ios/{projectName}.xcodeproj`或`/ios/{projectName}.xcworkspace`檔案，來查看`軟體包ID`。

接著我們使用Xcode開啟專案，點選最上層，右側即可找到Bundle Identifier，這也是我們要將Firebase註冊到iOS第一步驟所需輸入的iOS軟體包ID。（若是使用mac進行開發，在使用React Native CLI安裝完後，ios資料夾內就會有xcworkspace的檔案）。

![](3.png)

接下來回到Firebase安裝第二步驟，會提供一個`GoogleService-Info.plist`檔案，將他下載下來後，複製進去我們的專案內，可參考[官方文件](https://rnfirebase.io/)。（注意：此檔案是要複製到內層的專案內容中，並不是最外層的project中，以Xcode來看就是黃色資料夾的那層）

回到Firebase安裝第三步驟，新增Firebase SDK，這邊基本上就跟著網站上的內容操作（如果是使用mac來建立React Native CLI模板，則不需要額外建立Podfile）。

Firebase安裝第四步驟，會要我們新增初始化的code。這邊我會選擇Objective-C版本，並參考React Native Firebase的官方文件，先開啟`/ios/{projectName}/AppDelegate.m`文件，並在前面的import新增Firebase SDK
```
#import <Firebase.h>
```
並且找到此文件中的`didFinishLaunchingWithOptions`方法，在此方法最前面新增
```
if ([FIRApp defaultApp] == nil) {
    [FIRApp configure];
  }
```
並重新building或linking。
```
cd ios/
pod install --repo-update
cd ..
npx react-native run-ios
```
接下來就可以進入第五步驟。

在第五步驟，Firebase會測試專案是否連接Firebase，若連結成功，基本上Firebase安裝在iOS上就成功了。

## Android 安裝
Android安裝過程基本上與iOS大同小異。

一樣參考[官方文件](https://rnfirebase.io/)。

第一步一樣要找Android套件名稱（其實就跟iOS找`軟體包ID`一樣），只是這次我們要到`/android/app/src/main/AndroidManifest.xml`找；第二步驟將`google-services.json`移入專案；第三步驟根據React Native Firebase文件新增Firebase SDK，基本上就安裝完成，較iOS還要簡單。





