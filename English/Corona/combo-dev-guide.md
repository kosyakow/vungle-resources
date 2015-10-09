#VungleSDK Corona-developer guide

##Overview
The Vungle plugin offers easy integration of Vungle video ads using the `ads` library.

##Registration
If you haven't already done so, head over to our [dashboard](https://v.vungle.com/dashboard/login) and add your apps 
to your account. You need to do this in order to get your App ID. It’s in **red** on your app’s page.

##API Documentation
###ads library exposes the following functions:
- `init( providerName, appId [, adListener] )` -  initializes the Corona `ads` library by specifying the name of the ad 
network service provider and the application identifier.  
_providerName_ - `String` value for the privider name.  
_appId_ - `String` containing the app id.  
_adListener_ - `Listener`, `Function` or `table` that will handle ad lifecycle events from the Vungle plugin (See 
**Events** section). It is optional parameter.

- `getVersionString()` - returns the plugin and underlying SDK version numbers as a string.

- `showCacheFiles()` - writes the list of files in the Vungle cache to the log. This feature is only available for iOS.

- `isAdAvailable()` - Vungle downloads and caches the next video ad for optimal user experience. This function returns
`true` or `false` depending on the availability of a cached ad.

- `show( adUnitType [, params] )` - begins playing a full-screen video ad if one is cached and available for display.
For Vungle, this API returns a boolean true or false depending on the availability of a cached video ad.  
_adUnitType_ - `String` value that represents a type of the ad to show. Vungle supports the following types: "interstitial" - 
default video ad unit, "incentivized" - video ad unit with optional server-to-server callback for in-app rewards.    
_params_ - a `Table` that specifies properties for the ad request (see **Properties for the ad request** section). It is
optional parameter.

- `showEx(params)` - begins playing a full-screen video ad if one is cached and available for display.
For Vungle, this API returns a boolean true or false depending on the availability of a cached video ad.  
_params_ - a `Table` that specifies properties for the ad request (see **Properties for the ad request** section).

- `setSoundEnabled( isSoundEnabled )` - specifies whether the sound enabled or not at the start of the video ad.  
_isSoundEnabled_ - a boolean value that specifies whether the sound enabled or not at the start of the video ad.

The following methods are implemented only for iOS simulators:

- `clearCache()`  
- `enableLogging()`  
- `clearSleep()`  

The following method is deprecated and not implemented:

- `hide()` - unlike other ad providers, Vungle video ads hide themselves when closed by the user.
  
###Events
Vungle notifies you of the following events:
- `adStart`  
_type_ - adStart  
_isError_ - false if an ad started playing; true if an ad could not be played.  
_response_ - reason why ad could not be played if `isError` is true.  
- `adView`  
_type_ - adView  
_isError_ - false.  
_totalAdSeconds_ - the total length of the video in seconds.  
_secondsWatched_ - the length of the longest view by the user, in seconds.  
_isCompletedView_ - true if the user watched 80% of the video; false otherwise.  
- `adEnd`  
_type_ - adEnd  
_isError_ - false.  
_wasCallToActionClicked_ - true if the user clicked on the call-to-action (usually "Download"); false otherwise.  
- `cachedAdAvailable`  
_type_ - cachedAdAvailable  
_isError_ - false.

###Properties for the ad request
The `params` table parameter for `show()` and `showEx()` methods can include properties for the ad request.

####Common properties:  
| Key          | Description |
| :----------- |:----------- |
| `incentivized` | You can choose to be notified whenever a user has completed an ad. A typical use case of this is when you are offering some sort of value exchange (‘watch this video and receive 100gems!’). If you choose to make your ads incentivized, we’ll immediately send a message to your server along with a user id (that you provide) so that you can reward your users. YES means this ad will be incentivized. Instructions for setting up incentivized ads are [here](https://github.com/Vungle/vungle-resources/tree/master/English/Incentivized-Ads). |
| `userTag` | The key user is the one passed as user in the S2S call (if there are any). |
| `placement` | You'll want to pass an indicator of which ad was played, for example, 'Level2'. |
| `alertTitle` | String that is used as the title of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `alertText` | String that is used as the body text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `closeText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `continueText` | String title for the close button text of the alert dialog presented when a user closes an incentivized ad experience prematurely. |
| `key1..8` | We have 8 keys built in here |

####Properties applicable only for Android:  
| Key          | Description |
| :----------- |:----------- |
| `immersive` | Immersive mode. |
| `isAutoRotation` | boolean value that specifies the desired orientation for the video ad. |
| `isSoundEnabled` | boolean value that specifies whether the video will start with its sound matching the settings of your enclosing application or moted. |  

####Properties applicable only for IOS:  
| Key          | Description |
| :----------- |:----------- |
| `orientation` |  	Sets the orientation of the ad. See VungleAdOrientation. |
| `large` | Large buttons mode. |  
