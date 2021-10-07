# react-native-quick-admob [![npm](https://img.shields.io/npm/v/react-native-admob.svg)](https://www.npmjs.com/package/react-native-quick-admob) [![npm (next)](https://img.shields.io/badge/npm-baochau9xx-blue)](https://www.npmjs.com/package/react-native-quick-admob)

## Installation

You can use npm or yarn to install:

**npm:**

    npm i react-native-quick-admob

**Yarn:**

    yarn add react-native-quick-admob

In order to use this library, you have to link it to your project first. There's excellent documentation on how to do this in the [React Native Docs](https://facebook.github.io/react-native/docs/linking-libraries-ios.html#content).

Alternatively for iOS you can install the library with CocoaPods by adding a line to your `Podfile`;

    pod 'react-native-quick-admob', path: '../node_modules/react-native-quick-admob'

### iOS

For iOS you will have to add the [Google Mobile Ads SDK](https://developers.google.com/admob/ios/quick-start#import_the_mobile_ads_sdk) to your Xcode project.

### Android

    npx react-native link react-native-quick-admob

## Usage

```jsx
import {
  AdMobBanner,
  AdMobInterstitial,
  PublisherBanner,
  AdMobRewarded,
} from 'react-native-quick-admob'

// Display a banner
<AdMobBanner
  adSize="fullBanner"
  adUnitID="your-admob-unit-id"
  testDevices={[AdMobBanner.simulatorId]}
  onAdFailedToLoad={error => console.error(error)}
/>

// Display a DFP Publisher banner
<PublisherBanner
  adSize="fullBanner"
  adUnitID="your-admob-unit-id"
  testDevices={[PublisherBanner.simulatorId]}
  onAdFailedToLoad={error => console.error(error)}
  onAppEvent={event => console.log(event.name, event.info)}
/>

// Display an interstitial
AdMobInterstitial.setAdUnitID('your-admob-unit-id');
AdMobInterstitial.setTestDevices([AdMobInterstitial.simulatorId]);
AdMobInterstitial.requestAd().then(() => AdMobInterstitial.showAd());

// Display a rewarded ad
AdMobRewarded.setAdUnitID('your-admob-unit-id');
AdMobRewarded.requestAd().then(() => AdMobRewarded.showAd());
```

## Example

```jsx
import {
    AdMobBanner,
    AdMobInterstitial,
    PublisherBanner,
    AdMobRewarded,
} from "react-native-quick-admob";

LogBox.ignoreAllLogs();

const id = "ca-app-pub-1208380256452481/2030713140";
const idApp = "ca-app-pub-1208380256452481~1549682637";

const rewardTestID = "ca-app-pub-3940256099942544/5224354917";
const bannerTestID = "ca-app-pub-3940256099942544/6300978111";

const App = () => {
    AdMobRewarded.setAdUnitID(id);

    AdMobRewarded.addEventListener("adLoaded", () => console.log("Load xong"));

    AdMobRewarded.addEventListener("adFailedToLoad", (error) =>
        console.warn("adFailedToLoad: ", error)
    );

    AdMobRewarded.addEventListener("rewarded", (reward) =>
        console.log("rewarded: ", reward)
    );

    AdMobRewarded.addEventListener("adOpened", () =>
        console.log("AdMobRewarded => adOpened")
    );

    AdMobRewarded.addEventListener("videoStarted", () =>
        console.log("AdMobRewarded => videoStarted")
    );

    AdMobRewarded.addEventListener("adClosed", () => {
        console.log("AdMobRewarded => adClosed");
        AdMobRewarded.requestAd().catch((error) => console.warn(error));
    });

    return (
        <SafeAreaView
            style={{
                flex: 1,
                alignItems: "center",
                justifyContent: "center",
                paddingTop: 50,
            }}
        >
            <Button
                onPress={() => {
                    AdMobRewarded.requestAd().then(() =>
                        AdMobRewarded.showAd()
                    );
                }}
                title="Show Reward"
            />
            <View style={{ flex: 1 }} />

            <View style={{ width: "100%" }}>
                <PublisherBanner
                    adSize="fullBanner"
                    adUnitID={id}
                    testDevices={[PublisherBanner.simulatorId]}
                    onAdFailedToLoad={(error) => console.error(error)}
                    onAppEvent={(event) => console.log(event.name, event.info)}
                    style={{ width: "100%" }}
                />
            </View>
            <View style={{ width: "100%" }}>
                <AdMobBanner
                    adSize="fullBanner"
                    adUnitID={id}
                    // testDevices={[AdMobBanner.simulatorId]}
                    onAdFailedToLoad={(error) => console.error(error)}
                    style={{ width: "100%" }}
                />
            </View>
        </SafeAreaView>
    );
};
```

## End
