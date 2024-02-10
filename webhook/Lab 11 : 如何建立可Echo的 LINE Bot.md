如何建立可Echo的基本LINE Bot
===
## Overview

本Lab介紹如何透過 C# 以程式碼，以 .net core 開發框架，搭配 LineBotSDK ，快速建立一個可以回應用戶訊息(Echo)的LINE Bot。

## Prerequisites
0. 請先建立好LINE Bot帳號，並取得Channel Access Token與UserId [參考這裡](https://github.com/isdaviddong/HOL-LineBotSDK/blob/master/00.%20%E5%A6%82%E4%BD%95%E7%94%B3%E8%AB%8BLINE%20Bot.md)
1. 下載安裝 .net core sdk 8 以上版本 [here](https://dotnet.microsoft.com/download)
2. 安裝 Visual Studio Code 開發工具 [here](https://code.visualstudio.com/download)
3. 建立 .net core WebAPI 專案，在專案中引用 nuget 上的 LineBotSDK 套件。
4. 安裝好 Ngrok 便於在開發環境測試 [here](https://ngrok.com/)  

## Steps

### 建立 .net core WebAPI專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS D:\> md linetest
PS D:\> cd linetest
# 如果是 .net 8 之外的版本，請用 dotnet new webapi 替代
PS D:\linetest>dotnet new webapi --use-controllers
```
系統會出現類似底下畫面...
```bash
PS D:\linetest> dotnet new webapi  
The template "ASP.NET Core Web API" was created successfully.  
  
Processing post-creation actions...  
Running 'dotnet restore' on D:\linetest\linetest.csproj...  
D:\linetest\linetest.csproj 的還原於 210.75 ms 完成。  
  
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
安裝完成後我們建置(Build)一下，看結果如何:
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
完成後會看到類似底下畫面，請點選Startup.cs，將39行的程式碼註銷：
![enter image description here](https://i.imgur.com/ZQ7xZC1.png)

```
注意：
註銷上面的39行，是關閉強制使用 https 的設定。由於我們待會要在用戶端進行測試，關閉 https 將有助於測試的便利，避免在用戶端被其他應用程式阻擋，或是 ngrok, postman 等軟體無法測試的狀況。但在正式環境，我們建議你還原此設定，將會讓安全性更加的嚴謹。
```
###  安裝範本 (此動作只需做一次)
若你第一次執行，請在terminal執行底下指令安裝 LINE WebHook範本：
```dos
dotnet new --install isRock.Template.LineWebHook
```
完成後，當你執行 dotnet new 會看到列出的清單中有 linewebhook
![enter image description here](https://i.imgur.com/qEDFa88.png)

當你確定有該範本後，可在 Terminal (或VS Code中的終端機環境亦可)執行  dotnet new linewebhook  指令，你應當會看到類似底下訊息：
```dos
PS D:\linetest> dotnet new linewebhook  
The template "isRock.Template.LineWebHook" was created successfully.
```
完成後，在 VS Code的專案清單的Controller資料夾中(除了原本的WeatherForecastController.cs是系統內建的之外)，還會多出只少五支 *.cs 程式碼：

![enter image description here](https://i.imgur.com/laQr1wt.png)

請先關注 LineWebHookController.cs 這支(如上圖)。

### 修改 Channel Access Token 與  AdminUserId

在  LineWebHookController.cs 這支程式碼中，找到 "___Repleace_it_with_your_Channel_Access_Token___"，並置換成你的 channel access token，接著找到 "___Repleace_it_with_your_Admin_User_ID___"，並置換成你的UserID。

完成後類似底下這樣：
![enter image description here](https://i.imgur.com/tYcV4rJ.png)

請留意最上面一行的
```csharp
[Route("api/LineBotWebHook")]
```
這就是你的WebHook endpoint位置。

### 執行WebHook
請確定所有修改的程式碼都有儲存後，在 terminal(VS Code亦可)環境中執行該程式：
```dos
dotnet run
```
執行後結果應當如下：
![enter image description here](https://i.imgur.com/0UxMO6w.png)

上面這個畫面代表，你的WebHook已經在Localhost本機電腦上的5000與5001 port 被執行起來了，你可以嘗試用Chrome瀏覽器呼叫  http://localhost:5000/api/LineBotWebHook 
![enter image description here](https://i.imgur.com/sIy5iwp.png)

雖然看到上面的畫面，但結果是對的。
```
上述畫面是因為我們試圖用 http get方式去access只支援http post方法的endpoint。
(因為line bot要求採用 http post作為WebHook。你也可以使用postman以http post方式呼叫該endppint，應當會收到 http response 200的正常結果)
```

### 使用ngrok讓LINE Bot可以運行
請不要關閉應用程式的執行狀態。    
請註冊 NGROK帳號 https://dashboard.ngrok.com/login   
下載 NGROK: https://dashboard.ngrok.com/get-started/setup  
接著，將下載好的ngrok.zip壓縮檔解開，會看到其中有ngrok.exe執行檔，請將其放置在你覺得方便的資料夾下，並進入該資料夾，並取得token, 並執行指令，類似  
```dos
ngrok authtoken 2eeXX2nML9kBGsaa_5FtqZEnXSt2Ho7BXgPV3Ar4
```
接著，執行下列指令：
```dos
ngrok http 5000 --host-header="localhost:5000" --region us
```
執行結果如下：
![enter image description here](https://i.imgur.com/0PXeu6J.png)
請注意上面的 https 位置，請將該位置複製，並加上先前的route --> api/LineBotWebHook，組出一個完整的endpoint :
```
https://814273058c22.ngrok.io/api/LineBotWebHook
```

### 設定LINE Bot後台
接著，進入 LINE Bot管理後台，將上面得到的 endpoint 位置，設定在Webhook settings中:
![enter image description here](https://i.imgur.com/PhkuvrH.png)

完成後，可點選 Verify(請注意Use webhook必須勾選)：
![enter image description here](https://i.imgur.com/P3BbzVI.png)

若設定成功，應當會看到底下畫面：
![enter image description here](https://i.imgur.com/AsnjJaw.png)

這表示已成功設定，一切完成。

### 測試LINE Bot
接著，請確定您先前已經將你的LINE Bot加為好友，若確定無誤，你可以跟LINE Bot對話。不管你說什麼，你的LINE Bot都會原封不動的Echo你：

![enter image description here](https://i.imgur.com/FJ5bSTP.png)

因為我們在程式碼當中撰寫著：
```csharp
  //準備回覆訊息
  if (LineEvent.type.ToLower() == "message" && LineEvent.message.type == "text")
     responseMsg = $"你說了: {LineEvent.message.text}";
```
就這樣，你的第一個LINE Bot就完成了...

### 備註
```
若要中止此LINE Bot，可在VS Code的終端機中按下 Ctrl+C    
若要中止Ngrok，可在Ngrok執行所在的command line中按下 Ctrl+C
```

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.pubu.com.tw/ebook/103305)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.tenlong.com.tw/products/9789865022662)  
線上課程：[https://www.udemy.com/line-bot/](https://www.udemy.com/line-bot/)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.nuget.org/packages/LineBotSDK)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。

