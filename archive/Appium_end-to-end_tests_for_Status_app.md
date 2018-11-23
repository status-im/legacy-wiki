**Installation:**

  - Install latest (3.6+) Python <https://www.python.org/downloads/>
  - Go to /status-react/test/appium and run
    ```
     pip3 install -r requirements.txt (sudo might be required)
    ```
  - Install Genymotion <https://www.genymotion.com>
  - In Genymotion add two identical devices (Google Nexus 5 - 6.0.0 -
    API23 - 1080x1920)
  - Install Appium <http://appium.io/slate/en/master/?python#setup>
  - Complete only first and second step from "Additional setup for
    android app testing"
    <http://appium.io/slate/en/master/?python#additional-setup-for-android-app-testing>

**Local Running:**

  - Start Appium (just type appium in terminal and press enter)
  - Start two virtual devices and wait for them to boot
  - Go to /status-react/test/appium/tests and
    run:
    ```
     python3 -m pytest -m all -v --env=local --apk=/<path to apk>/apk_name.apk
    ```
  - Running with "-m all" starts all tests, if you want to start a
    specific group of test you can use "-m transaction" or "-m chat"
    instead of "-m all". Additional groups will be added after adding
    more tests.