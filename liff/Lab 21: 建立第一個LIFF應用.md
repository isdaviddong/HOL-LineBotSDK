Lab 21: 建立第一個LIFF應用
===
## Overview

本Lab介紹如何透過 C# 程式碼，以 .net core WebApp開發框架，建立第一個LIFF(LINE Front-end Framework)應用程式。

## Prerequisites
0. 請先建立好LINE Login Channel，詳細作法可[參考這裡](https://developers.line.biz/en/docs/liff/getting-started/#creating-a-provider-and-channel)
1.  建立完成LINE Login Channel之後，請進入LIFF分頁，並點選 Add 建立新的LIFF：(建立時 endpoint URL可以先隨意設定)
![enter image description here](https://i.imgur.com/d0gshyi.png)
2. 完成後，取得 LIFF ID(類似0000000000-spPeRmAn) 即可。
3. 安裝好 Ngrok 便於在開發環境測試 [here](https://ngrok.com/)  

## Steps

### 建立 .net core WebApp專案
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案
```bash
PS D:\> md lifftest
PS D:\> cd lifftest
PS D:\lifftest> dotnet new webapp
```
系統會出現類似底下畫面...
```bash
PS D:\lifftest> dotnet new webapp  
The template "ASP.NET Core Web App" was created successfully.
This template contains technologies from parties other than Microsoft, see https://aka.ms/aspnetcore/3.1-third-party-notices for details.

Processing post-creation actions...
Running 'dotnet restore' on D:\lifftest\lifftest.csproj...
  Determining projects to restore...
  Restored D:\lifftest\lifftest.csproj (in 186 ms).

Restore succeeded.
```

### 接著執行底下指令， 安裝 liff 範本...
```bash
PS D:\lifftest> dotnet new --install isRock.Template.LIFF 
PS D:\lifftest> dotnet new liff
```
上面第一行指令是在本基上安裝liff範本特見，第二行指令則是為專案安裝範本。

安裝完成後我們建置(Build)一下，看結果如何:
```bash
PS D:\lifftest> dotnet build
Microsoft (R) Build Engine version 16.7.0+7fb82e5b2 for .NET
Copyright (C) Microsoft Corporation. All rights reserved.

  Determining projects to restore...
  All projects are up-to-date for restore.
  lifftest -> D:\lifftest\bin\Debug\netcoreapp3.1\lifftest.dll
  lifftest -> D:\lifftest\bin\Debug\netcoreapp3.1\lifftest.Views.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:06.12
```
如果可以順利建置，你應該會收到類似上面這樣的訊息。

### 透過底下指令，開啟 VS Code進入開發環境
請在命令列輸入 code (空格) .
```bash
PS D:\lifftest> code .
PS D:\lifftest>
```
完成後會看到類似底下畫面，請點選Startup.cs，將43行的程式碼註銷：
![enter image description here](https://i.imgur.com/8iU0Nr1.png)

```
注意：
註銷上面的43行，是關閉強制使用 https 的設定。由於我們待會要在用戶端進行測試，關閉 https 將有助於測試的便利，避免在用戶端被其他應用程式阻擋，或是 ngrok, postman 等軟體無法測試的狀況。但在正式環境，我們建議你還原此設定，將會讓安全性更加的嚴謹。
```
###  修改程式碼

接著在  Liff.cshtml 這支程式碼中，找到 "_____請換成你的LiffAppID_____"，並置換成你先前建立LIFF時保留的LiffAddID。

完成後類似底下這樣：
![enter image description here](https://i.imgur.com/05Z3b8E.png)

### 執行WebApp
請確定所有修改的程式碼都有儲存後，在 terminal(VS Code亦可)環境中執行該程式：
```dos
dotnet run
```
執行後結果應當如下：
![enter image description here](https://i.imgur.com/0O1zI3C.png)

上面這個畫面代表，你的WebApp已經在Localhost本機電腦上的5000與5001 port 被執行起來了，你可以嘗試用Chrome瀏覽器呼叫  http://localhost:5000/index 
![enter image description here](https://i.imgur.com/vjODN3s.png)

看到上面的畫面後，代表我們的應用程式已經可以在開發環境執行。

### 使用ngrok讓LIFF可以運行
請不要關閉應用程式的執行狀態。  
接著，將下載好的ngrok.zip壓縮檔解開，會看到其中有ngrok.exe執行檔，請將其放置在你覺得方便的資料夾下，並進入該資料夾，執行下列指令：
```dos
ngrok http 5000 -host-header="localhost:5000"
```
執行結果如下：
![enter image description here](https://i.imgur.com/0PXeu6J.png)
請注意上面的 https 位置，請將該位置複製，並加上liff，組出一個完整的endpoint :
```
https://814273058c22.ngrok.io/liff
```

### 設定LINE Bot後台
接著，進入 LINE Login管理後台，將上面得到的 endpoint 位置，設定在Endpoint URL中:
![enter image description here](https://i.imgur.com/9H0U5wb.png)

完成後， 即可抓取LIFF URL，讓手機點選該URL即可測試使用。

### 備註
```
若要中止此WebApp，可在VS Code的終端機中按下 Ctrl+C    
若要中止Ngrok，可在Ngrok執行所在的command line中按下 Ctrl+C
```

相關參考資料
---
電子書：[https://www.pubu.com.tw/ebook/103305](https://www.pubu.com.tw/ebook/103305)  
實體書：[https://www.tenlong.com.tw/products/9789865022662](https://www.tenlong.com.tw/products/9789865022662)  
線上課程：[https://www.udemy.com/line-bot/](https://www.udemy.com/line-bot/)  
LineBotSDK：[https://www.nuget.org/packages/LineBotSDK](https://www.nuget.org/packages/LineBotSDK)  

如果需要即時取得更多相關訊息，可按[這裡](https://www.facebook.com/DotNetWalker/)加入FB專頁。若這篇文章對您有所幫助，請幫我們分享出去，謝謝您的支持。

