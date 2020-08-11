如何發送LINE訊息(Push Message)
===

## Overview
這個lab介紹如何透過 C# 以程式碼發送各種LINE基本訊息，包含貼圖、文字、圖片、聲音、影片、GPS 座標位置(Location)

## Prerequisites
0. 建立好LINE Bot帳號，並取得Channel Access Token與UserId [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立 .net core console 專案，在專案中引用 nuget 上的 LineBotSDK 套件。

## Steps

1. 建立 .net core console專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS D:\> md linetest
PS D:\> cd linetest
PS D:\linetest> dotnet new console
```
系統會出現類似底下畫面...
```bash
The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on D:\linetest\linetest.csproj...
  正在判斷要還原的專案...
  已還原 D:\linetest\linetest.csproj (251 ms 內)。
Restore succeeded.
```

2.接著執行底下指令， 安裝 LineBotSDK 套件...
```bash
PS D:\linetest> dotnet add package linebotsdk
```
系統會出現類似底下畫面...
```bash
PS D:\linetest> dotnet add package linebotsdk
  正在判斷要還原的專案...
  Writing C:\Users\...\AppData\Local\Temp\tmpC537.tmp
info : 正在將套件 'linebotsdk' 的 PackageReference 新增至專案 'D:\linetest\linetest.csproj'。
info : 正在還原 D:\linetest\linetest.csproj 的封裝...
info :   GET https://api.nuget.org/v3-flatcontainer/linebotsdk/index.json
info :   OK https://api.nuget.org/v3-flatcontainer/linebotsdk/index.json 1088 毫秒
info : 套件 'linebotsdk' 與專案 'D:\linetest\linetest.csproj' 中的所有架構相容。
info : 已將套件 'linebotsdk' 版本 '2.2.23' 的 PackageReference 新增至檔案 'D:\linetest\linetest.csproj'。
info : 正在認可還原...
info : 正在將資產檔案寫入磁碟。路徑: D:\linetest\obj\project.assets.json
log  : 已還原 D:\linetest\linetest.csproj (3.16 sec 內)。
```
安裝完成後我們順便建置(Build)一下，看結果如何:
```bash
PS D:\linetest> dotnet build
Microsoft (R) Build Engine for .NET Core 16.6.0+5ff7b0c9e 版
Copyright (C) Microsoft Corporation. 著作權所有，並保留一切權利。

  正在判斷要還原的專案...
  已還原 D:\linetest\linetest.csproj (712 ms 內)。
  linetest -> D:\linetest\bin\Debug\netcoreapp3.1\linetest.dll

建置成功。
    0 個警告
    0 個錯誤

經過時間 00:00:05.39
```
如果可以順利建置，你應該會收到類似上面這樣的訊息。

3. 透過底下指令，開啟 VS Code進入開發環境
請在命令列輸入 code (空格) .
```bash
PS D:\linetest> code .
PS D:\linetest>
```
![enter image description here](https://i.imgur.com/QQBSJWL.png)
完成後會看到類似上面的畫面。

4. 鍵入訊息發送程式碼
將上圖中的第9行程式註銷，換成底下程式碼
(請注意ChannelAccessToken和UserId 須換為你自己申請的LINE Bot相關資訊)：
```csharp
            //Console.WriteLine("Hello World!");
            var ChannelAccessToken = "_____replace_with_your_channel_access_token_____";
            var UserId = "_____replace_with_your_userId_____";
            var bot = new isRock.LineBot.Bot(ChannelAccessToken);
            //push text
            bot.PushMessage(UserId, "Hello World");
            //push sticker
            bot.PushMessage(UserId, 1, 3);
            //push image
            bot.PushMessage(UserId, new Uri("https://i.imgur.com/OQx8aid.png"));
            //push Audio
            var AudioMsg = new isRock.LineBot.AudioMessage(
            new Uri("https://arock.blob.core.windows.net/blogdata202008/test.mp3"), 6000);
            bot.PushMessage(UserId, AudioMsg);
            //push Video            
            var VideoMsg = new isRock.LineBot.VideoMessage(
            new Uri("https://arock.blob.core.windows.net/blogdata202008/POC.mp4"),
            new Uri("https://imgur.com/LPQhfXR.png")
            );
            bot.PushMessage(UserId, VideoMsg);
            //push location
            var LocationMsg = new isRock.LineBot.LocationMessage(
            "大安森林公園", "台北市大安區新生南路二段1號", 25.030000, 121.535833);
            bot.PushMessage(UserId, LocationMsg);
```
5. 在VS Code以hot key(C+S+`)切換至terminal： 
![enter image description here](https://i.imgur.com/24odB1m.png)
6. 鍵入 dotnet run 執行程式
```dos
PS D:\linetest> dotnet run
```
7. 檢視結果

![enter image description here](https://i.imgur.com/DRRmTcM.png)
![enter image description here](https://i.imgur.com/1VeXlmj.png)

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.blogger.com/blog/post/edit/4291069679343025964/3413012120805912874#)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.blogger.com/blog/post/edit/4291069679343025964/3413012120805912874#)  
線上課程：[https://www.udemy.com/line-bot/](https://www.blogger.com/blog/post/edit/4291069679343025964/3413012120805912874#)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.blogger.com/blog/post/edit/4291069679343025964/3413012120805912874#)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.blogger.com/blog/post/edit/4291069679343025964/3413012120805912874#)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。
