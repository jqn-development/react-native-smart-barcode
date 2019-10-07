# react-native-barcode-scan

[![npm](https://img.shields.io/npm/v/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/dm/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/dt/react-native-smart-barcode.svg)](https://www.npmjs.com/package/react-native-smart-barcode)
[![npm](https://img.shields.io/npm/l/react-native-smart-barcode.svg)](https://github.com/react-native-component/react-native-smart-barcode/blob/master/LICENSE)

A  barcode scanner component for React Native app.
The library uses [https://github.com/zxing/zxing][1] to decode the barcodes for android, and also supports ios.


## Installation

```
npm install react-native-barcode-scan --save
```

## Installation (iOS)

* Drag RCTBarCode.xcodeproj to your project on Xcode.

* Click on your main project file (the one that represents the .xcodeproj) select Build Phases and drag libRCTBarCode.a from the Products folder inside the RCTBarCode.xcodeproj.

* Look for Header Search Paths and make sure it contains $(SRCROOT)/../../../react-native/React as recursive.

* Dray raw folder to your project

* Add `Privacy - Camera Usage Description` property in your info.plist(for ios 10)

## Installation (Android)

* In `android/settings.gradle`

```
...
include ':react-native-barcode-scan'
project(':react-native-barcode-scan').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-barcode-scan/android')
```

* In `android/app/build.gradle`

```
...
dependencies {
    ...
    // From node_modules
    compile project(':react-native-barcode-scan')
}
```

* In MainApplication.java

```
...
private ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    //  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    protected boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }

    @Override
    protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
              new MainReactPackage()
      );
    }
  };

  public void setReactNativeHost(ReactNativeHost reactNativeHost) {
    mReactNativeHost = reactNativeHost;
  }

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }
...
```

* In AndroidManifest.xml, add camera permissions
```
...
<uses-permission android:name="android.permission.CAMERA"/>
<uses-permission android:name="android.permission.VIBRATE"/>

<uses-feature android:name="android.hardware.camera"/>
<uses-feature android:name="android.hardware.camera.autofocus"/>
...
```


## Full Demo

see [ReactNativeComponentDemos][0]

## Usage

Install the package from npm with `npm install react-native-barcode-scan --save`.
Then, require it from your app's JavaScript files with `import Barcode from 'react-native-barcode-scan'`.

```js


import React, {
    Component,
} from 'react'
import {
    View,
    StyleSheet,
    Alert,
} from 'react-native'

import Barcode from 'react-native-barcode-scan'
import TimerEnhance from 'react-native-smart-timer-enhance'

class BarcodeTest extends Component {

    // 构造
    constructor(props) {
        super(props);
        // 初始状态
        this.state = {
            viewAppear: false,
        };
    }

    render() {

        return (
            <View style={{flex: 1, backgroundColor: 'black',}}>
                {this.state.viewAppear ? <Barcode style={{flex: 1, }}
                                                  ref={ component => this._barCode = component }
                                                  onBarCodeRead={this._onBarCodeRead}/> : null}
            </View>
        )
    }

    componentDidMount() {
        this.timer = setTimeout(
              () => this.setState({viewAppear: true}),
              255
            );
    }

    componentWillUnmount () {
        this.timer && clearTimeout(this.timer);
    }

    _onBarCodeRead = (e) => {
        console.log(`e.nativeEvent.data.type = ${e.nativeEvent.data.type}, e.nativeEvent.data.code = ${e.nativeEvent.data.code}`)
        this._stopScan()
        Alert.alert(e.nativeEvent.data.type, e.nativeEvent.data.code, [
            {text: 'OK', onPress: () => this._startScan()},
        ])
    }

    _startScan = (e) => {
        this._barCode.startScan()
    }

    _stopScan = (e) => {
        this._barCode.stopScan()
    }

}

export default TimerEnhance(BarcodeTest)
```

## Props

Prop                   | Type   | Optional | Default   | Description
---------------------- | ------ | -------- | --------- | -----------
barCodeTypes           | array  | Yes      |           | determines the supported barcodeTypes
scannerRectWidth       | number | Yes      | 255       | determines the width of scannerRect
scannerRectHeight      | number | Yes      | 255       | determines the height of scannerRect
scannerRectTop         | number | Yes      | 0         | determines the top shift of scannerRect
scannerRectLeft        | number | Yes      | 0         | determines the left shift of scannerRect
scannerLineInterval    | number | Yes      | 3000      | determines the interval of scannerLine's movement
scannerRectCornerColor | string | Yes      | `#09BB0D` | determines the color of scannerRectCorner

[0]: https://github.com/cyqresig/ReactNativeComponentDemos
[1]: https://github.com/zxing/zxing
[2]: http://cyqresig.github.io/img/react-native-barcode-scan-preview-ios-v1.0.0.gif
[3]: http://cyqresig.github.io/img/react-native-barcode-scan-preview-android-v1.0.0.gif