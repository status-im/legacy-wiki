# 安裝:

  - 安裝最新版本 (3.6+) [Python](https://www.python.org/downloads/)
  - 安裝 [Git](https://git-scm.com/download/mac). 使用SSH
    status-react克隆或下載（選擇本地目錄）
    [repository](https://github.com/status-im/status-react) , 使用 `git
    clone git@github.com:status-im/status-react.git`在Terminal 或命令列裡.
    SSH密鑰是必需的，所以如果你沒有用 - [add
    it](https://help.github.com/articles/connecting-to-github-with-ssh/).

<!-- end list -->

    git clone git@github.com:status-im/status-react.git

*提示*: 如果您需要 **更新**您的克隆存儲庫, 請使用:

`git pull git@github.com:status-im/status-react.git`

或者這件事對我更好。進入你的status-im本地目錄，運行 \`git fetch -all\` and \`git reset --hard
origin/develop\`

  - 到 /status-react/test/appium 和執行 `pip3 install -r requirements.txt
    (sudo might be required)`
  - 安裝 Genymotion: <https://www.genymotion.com>
  - 在Genymotion中添加兩個相同的設備 (`Google Nexus 5 - 6.0.0 - API23 - 1080x1920`)
  - 安裝 Appium 'npm install -g appium'
  - 使用
    [this](https://github.com/appium/appium-doctor/blob/master/README.md)
    在啟動Appium之前診斷和修復常見的Node，iOS和Android配置問題

## 本地運行:

  - 啟動 Appium (只需在終端輸入appium，然後按enter即可)
  - 啟動兩個虛擬設備並等待它們啟動
  - 到 `/status-react/test/appium/tests` 並運行： `python3 -m pytest -m all
    -v --env=local --apk=/`<path to apk>`/apk_name.apk`
  - 以 `-m all`運行 開始所有測試，如果你想開始一個特定的測試組，你可以使用 `-m transaction` 或 `-m
    chat` 代替 `-m all`. 添加更多測試後將添加其他組

## 使用PyCharm運行

  - 安裝 [PyCharm
    Community](https://www.jetbrains.com/pycharm/download/#section=mac)
  - **打開**（不要創建新的！）狀態在PyCharm中react項目
  - 設定 Appium 資料夾 `As Sources Root.` 導航到Appium文件夾 -\> 右鍵單擊 -\> [Mark
    Directory as Sources root](http://prntscr.com/ht1gou)
  - 配置項目解釋器：到 [PyCharm -\> Preferences](http://prntscr.com/ht1j2g)-\>
    `cdm+` -\> 將搜索條件添加到搜索欄中 -\> [select Project Interpretator -\> Select
    Python 3.6](http://prntscr.com/ht1k2k)

''*'注意*: 你的解釋器(Interpreter )列表中可能沒有看到Python 3.6。 ''' 別擔心！保持在同一屏幕上單擊
[Show all](http://prntscr.com/hsvd55) -\> 點擊 `+` 然後在下一個屏幕上添加`add local`
-\> [System Interpreter -\> Select
Python 3.6](http://prntscr.com/hsvnhf)

  - 配置Python集成工具：到 [PyCharm -\>
    Preferences](http://prntscr.com/ht1j2g)-\> cdm+ -\> 添加集成工具以在搜索欄中搜索條件
    -\> [select Python Integrated tools -\> Select
    py.test](http://prntscr.com/ht1lks)
  - 到Appium目錄並找到pytest.ini文件。添加下面一行（它已經存在，你需要的是**添加你自己的路徑到apk**）,
    例如我在測試文件夾中有`1.apk`:

`addopts = -s -v --junitxml=result.xml --tb=line --env=local
--apk=/Users/in_no_cent/Documents/Status/status-react/test/appium/tests/1.apk
--log=True`

  - 啟動Appium，在Genymotion中運行2個設備，然後選擇您喜歡的任何測試 - \>右鍵單擊 - \>運行（測試以
    [test_](http://take.ms/TqBI7))
  - Enjoy :)

[Category:Status Application](Category:Status_Application "wikilink")
[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")