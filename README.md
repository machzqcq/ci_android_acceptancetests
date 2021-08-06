# Sample cucumber project for Mobile CI pattern
Mobile Automated acceptance tests for Android

# eBook
This codebase is explained in my eBook on [Continuous Integration and automation for mobile apps](https://leanpub.com/ci-mobile-app)  
It is free to download and read.

# Basic Flow & Steps
- App is available in home folder or `features/support/resources`. The app is a simple android contact app with package name "com.example.android.contactmanager" and available as `app-debug.apk`
- Using adb you can install the app in your emulator/device (Check that USB debugging is enabled)
- Install Java on your machine. The version of JDK that works without issues is `1.8.0_231` . I faced many issues using any version after that (for e.g. I had JDK 15 and 16 and neither was helpful to launch uiautomatorviewer. Sure we could work without uiautomatorviewer however that helps with solidying  your concepts, so you could ignore that after you gained expertise, not at the initial stages)
    - As you can see below I had multiple Java versions on my mac 10.15.7 in `~/..bash_profile`. Either using homebrew to install. I did not use jenv, however it is recommended per this [post](https://stackoverflow.com/questions/26252591/mac-os-x-and-multiple-java-versions). Execute `/usr/libexec/java_home -V` to list down all versions of java 
    - ```# Java
        #Manage multiple java versions. Execute "/usr/libexec/java_home -V" to see all versions
        export JAVA_8_231_HOME=$(/usr/libexec/java_home -v1.8.0_231)
        export JAVA_8_302_HOME=$(/usr/libexec/java_home -v1.8.0_302)
        export JAVA_15_HOME=$(/usr/libexec/java_home -v15.0.1)
        export JAVA_16_HOME=$(/usr/libexec/java_home -v16.0.2)
        export ADOPTOPENJDK_JAVA_8_HOME=$(/usr/libexec/java_home -v1.8.0_292)

        alias java8_231='export JAVA_HOME=$JAVA_8_231_HOME'
        alias java8_302='export JAVA_HOME=$JAVA_8_302_HOME'
        alias java15='export JAVA_HOME=$JAVA_15_HOME'
        alias java16='export JAVA_HOME=$JAVA_16_HOME'
        alias adoptopenjdk_java8='export JAVA_HOME=$ADOPTOPENJDK_JAVA_8_HOME'

        #default java8
        export JAVA_HOME=$JAVA_8_231_HOME
        export PATH=$PATH:$JAVA_HOME/bin
        export PATH=$PATH:$JAVA_HOME/lib
        ```
- Install android sdk and ensure all the variables are set. My Mac `~/.bash_profile` contained the below
    - ```# Android
        export ANDROID_HOME=/Users/pmacharl/Library/Android/sdk
        export PATH=$PATH:$ANDROID_HOME/platform-tools
        export PATH=$PATH:$ANDROID_HOME/tools
        export PATH=$PATH:$ANDROID_HOME/tools/bin
        export PATH=$PATH:$ANDROID_HOME/build-tools
        export PATH=$PATH:$ANDROID_HOME/build-tools/31.0.0
        export PATH=$PATH:$ANDROID_HOME/tools/lib/x86_64
        ```
- Install Ruby environment and `bundle install` from root folder. I have checked in Gemfile.lock file, because the exact dependencies that worked for me might help you. If you want to get latest gems , then delete Gemfile.lock and run `bundle update` to get latest dependencies for the project
    - Check `devices.yaml` to add/edit your specific configuration of devices/emulators that you plan to test it on
- Install [appium](https://appium.io/)
- Start appium server
- Run the tests
    - `DEVICE=huawei-nexus_6p-84B5T15A17000142 bundle exec cucumber --tags=@non-ci features/ci_android.feature`
    - `DEVICE=huawei-nexus_6p-84B5T15A17000142 bundle exec cucumber --tags=@ci_add_contact features/ci_android.feature`

# Parallel Execution
Coming soon. Please see my [book](https://leanpub.com/ci-mobile-app) as it already contains instructions on how to start multiple appium servers and connect to various targets and execute your tests