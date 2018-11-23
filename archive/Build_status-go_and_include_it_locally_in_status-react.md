## How to build status-go and include it locally for iOS

1.  Build status-go artifact for iOS:

(cd status-go && make statusgo-ios-simulator)

1.  You should see iOS framework cross compilation done. This builds the
    following artifact:

2.
3.  \> (cd status-go && find . -iname "Statusgo.framework")

4.  ./build/bin/statusgo-ios-9.3-framework/Statusgo.framework

5.
6.  Normally this is installed by Maven via Artifactory in this step

7.  mvn -f modules/react-native-status/ios/RCTStatus dependency:unpack

8.
9.  Locally you can see it here:

10. \> (cd status-react && find . -iname
    "Statusgo.framework")

11. ./modules/react-native-status/ios/RCTStatus/Statusgo.framework

12. ./modules/react-native-status/ios/RCTStatus/Statusgo.framework/Statusgo.framework

13.
14. Instead we are going to manually overwrite it.

<!-- end list -->

1.  For Xcode to pick up the new version, remove the whole framework
    first:

rm -r
status-react/modules/react-native-status/ios/RCTStatus/Statusgo.framework/

1.  Then copy over framework:

cp -R status-go/build/bin/statusgo-ios-9.3-framework/Statusgo.framework
status-react/modules/react-native-status/ios/RCTStatus/Statusgo.framework

1.  In Xcode, clean and build. If you have any scripts to do this, make
    sure that

2.  you don't accidentally run the mvn step to undo your manual install.

3.
<!-- end list -->

1.  It might also be a good idea to print something custom so you can
    easily tell
2.  the difference between an old and new version of status-go.