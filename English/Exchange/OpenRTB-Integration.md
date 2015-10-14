## Vungle OpenRTB Integration

### Overview
___________________________________________________________

The Vungle exchange supports demand partners to access Vungle Applications supply through the IAB OpenRTB programmatic standard.  
* Vungle supports the OpenRTB 2.3.1 Standard
* The current auction type is first price only
* Vungle's OpenRTB spec requires some fields that are optional
* Vungle's inventory strictly consists of only Mobile Video In-App Impressions, which are full-screen 100% viewable interstitials

__The following describes the typical flow of a bid request and response:__

1. Vungle SDK send an Ad Request to Vungle Exchange
2. Vungle Exchange then constructs an OpenRTB bid request object to be sent to DSP's
3. Vungle makes a HTTP POST request to all DSP partners endpoints 
4. Vungle runs their first price auction on all valid responses
5. The winning bid's ad markup along with Win Notification are returned to the mobile device 

### Auction Rules
___________________________________________________________

* Auction Timeout for Bid Request: __250ms__
* Auction Responses are parsed, including unwrapping of VAST Wrappers, to validate markup (see below)
* The Win Notification (_nurl_) is fired from device only when the creative is rendered (first frame of video)

### Ad Markup SLA's
___________________________________________________________

* Duration: Up to 30s video
* Minimum Video Quality: 250 kbps bitrate
* Format: At least 1 media file in .mp4
* Maximum of 2 VASTAdTagURI Redirects
* Maximum payload size of 5MB

### Bid Request
___________________________________________________________

