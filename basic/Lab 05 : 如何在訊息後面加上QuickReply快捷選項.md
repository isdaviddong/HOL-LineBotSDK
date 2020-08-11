如何在訊息後面加上QuickReply快捷選項
===
## Overview
QuickReply快捷選項是帶有圖示的功能按鈕，支援開啟時間日期選擇、地點選擇、相機拍照、照片選擇...等多種功能。可隨著某個訊息一同出現：

![圖片](https://i.imgur.com/pcGonTb.png)

本Lab介紹如何透過 C# 以程式碼，在推送LINE Bot訊息時，加上QuickReply快捷選項。

## Prerequisites
0. 建立好LINE Bot帳號，並取得Channel Access Token與UserId [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 3.1 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立 .net core console 專案，在專案中引用 nuget 上的 LineBotSDK 套件。 
4. 透過Simulator建立好一組訊息JSON

## Steps

### 建立 .net core console專案
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

### 接著執行底下指令， 安裝 LineBotSDK 套件...
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

### 透過底下指令，開啟 VS Code進入開發環境
請在命令列輸入 code (空格) .
```bash
PS D:\linetest> code .
PS D:\linetest>
```
![enter image description here](https://i.imgur.com/QQBSJWL.png)
完成後會看到類似上面的畫面。

### 建立bot物件實體
輸入底下程式碼，建立bot instance
(請將token與UserId換為你自己的)
```csharp
var  ChannelAccessToken = "___token___"; //Channel Access Token
var  UserId = "___UserId__"; //收訊者
var  bot = new  isRock.LineBot.Bot(ChannelAccessToken);
```

### 撰寫程式碼發送Flex Message
繼續在程式碼中加入底下內容:
```csharp

//Icon
const string IconUrl = "https://arock.blob.core.windows.net/blogdata201809/1337594581_package_edutainment.png";

//建立一個TextMessage物件
isRock.LineBot.TextMessage m =
    new isRock.LineBot.TextMessage("請在底下選擇一個選項");

//在TextMessage物件的quickReply屬性中加入items
m.quickReply.items.Add(
        new isRock.LineBot.QuickReplyMessageAction(
            $"一般標籤", "點選後顯示的text文字"));

m.quickReply.items.Add(
    new isRock.LineBot.QuickReplyMessageAction(
        $"有圖示的標籤", "點選後顯示的text文字", new Uri(IconUrl)));
//加入QuickReplyDatetimePickerAction
m.quickReply.items.Add(
    new isRock.LineBot.QuickReplyDatetimePickerAction(
        "選時間", "選時間", isRock.LineBot.DatetimePickerModes.datetime, new Uri("https://i.imgur.com/9qgoKFm.png")));
//加入QuickReplyLocationAction
m.quickReply.items.Add(
    new isRock.LineBot.QuickReplyLocationAction(
        "選地點", new Uri("https://i.imgur.com/JFenVZi.png")));
//加入QuickReplyCameraAction
m.quickReply.items.Add(
    new isRock.LineBot.QuickReplyCameraAction(
    "Show Camera", new Uri("https://i.imgur.com/YzlF0Vb.png")));
//加入QuickReplyCamerarollAction
m.quickReply.items.Add(
    new isRock.LineBot.QuickReplyCamerarollAction(
    "Show Cameraroll", new Uri(IconUrl)));

//透過Push發送訊息
bot.PushMessage(UserId, m);
```

檢視的結果如下:

![圖片](https://i.imgur.com/IyU4knZ.png)


