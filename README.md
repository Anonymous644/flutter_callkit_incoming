# Flutter Callkit Incoming

A Flutter plugin to show incoming call in your Flutter app(Custom for Android/Callkit for iOS).

## :star: Features

* Show an incoming call
* Start an outgoing call
* Custom UI Android/Callkit for iOS

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
          flutter_callkit_incoming: ^1.0.0+2
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
        'duration': 30000,
        'extra': <String, dynamic>{'userId': '1a2b3c4d'},
        'android': <String, dynamic>{
          'isCustomNotification': true,
          'isShowLogo': false,
          'ringtonePath': 'ringtone_default',
          'backgroundColor': '#0955fa',
          'backgroundUrl': 'https://i.pravatar.cc/500',
          'actionColor': '#4CAF50'
        },
        'ios': <String, dynamic>{
          'iconName': 'AppIcon40x40',
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
          'ringtonePath': 'Ringtone.caf'
        }
      };
      await FlutterCallkitIncoming.showCallkitIncoming(params);
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
        }
      });
    ```  

4. Properties

    | Prop            | Description
