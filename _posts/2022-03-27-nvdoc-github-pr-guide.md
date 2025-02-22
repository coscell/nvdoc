---
title: "使用 GitHub Pull Request 流程來發布文章"
date: 2022-03-27
categories:
  - 寫作
tags:
  - 寫作指南
---

摘要：本文是針對視障者使用 Git Pull Request 流程來將文章發布至  NVDOC 網站所撰寫的操作說明。

作者：蔡煥麟

## 前言

撰寫文章並發布至 nvdoc 網站的方法有兩種：

- 方法一：透過 GitHub 網站的 Pull Request 機制來提交變更。此方法由於牽涉儲存庫的合併請求流程，會需要事先了解 git 的相關操作。
- 方法二：在「[草稿儲存庫](https://github.com/visualaids/draft)」中編寫文章，並由相關人員校稿。待校稿完成後，通知 nvdoc 管理員，並由管理員來執行 Pull Request 流程與檔案的合併操作。此方法的程序比第一種方法簡單一些，但缺點是無法在 nvdoc 專案中自動累計文章作者與其校稿人員的貢獻度。

兩種方法各有優缺點，本文介紹的是第一種，也就是 GitHub 多人協作的標準做法。優點是可以了解目前業界廣泛採用的多人協同合作流程，而且所有參與文章寫作與校稿的人員，其貢獻度都會由 GitHub 自動統計。但如果你覺得此方法太過複雜，亦可參考另一篇文章：[文章的編寫與發布（簡易方法）](https://visualaids.github.io/nvdoc/%E5%AF%AB%E4%BD%9C/nvdoc-writing-posts/)。

## 簡介

這篇文章會說明七個操作，依序是：

1. [複製儲存庫到自己的帳號](#複製儲存庫到自己的帳號)
2. [把專案原始碼拉回本機電腦](#把專案原始碼拉回本機電腦)
3. [記住 GitHub 使用者帳號](#記住-github-使用者帳號)
4. [新增一篇文章](#新增一篇文章)
5. [將目前的修改保存至本機儲存庫](#將目前的修改保存至本機儲存庫)
6. [推送變更至 GitHub 主機](#推送變更至-github-主機)
7. [提交合併請求](#提交合併請求)

其中的第 1 步至第 3 步通常只需要做一次，往後的日常作業只需要執行第 4 至第 7 步。

文中會提到一些 git 指令以及相關術語，雖然我盡可能地解釋每一個 git 指令的用途，但絕對不可能涵蓋完整的 git 基礎知識。不足的部分，請上網搜尋相關資料。

在繼續進行之前，僅針對幾個重要術語來說明：

- Git：一種分散式版本控制程式，可用來保存檔案的修改紀錄，並提供版本控制的各種操作，例如分支、合併等等。
- GitHub：一個基於 Git 的免費版本控制平台（也有付費版本）。
- 儲存庫（repository）：經常簡稱為 repo，是一些相關檔案的集合，也就是把所有相關檔案放在一個儲存庫來管理的意思。一個 repo 通常代表一個專案的完整原始碼，所以我們有時也會說「專案原始碼」。

如果你想要快速了解本文總共使用了哪些 git 指令，可查看以下列表：

```
git clone https://github.com/你的GitHub帳號名稱/nvdoc.git
git config --global user.name "你的 GitHub 帳號名稱"
git config --global user.email "你的 GitHub 帳號 email"
git add .
git commit -m "新增文章"
git pull 
git push origin
```

備註：這裡假設你的作業環境是 Windows 10 或更新的版本。如果你的作業系統是 MacOS 或 Linux，git 指令應該是大同小異。

接下來，讓我們開始第一個步驟。

## 複製儲存庫到自己的帳號

把 GitHub 上面的某個儲存庫（repository）複製一份到自己的帳號，這個動作叫做 fork（分叉）。用白話來說，就是把某個專案的完整原始碼複製一份到你的 GitHub 帳號。這項操作沒有對應的指令，而必須在 GitHub 網頁上操作。步驟如下：

1. 開啟瀏覽器，[至 GitHub 登入你的帳號](https://github.com/login)。
2. 登入 GitHub 網站之後，透過瀏覽器的網址列進入 nvdoc 專案原始碼的首頁：<https://github.com/visualaids/nvdoc>。
3. 進入 nvdoc 專案的首頁之後，持續按 Tab 鍵來找到 Fork 按鈕。點擊 Fork 按鈕之後，稍等一下，整個 nvdoc 儲存庫就會複製一份到你的 GitHub 帳戶，而且頁面會自動切換至你的副本。你可以從網址列辨認目前所在的檔案庫是不是隸屬於你的帳戶：網址列裡面有出現你的帳號名稱，那就是了。

如此便完成 fork 操作了，此步驟通常只需要做一次。接下來，要把整個儲存庫的檔案從遠端 GitHub 主機拉回自己的電腦，以便進行相關的檔案操作（如新增、修改、刪除等等）。

## 把專案原始碼拉回本機電腦

現在要把 GitHub 主機上面的 nvdoc 完整原始碼拉回自己的電腦，以便修改其中的檔案。因此，你要選定一個本機電腦的工作目錄來存放這個專案的所有檔案。

以下操作步驟是以 `C:\work` 當作你日常的工作目錄。當此步驟順利完成，此工作目錄底下應該會有一個新建立的 nvdoc 子目錄。

首先，你必須知道掛在你的 GitHub 帳號底下的 nvdoc 儲存庫的位址。假設你的 GitHub 帳號的使用者名稱是 john，那麼你的 nvdoc 儲存庫的位址會是：`https://github.com/john/nvdoc.git`。

接著，便可以開啟命令列視窗，輸入以下命令，即可從你的 GitHub 帳戶取回 nvdoc 的完整原始碼：

```
cd c:\work
git clone https://github.com/john/nvdoc.git
```

**注意**：當你複製上面的指令時，請把其中的 "john" 替換成你自己的 GitHub 使用者帳號。

完成上述指令後，你可以開啟檔案總管來查看 `c:\work` 目錄底下是否新增了一個 nvdoc 子目錄。如果有，就表示儲存庫已經順利取出了。你可以先開啟該目錄下的 README.md 來查看其內容，裡面會有此專案的簡介。

取出專案原始碼的步驟通常只需要執行一次，便可反覆對本機的檔案進行修改，以及推送至遠端主機等日常操作。接著會說明幾個日常操作，包括：新增一篇文章（新增檔案）、將檔案保存至本機的版本儲存庫、以及把所有變更的檔案推送至 GitHub 主機。

**注意**：接下來的任何 git 指令，都必須在 `c:\work\nvdoc` 目錄下執行。

## 記住 GitHub 使用者帳號

進行任何寫入檔案的 git 操作時，都需要使用你的 GitHub 帳號，所以最好先把你的 GitHub 帳號保存在你的電腦上，以免經常需要輸入帳號。請執行以下兩個指令：

```
git config --global user.name "你的 GitHub 帳號名稱"
git config --global user.email "你的 GitHub 帳號 email"
```

**注意**：複製上述指令之後，請把雙引號裡面的帳戶資料改成你的 GitHub 使用者名稱以及 email。這裡的設定只跟帳號有關，稍後碰到驗證帳號的時候才會需要輸入密碼。

## 新增一篇文章

欲發布至 NVDOC 的文章，是存放在 nvdoc 工作目錄的 _posts 資料夾。因此，每當你要撰寫一篇新文章，請在這個 _posts 資料夾裡面建立一個新的 Markdown 檔案，檔案請按照此格式來命名：「yyyy-mm-dd-文章標題.md」。其中 yyyy 是四位數的西元年，mm 是兩位數的當前月份，dd 則是兩位數的日期。文章標題就是你打算撰寫的文章標題，中英文皆可，但請不要有空白字元。檔名中的文章標題如果需要隔開字詞，請一律使用減號，而不要使用空白字元。底下是兩個檔案名稱的範例：

- 2022-03-27-Windows-常用快速鍵.md
- 2022-03-27-Windows-shortcut-keys.md

檔名中的文章標題會成為日後發布於網站上的文章網址的一部份，所以如果能夠使用英文是最好的。中文網址雖然能夠正常運作，但是在轉貼連結的時候，網址當中的中文字元往往會經過編碼，不利閱讀。

檔案建立好之後，接下來便是輸入你的文章內容。文章內容是以 Markdown 語法撰寫，而且需要一個檔頭來描述文章資訊。這個部分的細節請參閱另一篇文章：[文章的編寫與發布（簡易方法）](https://visualaids.github.io/nvdoc/%E5%AF%AB%E4%BD%9C/nvdoc-writing-posts/)。

在賺寫文章的同時，你可以先執行以下指令，來將目前這個新增的檔案加入本機的版本儲存庫：

```
git add .
```

這個 `git add .` 指令會將目前工作目錄下所有新增的檔案加入版本控管，如果你在本機工作目錄增加了許多新檔案，但只有其中一個檔案想要加入版本控管，則可以明確指定檔案名稱，例如 `git add myfile.txt`。

將來，如果你只是修改既有檔案，就不再需要執行 `git add` 指令，因為它只是把尚未加入版控的檔案納入版控名單，如此而已。

## 將目前的修改保存至本機儲存庫

一旦你的工作進展到一個段落，覺得可以把目前的成果紀錄成一個版本，就可以使用 `git commit` 指令來將目前的修改成果保存至本機的版本儲存庫。範例：

```
git commit -m "新增文章"
```

其中的參數 `-m` 是用來指定這次修改的簡要說明，而這些簡要說明文字都會儲存在版本控制歷史，以便日後查閱。此範例的簡要說明文字是「新增文章」，你可以輸入任何文字，但不可以省略此參數。

這個 `git commit `指令會掃描目前工作目錄下所有已經加入版控的檔案，只要發現任何有修改的檔案，都會儲存在這次的版本紀錄。但請注意，它只會把變更儲存在本機硬碟。當你覺得工作完成的進度已經可以發布至遠端 GitHub 主機，便可參考下一節的說明來把所有變更推送至遠端 GitHub 主機。

## 推送變更至 GitHub 主機

在本機工作目錄下對檔案進行的任何修改，經過 `git commit` 保存之後，還必須透過提交變更的程序來將檔案推送至 GitHub 主機，這些修改的內容才會出現在 NVDOC 網站上。當你把文章寫好，確定要發布至網站時，可參考以下步驟來發布文章。

首先，開啟命令視窗，輸入以下指令：

```
git pull 
git push origin
```

其中的 `git pull` 會先從遠端主機拉回最新的版本。雖然這個指令不是必須的，但是在多人協作的情況下，你在本機修改的檔案很可能已經有人先你一步送交至遠端主機。因此，最保險的做法是先用 `git pull` 把遠端主機的最新版本拉回來，跟你的本機工作目錄的檔案合併，然後再執行 `git push origin` 指令來推送你的變更。

請注意，當你第一次執行 `git push` 指令時，Git 會要求你先登入的 GitHub 帳號，並彈出一個視窗，標題是「Connect to GitHub」，此時請在此視窗中點擊「Sign in with your browser」按鈕，以便開啟瀏覽器來驗證你的 GitHub 使用者帳戶。接著在網頁中輸入你的 GitHub 帳號和密碼，若成功登入，網頁會顯示「Authentication Succeed」，便可關閉瀏覽器，然後回到先前的命令視窗，此時你應該會看到 `git push` 指令執行過程所輸出的一些訊息，例如 "Writing objects: 100%"。

剛才那一段開啟瀏覽器來輸入帳號密碼的程序，一旦成功執行一遍，日後再次執行 `git push` 指令時，就會直接使用你剛才輸入的帳號密碼，不會再要求驗證身分。

備註：如果你的使用者帳戶沒有寫入遠端 GitHub 儲存庫的權限，會看到類似這樣的錯誤訊息："remote: Permission to visualadis/nvdoc.git denied"。

## 提交合併請求

前面六個步驟當中，第 1 步至第 3 步通常都只需要執行一次。第 4 步到第 6 步則是你日常反覆執行的工作。這幾個日常反覆執行的工作，一旦達到某種成果，例如解決了一個 bug 或完成一項功能，而你覺得可以正式發布時，就可以進行本節所介紹的操作：提交合併請求，也就是 Pull Request。

Pull Request 是一種通知與審核機制，簡稱 PR。當你想要將自己修改的部分貢獻給原作者，就可以發 PR 給原作者，請對方審閱你的變更。若原作者接受你的變更，就會執行合併操作，而你對該專案的貢獻也會自動被 GitHub 累計，你的名字也會出現在該專案的貢獻者名單中。

欲建立一個 Pull Request，請先開啟瀏覽器，[至 GitHub 網站](https://github.com/)登入你的帳戶。

登入 GitHub 之後，修改以下網址來直接開啟 Pull Request 的變更比較頁面：

<https://github.com/visualaids/nvdoc/compare/master...使用者帳號:master>

請務必把上列網址當中的「使用者帳號」替換成你的 GitHub 使用者帳號，才能順利進行後續操作。

進入 Pull Requests 的變更比較頁面之後，NVDA 會連續報讀許多文字，請耐心聽到最後，並注意最後的語音報讀是否為「Able to merge. These branches can be automatically merged.」有這串文字的話，就表示前述操作無誤，而且你的變更已經可以準備合併至 nvdoc 原始專案。接著找到 Create pull request 按鈕，點此按鈕之後，找到 Title 文字框，會聽到 NVDA 語音會報讀「Title 編輯區必要的無效的輸入」，在此欄位中輸入簡單的描述，例如：「請求合併」。

接著在頁面下方找到 Create pull request 按鈕，點此按鈕即可建立 PR。接下來，就是等待原專案的作者或管理員審閱你的變更請求，一旦完成合併，你的文章應該就會出現在 nvdoc 網站。

你也可以到 [nvdoc 原始專案的 Pull requests 頁面](https://github.com/visualaids/nvdoc/pulls)查看自己建立的 PR 是否還在等待合併。

## 結語

以上就是透過 GitHub Pull Request 機制來發布文章的作法。如果你覺得太過複雜，亦可參考另一篇文章：[文章的編寫與發布（簡易方法）](https://visualaids.github.io/nvdoc/%E5%AF%AB%E4%BD%9C/nvdoc-writing-posts/)。
