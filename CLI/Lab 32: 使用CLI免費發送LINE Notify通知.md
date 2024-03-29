使用CLI免費發送LINE Notify通知
===
## Overview
這個 Lab 介紹如何透過Command Line來免費發送的LINE Notify通知
## Prerequisites
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 LINE CLI 工具1.0.19以上版本 [here](https://www.nuget.org/packages/line.cli/)
 
## Steps

1. 在command line輸入 dotnet --version，確定您的執行環境具備 .net core 3.x 以上版本
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
PS C:\> dotnet tool install --global line.cli 
```
如果系統會出現類似底下畫面...
```bash
工具 'line.cli' 已安裝。
```
2. 申請 LINE Notify Token
前往[LINE Notify](https://notify-bot.line.me/my/)，以個人帳號登入，至個人頁面的最下方『發行權杖』：  
![圖片](https://i.imgur.com/6JiBvM0.png)  
4. 選擇訊息要發送到哪一個群組(亦可發送給自己一人)：  
![image](https://github.com/isdaviddong/HOL-LineBotSDK/assets/4635798/6126daab-6497-45b1-a7bd-74296b7505dc)

5. 按下『發行』後，可取得類似底下的 Notify Token:  
![image](https://github.com/isdaviddong/HOL-LineBotSDK/assets/4635798/b6ca1185-296b-481d-ad71-61cfca2df6bd)
6. 如果是發送給群組，記得將 LINE Notify帳號加入該群組中。
7. 未來，只需要透過底下Command Line指令，即可免費發送LINE訊息給特定群組：
```bash
PS C:\> line notify -n [NotifyToken] -m 要發送的訊息 
```
你會發現訊息就會出現在LINE通知中：  
 ![image](https://github.com/isdaviddong/HOL-LineBotSDK/assets/4635798/e443d337-67ed-43df-9a94-3641cb190a85)

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.pubu.com.tw/ebook/103305)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.tenlong.com.tw/products/9789865022662)  
線上課程：[https://www.udemy.com/line-bot/](https://www.udemy.com/line-bot/)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.nuget.org/packages/LineBotSDK)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
