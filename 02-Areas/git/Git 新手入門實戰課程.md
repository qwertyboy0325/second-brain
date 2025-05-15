

嗨！歡迎來到 Git 的世界！Git 是一個強大的工具，可以幫助你管理專案的版本、與他人協作，甚至回到過去的任何一個時間點。別擔心，我們會一步一步帶你入門。

## 課前準備：為什麼要學 Git？

想像一下：
*   你正在寫一份報告，存了 `報告_v1.docx`, `報告_v2.docx`, `報告_最終版.docx`, `報告_真的最終版.docx`... 是不是很混亂？
*   你改了一段程式碼，結果整個壞掉了，想回到修改前的樣子卻找不回來？
*   你想和朋友一起開發一個小專案，但不知道怎麼同步彼此的進度？

Git 就是來解決這些問題的！它就像：
*   **程式碼的時光機**：記錄每一次修改，隨時可以回到過去。
*   **團隊合作的神器**：讓多人可以同時在同一個專案上工作，而不會互相覆蓋。
*   **安全的備份機制**：將你的心血備份到遠端。

---

## 階段一：安裝與基本設定

### 1. 安裝 Git
首先，我們需要在你的電腦上安裝 Git。

*   **Windows**: 前往 [https://git-scm.com/download/win](https://git-scm.com/download/win) 下載並安裝。安裝時，建議勾選 "Git Bash Here" (方便後續操作)。
*   **macOS**: 通常內建，或透過 Homebrew (`brew install git`) 安裝。
*   **Linux**: 使用你的套件管理器安裝 (例如 `sudo apt install git` 或 `sudo yum install git`)。

安裝完成後，打開你的終端機 (Terminal) 或 Git Bash (Windows 用戶如果在安裝時有勾選，可以在資料夾中按右鍵選擇 "Git Bash Here")。

輸入以下指令檢查是否安裝成功，如果成功會顯示 Git 的版本號：
```bash
git --version
```
### 2. 初次設定 (非常重要！)

設定你的使用者名稱和 Email，這些資訊會出現在你每一次的「存檔紀錄」中。
```bash
git config --global user.name "你的英文名字或暱稱" 
git config --global user.email "你的Email"
```
將 "你的英文名字或暱稱" 和 "你的Email" 替換成你自己的資訊。

你可以用以下指令檢查設定是否成功：
```bash
git config --list
```
你會看到剛剛設定的 user.name 和 user.email。

---

## 階段二：你的第一個本地倉庫

Git 的核心是「倉庫」(Repository)，你可以把它想像成一個專案的「保險箱」，裡面存放了專案的所有版本歷史。

### 1. 建立倉庫 (Repository)

讓我們來建立第一個 Git 倉庫：

1. 在你的電腦上建立一個新的資料夾，例如取名為 my-first-git-project。
    
2. 打開終端機/Git Bash，並**進入**這個資料夾：
	```
	cd path/to/my-first-git-project
	```
	將 path/to/ 替換成你實際的路徑。
3. **初始化 Git 倉庫**：
```bash
git init
```
這個指令會在 my-first-git-project 資料夾中建立一個隱藏的 .git 子資料夾。這個 .git 資料夾就是 Git 用來追蹤所有版本歷史的地方，**千萬不要手動修改或刪除它**！
### 2. Git 的三大區域與核心流程

理解 Git 的工作流程非常重要：

1. **工作目錄 (Working Directory)**：就是你目前看到的專案資料夾，你實際修改檔案的地方。
    
2. **暫存區 (Staging Area / Index)**：一個「待辦清單」或「購物車」，準備要被「存檔」(Commit) 的檔案會先放到這裡。
    
3. **倉庫 (Repository / .git directory)**：永久儲存你所有版本的地方，也就是 .git 資料夾裡面的內容。
    

**核心流程**：  
工作目錄 --(執行 git add)--> 暫存區 --(執行 git commit)--> 倉庫

### 3. 實戰演練：第一個 Commit (提交/存檔)

1. 在 my-first-git-project 資料夾裡建立一個新的文字檔，例如 hello.txt。
    
    - 你可以用文字編輯器 (如 VS Code, Sublime Text, 記事本) 建立。
        
    - 在 hello.txt 裡面寫一些內容，例如："Hello Git!"
        
2. **查看狀態 (Status)**：
	```bash
	git status
	```
你會看到 hello.txt 出現在 "Untracked files" (未追蹤的檔案) 列表裡。這表示 Git 知道這個檔案存在，但還沒有開始追蹤它的變化。
3. **加入暫存區 (Add)**：
```bash
	git add hello.txt
```
這個指令告訴 Git：「我要開始追蹤 hello.txt 這個檔案，並且準備把它存檔。」

- 小技巧：如果想加入所有變動的檔案，可以使用 git add . (點代表當前目錄所有檔案)。
    

	再次執行 git status，你會看到 hello.txt 變成 "Changes to be committed" (準備提交的變更)。
4. **提交到倉庫 (Commit)**：
```bash
git commit -m "新增 hello.txt 並加入問候語"
```
- -m 後面接的是這次提交的「訊息」(message)。**好的提交訊息非常重要！** 它應該簡潔明瞭地描述這次提交做了什麼。
    
- 這個指令會把暫存區裡的所有內容正式存入倉庫，形成一個新的版本。
    

再次執行 git status，你會看到 "nothing to commit, working tree clean"，表示所有變更都已經存檔了。
恭喜！你完成了第一個 Git Commit！

### 4. 修改檔案並再次提交

1. 打開 hello.txt，修改它的內容，例如改成 "Hello Git! Welcome to version control." 然後儲存。
    
2. 執行 git status。你會看到 hello.txt 顯示為 "modified" (已修改)。
    
3. 像之前一樣，把修改加入暫存區並提交：
```bash
git add hello.txt
git commit -m "更新 hello.txt 的問候語"
```

### 5. 查看提交歷史 (Log)

想看看我們都做了哪些「存檔」嗎？
```bash
git log
```

你會看到剛剛的兩次提交紀錄，包含：

- 一長串的 commit hash (每個版本的唯一ID)
- 作者 (Author)
- 日期 (Date)
- 提交訊息

如果想看更簡潔的日誌：
```bash
git log --oneline
```
### 6. 忽略不需要版本控制的檔案 (.gitignore)

有些檔案我們不希望被 Git 追蹤，例如：

- 編譯產生的檔案 (如 .exe, .o)
    
- 日誌檔 (.log)
    
- 包含敏感資訊的設定檔 (如密碼檔)
    
- 作業系統自動產生的檔案 (如 macOS 的 .DS_Store)
    

這時候就需要 .gitignore 檔案：

1. 在你的專案根目錄 (例如 my-first-git-project 裡面) 建立一個名為 .gitignore 的檔案 (注意，前面有個點)。
    
2. 打開 .gitignore，每一行寫一個你想要忽略的檔案或資料夾名稱/模式。例如：
```.gitignore
# 忽略所有 .log 結尾的檔案
*.log

# 忽略 secret.txt 檔案
secret.txt

# 忽略 node_modules 資料夾及其底下所有內容
node_modules/

# 忽略 macOS 的 .DS_Store 檔案
.DS_Store
```
3. 儲存 .gitignore 檔案。
    
4. .gitignore 本身也需要被 Git 管理，所以把它加入暫存區並提交：
```bash
git add .gitignore
git commit -m "新增 .gitignore 檔案，忽略特定檔案"
```
之後，被列在 .gitignore 裡的檔案就不會出現在 git status 的未追蹤列表裡了。