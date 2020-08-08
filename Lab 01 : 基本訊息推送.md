<h1 id="line訊息推送push-message">LINE訊息推送(Push Message)</h1>
<h2 id="overview">Overview</h2>
<p>這個lab介紹如何透過 C# 已程式碼發送各種LINE基本訊息，包含貼圖、文字、圖片、聲音、影片、GPS 座標位置(Location)</p>
<h2 id="prerequisites">Prerequisites</h2>
<ol start="0">
<li>建立好LINE Bot帳號，並取得Channel Access Token與UserId</li>
<li>下載安裝 .net core sdk 3.1 以上版本 <a href="https://dotnet.microsoft.com/download">here</a></li>
<li>安裝 Visual Studio Code 開發工具 <a href="https://code.visualstudio.com/download">here</a></li>
<li>建立 .net core console 專案，在專案中引用 nuget 上的 LineBotSDK 套件。</li>
</ol>
<h2 id="steps">Steps</h2>
<ol>
<li>建立 .net core console專案<br>
在命令列模式建立資料夾，接著透過 dotnet new 指令建立專案</li>
</ol>
<pre class=" language-bash"><code class="prism  language-bash">PS D:\<span class="token operator">&gt;</span> md linetest
PS D:\<span class="token operator">&gt;</span> <span class="token function">cd</span> linetest
PS D:\linetest<span class="token operator">&gt;</span> dotnet new console
</code></pre>
<p>系統會出現類似底下畫面…</p>
<pre><code>The template "Console Application" was created successfully.

Processing post-creation actions...
Running 'dotnet restore' on D:\linetest\linetest.csproj...
  正在判斷要還原的專案...
  已還原 D:\linetest\linetest.csproj (251 ms 內)。
Restore succeeded.
</code></pre>
<p>2.接著執行底下指令， 安裝 LineBotSDK 套件…</p>
<pre><code>PS D:\linetest&gt; dotnet add package linebotsdk
</code></pre>
<p>系統會出現類似底下畫面…</p>
<pre><code>PS D:\linetest&gt; dotnet add package linebotsdk
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
</code></pre>
<p>安裝完成後我們順便建置(Build)一下，看結果如何:</p>
<pre><code>PS D:\linetest&gt; dotnet build
Microsoft (R) Build Engine for .NET Core 16.6.0+5ff7b0c9e 版
Copyright (C) Microsoft Corporation. 著作權所有，並保留一切權利。

  正在判斷要還原的專案...
  已還原 D:\linetest\linetest.csproj (712 ms 內)。
  linetest -&gt; D:\linetest\bin\Debug\netcoreapp3.1\linetest.dll

建置成功。
    0 個警告
    0 個錯誤

經過時間 00:00:05.39
</code></pre>
<p>如果可以順利建置，你應該會收到類似上面這樣的訊息。</p>
<ol start="3">
<li>透過底下指令，開啟 VS Code進入開發環境<br>
請在命令列輸入 code (空格) .</li>
</ol>
<pre><code>PS D:\linetest&gt; code .
PS D:\linetest&gt;
</code></pre>
<p><img src="https://i.imgur.com/QQBSJWL.png" alt="enter image description here"><br>
完成後會看到類似上面的畫面。</p>
<ol start="4">
<li>鍵入訊息發送程式碼<br>
將上圖中的第9行程式註銷，換成底下程式碼(請注意ChannelAccessToken和UserId 須換為你自己申請的LINE Bot相關資訊)：</li>
</ol>
<pre class=" language-csharp"><code class="prism  language-csharp">            <span class="token comment">//Console.WriteLine("Hello World!");</span>
            <span class="token keyword">var</span> ChannelAccessToken <span class="token operator">=</span> <span class="token string">"_____replace_with_your_channel_access_token_____"</span><span class="token punctuation">;</span>
            <span class="token keyword">var</span> UserId <span class="token operator">=</span> <span class="token string">"_____replace_with_your_userId_____"</span><span class="token punctuation">;</span>
            <span class="token keyword">var</span> bot <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">isRock<span class="token punctuation">.</span>LineBot<span class="token punctuation">.</span>Bot</span><span class="token punctuation">(</span>ChannelAccessToken<span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push text</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> <span class="token string">"Hello World"</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push sticker</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> <span class="token number">1</span><span class="token punctuation">,</span> <span class="token number">3</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push image</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> <span class="token keyword">new</span> <span class="token class-name">Uri</span><span class="token punctuation">(</span><span class="token string">"https://i.imgur.com/OQx8aid.png"</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push Audio</span>
            <span class="token keyword">var</span> AudioMsg <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">isRock<span class="token punctuation">.</span>LineBot<span class="token punctuation">.</span>AudioMessage</span><span class="token punctuation">(</span>
            <span class="token keyword">new</span> <span class="token class-name">Uri</span><span class="token punctuation">(</span><span class="token string">"https://arock.blob.core.windows.net/blogdata202008/test.mp3"</span><span class="token punctuation">)</span><span class="token punctuation">,</span> <span class="token number">6000</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> AudioMsg<span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push Video            </span>
            <span class="token keyword">var</span> VideoMsg <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">isRock<span class="token punctuation">.</span>LineBot<span class="token punctuation">.</span>VideoMessage</span><span class="token punctuation">(</span>
            <span class="token keyword">new</span> <span class="token class-name">Uri</span><span class="token punctuation">(</span><span class="token string">"https://arock.blob.core.windows.net/blogdata202008/POC.mp4"</span><span class="token punctuation">)</span><span class="token punctuation">,</span>
            <span class="token keyword">new</span> <span class="token class-name">Uri</span><span class="token punctuation">(</span><span class="token string">"https://imgur.com/LPQhfXR.png"</span><span class="token punctuation">)</span>
            <span class="token punctuation">)</span><span class="token punctuation">;</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> VideoMsg<span class="token punctuation">)</span><span class="token punctuation">;</span>
            <span class="token comment">//push location</span>
            <span class="token keyword">var</span> LocationMsg <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">isRock<span class="token punctuation">.</span>LineBot<span class="token punctuation">.</span>LocationMessage</span><span class="token punctuation">(</span>
            <span class="token string">"大安森林公園"</span><span class="token punctuation">,</span> <span class="token string">"台北市大安區新生南路二段1號"</span><span class="token punctuation">,</span> <span class="token number">25.030000</span><span class="token punctuation">,</span> <span class="token number">121.535833</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
            bot<span class="token punctuation">.</span><span class="token function">PushMessage</span><span class="token punctuation">(</span>UserId<span class="token punctuation">,</span> LocationMsg<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<ol start="5">
<li>在VS Code以hot key(C+S+`)切換至terminal：<br>
<img src="https://i.imgur.com/24odB1m.png" alt="enter image description here"></li>
<li>鍵入 dotnet run 執行程式</li>
</ol>
<pre class=" language-dos"><code class="prism  language-dos">PS D:\linetest&gt; dotnet run
</code></pre>
<ol start="7">
<li>檢視結果<br>
<img src="https://i.imgur.com/DRRmTcM.png" alt="enter image description here"></li>
</ol>

