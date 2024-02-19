## Installation

* [Install](#heading--1)
* [Setups](#heading--2)
  * [Setup IDE](#heading--2-1)
  * [Setup Emulator](#heading--2-2)
* [Verify Installation](#heading--3)


<a id="heading--1"></a>

### Install

- For installing Flutter, refer to the official documentation:
https://docs.flutter.dev/get-started/install

- This covers downloading and setting up Flutter on your specific platform.

- Once installed, check that you have the latest Flutter version with:

    ```
    flutter --version
    ```

- Update to new releases `stable branch` when prompted for access to latest features and fixes.

<a id="heading--2"></a>

### Setups

<a id="heading--2-1"></a>

#### Setup IDE

- [Android Studio IDE](https://developer.android.com/studio?gad_source=1)

    - The Flutter plugin provides the best experience with project creation, running and debugging. 
    - Install the Flutter plugin by going to Preferences/Settings -> Plugins -> Flutter.


- [VS Code](https://code.visualstudio.com/download)

    - Install the Flutter extension which enables support for Dart/Flutter
    - Install the Flutter and Dart extensions from the Visual Studio Code marketplace.


<a id="heading--2-2"></a>

#### Setup Emulators

To run and test your Flutter apps, you'll need emulators or physical devices. Here's how to configure them:

- Android Emulator:
  
  Set up an Android Virtual Device (AVD) in Android Studio.

- iOS Simulator: 
  
  Configure an iOS simulator in Xcode (macOS only).

- Physical Devices: 
  
  Connect your Android or iOS device to your computer for testing.


<a id="heading--3"></a>

### Verify Installation

- Once Flutter is installed, run:

    ```
    flutter doctor
    ```

- This checks your environment to ensure you have all of the dependencies needed to start building Flutter apps. It will validate:
  - Flutter environment path set correctly
  - Connected devices for testing (or need emulator setup)
  - Platform requirements like Android Studio, Xcode, Chrome etc.

Follow any fixes suggested by `flutter doctor`.