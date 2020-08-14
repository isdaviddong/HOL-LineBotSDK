如何在WebHook中取得用戶個人資訊(名稱、頭像、狀態)
===
## Overview

本Lab介紹如何在WebHook中，取得用戶的個人身分資訊。

## Prerequisites
0. 請先建立好LINE Bot帳號，並取得Channel Access Token與UserId [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立 .net core WebAPI 專案，在專案中引用 nuget 上的 LineBotSDK 套件。
4. 安裝好 Ngrok 便於在開發環境測試 [here](https://ngrok.com/)  
5. 完成 Lab 11，建立好WebHook [here](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/webhook/Lab%2011%20:%20%E5%A6%82%E4%BD%95%E5%BB%BA%E7%AB%8B%E5%8F%AFEcho%E7%9A%84%20LINE%20Bot.md)

## Steps

#### 依照Lab11，建立好可Echo的LINE Bot
請先完成先前介紹的Lab，建立好可以Echo的LINE Bot之後，請注意底下程式碼的第29行：
![圖片](https://i.imgur.com/6CdIByx.png)

請將上述的程式碼註銷，並加上 { }。

#### 加上取得用戶資訊的程式碼
接著，請在{...}中，加上底下程式碼，以取得用戶相關資訊:
```csharp
var user = this.GetUserInfo(LineEvent.source.userId);
responseMsg = $"名稱: {user.displayName} \n 狀態: {user.statusMessage} \n pictureUrl: {user.pictureUrl}";
```
完成後的程式碼大致如下:
![圖片](https://i.imgur.com/Cbhs0yQ.png)

完成後，請以dotnet run執行，並透過ngrok取得webhook，並設定於LINE Bot後台。

測試結果如下：
![圖片](https://i.imgur.com/U3psUNs.png)

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.pubu.com.tw/ebook/103305)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.tenlong.com.tw/products/9789865022662)  
線上課程：[https://www.udemy.com/line-bot/](https://www.udemy.com/line-bot/)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.nuget.org/packages/LineBotSDK)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。

