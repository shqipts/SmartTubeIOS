# Unofficial unsigned iOS build

This fork and its IPA release are unofficial builds of milika/SmartTubeIOS, not endorsed by the upstream maintainer. Original authorship and the GPL-3.0 license are retained; see LICENSE.

## Changes in this build

- Disabled Firebase initialization and Crashlytics reporting.
- Removed the missing private GoogleService-Info.plist from the iOS resources build phase.
- Disabled the Crashlytics upload script and its input paths.
- Added a local `just build-unsigned` recipe (its paths reflect the original build machine).

No signing credentials or Firebase credentials are included. Firebase dependencies remain in the project, but initialization/reporting is disabled.

## Rebuild

Use Xcode with the iPhoneOS SDK and resolve Swift package dependencies. From this repository:

```sh
xcodebuild build -workspace SmartTube.xcworkspace -scheme SmartTube \
  -configuration Release -destination 'generic/platform=iOS' \
  -derivedDataPath ./build \
  CODE_SIGNING_ALLOWED=NO CODE_SIGNING_REQUIRED=NO CODE_SIGN_IDENTITY= DEVELOPMENT_TEAM=
mkdir -p dist/Payload
cp -R build/Build/Products/Release-iphoneos/SmartTube.app dist/Payload/
(cd dist && zip -r SmartTube-unsigned.ipa Payload)
```

The published IPA is SmartTube 5.6 (24), arm64 for physical iOS devices, minimum iOS 17. The app and its three extensions are unsigned. It cannot be installed on stock iOS without appropriate signing; signing methods may require changes to entitlements or extension support. Build and archive integrity were checked, but playback and other runtime behavior were not tested on a physical device.

SHA-256 of the published IPA:
`b6bf11f2184a1820a6c9ce896741b1b951939fc90376449fbba597628b48f330`

The source at the release tag includes the modifications used to produce this build and is distributed under GPL-3.0.
