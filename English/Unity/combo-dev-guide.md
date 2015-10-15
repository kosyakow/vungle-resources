# VungleSDK Unity- Combo Developer Guide (Android and iOS)

## Setup

* If you haven't already done so, head over to our [dashboard](https://v.vungle.com/dashboard/login) and add your apps to your account. You need to do this in order to get your App ID. It’s in **red** on your app’s page. You'll need to make separate apps for Android and iOS.

* To initialize Vungle, pass both App IDs to the `init` method. The SDK will automatically begin caching a video. 

* You can use the `isAdvertAvailable` method to determine if an ad is available to display. 

* Once you are ready to show the ad, just call `playAd`. The `playAd` method takes several options that can be used to customize the way it is displayed.

## API Documentation

#### Vungle.cs exposes the following methods:
```
// Initializes the Vungle SDK optionally with an age and gender
public static void init( string androidAppId, string iosAppId )

public static void init( string androidAppId, string iosAppId, int age, VungleGender gender )

// Sets if sound should be enabled or not
public static void setSoundEnabled( bool isEnabled )

// Checks to see if a video is available
public static bool isAdvertAvailable()

// Plays an ad with the given options. The user option is only supported for incentivized ads.
public static void playAd( bool incentivized = false, string user = "" )

// Plays an ad with more options. Use Dictionary<string,object> to set options.
public static void playAdWithOptions( Dictionary<string,object> options )

// Clear cache (for ios only)
public static void clearCache()

// Clear sleep (for ios only)
public static void clearSleep()

// Set enable/disable fire log event (for ios only)
public static void setLogEnable(bool enable)
```
#### Vungle.cs exposes the following properties:
```
//Readonly property that returns the plugin and underlying SDK version numbers as a string.
public static string VersionInfo
```

#### Options
| Key          | Description |
| :----------- |:----------- |
| `incentivized` | You can choose to be notified whenever a user has completed an ad. A typical use case of this is when you are offering some sort of value exchange (‘watch this video and receive 100gems!’). If you choose to make your ads incentivized, we’ll immediately send a message to your server along with a user id (that you provide) so that you can reward your users. YES means this ad will be incentivized. Instructions for setting up incentivized ads are [here](https://github.com/Vungle/vungle-resources/tree/master/English/Incentivized-Ads). |
| `orientation` | Sets the orientation of the ad. For ios see VungleAdOrientation, for Android set true for matchVideo and false for autoRotate |
| `large` | Large buttons mode |
| `userTag` | The key user is the one passed as user in the S2S call (if there are any). |
| `placement` | You'll want to pass an indicator of which ad was played, for example, 'Level2'. |
| `alertTitle` | String that is used as the title of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `alertText` | String that is used as the body text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `closeText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `continueText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `immersive` | Immersive mode |
| `key1..8` | We have 8 keys built in here |

#### Vungle.cs fires the following events:
```
// Fired when a Vungle ad starts
public static event Action onAdStartedEvent;

// Fired when a Vungle ad finishes. This event is deprecated. Please use onAdFinishedEvent instead.
public static event Action onAdEndedEvent;

// Fired when a Vungle ad is cached and ready to be displayed
public static event Action onCachedAdAvailableEvent;

// Fired when a Vungle video is dismissed and provides the time watched and total duration in that order.  
//This event is deprecated. Please use onAdFinishedEvent instead.  
public static event Action<double,double> onAdViewedEvent;

// Fired when the SDK sends a log event.
public static event Action<string> onLogEvent;

// Fired when a Vungle ad finished and provides the entire information about this event: time watched,   
// total duration, flag that indicates that an ad is completely viewed and flag that indicates was  
// call to action clicked or not.
public static event Action<AdFinishedEventArgs> onAdFinishedEvent;
```
