---
title: "Http 協議"
date: 2021-10-23T10:07:47+06:00
draft: false


# meta description
description: "Http 請求及回應"

# taxonomies
categories:
  - "Http"
tags:
  - "Http"


# post type
type: "featured"
---

<hr>

### 資源 (Resource)

當我們想要查看某篇文章，例如我的筆記中 **Spring 中控制反轉及依賴注入** 這篇文章時，我們會在瀏覽器中輸入網址 http://jimchien666.github.io/blog/post1，當瀏覽器回傳這個頁面時，我們就可以觀看這篇文章，這個回傳結果 (文章)，就是資源 (Resource)。

### URL
從前面知道我們輸入網址 http://jimchien666.github.io/blog/post1 可以得到資源，我們稱這個網址為 URL (uniform resource locator)，他定位了網路上的某個特定資源，也就是這篇[文章](http://jimchien666.github.io/blog/post1)，而資源不僅包含文章，圖像、影片、文件等等都是。不同的路徑，不同的 Url 就搭配不同的資源。

而對於 URL，我們可以把他分成三個部分：
1. http，也就是 :// 之前的部分就是我們所說的URL scheme。說明他用什麼方式取得資源，這邊是用 http 方式取得資源。

2. jimchien666.github.io 就是主機(server)，我們可以從 DNS 系統查找到該域名對應的 IP，再利用 IP 去找到伺服器。

3. /blog/post1 是URL 路徑。伺服器透過這個路徑，回應特定的資源。

### 資源與其類型
剛剛說到，資源可以有不同檔案類型，如 Html文件、圖像、影片、文件等等，而這些資源瀏覽器要用相對應的方式去顯示，所以這時候伺服器必須告訴我們傳過來的資源類型(content type)，好讓瀏覽器能夠正確顯示他。這邊 Tttp 就使用了多用途網際網路郵件擴展 (Multipurpose Internet Mail Extensions，簡稱：MIME) 來指定資源類型。因此在資源回傳為 Html 文件時，伺服器回傳資源類型為 text/html，而瀏覽器就會將他視為 Html 文件並將他正確顯示。

我們了解到 URL 與 資源之間的關係，也知道了 URL 是如何組成的，也知道當瀏覽器拿到資源後如何將資源正確的顯示，而在 URL 的組成中有提到，最開頭取得資源的方式是利用 Http 取得，實際上是如何運作及規範是什麼呢，我們得繼續去了解。

### 請求 (Request) 和回應 (Response)
假設想知道時間，你去找個路人問"請問現在幾點"，你希望路人能夠回應你，首先你必須確認路人聽得懂你說話，再來他必須有手錶或任何取得時間的方法。

我們回到 HTTP 的工作模式，HTTP 是一種請求 (Request) 和回應 (Response) 的協議，瀏覽器利用伺服器能夠理解的格式向伺服器送出一個請求，伺服器接收請求並予以回應，請求跟回應是成對的出現在一個 HTTP 工作之中的消息。而 HTTP 協定訂定了請求跟回應之間消息的內容，以便每個利用 HTTP 方式進行溝通時能夠理解並給予相對應資源。那麼我們必須得理解請求及回應各自的規範。

### HTTP 請求
分成三個部分

1. 方法
每個 HTTP 請求都會有一個請求方法，該方法描述你想對請求資源做什麼，例如用 GET 方法代表想要取得該資源，以下是常見的 HTTP 方法。

| 方法        | 描述           |
| ------------- |--------------|
|GET	|取得資源|
|PUT	|儲存資源|
|DELETE	|刪除資源|
|POST	|新增資源|
|HEAD	|取得資源 Header|

在這 5 種方法中，只有 2 種是最常見的：GET 和 POST。當 Web 瀏覽器想要取得頁面、圖像、影片或文件等資源時，它會發出 GET 請求。GET 請求是最常見的請求類型。

POST 請求通常由網頁上的 <form> 生成
如下所示：
```
<form action="/account/create" method="POST">
    First name:
    <input id="firstName" name="firstName" type="text" />
    Last name:
    <input id="lastName" name="lastName" type="text" />
    <input type="submit" value="submit"/>
<form>
```
當使用者點擊 submit 按鈕時，瀏覽器會送出表單。表單告訴瀏覽器使用 POST 請求，POST 的路徑是 /account/create。瀏覽器發出的實際 HTTP 請求如下所示：
```
POST http://localhost:8080/account/create HTTP/1.1
主機：localhost:8080

firstName=Jim&lastName=Chien
```

2. Header
在 HTTP 1.1 中，host 是必要的 HTTP Header，其他大部分是遠則要不要帶，Header 可以幫助伺服器處理請求的有用訊息，常見 Header 如下。

| Header        | Description           |
| ------------- |--------------|
|Referer|	When the user clicks on a link, the client can send the URL of the referring page in this header.|
|User-Agent|	Information about the user agent (the software) making the request. Many applications use the information in this header, when present, to figure out what browser is making the request (IE 6 versus IE 9 versus Chrome, etc).|
|Accept|	Describes the media types (remember MIME types from Part I?) the user-agent is willing to accept.|
|Accept-Language|	Describes the languages the user-agent prefers.|
|Cookie|	Contains cookie information, which we will look at in a later article. Cookie information generally helps a server track or identify a user.|
|If-Modified-Since|	Will contain a date of when the user-agent last retrieved (and cached) the resource. The server only has to send back the entire resource if it's been modified since that time.|

3. URL 請求路徑


### HTTP 回應

回應會有：
1. 狀態碼 (Status Code):

|Range|	Category|
| ------------- |--------------|
|100-199|	Informational|
|200-299|	Successful|
|300-309|	Redirection|
|400-499|	Client Error|
|500-599|	Server Error|

2. Header
有我們前面所說的資源類型(Content Type)。

3. 回應內容

### 小結
至此，我稍微整理了我學習到的 HTTP 相關的規範，了解如何請求及回應，這兩個東西會成對出現，確保有來有往，請求中我們需要 URL 及 Method。回應中，我們有狀態碼，及回應內容。

以下是這次分享的參考資料，蠻推薦看看的：
- https://odetocode.com/Articles/741.aspx

- https://odetocode.com/Articles/742.aspx