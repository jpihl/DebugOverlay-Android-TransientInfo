DebugOverlay-Android-TransientInfo
==================================
[![Android Arsenal](https://img.shields.io/badge/Android%20Arsenal-DebugOverlay--Android--TransientInfo-brightgreen.svg?style=flat)](https://android-arsenal.com/details/1/5516)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/com.jpihl/debugoverlay-ext-transient-info/badge.svg)](https://maven-badges.herokuapp.com/maven-central/com.jpihl/debugoverlay-ext-transient-info)
[![API 16+](https://img.shields.io/badge/API-16%2B-brightgreen.svg?style=flat)](https://android-arsenal.com/api?level=16)
[![License](https://img.shields.io/badge/license-Apache%202-brightgreen.svg)](https://www.apache.org/licenses/LICENSE-2.0)

**DebugOverlay-TransientInfo** is an extension to the [DebugOverlay][1]
library for Android. It allows custom data to be added and remove from the
overlay while the application is running. This is useful when certain types of
information is only relevant or available in specific parts of the application.

<img src="art/overlay_with_transient_info_module_small.png" width="50%" alt="DebugOverlay Screen Capture">

Requirements
------------
API Level 16 (Android 4.1) and above.

Setup
-----

Gradle:

```groovy
dependencies {
  debugCompile 'com.ms-square:debugoverlay:1.1.3'
  releaseCompile 'com.ms-square:debugoverlay-no-op:1.1.3'
  testCompile 'com.ms-square:debugoverlay-no-op:1.1.3'

  compile ('com.jpihl:debugoverlay-ext-transient-info:1.0.0') {
    exclude module: 'debugoverlay'
  }
}
```

or

```groovy
dependencies {
  // this will use a full debugoverlay lib even in the test/release build
  compile 'com.jpihl:debugoverlay-ext-transient-info:1.0.0'
}
```

Usage
-----

### Simple Example

In your `Application` class:

```java
public class ExampleApplication extends Application {

  @Override public void onCreate() {
    super.onCreate();
    new DebugOverlay.Builder(this)
            .modules(new TransientInfoModule(1000),
                     new ...)
            .build()
            .install();
    // Normal app init code...
  }
}
```

In your `Activity` class where you want to monitor something:

```java
public class ExampleActivity extends Activity {

  private final MyVideoPlayer videoPlayer = new MyVideoPlayer();

  @Override public void onCreate() {
    super.onCreate();
    TransientInfoModule.addProvider(new InfoProvider() {
      @Override String getInfo()
      {
        return "Frame drops: " + videoPlayer.frameDrops();
      }
    });
    // Normal activity onCreate code...
  }

  @Override public void onDestroy() {
    super.onCreate();
    TransientInfoModule.clearProviders();
    // Normal activity onDestroy code...
  }
}
```

License
-------

    Copyright 2017 Manabu Shimobe

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.

[1]: https://github.com/Manabu-GT/DebugOverlay-Android/blob/master
