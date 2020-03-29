# android 环境安装

- sudo brew install android-platform-tools

- android studio 初步认识：
  - 1.android studio 介绍：http://ask.android-studio.org/?/question/789
  - 2.android studio 概述一：http://ask.android-studio.org/?/question/791
  - 3.android studio 概述二：http://ask.android-studio.org/?/question/804

- adb connect 30.131.106.18
  - adb install -r
  - adb uninstall com.package
  - adb kill-server
  - adb start-server
  - adb logcat -v time
  - adb logcat | grep "MTOP"
  - adb logcat | grep UUID

  - adb shell dumpsys meminfo com.package
  - adb shell top -m 5 -d 1 |grep "com.package"

  - 获取系统版本：adb shell getprop ro.build.version.release
  - 获取系统api版本：adb shell getprop ro.build.version.sdk
