For core contributors:

Artifactory is where we host artifacts. It can be found here:

<http://139.162.11.12:8081/artifactory/webapp/#/home>

Admin details are in Lastpass.

1.  1.  How to fix artifact like \`function\` missing?

1\. Git clone <https://github.com/status-im/function> 2. \`mvn package\`
3. Take resulting \`target/function-0.0.1.jar\` and upload it to
Artifactory 4. Press 'Artifacts' on left hand side and go to
libs-release-local/status-im/function/0.0.1 5. Click Deploy, check
target repo is 'libs-release-local' and target path is right. 6. Add the
generated jar file and confirm