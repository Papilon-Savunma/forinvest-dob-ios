# Forinvest-DOB-iOS

[![pod - 2.0.4](https://img.shields.io/badge/pod-2.0.4-blue)](https://cocoapods.org/)

## Requirements

- +iOS 13.0

## Installation

TacirlerSDK is available through [CocoaPods](https://cocoapods.org). To install
it, simply add the following lines to your Podfile:

```ruby
use_frameworks!

platform :ios, '13.0'

target 'YOUR-TARGET-NAME' do
  pod 'forinvest-dob-ios'
end

post_install do |installer|
  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['BUILD_LIBRARY_FOR_DISTRIBUTION'] = 'YES'
      config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '13.0'
    end
  end
end
```

## Configuration

- In target app select `Signing & Capabilities` tab and click `+Capability` button and add `Near Field Communication Tag Reading` capability.
- Add your Info.plist file necessary permissions;

```
<!--
FOR NFC
-->
<key>com.apple.developer.nfc.readersession.iso7816.select-identifiers</key>
<array>
    <string>A0000002471001</string>
</array>
<key>NFCReaderUsageDescription</key>
<string>Permission string</string>

<!--
FOR Camera
-->
<key>NSCameraUsageDescription</key>
<string>Permission string</string>
<key>NSMicrophoneUsageDescription</key>
<string>Permission string</string>

```

- Make sure you have these lines in your `.entitlements` file;

```
<dict>
    <key>com.apple.developer.nfc.readersession.formats</key>
    <array>
        <string>TAG</string>
    </array>
</dict>
```

## Example usage with customization options

```swift
import UIKit
import TacirlerSDK

class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Perform necessary tasks in case of memory shortage here.
    }

    @IBAction func clicked(_ sender: Any) {
        // Customize your theme settings
        // if you comment out this section it will be show the default app color theme in your device/simulator.
        let customTheme = SDKConfiguration(
            backgroundColor: UIColor.lightGray,
            contentViewBackgroundColor: UIColor.cyan,
            buttonEnabledBackgroundColor: UIColor.green,
            buttonDisabledBackgroundColor: UIColor.red,
            buttonTextColor: UIColor.purple,
            placeHolderTextColor: UIColor.brown,
            borderColor: UIColor.cyan,
            tintColor: UIColor.purple,
            iconColor: UIColor.orange,
            checkBoxColor: UIColor.brown,
            viewControllerTitleColor: UIColor.red,
            logoImageName: "logo3",
            fontType: .aguafinaScriptRegular
        )

        // Set the theme through SDKConfigManager
        SDKConfigManager.shared.sdkConfiguration = customTheme

        // Create TacirlerSDKViewController
        let vc = TacirlerSDKViewController()
        vc.authorizeToken(
        // put your token and license id in here
        // if you don't available or lost it contact with:
        // yasinkoker@papilon.com.tr
            inputToken: "YOUR_TOKEN",
            licenseID: "YOUR_LICENSE_ID"
        )

        // Present TacirlerSDKViewController modally
        vc.modalTransitionStyle = .coverVertical
        vc.modalPresentationStyle = .fullScreen
        present(vc, animated: true)
    }
}

```

## Important Notes

If you are using XCode version 14.3.1 for your development, you can directly pull and use the updated SDK with a 'pod update' command.

However, if you have upgraded to XCode version 15.0, you may encounter the following error when building after 'pod update':
<br>
<b>DT_TOOLCHAIN_DIR cannot be used to evaluate LIBRARY_SEARCH_PATHS, use TOOLCHAIN_DIR instead</b>

Until Apple or CocoaPods releases a new update related to this issue, as a temporary solution after every 'pod update,' please follow these steps in the terminal inside your project folder:
Run the following script:

```bash
find . -name "*.xcconfig" -type f -exec grep -l 'DT_TOOLCHAIN_DIR' {} \; | while IFS= read -r file; do sed -i '' 's/DT_TOOLCHAIN_DIR/TOOLCHAIN_DIR/g' "$file"; done
```

Then, reopen the project and build it again. After this step, you may encounter a runtime error like the following:
<br>
<b>Sandbox: rsync(35790) deny(1) file-write-create</b>

For that, you can follow these steps:

Open the Xcode project.<br>
Click on the project name in the left sidebar to open the project settings.<br>
Select the target you want to check in the "Targets" section.<br>
Click on the "Build Settings" tab.<br>
In the search bar, type "ENABLE_USER_SCRIPT_SANDBOXING."<br>
If the value of ENABLE_USER_SCRIPT_SANDBOXING is set to "No," then it is disabled. If it is set to "Yes," then it is enabled.

## SDK Flow

#### KVKK Approval Screen

<img src="./images/IMG_0023.PNG" width="390" height="844">
User needs to check both checkmark to proceed.

#### KVKK Policy and Commercial and Electronic Message Screens

<img src="./images/IMG_0024.PNG" width="390" height="844">
<img src="./images/IMG_0025.PNG" width="390" height="844">

#### MASAK Statement Screen

<img src="./images/IMG_0026.PNG" width="390" height="844">

#### Daily and Monthly Notifications Screen

<img src="./images/IMG_0027.PNG" width="390" height="844">
User needs to check checkmark to proceed.

#### NFC Availability Check Screen

<img src="./images/IMG_0028.PNG" width="390" height="844">
In this screen NFC Availability control should be done, otherwise, user cannot proceed.

#### Form Screen

<img src="./images/IMG_0029.PNG" width="390" height="844">
User needs to fill the form to proceed

#### Questionnaire Screen

<img src="./images/IMG_0030.PNG" width="390" height="844">
User needs to choose one option to proceed. If they choose other or "Tacirler Investment Personal", user needs to fill text field.

#### Phone Number Screen

<img src="./images/IMG_0031.PNG" width="390" height="844">

#### SMS OTP Code Screen

<img src="./images/IMG_0032.PNG" width="390" height="844">

#### MRZ Scanner Screen

<img src="./images/IMG_0033.PNG" width="390" height="844">
TR Identity card should be shown to the camera to proceed.

#### NFC Reader Screen

<img src="./images/IMG_0036.PNG" width="390" height="844">
<img src="./images/IMG_0037.PNG" width="390" height="844">
TR Identity card should be shown to backside of the phone to proceed.

#### Information Check Screen

<img src="./images/IMG_0038.PNG" width="390" height="844">

#### Selfie Screen

<img src="./images/IMG_0039.PNG" width="390" height="844">

#### Selfie Check Screens

<img src="./images/IMG_0041.PNG" width="390" height="844">
<img src="./images/IMG_0042.PNG" width="390" height="844">

#### Address Verification Screens

<img src="./images/IMG_0053.PNG" width="390" height="844">

1. Verification with Place of Residence
   <img src="./images/IMG_0043.PNG" width="390" height="844">

<img src="./images/IMG_0044.PNG" width="390" height="844">
QR Code that is on the place of residence form can be scanned.

2. Verification with Address No from E-devlet
   <img src="./images/IMG_0045.PNG" width="390" height="844">

#### Client Information Screen

<img src="./images/IMG_0046.PNG" width="390" height="844">

#### Video Call Screens

<img src="./images/IMG_0047.PNG" width="390" height="844">

<img src="./images/IMG_0048.PNG" width="390" height="844">

<img src="./images/IMG_0049.PNG" width="390" height="844">

#### Result Screens

<img src="./images/IMG_0051.PNG" width="390" height="844">
Application failed

<img src="./images/IMG_0052.PNG" width="390" height="844">
Application succeded

## Author

Papilon Savunma
