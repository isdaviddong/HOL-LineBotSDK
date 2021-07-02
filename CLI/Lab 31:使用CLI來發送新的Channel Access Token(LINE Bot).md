使用CLI來發送新的Channel Access Token(LINE Bot)
===

## Overview
這個lab介紹如何透過CLI來Issue Channel Access Tokem
## Prerequisites
0. 建立好LINE Bot帳號，並取得Channel ID與Channel Secret [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 LINE CLI 工具1.0.19以上版本 [here](https://www.nuget.org/packages/line.cli/)
 
## Steps

1. 在command line輸入 dotnet --version，確定您是3.x以上版本
```bash
PS C:\> dotnet --version
```
系統會出現類似底下畫面...
```bash
PS C:\> dotnet --version
5.0.202
```

2.接著執行底下指令， 安裝 LINE CLI 工具...
```bash
PS C:\> dotnet tool install --global line.cli --version 1.0.19
```
如果系統會出現類似底下畫面...
```bash
工具 'line.cli' 已安裝。
```
請執行底下指令以確定是最新版:
```bash
dotnet tool update --global line.cli --version 1.0.19
```
如果可以順利建置，你應該會收到類似下面這樣的訊息。
```bash
PS C:\> dotnet tool update --global line.cli --version 1.0.19
已使用最新穩定版本 ('1.0.19' 版) 來重新安裝工具 'line.cli'。
```
2. 請準備好你的 client id 與 client secret，並且輸入底下指令：
```bash
line token create --id 1658849790 --secret c7690f46fac88bffa9c5e211eeb726eb
```

```bash
PS C:\> line token create --id 1658849790 --secret c7690f46fac88bffa9c5e211eeb726eb
{"access_token":"Yv0K/mPPnrnsaXrDI+YeSknzg0hlxRSthJpGFZAz9QkBSYisyQaH95KNdHWM1mTXzthmEIqgzJnjRGebALNbM3fPkawgG/xpQocoxQ9WC4iRSe5SJsDy4uSf5lkJ2CYsaHuaCNDlFEC2VM9daIJnRY9PbdgDzCFqoOLOYbqAITQ=","expires_in":2592000,"token_type":"Bearer"}
```
你會發現新的 channel access token將會以JSON的方式呈現在畫面上

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.pubu.com.tw/ebook/103305)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.tenlong.com.tw/products/9789865022662)  
線上課程：[https://www.udemy.com/line-bot/](https://www.udemy.com/line-bot/)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.nuget.org/packages/LineBotSDK)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
