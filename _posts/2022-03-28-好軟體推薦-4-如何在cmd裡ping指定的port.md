---
title: "好軟體推薦-4 如何在cmd裡ping指定的port"
categories:
  - 好軟體推薦
tags: 
  - 軟體使用
date: 2022-03-28
---

作者:小羔羊

### 好軟體推薦-4 如何在cmd裡ping指定的port

這次要推薦一個在windows的cmd，也就是命令提示字元裡，可以ping指定port(又稱連接埠、端口)的小工具，
這個工具對有使用server架設一些服務的人非常有用，提供給需要但還不知道這個工具的朋友作參考，
當然，如果你是小白，也可以看看，用來測試網路問題也有些幫助喔。


### 什麼是ping

先簡單介紹一下什麼事ping，在以前我們要測試一個網站有沒有暢通，或是想測試本地到網站主機的來回延遲時間時，會用ping的指令，
你可以按win+r，之後輸入cmd按enter，就能打開cmd(也就是命令題是字元)的視窗，
而後在裡面輸入→
ping ip地址或網域名稱
比方說輸入→
ping google.com
你會在cmd的視窗裡看到像下面這樣的內容，NVDA可以用小鍵盤7、9看→

---

Ping google.com [172.217.160.110] (使用 32 位元組的資料):
回覆自 172.217.160.110: 位元組=32 時間=11ms TTL=116
回覆自 172.217.160.110: 位元組=32 時間=11ms TTL=116
回覆自 172.217.160.110: 位元組=32 時間=10ms TTL=116
回覆自 172.217.160.110: 位元組=32 時間=11ms TTL=116

172.217.160.110 的 Ping 統計資料:
    封包: 已傳送 = 4，已收到 = 4, 已遺失 = 0 (0% 遺失)，
大約的來回時間 (毫秒):
    最小值 = 10ms，最大值 = 11ms，平均 = 10ms


---


* 透過這個指令，你可以看到網域名稱所指向的ip地址→
172.217.160.110
這個地址就像大家所熟知的地址一樣，你可以透過這串數字來找到在萬里之外的主機，也可以用ip查詢主機的地理位置，
想查詢ip地理位置可以在瀏覽器搜尋(ip地理位置查詢)，有很多網站都有查詢ip地理位置的功能
* 以及網路是否穩定的參考依據→
封包: 已傳送 = 4，已收到 = 4, 已遺失 = 0 (0% 遺失)，
意思是傳送了4個封包，每個封包都有成功傳輸，並且途中沒有封包遺失，當網路不穩定時就有可能遺失封包，也被大家稱之為丟包
* 還有來回的延遲時間
    最小值 = 10ms，最大值 = 11ms，平均 = 10ms
這裡的值也很重要，拿個最實際的來舉例，如果你是測試遊戲的主機，那麼這裡的時間越長，玩家的體驗也就越卡鈍


### 為什麼我有時候使用ping，卻顯示要求等候逾時?

根據我所知，目前台灣的中華電信光世代網路，如果是使用中華電信的小烏龜來進行PPPOE撥接連線的話，從其他網路使用ping指令都會顯示要求等候逾時，
包括現在使用華碩品牌的router(路由器)撥接的網路，也是ping不通的，
這是因為現在很多用來連線的裝置都禁止被外網ping，或許是這種機制能提升安全性，
這就像我叫特種兵，特種兵卻不吭聲，我就無法聽音辨位他是否在我旁邊、離我多遠，是一樣的到裡，
但要注意的是，當顯示等候逾時的時候，不一定是對方禁止被ping，也有可能是對方網路不通，所以用普通的ping不在能準確的得知對方的主機是否有在運作。

### 可以ping指定port(連接埠、端口)的工具

那麼，接下來要介紹的這個工具就可以解決上述提到的問題，我先把過程拆成幾塊來描述→

