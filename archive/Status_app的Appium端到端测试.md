# 安装:

  - 安装最新版本 (3.6+) [Python](https://www.python.org/downloads/)
  - 安装 [Git](https://git-scm.com/download/mac). 使用SSH
    status-react克隆或下载（选择本地目录）
    [repository](https://github.com/status-im/status-react) , 使用 `git
    clone git@github.com:status-im/status-react.git`在Terminal 或命令列裡.
    SSH密钥是必需的，所以如果你没有用 - [add
    it](https://help.github.com/articles/connecting-to-github-with-ssh/).

<!-- end list -->

    git clone git@github.com:status-im/status-react.git

*提示*: 如果您需要 **更新**您的克隆存储库, 请使用:

`git pull git@github.com:status-im/status-react.git`

或者这件事对我更好。进入你的status-im本地目录，运行 \`git fetch -all\` and \`git reset --hard
origin/develop\`

  - 到 /status-react/test/appium 和执行 `pip3 install -r requirements.txt
    (sudo might be required)`
  - 安装 Genymotion: <https://www.genymotion.com>
  - 在Genymotion中添加两个相同的设备 (`Google Nexus 5 - 6.0.0 - API23 - 1080x1920`)
  - 安装 Appium 'npm install -g appium'
  - 使用
    [this](https://github.com/appium/appium-doctor/blob/master/README.md)
    在启动Appium之前诊断和修复常见的Node，iOS和Android配置问题

## 本地运行:

  - 启动 Appium (只需在终端输入appium，然后按enter即可)
  - 启动两个虚拟设备并等待它们启动
  - 到 `/status-react/test/appium/tests` 并运行： `python3 -m pytest -m all
    -v --env=local --apk=/`<path to apk>`/apk_name.apk`
  - 以 `-m all`运行 开始所有测试，如果你想开始一个特定的测试组，你可以使用 `-m transaction` 或 `-m
    chat` 代替 `-m all`. 添加更多测试后将添加其他组

## 使用PyCharm运行

  - 安装 [PyCharm
    Community](https://www.jetbrains.com/pycharm/download/#section=mac)
  - **打开**（不要创建新的！）状态在PyCharm中react项目
  - 设定 Appium 资料夹 `As Sources Root.` 导航到Appium文件夹 -\> 右键单击 -\> [Mark
    Directory as Sources root](http://prntscr.com/ht1gou)
  - 配置项目解释器：到 [PyCharm -\> Preferences](http://prntscr.com/ht1j2g)-\>
    `cdm+` -\> 将搜索条件添加到搜索栏中 -\> [select Project Interpretator -\> Select
    Python 3.6](http://prntscr.com/ht1k2k)

''*'注意*: 你的解释器(Interpreter )列表中可能没有看到Python 3.6。 ''' 别担心！保持在同一屏幕上单击
[Show all](http://prntscr.com/hsvd55) -\> 点击 `+` 然后在下一个屏幕上添加`add local`
-\> [System Interpreter -\> Select
Python 3.6](http://prntscr.com/hsvnhf)

  - 配置Python集成工具：到 [PyCharm -\>
    Preferences](http://prntscr.com/ht1j2g)-\> cdm+ -\> 添加集成工具以在搜索栏中搜索条件
    -\> [select Python Integrated tools -\> Select
    py.test](http://prntscr.com/ht1lks)
  - 到Appium目录并找到pytest.ini文件。添加下面一行（它已经存在，你需要的是**添加你自己的路径到apk**）,
    例如我在测试文件夹中有`1.apk`:

`addopts = -s -v --junitxml=result.xml --tb=line --env=local
--apk=/Users/in_no_cent/Documents/Status/status-react/test/appium/tests/1.apk
--log=True`

  - 启动Appium，在Genymotion中运行2个设备，然后选择您喜欢的任何测试 - \>右键单击 - \>运行（测试以
    [test_](http://take.ms/TqBI7))
  - Enjoy :)

[Category:Status Application](Category:Status_Application "wikilink")
[Category:Developer
Documentation](Category:Developer_Documentation "wikilink")