The following table is a supported subset of the [OpenRTB 2.3.1 Spec](http://www.iab.net/media/file/OpenRTB_API_Specification_Version_2_3_1.pdf)  
Vungle will always try to pass all fields to DSP.  

Vungle only supports OpenRTB Request/Responses in _Content-Type: application/json_


| Object     | Parameter         | Description                                           | 
| ---------- | ----------------- | ----------------------------------------------------- |
| BidRequest |                id | Vungle Unique Bid Request id                          |
| BidRequest |                at | integer: 1 - Default first priced auction             |
| BidRequest |              tmax | integer: 250 - Default auction timeout                |
|        imp |                id | Vungle Unique Impression id                           |
|        imp |    displaymanager | string: _"Vungle"_                                    | 
|        imp | displaymanagerver | string: Vungle SDK Version                            | 
|        imp |             instl | integer: 1 - Default Insterstitials                   | 
|        imp |          bidfloor | float: Vungle Campaign specified floor                | 
|        imp |       bidfloorcur | string: "USD" - Default                               |  
|      video |             mimes | string: "video/mp4"                                   |
|      video |       minduration | integer: min video duration                           |
|      video |       maxduration | integer: max video duration                           |     
|      video |         protocols | integer: VAST 1.0/2.0 + Wrapper                       |
|      video |                 w | integer: width of device screen                       |
|      video |                 h | integer: height of device screen                      |  
|      video |        startdelay | integer: 0 - None                                     |
|      video |         linearity | integer: 1 - Linear                                   |
|      video |        minbitrate | integer: 250 - Default                                |
|      video |        maxbitrate | integer: 2500 - Default                               |
|      video |     boxingallowed | integer array                                         |
|      video |    playbackmethod | integer array                                         |     
|      video |          delivery | integer array                                         |   
|      video |               pos | integer: 7 - Default                                  |
|        app |                id | Vungle App Id for Application                         | 
|        app |              name | string: App Store Application Name                    | 
|        app |            bundle | string: Application Package Name                      |
|        app |          storeurl | string: App Store URL                                 |
|        app |               cat | string array: App IAB Category                        |
|        app |     privacypolicy | integer: based on app privacy setttings within Vungle |
|  publisher |                id | Vungle Account Id for Publisher                       |
|  publisher |              name | string: Publisher Account Name                        |
|  publisher |               cat | string array: Publisher IAB Category                  |
|     device |                ua | string: Browser User Agent (if available)             |
|     device |               dnt | integer: Do Not Track setting from User Device        |
|     device |               lmt | integer: Do Not Track setting from User Device        |
|     device |                ip | string: IPv4 address from User Device                 |
|     device |        devicetype | integer: Phone type, typically smartphone or tablet   |
|     device |             model | string: Phone model from iOS or Android frameworks    |
|     device |                os | string: Device operating system                       |
|     device |               osv | string: Devier operating system version               |
|     device |                 h | string: Screen device height                          |
|     device |                 w | string: Screen device width                           |
|     device |          language | string: Device language setting, 2 letter code        |
|     device |               ifa | string: Device Advertiser Id                          |
|        geo |               lat | float: Device latitude coordinate                     |
|        geo |               lon | float: Device longitude coordinate                    |
|        geo |              type | integer: Location data source                         |
|        geo |           country | string: Two letter country code from IP               |
|        geo |            region | string: Two letter region code from IP                |
|       user |                id | string: Vungle Exchange user id                       |
|       user |            gender | string: Publisher supplied gender information         | 
|       user |               yob | integer: Publisher supplied age information           |



### Bid Response
___________________________________________________________

Supported Ad Markup Formats:
* VAST 1.0/2.0
* VAST Wrapper 1.0/2.0

BidResponse _adm_ field must contain:
1. Fully formed URL encoded VAST XML
2. URL to a resource that contains VAST XML

No Bid Handling methods:
1. HTTP 204 Response
2. Empty JSON Object
3. Response with _nbr_ attribute filled in

Auction Caveats:

* Vungle __does not__ support multiple bids in a single response.  If multiple bids are sent, only the first bid will be validated.
* Vungle __does not__ support sending in the ad markup through the BidResponse _nurl_ field.  
Please don't do this.


| Object      | Parameter         | Required    | Description                                               | 
| ----------  | ----------------- | ----------- | --------------------------------------------------------- |
| BidResponse |                id |         yes | string: Vungle BidRequest id echoed back in the response  | 
| BidResponse |             bidid | recommended | string: Bidder generated id for tracking                  |
| BidResponse |               cur |          no | string: Vungle default is USD, others will be ignored     |
| BidResponse |               nbr | recommended | integer: No bid reason defined by bidder                  |
|     seatbid |              seat |          no | string: Bidder defined seat id of the bid                 |
|         bid |                id |         yes | string: Bidder genereated id for tracking unique to bid   |
|         bid |             impid |         yes | string: Vungle impressions id echoed back in the response |
|         bid |             price |         yes | float: Bid price for the impressions in CPM only          |
|         bid |              adid | recommended | string: Bidder defined id for the ad campaign             |
|         bid |              nurl | recommended | string: Win notice URL for bid, fired off from device     |
|         bid |               adm |         yes | string: Ad markup VAST XML or URL that refers to VAST XML |
|         bid |           adomain |          no | string array: Advertiser domain                           |
|         bid |            bundle |          no | string: Package name of advertiser application            |
|         bid |               cid |          no | string: Advertiser campaign name/id                       |
|         bid |              crid |          no | string: Advertiser creative name/id                       |
|         bid |               cat |          no | string array: IAB content category of creative            |
|         bid |              attr |          no | integer array: Creative attributes                        |




### Bid Examples
___________________________________________________________

Request Example:

```
{
  "id": "cdc-7c6d-4029-dcd|1337", // Globally unique ID to track the BidRequest from the beginning of the time.
  "imp": [
    {
      "id": "5376606", // A unique ID used to track a single impression in Vungle
      "video": [
        "video/mp4" 
      ],
      "displaymanager": "Vungle", 
      "displaymanagerver": "test-phone/3.3.1", // Vungle SDK version, e.g. Vungle/3.3.1
      "instl": 1, 
      "bidfloor": 15, // Number in CPM as specified in OpenRTB 2.3
      "bidfloorcur": "USD" 
    }
  ],
  "app": {
    "id": "test-pub-app-id", // ID to uniquely identify a publisher application in Vungle.
    "name": "test-pub-app-name", // Name of the publisher app
    "bundle": "com.x.vungle.bundle.id", // Package name
    "storeurl": "http;//test/pub/app/store",
    "cat": [
      "IAB1" 
    ],
    "privacypolicy": 1, // I don't think we need to explose implementation details, but whether the app is COPPA compliant.
    "publisher": {
      "id": "test-pub-app-id", // Vungle App Id
      "name": "test-pub-app-name", // Vungle App Name
      "cat": [
        "IAB1" 
      ]
    }
  },
  "device": { 
    "ua": "Mozilla/5.0 (iPhone; CPU iPhone OS 8_4 like Mac OS X) AppleWebKit/600.1.4 (KHTML, like Gecko) Mobile/12H141",
    "geo": { 
      "lat": 31.19283,
      "long": 129.99392,
      "type": 1, // OpenRTB 2.3 5.16
      "country": "US",
      "region": "CA"
    },
    "dnt": 0,
    "lmt": 0,
    "ip": "192.168.1.1",
    "devicetype": 1,
    "model": "iPhone 6",
    "os": "iOS",
    "osv": "8.0",
    "w": 1080,
    "h": 1920,
    "js": 1, // @const
    "language": "en",
    "ifa": "test-ifa"
  },
  "user": {
    "id": "923123", 
    "gender": "F",
    "yob": 1985
  },
  "at": 1, // First price auction
  "tmax": 250 // Auction timeout
}
```


Response Example:
```
{
  "id": "abc-123-456-xyz|1337", // Needs to be same as the bid request ID
  "bidid": "5508", // Bidder defined ID that will likely be used in macro substitution.
  "cur": "USD", // Optinal, needs to be the same as bid request currency.
  "seatbid": [
    {
      "seat": "7735", // Bidder defined seat ID, for now, the first one is the one that will be taken.
      "bid": [
        {
          "id": "5508", // Bidder bidid
          "impid": "lol-987-654-321", // Needs to be the same as the imp.id we provide.
          "price": 500, // Their price in CPM.
          "adid": "618", // Bidders Ad Id, likely needed during macro substitution.
          "nurl": "http://bidder.com/won?price=${AUCTION_PRICE}", // Win URL
          "adm": "http://bidder.com/vast?id=${AUCTION_ID}" // Ad markup: plain HTTP URL or VAST 1.0/2.0.
        }
      ]
    }
  ]
}
```
