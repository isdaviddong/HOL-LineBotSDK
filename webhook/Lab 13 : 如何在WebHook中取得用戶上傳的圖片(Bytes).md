如何在WebHook中取得用戶上傳的圖片(Bytes)
===
## Overview

本Lab介紹如何在WebHook中，取得用戶上傳的圖檔Body Bytes。

## Prerequisites
0. 請先建立好LINE Bot帳號，並取得Channel Access Token與UserId [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立 .net core WebAPI 專案，在專案中引用 nuget 上的 LineBotSDK 套件。
4. 安裝好 Ngrok 便於在開發環境測試 [here](https://ngrok.com/)  
5. 完成 Lab 11，建立好可Echo的基本WebHook [[可參考here](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/webhook/Lab%2011%20:%20%E5%A6%82%E4%BD%95%E5%BB%BA%E7%AB%8B%E5%8F%AFEcho%E7%9A%84%20LINE%20Bot.md)]

## Steps

#### 依照Lab11，建立好可Echo的LINE Bot
請先完成先前介紹的Lab，建立好可以Echo的LINE Bot之後，請注意底下程式碼的第29行：  

![圖片](https://i.imgur.com/6CdIByx.png)

### 加入程式碼
請在29行下方，加入底下的程式碼：
```csharp
else if (LineEvent.type.ToLower() == "message" && LineEvent.message.type == "image")
{
    responseMsg = $"你說了: {LineEvent.message.text}";
    var FileBody = this.GetUserUploadedContent(LineEvent.message.id);
    responseMsg = $"Hi, 收到你上傳了一個檔案，大小為 {FileBody.Length} Bytes";
}
```
上面這段程式碼是判斷用戶傳來的訊息是否為圖片，如果是，則透過 API 抓取該圖片的bytes，然後設定顯示文字，回應用戶圖片的大小。
```dos
請注意，測試時建議你必須透過 ngrok 將運行的 endpoint 位置轉換成 LINE 伺服器能取得的 外部 URL。
這部分如果有問題，可以參考 Lab 11。
```
執行後結果如下：  
![圖片](https://i.imgur.com/JHL5WA2.png)

可以正確抓到圖檔資訊了。

