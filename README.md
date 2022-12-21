# Flutter Callkit Incoming

A Flutter plugin to show incoming call in your Flutter app(Custom for Android/Callkit for iOS).

[![pub package](https://img.shields.io/pub/v/flutter_callkit_incoming.svg)](https://pub.dev/packages/flutter_callkit_incoming)
[![Build Status](https://github.com/hiennguyen92/flutter_callkit_incoming/actions/workflows/main.yml/badge.svg)](https://github.com/hiennguyen92/flutter_callkit_incoming/actions/workflows/main.yml)

<a href="https://www.buymeacoffee.com/hiennguyen92" target="_blank"><img src="https://cdn.buymeacoffee.com/buttons/default-orange.png" alt="Buy Me A Coffee" height="41" width="174"></a>

## :star: Features

* Show an incoming call
* Start an outgoing call
* Custom UI Android/Callkit for iOS
* Example using Pushkit/VoIP for iOS

  <br>

## iOS: ONLY WORKING ON REAL DEVICE, not on simulator(Callkit framework not working on simulator)

<br>

## 🚀&nbsp; Installation

1. Install Packages

  * Run this command:
    ```console
    flutter pub add flutter_callkit_incoming
    ```
  * Add pubspec.yaml:
    ```console
        dependencies:
          flutter_callkit_incoming: any
    ```
2. Configure Project
  * Android
     * AndroidManifest.xml
     ```
      <manifest...>
          ...
          <!-- 
              Using for load image from internet
          -->
          <uses-permission android:name="android.permission.INTERNET"/>
      </manifest>
     ```
  * iOS
     * Info.plist
      ```
      <key>UIBackgroundModes</key>
      <array>
          <string>processing</string>
          <string>remote-notification</string>
          <string>voip</string>
      </array>
      ```

3. Usage
  * Import
    ```console
    import 'package:flutter_callkit_incoming/flutter_callkit_incoming.dart';
    ``` 
  * Received an incoming call
    ```dart
      this._currentUuid = _uuid.v4();
      var params = <String, dynamic>{
        'id': _currentUuid,
        'nameCaller': 'Hien Nguyen',
        'appName': 'Callkit',
        'avatar': 'https://i.pravatar.cc/100',
        'handle': '0123456789',
        'type': 0,
        'textAccept': 'Accept',
        'textDecline': 'Decline',
        'textMissedCall': 'Missed call',
        'textCallback': 'Call back',
        'duration': 30000,
        'extra': <String, dynamic>{'userId': '1a2b3c4d'},
        'headers': <String, dynamic>{'apiKey': 'Abc@123!', 'platform': 'flutter'},
        'android': <String, dynamic>{
          'isCustomNotification': true,
          'isShowLogo': false,
          'isShowCallback': false,
          'isShowMissedCallNotification': true,
          'ringtonePath': 'system_ringtone_default',
          'backgroundColor': '#0955fa',
          'backgroundUrl': 'https://i.pravatar.cc/500',
          'actionColor': '#4CAF50',
          'incomingCallNotificationChannelName': "Incoming Call",
          'missedCallNotificationChannelName': "Missed Call"
        },
        'ios': <String, dynamic>{
          'iconName': 'CallKitLogo',
          'handleType': 'generic',
          'supportsVideo': true,
          'maximumCallGroups': 2,
          'maximumCallsPerCallGroup': 1,
          'audioSessionMode': 'default',
          'audioSessionActive': true,
          'audioSessionPreferredSampleRate': 44100.0,
          'audioSessionPreferredIOBufferDuration': 0.005,
          'supportsDTMF': true,
          'supportsHolding': true,
          'supportsGrouping': false,
          'supportsUngrouping': false,
          'ringtonePath': 'system_ringtone_default'
        }
      };
      await FlutterCallkitIncoming.showCallkitIncoming(params);
    ```
  * Show miss call notification
    ```dart
      this._currentUuid = _uuid.v4();
      var params = <String, dynamic>{
        'id': this._currentUuid,
        'nameCaller': 'Hien Nguyen',
        'handle': '0123456789',
        'type': 1,
        'textMissedCall': 'Missed call',
        'textCallback': 'Call back',
        'extra': <String, dynamic>{'userId': '1a2b3c4d'},
      };
      await FlutterCallkitIncoming.showMissCallNotification(params);
    ```

  * Started an outgoing call
    ```dart
      this._currentUuid = _uuid.v4();
      var params = <String, dynamic>{
        'id': this._currentUuid,
        'nameCaller': 'Hien Nguyen',
        'handle': '0123456789',
        'type': 1,
        'extra': <String, dynamic>{'userId': '1a2b3c4d'},
        'ios': <String, dynamic>{'handleType': 'generic'}
      };
      await FlutterCallkitIncoming.startCall(params);
    ```

  * Ended an incoming/outgoing call
    ```dart
      var params = <String, dynamic>{'id': this._currentUuid};
      await FlutterCallkitIncoming.endCall(params);
    ```

  * Ended all calls
    ```dart
      await FlutterCallkitIncoming.endAllCalls();
    ```

  * Get active calls. iOS: return active calls from Callkit, Android: only return last call
    ```dart
      await FlutterCallkitIncoming.activeCalls();
    ```
    Output
    ```json
    [{"id": "8BAA2B26-47AD-42C1-9197-1D75F662DF78", ...}]
    ```

  * Get device push token VoIP. iOS: return deviceToken, Android: Empty

    ```dart
      await FlutterCallkitIncoming.getDevicePushTokenVoIP();
    ```
    Output

    ```json
    <device token>

    //Example
    d6a77ca80c5f09f87f353cdd328ec8d7d34e92eb108d046c91906f27f54949cd
    
    ```
    Make sure using `SwiftFlutterCallkitIncomingPlugin.sharedInstance?.setDevicePushTokenVoIP(deviceToken)` inside AppDelegate.swift (<a href="https://github.com/hiennguyen92/flutter_callkit_incoming/blob/master/example/ios/Runner/AppDelegate.swift">Example</a>)
    ```swift
    func pushRegistry(_ registry: PKPushRegistry, didUpdate credentials: PKPushCredentials, for type: PKPushType) {
        print(credentials.token)
        let deviceToken = credentials.token.map { String(format: "%02x", $0) }.joined()
        //Save deviceToken to your server
        SwiftFlutterCallkitIncomingPlugin.sharedInstance?.setDevicePushTokenVoIP(deviceToken)
    }
    
    func pushRegistry(_ registry: PKPushRegistry, didInvalidatePushTokenFor type: PKPushType) {
        print("didInvalidatePushTokenFor")
        SwiftFlutterCallkitIncomingPlugin.sharedInstance?.setDevicePushTokenVoIP("")
    }
    ```


  * Listen events
    ```dart
      FlutterCallkitIncoming.onEvent.listen((event) {
        switch (event!.name) {
          case CallEvent.ACTION_CALL_INCOMING:
            // TODO: received an incoming call
            break;
          case CallEvent.ACTION_CALL_START:
            // TODO: started an outgoing call
            // TODO: show screen calling in Flutter
            break;
          case CallEvent.ACTION_CALL_ACCEPT:
            // TODO: accepted an incoming call
            // TODO: show screen calling in Flutter
            break;
          case CallEvent.ACTION_CALL_DECLINE:
            // TODO: declined an incoming call
            break;
          case CallEvent.ACTION_CALL_ENDED:
            // TODO: ended an incoming/outgoing call
            break;
          case CallEvent.ACTION_CALL_TIMEOUT:
            // TODO: missed an incoming call
            break;
          case CallEvent.ACTION_CALL_CALLBACK:
            // TODO: only Android - click action `Call back` from missed call notification
            break;
          case CallEvent.ACTION_CALL_TOGGLE_HOLD:
            // TODO: only iOS
            break;
          case CallEvent.ACTION_CALL_TOGGLE_MUTE:
            // TODO: only iOS
            break;
          case CallEvent.ACTION_CALL_TOGGLE_DMTF:
            // TODO: only iOS
            break;
          case CallEvent.ACTION_CALL_TOGGLE_GROUP:
            // TODO: only iOS
            break;
          case CallEvent.ACTION_CALL_TOGGLE_AUDIO_SESSION:
            // TODO: only iOS
            break;
          case CallEvent.ACTION_DID_UPDATE_DEVICE_PUSH_TOKEN_VOIP:
            // TODO: only iOS
            break;
        }
      });
    ```
  * Call from Native (iOS/Android) 

    ```swift
      //Swift iOS
      var info = [String: Any?]()
      info["id"] = "44d915e1-5ff4-4bed-bf13-c423048ec97a"
      info["nameCaller"] = "Hien Nguyen"
      info["handle"] = "0123456789"
      info["type"] = 1
      //... set more data
      SwiftFlutterCallkitIncomingPlugin.sharedInstance?.showCallkitIncoming(flutter_callkit_incoming.Data(args: info), fromPushKit: true)
    ```
    
    ```kotlin
        //Kotlin/Java Android
        FlutterCallkitIncomingPlugin.getInstance().showIncomingNotification(...)
    ```

    <br>

    ```swift
      //OR
      let data = flutter_callkit_incoming.Data(id: "44d915e1-5ff4-4bed-bf13-c423048ec97a", nameCaller: "Hien Nguyen", handle: "0123456789", type: 0)
      data.nameCaller = "Johnny"
      data.extra = ["user": "abc@123", "platform": "ios"]
      //... set more data
      SwiftFlutterCallkitIncomingPlugin.sharedInstance?.showCallkitIncoming(data, fromPushKit: true)
    ```
    
    <br>

    ```objc
      //Objective-C
      #if __has_include(<flutter_callkit_incoming/flutter_callkit_incoming-Swift.h>)
      #import <flutter_callkit_incoming/flutter_callkit_incoming-Swift.h>
      #else
      #import "flutter_callkit_incoming-Swift.h"
      #endif

      Data * data = [[Data alloc]initWithId:@"44d915e1-5ff4-4bed-bf13-c423048ec97a" nameCaller:@"Hien Nguyen" handle:@"0123456789" type:1];
      [data setNameCaller:@"Johnny"];
      [data setExtra:@{ @"userId" : @"HelloXXXX", @"key2" : @"value2"}];
      //... set more data
      [SwiftFlutterCallkitIncomingPlugin.sharedInstance showCallkitIncoming:data fromPushKit:YES];
    ```
    
    <br>

    ```swift
      //send custom event from native
      SwiftFlutterCallkitIncomingPlugin.sharedInstance?.sendEventCustom("customEvent", body: ["customKey": "customValue"])

    ```

    ```kotlin
        //Kotlin/Java Android
        FlutterCallkitIncomingPlugin.getInstance().sendEventCustom(event: String, body: Map<String, Any>)
    ```

4. Properties

    | Prop            | Description
