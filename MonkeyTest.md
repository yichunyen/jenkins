# Purpose

Random click on the device to simulate human behavior. This assists developer in verifying whether switching pages causes any existing crashes and detects them early to facilitate developers in making corrections and improving product quality.

# Preparation

Before you use adb command, please install adb package. Or in your command line type `adb devices` to check your connected devices.

# Command

### Basic

```bash
# Identity app package id to click randomly in 500 times
adb shell monkey -p your.package.name -v 500
```

### Click Duration

```bash
# Identity app package id to click randomly in 500 times every 500ms
# --throttle duration in ms
adb shell monkey --throttle 500 -p your.package.name -v 500
```

### Record click process to export file

Note: if you set the seed value, you can import the file to follow every clicks.

```bash
# Save record click process in root directory as monkeyTest.log
adb shell monkey -p your.package.name -v 500 > monkeyTest.log
```

### ADB Commands

```bash
# Wake up mobile
adb shell input keyevent 26

# Let mobile sleep
adb shell input keyevent 223

# Swipe Pin
adb shell input swipe 300 1000 300 500

# Input Text(Pin)
adb shell input text 0000
```

# Jenkinsfile

- Need to setup the ADB path in stages or Jenkins System Configuration.
- If you want to test on ReadMi devices, need to insert SIM card to enable automation setting.
- pct parameters to avoid click volume and power keys
- To avoid turn off network connection, please move  the network setting to next layer or hide.
- Every devices has different reaction time. If you want to set the duration, please update the click times to match your wish duration.

```groovy
pipeline {
    stages{

        stage('Assemble App') {
            steps {
                sh './gradlew assembleDebug'
            }
        }
        
        stage('Wake Up Device') {
            steps {
                sh ADB + ' shell input keyevent 26'
                sh ADB + ' shell input swipe 300 1000 300 500'
                sh ADB + ' shell input text 0000'
            }		
        }
        
        stage('Install App') {
            steps {
                sh 'cd ' + env.WORKSPACE + '/app/build/outputs/apk/ebug'
                    script {
                        def apkFile = sh(script: 'find . -name "*.apk" | head -n 1', returnStdout: true).trim()
                        sh ADB + " install -r ${apkFile}"
                    }
                }
            }
        }
        
        stage('Monkey Test') {
            steps {
                script {
                    sh ADB + ' shell monkey --throttle 500 --pct-syskeys 0 --pct-anyevent 0 --pct-appswitch 0 --pct-majornav 0 --pct-nav 0 --pct-trackball 0 --pct-motion 0 --pct-touch 0 -p your.package.name -v 6500 > monkey.log'
                }
            }
        } // end of monkey test stage
        
        stage('Completed') {
            steps {
                // Let device sleep
                sh ADB + ' shell input keyevent 223'
            }
        }
    }
}
```

# Reference

- Official: [https://developer.android.com/studio/test/other-testing-tools/monkey](https://developer.android.com/studio/test/other-testing-tools/monkey)
- ADB Command(Chinese)：[https://zhuanlan.zhihu.com/p/89060003](https://zhuanlan.zhihu.com/p/89060003)
