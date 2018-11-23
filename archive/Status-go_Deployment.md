1.  ```
    make statusgo-android; make statusgo-ios-simulator
    ```

2.  ```
    cd build/bin/statusgo-ios-9.3-framework/
    ```

3.  ```
    zip -r status-go-ios.zip Statusgo.framework
    ```

4.  copy 

        -gcommit` part from `git describe

5.  version name will be 

        branch-gcommit

6.  log in
    here [1](http://139.162.11.12:8081/artifactory/webapp/)(http://139.162.11.12:8081/artifactory/webapp/) (in
    LastPass)

7.  press deploy
    in 

        libs-release-local

     dir [2](http://139.162.11.12:8081/artifactory/webapp/#/artifacts/browse/tree/General/libs-release-local)(http://139.162.11.12:8081/artifactory/webapp/\#/artifacts/browse/tree/General/libs-release-local)

8.  upload \`statusgo-android-16.aar\` from \`build/bin/\`

9.  \`Deploy as Maven Artifact\`, Group Id: \`status-im\`, Artifact
    ID: \`status-go\`, Version: \`branch-gcommit\`, select \`Generate
    default POM\`

10. press
    deploy

11. upload \`status-go-ios.zip\` from \`build/bin/statusgo-ios-9.3-framework\`

12. \`Deploy as Maven Artifact\`, Group Id: \`status-im\`, Artifact
    ID: \`status-go-ios-simulator\`, Version: \`branch-gcommit\`,
    select \`Generate default POM\`