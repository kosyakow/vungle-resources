# VungleSDK Unity- Android Developer Guide

* For compatibility purposes with Android 5.+, the Vungle Unity plugin requires JDK 7 to be installed on the development system

## Setup

* If you haven't already done so, head over to our [dashboard](https://v.vungle.com/dashboard/login) and add your app to your account. You need to do this in order to get your App ID. It’s in **red** on your app’s page.

* To initialize Vungle, pass your App ID to the `init` method. The SDK will automatically begin caching a video. 

* You can use the `isVideoAvailable` method to determine if an ad is available to display. 

* Once you are ready to show the ad, just call `playAd`. The `playAd` method takes several options that can be used to customize the way it is displayed.

## API Documentation

#### VungleAndroid.cs exposes the following methods:
```
// Starts up the SDK with the given appId
public static void init( string appId )

// Call this when your application is sent to the background
public static void onPause()

// Call this when your application resumes
public static void onResume()

// Checks to see if a video is available
public static bool isVideoAvailable()

// Sets if sound should be enabled or not
public static void setSoundEnabled( bool isEnabled )

// Sets the allowed orientations of any ads that are displayed
public static void setAdOrientation( VungleAdOrientation orientation )

// Checks to see if sound is enabled
public static bool isSoundEnabled()

// Plays an ad with the given options. The user option is only supported for incentivized ads.
public static void playAd( bool incentivized = false, string user = "" )

// Plays an ad with the given options.
public static void playAdEx( Dictionary<string,object> options)
```
#### Options
| Key          | Description |
| :----------- |:----------- |
| `incentivized` | You can choose to be notified whenever a user has completed an ad. A typical use case of this is when you are offering some sort of value exchange (‘watch this video and receive 100gems!’). If you choose to make your ads incentivized, we’ll immediately send a message to your server along with a user id (that you provide) so that you can reward your users. YES means this ad will be incentivized. Instructions for setting up incentivized ads are [here](https://github.com/Vungle/vungle-resources/tree/master/English/Incentivized-Ads). |
| `orientation` |Set true for matchVideo and false for autoRotate |
| `userTag` | The key user is the one passed as user in the S2S call (if there are any). |
| `placement` | You'll want to pass an indicator of which ad was played, for example, 'Level2'. |
| `alertTitle` | String that is used as the title of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `alertText` | String that is used as the body text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `closeText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `continueText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `immersive` | Immersive mode |
| `key1..8` | We have 8 keys built in here |

#### VungleAndroidManager.cs fires the following events:
```
// Fired when a Vungle ad starts
public static event Action onAdStartEvent;

// Fired when a Vungle ad finishes. This event is deprecated. Please use onAdFinishedEvent instead.
public static event Action onAdEndEvent;

// Fired when a Vungle ad is cached and ready to be displayed
public static event Action onCachedAdAvailableEvent;

// Fired when a Vungle video is dismissed. Includes the watched and total duration in milliseconds.
// This event is deprecated. Please use onAdFinishedEvent instead.
public static event Action<double,double> onVideoViewEvent;


// Fired when a Vungle ad finished and provides the entire information about this event: time watched,   
// total duration, flag that indicates that an ad is completely viewed and flag that indicates was  
// call to action clicked or not.  
public static event Action<AdFinishedEventArgs> onAdFinishedEvent;
```