* 域名跟ip的關係
我們在瀏覽器上打的網址，像google.com，這個就被稱之為域名，說起來域名的由來只是為了讓我們人腦好記憶而生，
其實在我們打完按下enter後，瀏覽器會將這串網指送到名為DNS的電腦上，詢問這串網址對應的是哪個ip，
之後才會透過一串數字，例如ipv4的話會像xxx.xxx.xx.xx，之後找到地址所指向的電腦來連線，
通俗點來說這串數字就像我們生活中的實際地址一樣
* 防火牆控制的連接埠
我們系統的防火牆是為了阻擋駭客入侵而存在，但為了能上網，跟其他電腦連線傳遞資料，因此需要有個跟外面溝通的入口，這個入口稱之為port(端口或連接埠)
每個系統上會有65535個port，這些port就像門一樣，要開了別人才能連到你的電腦拿資料
* 監聽連接埠
在打開門之後，還須要有軟體在門後面接收、傳遞資料，這就像開旅店的要有個前台小妹，接待前來的客人，並引導客人到旅店的某些位置，
如果開了門卻沒有小妹在門後接待，那這個門也是不起作用的喔
* 普通的ping
普通的ping就像我們透過地址找到一家商店，透過觀察商店是否有開燈，來判斷商店是否營業，
但現在很多商店故意不開燈，或拉上了窗簾，讓你以為他沒開，或根本沒這家店，
透過這種方式隱藏自己，讓不法分子無法快速判斷商店是否有營業，偷取裡面的資料
* 指定ping的port
雖然商店透過關燈的方式隱藏自己，但對於正常的客人來說並沒有影響，
例如我們用瀏覽器打開一個網站，而網站普遍都是使用80或443的port，瀏覽器自動帶我們走像開著的門，也就是80或443號門，因此一樣能正常連線，
在這種情況下，我們就可以透過工具，指定要走80或443號門到某個地址，到了門前就能有錢台小妹接待我們，我們就能用ping指令問問我們跟前台小妹有多少的延遲，是不是能接收到我們送去的封包等等，
如此，也就完成了與主機溝通的流程了，之後只要知道對方的電腦開了哪個port，且有軟體在背後監聽，都能用這種方式ping的到啦!



#### 下載


* 在網頁中找到tcping.exe點enter即可下載→
[點我前往可以ping指定port的工具首頁](https://www.elifulkerson.com/projects/tcping.php)
* 從我的服務器下載→
[點我從小羔羊檔案服務器下載tcping.exe](https://file.lamb.tw/f/df3cee477984448c94a6/?dl=1)


#### 使用方法


1. 你可以將下載的tcping.exe放到以下路徑→
C:\Windows\System32
但沒有必須放到這裡，放到這裡只是方便未來打開cmd就能直接使用
1. 之後按win+r，輸入cmd，按enter，在打開的cmd視窗內輸入
tcping ip地址或網域名稱 port(連接埠、端口)
例如→
tcping a.lamb.tw 80
上面這串指令就是ping a.lamb.tw這個域名的80port，80port是普遍網站所使用的連接埠
1. 這樣以後只要打開cmd，輸入一串簡單的指令就能ping指定的port囉~



### 補充

上面下載的tcping，你可以改為自己喜歡的名字，但不能跟其他軟體有衝突，例如不能改為ping，
像我就改成pping.exe，輸入指令時只要打
pping ip port
這樣就能方便一點了，
另外ping還有幾個其他的用法，例如→

1. -t 不斷的ping，直到我叫停
-t這個參數可以讓電腦不斷地執行ping的動作，值到我們按ctrl+c來停止，
如果使用普通的ping指令，會像這樣→
ping a.lamb.tw -t
使用tcping.exe來測指定port的話會像這樣→
tcping -t a.lamb.tw 80
1. -n 指定要ping多少次
下面以ping10次為例
普通的ping→
ping a.lamb.tw -n 10
使用tcping.exe的話→
tcping -n 10 a.lamb.tw 80
1. -b 發出音效
可以讓ping時發出聲音，下面是四種模式
"-b 1"将发出"向下"的蜂鸣音。 如果主机已启动，但现在未启动，请发出哔哔声。
"-b 2"将发出"向上"的蜂鸣音。 如果主机已关闭，但现在已启动，请发出哔哔声。
"-b 3"将发出"更改时"的蜂鸣音。 如果主机是单向的，但现在是另一种方式，请发出哔哔声。
"-b 4"将"始终"发出蜂鸣音。
這個功能應該只有tcping可以使用，用起來會像這樣→
tcping -b 4 a.lamb.tw 80
1. 最後比較會用到的還有下面三個，使用方法跟上面一樣，將參數寫在tcping後面
-j 測試抖動，應該也可以被用來測試本地到server的穩定性
-4 以ipv4協定做測試，也就是四串數字的ip位置
-6 以ipv6做測試，也就是長的像一串英文+數字的亂碼，也是ip位置


---


最後還有一些進階的用法，可以參考tcping網站上的說明，我們下次見!



---

如果想閱讀我的更多原創文章，可以到我的網站→[小羔羊分享站](https://lamb.tw/)
