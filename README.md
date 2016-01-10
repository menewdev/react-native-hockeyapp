# react-native-hockeyapp
[HockeyApp](http://hockeyapp.com) integration for React Native.

## Requirements

- iOS 7+
- Android
- React Native >0.14
- CocoaPods

## Installation

```bash
npm install react-native-hockeyapp --save
```

## iOS

You will need:

CocoaPods ([Setup](https://guides.cocoapods.org/using/getting-started.html#installation))

### Podfile

Add to your `ios/Podfile`:
```ruby
pod "HockeySDK"
```


## Android

### Google project configuration

* In `android/setting.gradle`

```gradle
...
include ':react-native-hockeyapp', ':app'
project(':react-native-hockeyapp').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-hockeyapp/android')
```

* In `android/build.gradle`

```gradle
...
repositories {
    jcenter()
    mavenCentral()
}
dependencies {
    classpath 'com.android.tools.build:gradle:1.3.1'
    classpath 'net.hockeyapp.android:HockeySDK:3.7.0-rc.1' // <--- add this
}
```

* In `android/app/build.gradle`

```gradle
apply plugin: "com.android.application"
...
dependencies {
    compile fileTree(dir: "libs", include: ["*.jar"])
    compile "com.android.support:appcompat-v7:23.0.1"
    compile "com.facebook.react:react-native:0.14.+"
    compile project(":react-native-hockeyapp") // <--- add this
}
```

* Manifest file
```xml
<application ..>
    <activity android:name="net.hockeyapp.android.UpdateActivity" />
    <activity android:name="net.hockeyapp.android.FeedbackActivity" />
</application>
```

* Register Module (in MainActivity.java)

```java
import com.slowpath.hockeyapp.RNHockeyAppModule; // <--- import
import com.slowpath.hockeyapp.RNHockeyAppPackage;  // <--- import

public class MainActivity extends Activity implements DefaultHardwareBackBtnHandler {
  ......

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    mReactRootView = new ReactRootView(this);

    mReactInstanceManager = ReactInstanceManager.builder()
      .setApplication(getApplication())
      .setBundleAssetName("index.android.bundle")
      .setJSMainModuleName("index.android")
      .addPackage(new MainReactPackage())
      .addPackage(new RNHockeyAppPackage(this)) // <------ add this line to yout MainActivity class
      .setUseDeveloperSupport(BuildConfig.DEBUG)
      .setInitialLifecycleState(LifecycleState.RESUMED)
      .build();

    mReactRootView.startReactApplication(mReactInstanceManager, "AndroidRNSample", null);

    setContentView(mReactRootView);
  }
  ......

}
```

# Usage

From your JS files for both iOS and Android:

```js
var HockeyApp = require('react-native-hockeyapp');

componentWillMount() {
    HockeyApp.configure(HOCKEY_APP_ID, true);
}

componentDidMount() {
    HockeyApp.start();
    HockeyApp.checkForUpdate();
}
```

You have available these methods:
```js
HockeyApp.configure(HOCKEY_APP_ID:string, autoSendCrashReports:boolean = true); // Configure the settings
HockeyApp.start(); // Start the HockeyApp integration
HockeyApp.checkForUpdate(); // Check if there's new version and if so trigger update
HockeyApp.feedback(); // Ask user for feedback.
HockeyApp.generateTestCrash(); // Generate test crash. Only works in no-debug mode.
```
