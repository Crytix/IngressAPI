# IngressAPI
Ingress API (multiple devices supported)

# Description

There are two goals, one is [Intel official map] (https://www.ingress.com/intel), the other is the game using the API

This article only relevant API appreciation, please do not do [TOS] (https://www.ingress.com/terms) things

Has been packaged into a library

`Pip3 install ingressAPI`

`From ingressAPI import IntelMap`

# Intel Map

## Get API

1. After logging in to your Google account, open the intel interface and open the Network tab
2. Refresh the page to get all the xhr requests
[Chrome] (https://ww1.sinaimg.cn/large/006tNbRwgw1farvi9y1ymj30qq0tujvn.jpg)
3. These `get *` request is the ingress of the API, not in a hurry, one by one to see

## Precautions

1. HTTP Request Headers there is a `x-csrftoken`, the specific definition please turn all kinds of white hat tutorial. The value of this parameter is the same as the csrftoken of the cookie! [Csrftoken-same-cookie] (https://ww4.sinaimg.cn/large/006tNbRwgw1farvqqqf7mj30r20vi12i.jpg)
2. HTTP Request Headers which have a bunch of `:` at the beginning of the parameters, it is not necessary to ignore it
3. ingress All APIs are POST JSON data interactions, so HTTP Method and Conten-Type are determined, and the following is not repeated
4. Through the Intel Map API parameters v equivalent to Session, as to how to get, please see the Intel home page source code Finally, the name of the js contains the v value, that is, the next part of the code! [V] (http : //ww3.sinaimg.cn/large/006tNbRwgw1farw1l250nj31kw0atdkd.jpg)

## getGameScore

Obviously, this API is used to get the whole game score

API URL: https://www.ingress.com/r/getGameScore
API Request: v,
API Response: result: [Enl, Res],

** example **:


! [GetGameScore] (https://ww3.sinaimg.cn/large/006tNbRwgw1farw9fbte8j31fc06sgnj.jpg)
! [GetGameScore] (https://ww3.sinaimg.cn/large/006tNbRwgw1farw6qgzgwj30ac05wq37.jpg)


## getRegionScoreDetails

This API is used to get the current score for that area

API URL: https://www.ingress.com/r/getRegionScoreDetails
API Request: latE6, lngE6, v,
API Response: result: {gameScore: [Res, Enl], regionName, regionVertices: [[Res, Enl], ...], scoreHistory: [[id, Enl, Res], ...], timeToEndOfBaseCycleMs, topAgents: [{Nick, team}]}

Specific latE6, lngE6 See below getEntities API



** example **:
! [Code] (http://ww1.sinaimg.cn/large/006tNbRwgw1fasr0bg6ehj31kw0aiwhd.jpg)
! [GetRegionScoreDetails] (https://ww2.sinaimg.cn/large/006tNbRwgww1fasqwpmodij30jy1csaf2.jpg)
! [GetRegionScoreDetails] (https://ww4.sinaimg.cn/large/006tNbRwgw1fasqwqamvwj30k00pywgy.jpg)


## getEntities

This is used to get Entity, that is, link, field, portal

API URL: https://www.ingress.com/r/getEntities
API Request: tileKeys: [tilename1, tilename2, ...], v,
API Response: result: {terename1: {gameEntities: [[guid, time, [type, faction, guid1, lng1, lat1, ...]], ...}}, ...}}

To explain what the various variables are mean

TileKeys is a string that represents a rectangle and is a high-level MBR (minimum unit matrix) of a controllable parameter. This is a query parameter for a typical two-dimensional map. The specific format is `zoom_x_y_minlevel_maxlevel_health`, where zoom represents the level of the region's amplification Is the value of the ingress.intelmap.zoom in the cookie, the point + or - button, the map zoom, the value of ± 1, on intel, this is worth 3-21, the maximum, the smaller the number of displayed the smaller the range, ; X, y for the current zoom area number, you can refer to [tilenames] (http://wiki.openstreetmap.org/wiki/Slippy_map_tilenames)

The next three are simple, minimal, corresponding to Intel on the choice of Level and Health of the three values, namely the minimum po level, the maximum po level, the maximum health percentage

This is related to the conversion of latitude and longitude and actual latitude and longitude in tilename, see [github-iitc] (https://github.com/iitc-project/ingress-intel-total-conversion/blob/7dc38a89e708318eb94c201d9cc6f2b5e158ab36/code/map_data_calc_tools.js # L159)

The following code is for the latitude and longitude tiles
! [Lat-lng] (https://ww4.sinaimg.cn/large/006tNbRwgw1farzd3oygpj31kw0q7jx0.jpg)

In addition, the return value of Lng_x_, Lat_x_, is 6 decimal places, time is determined to milliseconds Unix timestamp

Guid is the identity of each entity, all users, items, link, portal, ... have a unique logo GUID

Type is the type of the entity, e: lin (followed by six elements directly, two groups of guid latitude and longitude), r: field (followed by a list, three groups of guid latitude and longitude), p: portal (a group of guid latitude and longitude, after Is the level, the energy (0.0-100.0), the number of feet, po map, po name, [], false, false, null (these four do not understand what), update timestamp)

Faction stands for camp, E: Enl, R: Res

** example **

! [Code] (https://ww4.sinaimg.cn/large/006tNbRwgw1farzwpfnizj31g20aowh0.jpg)
Too long, paste a part
! [Example] (https://ww2.sinaimg.cn/large/006tNbRwgww1farzyfwdnwj31kw1astif.jpg)


## getPortalDetails

Used to get detailed information about the portal

API URL: https://www.ingress.com/r/getPortalDetail
API Request: guid, v
API Response: result: [type, faction, lng, lat, level, energy, resonator_nums, picurl, name, [], false, false, null, updatetime, [[agentname, modtype, modlevel, {a: value, Value}], ...], [['agentname', resonator_level, energy], ...], belongs, ["", "", []]]

Meaning is clear, that is, the details of the portal

{A: value, b: value} that is mod effect, see the following diagram is easy to understand

** example **

! [Code] (http://ww1.sinaimg.cn/large/006tNbRwgw1fas0j4xa0aj31h80amq5m.jpg)
! [Example1] (https://ww1.sinaimg.cn/large/006tNbRwgw1fas0ggoxy3j31fk1c4jz2.jpg)
! [Example2] (https://ww1.sinaimg.cn/large/006tNbRwgwg1fas0gkdjpej314c1ccgp5.jpg)
! [Example3] (https://ww3.sinaimg.cn/large/006tNbRwgww1fas0gn6bk8j30hi07m74b.jpg)

## getArtifactPortals

The guess is to see if a new portal is generated

API URL: https://www.ingress.com/r/getArtifactPortals
API Request: v
APi Resonse: ibid

## getPlexts

Used to get comm information

API URL: https://www.ingress.com/r/getPlexts
API Request: ascendingTimestampOrder, maxLatE6, maxLngE6, minLatE6, minLngE6, maxTimestampMs, minTimestampMs, tab, v
API Response: result: [[guid, updatetime, {plext: {categories, plextType, team, text, markup: [#]}}, ...]
According to different categories (1 = all, 2 = faction), different plextType, markup the list of different, explain the contents of #
SYSTEM_BROADCAST:
[PLATER: {plain, team}], [TEXT: {plain}], [PORTAL: {name, address, latE6, lngE6, name, plain, team}], [TEXT: {plain}], [TEXT: { Plain}], [TEXT: {plain}]
The latter three are the end of the message, such as '-' '28' 'Mus'
There may also be two tails, such as [TEXT] [PORTAL]
There may also be no message tail, such as the dump behavior
PLAYER_GENERATED:
[SECURE: {plain}]
[SENDER: {plain, team}], [TEXT: {plain}] [AT_PLAYER: {plain, team}]

Obviously [SECURE] is optional
Each occurrence of an occurrence will cause [AT_PLAYER] to appear and fill in the text with [TEXT], so this list is not long (I guess, 2333)


Explain E6, obviously this is 4 points, to determine a rectangular area, then the capture of the message from this area

AscendingTimestampOrder (Bool) that whether the output by time asc

! [Code] (https://ww2.sinaimg.cn/large/006tNbRwgw1fas1zzzm12j31kw0hv43f.jpg)
! [Example] (https://ww2.sinaimg.cn/large/006tNbRwgw1fas1yjmqelj31kw0nhq7w.jpg)

## https://www.ingress.com/r/sendPlext

Send a message

API URL:
API Request: latE6, lngE6, message, tab, v,
API Response: result

This API, see the top of the message can be received, pay attention to the value of tab is all or faction

## redeemReward

Use passcode

API URL: https://www.ingress.com/r/redeemReward
API Request: passcode, v,
API Response:
1. error


## sendInviteEmail

invite

API URL: https://www.ingress.com/r/sendInviteEmail
API Request: inviteeEmailAddress, v
API Response: result

## The first part ends

[Code] (https://github.com/lc4t/ingress-api)

# Game API (iOS)

## Get API

1. With Charles as the transit, the iPhone forwards to Charles and goes to ss
2. Install the HTTPS certificate

## Precautions

1. In the HTTP Request Headers, there are also X-XsrfToken
2. There are fixed values ​​User-Agent: Nemesis (gzip), Accept-Encoding: gzip, Accept-Language: Your own language
3. Also need Authorization in HTTP Request Headers
4. Identity:
5.
6. No special instructions are POST request
7. API used in the domain name: (gstatic) | (upsight-api) | (appspot.com) | (google-analytics.com)
8. google-analytics one for a, are GET request, in the course of the game this bag too lazy to write

## Google Data Statistics

During this time User-Agent is Ingress / 1.110.0 CFNetwork / 808.2.16 Darwin / 16.3.0

### Start

When you open the game, the first request is sent to https://www.googleadservices.com/pagead/conversion/999636601/

Say the next parameter

Appversion: 1.110.0
Auto: 1
Bundleid: com.google.ingress
Lat: 1
Muid: n4nISlWfVzY2pH_42u0NMw
Osversion: 10.2
Sdkversion: ct-sdk-i-v3.1.1
Timestamp: 1457613595.596337
Usage_tracking_enabled: 1

Only timestamp is changing

### Config

Followed by the POST request configuration file, https: //bootstrap.upsight-api.com/config/v1/f1f4c1b2962446b38f72d5a456aac73c/

Note that the beginning of this, Headers with an X-US-Ref-Id (when asked to upsight-api.com when there are several other API station with X-XsrfToken), each time to start the APP value

The contents of this request are many,
`` `Json
{
"Sdk.version": "4.0.2",
"Device.manufacturer": "Apple",
"Screen.width": 375,
"App.token": "f1f4c1b2962446b38f72d5a456aac73c",
"App.version": "1.110.0",
"Device.limit_ad_tracking": true,
"Screen.height": 667,
"Bundle.hash": *,
"Ids.idfv": *,
"Sdk.build": "+ release.91a51d8",
"Request_ts": 1481902230,
"Device.carrier": "China Unicom",
"Device.hardware": "iPhone8,1",
"Locale": "zh_CN",
"App.bundleid": "com.google.ingress",
"Sid": "622357641234265637",
"Location.tz": "+0800",
"Device.connection": "Wifi",
"Device.os_version": "10.2",
"Device.type": "phone",
"Identifiers": "pub",
"Opt_out": false,
"Device.jailbroken": false,
"Screen.scale": 2,
"Device.os": "ios",
"Sessions": [{
"Session_num": 5558,
"Install_ts": 1457609995,
"Session_start": 1481901886,
"Events": [{
"Ts": 1481902230,
"Seq_id": 252628,
"Type": "upsight.config.expired",
"User_attributes": {
"Days_inactive": -1
"Hashed_user_id": *,
"Player_approx_lat": 30.76,
"Player_approx_lng": 103.93,
"Language": "zh_CN",
"Faction": "ALIENS",
"Distance_to_portal": -1,
"Agent_level": 13
}
}],
"Past_session_time": 2873649
}]
} `` `

You can see here is actually upload the session of the backup, device information, APP information

Will return a configuration

`` `Json
{
"Errobj": null,
"Response": [{
"Content": {
"ConfigurationList": [{
"Configuration": {
"Queues": [{
"Max_age": 120,
"Count_network_fail_retries": false,
"Name": "batch",
"Retry_interval": 60,
"Host": "batch.upsight-api.com",
"Max_retry_count": 3,
"Url_fmt": "{protocol}: // {host} / batch / {version} / {app_token} /"
"Protocol": "https",
"Max_size": 50
}, {
"Max_age": 0,
"Count_network_fail_retries": true,
"Name": "immediate",
"Retry_interval": 5,
"Host": "single.upsight-api.com",
"Max_retry_count": 1,
"Url_fmt": "{protocol}: // {host} / sdk / {version} / events / {app_token} /"
"Protocol": "https",
"Max_size": 1
}, {
"Max_age": 0,
"Count_network_fail_retries": true,
"Name": "config",
"Retry_interval": 15,
"Host": "bootstrap.upsight-api.com",
"Max_retry_count": 2,
"Url_fmt": "{protocol}: // {host} / config / {version} / {app_token} /"
"Protocol": "https",
"Max_size": 1
}],
"Identifiers": [{
"Ids": ["ids.idfv", "ids.android_id"],
"Name": "pub"
}, {
"Ids": ["ids.idfa", "ids.aid"],
"Name": "ad"
}],
"Route_filters": [{
"Filter": "*",
"Queues": ["batch", "trash"]
}, {
"Filter": "upsight. *",
"Queues": ["batch", "trash"]
}, {
"Filter": "upsight.milestone",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.uxm.enumerate",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.content.unrendered",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.campaign. *",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.content. *",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.comm.register",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.comm.unregister",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.session.start",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "pub.RequestContentSize. *",
"Queues": ["trash"]
}, {
"Filter": "pub.RequestDuration. *",
"Queues": ["trash"]
} {
"Filter": "pub.RequestPayloadSize. *",
"Queues": ["trash"]
}, {
"Filter": "upsight.config.expired",
"Queues": ["config", "trash"]
} {
"Filter": "upsight.monetization.iap",
"Queues": ["immediate", "batch", "trash"]
}, {
"Filter": "upsight.data_collection",
"Queues": ["immediate", "batch", "trash"]
}],
"Identifier_filters": [{
"Filter": "*",
"Identifiers": "pub"
}]
},
"Type": "upsight.configuration.dispatcher"
}, {
"Configuration": {
"RetryMultiplier": 120,
"RequestInterval": 3600,
"RetryPowerBase": 1.61,
"RetryPowerExponentMax": 10
},
"Type": "upsight.configuration.configurationManager"
}, {
"Configuration": {
"Push_token_ttl": 604800,
"Auto_register": true
},
"Type": "upsight.configuration.push"
}]
},
"Type": "upsight.configuration"
}],
"Error": null
}
`` ``

Here is not elaborate



### batch

This is POST to https://batch.upsight-api.com/batch/v1/f1f4c1b2962446b38f72d5a456aac73c/

`` `Json
{
"Sdk.version": "4.0.2",
"Device.manufacturer": "Apple",
"Screen.width": 375,
"App.token": "*",
"App.version": "1.110.0",
"Device.limit_ad_tracking": true,
"Screen.height": 667,
"Bundle.hash": "*",
"Ids.idfv": "*",
"Sdk.build": "+ release.91a51d8",
"Request_ts": 1481902232,
"Device.carrier": "China Unicom",
"Device.hardware": "iPhone8,1",
"Locale": "zh_CN",
"App.bundleid": "com.google.ingress",
"Sid": *,
"Location.tz": "+0800",
"Device.connection": "Wifi",
"Device.os_version": "10.2",
"Device.type": "phone",
"Identifiers": "pub",
"Opt_out": false,
"Device.jailbroken": false,
"Screen.scale": 2,
"Device.os": "ios",
"Sessions": [{
"Session_num": 5558,
"Install_ts": 1457609995,
"Session_start": 1481901886,
"Events": [{
"Ts": 1481902146,
"Seq_id": 252627,
"Type": "upsight.session.pause",
"User_attributes": {
"Days_inactive": -1
"Hashed_user_id": "*",
"Player_approx_lat": *,
"Player_approx_lng": *,
"Language": "zh_CN",
"Faction": "ALIENS",
"Distance_to_portal": -1,
"Agent_level": 13
}
}, {
"Ts": 1481902230,
"Seq_id": 252629,
"Type": "upsight.session.resume",
"User_attributes": {
"Days_inactive": -1
"Hashed_user_id": "*",
"Player_approx_lat": *,
"Player_approx_lng": *,
"Language": "zh_CN",
"Faction": "ALIENS",
"Distance_to_portal": -1,
"Agent_level": 13
}
}, {
"Ts": 1481902230,
"Pub_data": {
"Ui_type": "FOREGROUND",
"GL_MAX_TEXTURE_SIZE": 4096
},
"Seq_id": 252630,
"Type": "pub.Stats.GL_MAX_TEXTURE_SIZE",
"User_attributes": {
"Days_inactive": -1
"Hashed_user_id": "*",
"Player_approx_lat": *,
"Player_approx_lng": *,
"Language": "zh_CN",
"Faction": "ALIENS",
"Distance_to_portal": -1,
"Agent_level": 13
}
}],
"Past_session_time": 2873649
}]
}
`` ``
And the difference above is the event, interested diff look

What to return

`` `Json
{
"Errobj": null,
"Error": null,
"Response": []
}
`` ``

### OAuth2 login

Here is OAuth2 certification, not the focus, are omitted

The last API is POST to https://www.googleapis.com/oauth2/v4/token,

Client_id
Code
Grant_type
Redirect_uri
Verifier

Then still returns a JSON,
`` `Json
{
"Access_token": *,
"Token_type": "Bearer",
"Expires_in" .: 3600,
"Refresh_token": *,
"Id_token": *
}
`` ``

** from here to bring Authorizatoin ** (to appspot request)

Its format is `Bearer access_token`

#### collect

After that, ingress will send a statistic, GET pass to `https: // ssl.google-analytics.com / collect`

Where the User-Agent is set to `GoogleAnalytics / 3.10 (iPhone; U; CPU iPhone OS 10.2 like Mac OS X; zh-hans-cn-cn)

Let's talk about the parameters below


An: niantic-ingress
Av: 1.110.0
A: 1360117771
Tid: UA-30116200-4
Cd: NiOSAuthenticationInfoViewController
T: screenview
Cid: *
Ul: zh-hans-cn
Aid: com.google.ingress
Ds: app
_u: *
Sr: 375x667
V: 1
_crc: 0
_v: mi3.1.0
Sf: 100
Ht: 1481901425098
Qt: 155026
z: *

### hanshake

URL: https://m-dot-betaspike.appspot.com/handshake

Here is still POST form submitted, but the content is JSON, suspected to be wrong

`` `Json
{
"NemesisSoftwareVersion": "2016-11-18T13: 44: 21Z + c90631584ea7-i + opt",
"DeviceSoftwareVersion": "10.2",
"A": *,
"Reason": "SUP"
}
`` ``

It's important to return something
First is a crack string `)]} '`
Then was a huge JSON, returned:
```json
{
    "result": {
        "playerEntity": [user_guid, 1481899737145, {
            "playerPersonal": {
                "ap": "14031060",
                "energy": 10255,
                "allowNicknameEdit": false,
                "allowFactionChoice": false,
                "verifiedLevel": 13,
                "mediaHighWaterMarks": {
                    "General": 1809,
                    "RESISTANCE": 1328,
                    "ALIENS": 1327
                },
                "energyState": "XM_OK",
                "notificationSettings": {
                    "shouldSendEmail": false,
                    "maySendPromoEmail": false,
                    "shouldPushNotifyForAtPlayer": true,
                    "shouldPushNotifyForPortalAttacks": true,
                    "shouldPushNotifyForInvitesAndFactionInfo": true,
                    "shouldPushNotifyForNewsOfTheDay": true,
                    "shouldPushNotifyForEventsAndOpportunities": true,
                    "locale": "zh-Hans-CN"
                },
                "profileSettings": {
                    "areMetricsPublic": false
                },
                "verificationState": "UNVERIFIED",
                "extraXmTankEnergy": 0
            },
            "controllingTeam": {
                "team": "RESISTANCE"
            },
            "avatar": {
                "foreground": {
                    "layerId": "foreground4",
                    "tintColor": 16249235,
                    "imageUrl": "http://lh3.googleusercontent.com/COPtJLf8PEP0dgeehOg425-ASLkkQtrphoViG6Hy19mbHtyBDvanqWB4a6jPCaHhYpLYNaw9Ot9-0cfYm0Qc"
                },
                "background": {
                    "layerId": "background3",
                    "tintColor": 481625,
                    "imageUrl": "http://lh3.googleusercontent.com/WorWGSonh3XJrC7RWpVuBkTKcHoF2QtsolzqhmNUrZcaHKmfRgpTO_5QmpYTbBvkLkbTlF5LRW9gS_xDmTdY"
                }
            }
        }],
        "nickname": *,
        "xsrfToken": *,
        "storage": {
            "tutorial_complete_scanner_intro": "true:delim:1481895477261:delim:true",
            "game_intro_has_played": "true:delim:1457410095779:delim:true",
            "mission_complete_7": "ENDED_BY_NAGGING:delim:1457876587760:delim:true",
            "mission_complete_6": "ENDED_BY_NAGGING:delim:1457876587759:delim:true",
            "mission_complete_5": "ENDED_BY_NAGGING:delim:1457876587758:delim:true",
            "mission_complete_4": "SUCCESS:delim:1457876587753:delim:true",
            "mission_complete_3": "ENDED_BY_NAGGING:delim:1457876587757:delim:true",
            "mission_complete_0": "ENDED_BY_NAGGING:delim:1457876587754:delim:true",
            "mission_complete_1": "ENDED_BY_NAGGING:delim:1457876587755:delim:true",
            "mission_complete_2": "ENDED_BY_NAGGING:delim:1457876587756:delim:true",
            "all_missions_complete_announcement_made": "true:delim:1457410237730:delim:true",
            "tutorial_complete_hack": "true:delim:1457410237722:delim:true",
            "training_portal_photo_url": "http://lh6.ggpht.com/xZU07oPfUpRDnz_2og2L1E3O4T74iMUyCVtB5lL6m16lh0PnZPtBMf8XjkpYbq8Veby0eHNhCGNRwQrYNg:delim:1457439507178:delim:true",
            "training_portal_lng_degrees": *,
            "training_portal_lat_degrees": *,
            "training_portal_title": "本科9栋:delim:1457439507178:delim:true",
            "tutorial_complete_xm": "true:delim:1457412143790:delim:true",
            "tutorial_complete_xmp": "true:delim:1457412131239:delim:true",
            "tutorial_complete_profile_link": "true:delim:1457422332944:delim:true",
            "tutorial_complete_deploy": "true:delim:1457488741871:delim:true"
        },
        "pregameStatus": {
            "action": "NO_ACTIONS_REQUIRED",
            "dialogText": ""
        },
        "initialKnobs": {
            "bundleMap": {
                "ClientFeatureKnobs": {
                    "enableEmbeddedYouTubePlayback": true,
                    "showGlobalMap": true,
                    "logSkipRegex": "\\b\\B",
                    "enableParticleFilter": true,
                    "enableGAViolationReporting": false,
                    "portalKeyCardRefreshIntervalSecs": 5,
                    "portalInfoRefreshIntervalSecs": 2,
                    "enableInviteNag": true,
                    "inviteNagDelayDays": 14,
                    "enableVerificationNag": false,
                    "verificationNagDelayDays": 3,
                    "refreshTimeMs": "-1",
                    "playerProfileCacheExpirationSecs": "60",
                    "enableDelayGpsPause": true,
                    "enableAdvancedAnalytics": true,
                    "playerVerificationEnabled": 0,
                    "enableExtraNativeRenderedText": -1,
                    "enableShareMission": 10740,
                    "profileUrlFormat": "http://plus.google.com/%s",
                    "profileUrlFormatApp": "gplus://plus.google.com/%s",
                    "factionChoiceBeforeSpaceToFace": true,
                    "minGlobBundleSizeXm": 1500,
                    "enableMissions": 10610,
                    "enableBadAnalyticsTrackingEventLogging": 10570,
                    "enableIOSPushNotifications": 10640,
                    "iOSPortalDiscoveryProcessIntervalMS": "200000",
                    "enableMissionsConnectivityRecovery": 10610,
                    "enableMissionsStatePolling": 10610,
                    "enableHackLongPressIndicator": 10612,
                    "enableIOSNativeYoutubePlayer": 10620,
                    "enableIOSPortalDetailPanel": 10630,
                    "enableIOSMultiPhotoViewer": 10640,
                    "enableIOSPhotoSubmission": 10640,
                    "enableIOSGridPhotoViewer": 10650,
                    "enableCommunityTab": 10740,
                    "enableBoostRechargeV2": 10760,
                    "enableSetLocale": 10771,
                    "disableNewPortalSubmissions": -1,
                    "enableLocalLinkChecks": 10790,
                    "enableStore": 10851,
                    "enableRecycleConfirmationDialog": 0,
                    "enableGlyphCommandChannel": 10900,
                    "enableGlyphRedoButton": 10910,
                    "upsightLocationDecimalPlacesToRound": 2,
                    "enableExtendedFireMenu": 11010
                },
                "SmartNotificationsClientKnobs": {
                    "enableSmartNotifications": 10670,
                    "maximumCachedHandshakeResultAgeMs": "43200000",
                    "enableR": 10691,
                    "enableRIOS": 10700,
                    "playerRProbabilityE6": 1000000,
                    "rLPlayerMs": "864000000",
                    "rMinPollingDelayMs": "900000",
                    "maximumPortalCacheSize": 5000,
                    "portalCacheEntryExpirationTimeMs": "1209600000",
                    "nearbyPortalDistanceM": 200,
                    "loiteringMs": "300000",
                    "noLongerNearbyDistanceM": 100,
                    "rNearbyPortalDistanceGrowthRate": 50,
                    "rMaxGrownNearbyPortalDistanceM": 1000,
                    "rWaitPeriodsMs": ["86400000", "172800000", "259200000", "432000000"],
                    "rIOSAlwaysLocationAccessMinIntervalMs": "5256000000",
                    "amSessionLengthMs": "1800000",
                    "enableAm": 10700,
                    "playerAmProbabilityE6": 1000000,
                    "amNearbyPortalDistanceM": 37,
                    "amNoLongerNearbyDistanceM": 40,
                    "amAndroidLocationRequestPriority": 102,
                    "amAndroidLocationRequestIntervalMs": "30000",
                    "amAndroidLocationRequestFastestIntervalMs": "5000",
                    "amAndroidLocationRequestSmallestDisplacementM": 0
                },
                "TranslationClientKnobs": {
                    "enableCloudTranslations": 10860,
                    "translationUrls": {
                        "zh_TW.json": "http://storage.googleapis.com/ingress-translations/2016.11.04/zh_TW.json",
                        "ru": "http://storage.googleapis.com/ingress-translations/2016.11.04/ru.json",
                        "fr": "http://storage.googleapis.com/ingress-translations/2016.11.04/fr.json",
                        "en": "http://storage.googleapis.com/ingress-translations/2016.11.04/en.json",
                        "nl": "http://storage.googleapis.com/ingress-translations/2016.11.04/nl.json",
                        "pt": "http://storage.googleapis.com/ingress-translations/2016.11.04/pt.json",
                        "no": "http://storage.googleapis.com/ingress-translations/2016.11.04/no.json",
                        "de": "http://storage.googleapis.com/ingress-translations/2016.11.04/de.json",
                        "ko": "http://storage.googleapis.com/ingress-translations/2016.11.04/ko.json",
                        "it": "http://storage.googleapis.com/ingress-translations/2016.11.04/it.json",
                        "sv": "http://storage.googleapis.com/ingress-translations/2016.11.04/sv.json",
                        "pl": "http://storage.googleapis.com/ingress-translations/2016.11.04/pl.json",
                        "cs": "http://storage.googleapis.com/ingress-translations/2016.11.04/cs.json",
                        "zh_CN.json": "http://storage.googleapis.com/ingress-translations/2016.11.04/zh_CN.json",
                        "ja": "http://storage.googleapis.com/ingress-translations/2016.11.04/ja.json",
                        "es": "http://storage.googleapis.com/ingress-translations/2016.11.04/es.json"
                    }
                },
                "GlyphCommandClientKnobs": {
                    "commands": {
                        "ikj": "GLYPH_ACCEPT_MORE_KEYS",
                        "jkgh": "GLYPH_ACCEPT_COMPLEX_HACK",
                        "hkg": "GLYPH_ACCEPT_LESS_KEYS",
                        "ji": "GLYPH_ACCEPT_SIMPLE_HACK"
                    },
                    "hints": ["GLYPH_HINT_LESS_KEYS", "GLYPH_HINT_MORE_KEYS", "GLYPH_HINT_SIMPLE_HACK"]
                },
                "StagingKnobs": {
                    "playerStoreProbabilityE6": 1000000,
                    "oldAnalyticsProbabilityE6": 1000000,
                    "newAnalyticsProbabilityE6": 1000000
                },
                "InventoryKnobs": {
                    "maxInventoryItems": 2000,
                    "resourceLimits": {
                        "KEY_CAPSULE": 5
                    },
                    "cmuRestockItemId": "ingress.cmu.large",
                    "itemRestockResourceToId": {},
                    "powerupRestockToId": {
                        "LOOK": "ingress.beacon.look",
                        "RES": "ingress.beacon.res",
                        "NIA": "ingress.beacon.nia",
                        "ENL": "ingress.beacon.enl",
                        "FRACK": "ingress.fracker",
                        "MEET": "ingress.beacon.meet"
                    }
                },
                "PortalKnobs": {
                    "resonatorLimits": {
                        "bands": [{
                            "remaining": 1,
                            "applicableLevels": [8]
                        }, {
                            "remaining": 4,
                            "applicableLevels": [2]
                        }, {
                            "remaining": 8,
                            "applicableLevels": [1]
                        }, {
                            "remaining": 2,
                            "applicableLevels": [5]
                        }, {
                            "remaining": 4,
                            "applicableLevels": [3]
                        }, {
                            "remaining": 2,
                            "applicableLevels": [6]
                        }, {
                            "remaining": 4,
                            "applicableLevels": [4]
                        }, {
                            "remaining": 1,
                            "applicableLevels": [7]
                        }]
                    },
                    "maxModsPerPlayer": 2,
                    "radiusParamsSet": [],
                    "allowRegionSpecificSubmissions": false,
                    "minLevelForSubmission": 2
                },
                "recycleKnobs": {
                    "recycleValuesMap": {
                        "BOOSTED_POWER_CUBE": [160, 160, 160, 160, 160, 160, 160, 160],
                        "MULTIHACK": [20, 40, 60, 80, 100, 120, 140, 160],
                        "EMP_BURSTER": [20, 40, 60, 80, 100, 120, 140, 160],
                        "INTEREST_CAPSULE": [20, 40, 60, 80, 100, 120, 140, 160],
                        "TURRET": [20, 40, 60, 80, 100, 120, 140, 160],
                        "MEDIA": [20, 40, 60, 80, 100, 120, 140, 160],
                        "EXTRA_SHIELD": [20, 40, 60, 80, 100, 120, 140, 160],
                        "ULTRA_STRIKE": [20, 40, 60, 80, 100, 120, 140, 160],
                        "POWER_CUBE": [1000, 2000, 3000, 4000, 5000, 6000, 7000, 8000],
                        "FLIP_CARD": [20, 40, 60, 80, 100, 120, 140, 160],
                        "ULTRA_LINK_AMP": [20, 40, 60, 80, 100, 120, 140, 160],
                        "CAPSULE": [20, 40, 60, 80, 100, 120, 140, 160],
                        "HEATSINK": [20, 40, 60, 80, 100, 120, 140, 160],
                        "RES_SHIELD": [20, 40, 60, 80, 100, 120, 140, 160],
                        "PORTAL_POWERUP": [20, 40, 60, 80, 100, 120, 140, 160],
                        "LINK_AMPLIFIER": [20, 40, 60, 80, 100, 120, 140, 160],
                        "EMITTER_A": [20, 40, 60, 80, 100, 120, 140, 160],
                        "PORTAL_LINK_KEY": [500, 500, 500, 500, 500, 500, 500, 500],
                        "KEY_CAPSULE": [20, 40, 60, 80, 100, 120, 140, 160],
                        "FORCE_AMP": [20, 40, 60, 80, 100, 120, 140, 160]
                    }
                },
                "WeaponRangeKnobs": {
                    "xmpDamageRangeMap": {
                        "1": 42,
                        "3": 58,
                        "2": 48,
                        "5": 90,
                        "4": 72,
                        "7": 138,
                        "6": 112,
                        "8": 168
                    },
                    "ultraStrikeDamageRangeMap": {
                        "1": 10,
                        "3": 16,
                        "2": 13,
                        "5": 21,
                        "4": 18,
                        "7": 27,
                        "6": 24,
                        "8": 30
                    },
                    "maxPowerBoostPercentage": 20,
                    "powerBoostPeriodHardS": 0.6,
                    "powerBoostPeriodEasyS": 1.5,
                    "powerBoostWindowHardS": 0.06,
                    "powerBoostWindowEasyS": 0.1,
                    "powerBoostPowAnimation": 1.0,
                    "powerBoostPowScore": 1.0,
                    "maxFireRateSeconds": 1.5
                },
                "XmCostKnobs": {
                    "xmpFiringCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "shieldDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "linkAmplifierDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "forceAmplifierDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "heatsinkDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "multihackDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "turretDeployCostByLevel": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                    "resonatorDeployCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "resonatorUpgradeCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "portalHackFriendlyCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "portalHackNeutralCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "portalHackEnemyCostByLevel": [50, 100, 150, 200, 250, 300, 350, 400],
                    "flipCardCostByLevel": [1000, 2000, 3000, 4000, 5000, 6000, 7000, 8000],
                    "portalModByLevel": {
                        "MULTIHACK": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "HEATSINK": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "TURRET": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "EXTRA_SHIELD": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "FORCE_AMP": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "ULTRA_LINK_AMP": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "RES_SHIELD": [200, 400, 600, 800, 1000, 1200, 1400, 1600],
                        "LINK_AMPLIFIER": [200, 400, 600, 800, 1000, 1200, 1400, 1600]
                    },
                    "powerupByLevel": {
                        "PORTAL_POWERUP": [200, 400, 600, 800, 1000, 1200, 1400, 1600]
                    },
                    "boostRechargeResonatorsCost": 15000
                },
                "ScannerKnobs": {
                    "updateIntervalMs": 30000,
                    "updateDistanceM": 10,
                    "maxUpdateIntervalMs": 30000,
                    "minUpdateIntervalMs": 5000,
                    "updateIntervalThrottlingTriggerVelocityMps": 3.0,
                    "updateIntervalThrottlingRate": 0.65,
                    "rangeM": 300,
                    "missionsUpdateIntervalMs": 60000,
                    "missionsUpdateDistanceM": 500,
                    "maxLinkAutoCheckRangeM": 200000,
                    "reportRequestPerformancePercentE6": 0,
                    "crittercismSendPerfDataProbabilityE6": 0,
                    "crittercismSendLogcatProbabilityE6": 0
                },
                "PortalModSharedKnobs": {
                    "diminishingValues": {
                        "FORCE_AMPLIFIER": [1000, 250, 125, 125],
                        "ATTACK_FREQUENCY": [1000, 250, 125, 125],
                        "LINK_RANGE_MULTIPLIER": [1000, 250, 125, 125],
                        "TEMP_LINK_RANGE_MULTIPLIER_SORTED": [1000, 250, 125, 125],
                        "TEMP_ATTACK_FREQUENCY_SORTED": [1000, 250, 125, 125],
                        "BURNOUT_INSULATION": [1000, 500, 500, 500],
                        "HACK_SPEED": [1000, 500, 500, 500],
                        "TEMP_FORCE_AMPLIFIER_SORTED": [1000, 250, 125, 125]
                    },
                    "directValues": {
                        "OUTGOING_LINKS_BONUS": [8, 8, 8, 8]
                    }
                },
                "NearbyPortalKnobs": {
                    "repopulateDistanceMeters": 500.0,
                    "repopulateTimeMilliseconds": "30000"
                },
                "PlayerAnnounceSharedKnobs": {
                    "featureActivationDate": "1377241200000"
                },
                "PlayerVerificationSharedKnobs": {
                    "unverifiedXmCapacity": 3000,
                    "unverifiedInventoryCapacity": 100,
                    "unverifiedApCapacity": 19999,
                    "enableVerificationFeature": false,
                    "enableVerifiedGameActions": false,
                    "numPinCodeDigits": 4
                },
                "PlayerLevelKnobs": {
                    "levelUpRequirements": {
                        "11": {
                            "apRequired": "6000000",
                            "achievementsRequired": {
                                "SILVER": 6,
                                "GOLD": 4
                            }
                        },
                        "10": {
                            "apRequired": "4000000",
                            "achievementsRequired": {
                                "SILVER": 5,
                                "GOLD": 2
                            }
                        },
                        "13": {
                            "apRequired": "12000000",
                            "achievementsRequired": {
                                "PLATINUM": 1,
                                "GOLD": 7
                            }
                        },
                        "12": {
                            "apRequired": "8400000",
                            "achievementsRequired": {
                                "SILVER": 7,
                                "GOLD": 6
                            }
                        },
                        "15": {
                            "apRequired": "24000000",
                            "achievementsRequired": {
                                "PLATINUM": 3
                            }
                        },
                        "14": {
                            "apRequired": "17000000",
                            "achievementsRequired": {
                                "PLATINUM": 2
                            }
                        },
                        "16": {
                            "apRequired": "40000000",
                            "achievementsRequired": {
                                "PLATINUM": 4,
                                "BLACK": 2
                            }
                        },
                        "1": {
                            "apRequired": "0",
                            "achievementsRequired": {}
                        },
                        "3": {
                            "apRequired": "20000",
                            "achievementsRequired": {}
                        },
                        "2": {
                            "apRequired": "2500",
                            "achievementsRequired": {}
                        },
                        "5": {
                            "apRequired": "150000",
                            "achievementsRequired": {}
                        },
                        "4": {
                            "apRequired": "70000",
                            "achievementsRequired": {}
                        },
                        "7": {
                            "apRequired": "600000",
                            "achievementsRequired": {}
                        },
                        "6": {
                            "apRequired": "300000",
                            "achievementsRequired": {}
                        },
                        "9": {
                            "apRequired": "2400000",
                            "achievementsRequired": {
                                "SILVER": 4,
                                "GOLD": 1
                            }
                        },
                        "8": {
                            "apRequired": "1200000",
                            "achievementsRequired": {}
                        }
                    },
                    "maxPlayerLevel": 16,
                    "xmCapacityByLevel": {
                        "11": 14500,
                        "10": 13000,
                        "13": 17500,
                        "12": 16000,
                        "15": 20500,
                        "14": 19000,
                        "16": 22000,
                        "1": 3000,
                        "3": 5000,
                        "2": 4000,
                        "5": 7000,
                        "4": 6000,
                        "7": 9000,
                        "6": 8000,
                        "9": 11500,
                        "8": 10000
                    },
                    "boostedPowerCubeCapacityByLevel": {
                        "11": 40800,
                        "10": 38400,
                        "13": 45600,
                        "12": 43200,
                        "15": 50400,
                        "14": 48000,
                        "16": 52800,
                        "1": 18000,
                        "3": 22500,
                        "2": 20250,
                        "5": 27000,
                        "4": 24750,
                        "7": 31500,
                        "6": 29250,
                        "9": 36000,
                        "8": 33750
                    }
                },
                "MapCompositionRootKnobs": {
                    "mapAreas": [{
                        "description": "S Korea",
                        "epoch": 1,
                        "mapProvider": "OSM",
                        "boundingRects": [{
                            "north": 37.69547616496338,
                            "south": 34.83562779241368,
                            "east": 131.16790087890627,
                            "west": 124.44700830078114
                        }, {
                            "north": 37.69547616496338,
                            "south": 33.025620228813466,
                            "east": 128.76189501953127,
                            "west": 124.44700830078114
                        }, {
                            "north": 37.84094374831599,
                            "south": 37.561299473030594,
                            "east": 126.86126025390627,
                            "west": 126.18010156249989
                        }, {
                            "north": 38.358197906100926,
                            "south": 37.62589818166485,
                            "east": 129.34966357421877,
                            "west": 126.65800683593739
                        }, {
                            "north": 38.634814820135226,
                            "south": 38.358197906100926,
                            "east": 128.55040820312502,
                            "west": 128.14116113281239
                        }]
                    }, {
                        "description": "World",
                        "epoch": -1,
                        "mapProvider": "GMM",
                        "mapProviderName": "",
                        "boundingRects": [{
                            "north": 90.0,
                            "south": -90.0,
                            "east": 180.0,
                            "west": -180.0
                        }]
                    }],
                    "mapProviders": [{
                        "name": "GMM",
                        "baseUrl": "http://mobilemaps.clients.google.com/glm/mmap",
                        "queryFormat": ""
                    }, {
                        "name": "OSM",
                        "baseUrl": "http://maps.nianticlabs.com",
                        "queryFormat": "ntp-maptiles/epoch-%04d/%s.gmm"
                    }]
                }
            },
            "syncTimestamp": "1481903753065"
        }
    }
}
```

Here things can do a tutorial, basically, the effect of various items, upgrade the requirements are written in the inside very clear, welcome to other students to be a statistic

For example, ultraStrikeDamageRangeMap describes the level of US role at each level

Here is probably for the heat update, the settings on the server (anyway, run my traffic is not, gg)



### setLocale

Then POST to https://m-dot-betaspike.appspot.com/rpc/emptyBasket/setLocale

Direct form content is simple and violent

{"Params": ["zh-Hans-CN"]}

Return is also an empty {}

### globalRegionMap

This is GET to https://m-dot-betaspike.appspot.com/globalRegionMap Get the game to start the picture of the rotation of the earth

Response is a 256 * 128 black and green map


### batch for the second time

And trigger a device information upload, in addition to request_ts updated, a little different, in the event

`` `Json
"Pub_data": {
"Type": "upsight.session.pause",
"Ui_type": "FOREGROUND"
},
...
"Type": "pub.StartupWheelsOfAdaSubActivity",
`` ``



This is all before clicking `I understand the` button before the request


## Game API

### getGameScore

Obviously used to get the game's score

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getGameScore
API Request: params: []
API Response:

`` `Json
{
"Result": {
"AlienScore": "569243223"
"ResistanceScore": "723155462"
},
"GameBasket": {
"GameEntities": [],
"Inventory": [],
"DeletedEntityGuids": []
}
}
`` ``

Too lazy to explain, directly posted back to the package it

## getObjectsInCells

Used to get map elements

API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/getObjectsInCells
API Request: params: {clientBasket: {clientBlob}, knobSyncTimestamp, energyGlobGuids: [], playerLocation: value, dates: [value], cellsAsHex: [...]

Here clientBasket is to verify the information, the whole process will not change, probably to reverse the knowledge of the algorithm

PlayerLocation is 'hex, hex' such string, latitude longitude (6 decimal places to remove the decimal point) into hexadecimal after the space connection

Dates is a 20-member list (not sure)

CellsAsHex is the cell token token, 20 16-bit hex value of the list


API Response:

```json
{
	"result": "1481906071300",
	"gameBasket": {
		"gameEntities": [
			[guid, time, {
				"capturedRegion": {
					"vertexA": {
						"guid": guid,
						"location": {
							"latE6": *,
							"lngE6": *
						}
					},
					"vertexB": {
						...
					},
					"vertexC": {
						...
					}
				},
				"entityScore": {
					"entityScore": "8"
				},
				"creator": {
					"creatorGuid": user_guid,
					"creationTime": time
				},
				"controllingTeam": {
					"team": "RESISTANCE"
				}
			}],
			...,
			[guid, time, {
				"photoStreamInfo": {
					"coverPhoto": {
						"portalImageGuid": *,
						"imageUrl": *,
						"attributionMarkup": ["PLAYER", {
							"plain": agent_name,
							"guid": guid,
							"team": "ALIENS"
						}],
						"voteCount": 0
					},
					"photoCount": 1
				},
				"locationE6": {
					"latE6": *,
					"lngE6": *
				},
				"discoverer": {
					"playerMarkupArgSet": {
						"plain": agent_name,
						"guid": *,
						"team": "ALIENS"
					}
				},
				"resonatorArray": {
					"resonators": [{
						"level": 3,
						"distanceToPortal": 28,
						"ownerGuid": agent_guid,
						"energyTotal": 1327,
						"slot": 0,
						"id": uuid
					}, ...]
				},
				"descriptiveText": {
					"map": {
						"TITLE": *,
						"ADDRESS": *
					}
				},
				"turret": {},
				"portalV2": {
					"linkedEdges": [],
					"linkedModArray": [{
						"installingUser": agent_guid,
						"stats": {
							"MITIGATION": "30",
							"REMOVAL_STICKINESS": "0"
						},
						"rarity": "COMMON",
						"displayName": "Portal Shield",
						"type": "RES_SHIELD"
					}, ...],
					"missionStartPoint": false
				},
				"controllingTeam": {
					"team": "RESISTANCE"
				},
				"captured": {
					"capturingPlayerId": agent_guid
				},
				"defaultActionRange": {}
			}],
			...,

			[guide, time, {
				"edge": {
					"originPortalGuid": guid,
					"destinationPortalGuid": guid,
					"originPortalLocation": {
						"latE6": *,
						"lngE6": *
					},
					"destinationPortalLocation": {
						"latE6": *,
						"lngE6": *
					}
				},
				"creator": {
					"creatorGuid": *,
					"creationTime": *
				},
				"controllingTeam": {
					"team": "RESISTANCE"
				}
			}],
			...,
			[guid, time, {
				"imageByUrl": {
					"imageUrl": *
				},
				"photoStreamInfo": {
					"coverPhoto": {
						"portalImageGuid": guid,
						"imageUrl": *,
						"attributionMarkup": ["PLAYER", {
							"plain": agent_name,
							"guid": guid,
							"team": "ALIENS"
						}],
						"voteCount": 5
					},
					"photoCount": 1
				},
				"locationE6": {
					"latE6": *,
					"lngE6": *
				},
				"discoverer": {
					"playerMarkupArgSet": {
						"plain": agent_name,
						"guid": *,
						"team": "ALIENS"
					}
				},
				"resonatorArray": {
					"resonators": [...]
				},
				"descriptiveText": {
					"map": {
						"TITLE": *,
						"DESCRIPTION": *,
						"ADDRESS": *
					}
				},
				"turret": {},
				"portalV2": {
					"linkedEdges": [{
						"edgeGuid": guid,
						"otherPortalGuid": guid",
						"isOrigin": false
					}, ...],
					"linkedModArray": [..., null],
					"missionStartPoint": false
				},
				"controllingTeam": {
					"team": "RESISTANCE"
				},
				"captured": {
					"capturingPlayerId": agent_guid
				},
				"defaultActionRange": {}
			}],
			...,
			[guid, time, {
				"timedPowerupSet": {
					"powerUpItems": []
				},
				"imageByUrl": {
					"imageUrl": *
				},
				"photoStreamInfo": {
					"coverPhoto": {
						"portalImageGuid": guid,
						"imageUrl": *,
						"attributionMarkup": ["PLAYER", {
							"plain": agent_user,
							"guid": guid,
							"team": "ALIENS"
						}],
						"voteCount": 2
					},
					"photoCount": 1
				},
				"locationE6": {
					"latE6": *,
					"lngE6": *
				},
				"discoverer": {
					"playerMarkupArgSet": {
						"plain": agent_user,
						"guid": guid,
						"team": "ALIENS"
					}
				},
				"resonatorArray": {
					"resonators": [...]
				},
				"descriptiveText": {
					"map": {
						"ADDRESS": *,
						"TITLE": (
					}
				},
				"turret": {},
				"portalV2": {
					"linkedEdges": [...],
					"linkedModArray": [null, null, null, null],
					"missionStartPoint": false
				},
				"controllingTeam": {
					"team": "RESISTANCE"
				},
				"captured": {
					"capturingPlayerId": agent_guid
				},
				"defaultActionRange": {}
			}],
			...,
		}],
		"apGains": [],
		"inventory": [],
		"deletedEntityGuids": [],
		"energyGlobGuids": [guid, ...],
		"energyGlobTimestamp": "1481906071300",
		"refreshEntityGuids": [guid, ...]
	}
}
```

clear. The The Do not say it

### getCurrentMission

API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/getCurrentMission
API Request: params: {clientBasket: {clientBlob}, knobSyncTimestamp, energyGlobGuids: [], playerLocation: value}
API Response: Task and gameBasket, here because I did not the current task, omitted


### getNewsOfTheDay


API URL: https: //m-dot-betaspike.appspot.com/rpc/playerUndecorated/getNewsOfTheDay
API Request: params: [value]

The value is a string that does not make any ghosts

API Response: gameBasket: {gameEntities: [], inventory: [], deletedEntityGuids: []}


This API has to be active when the test

### getPaginatedPlexts

Get comm message


API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/getPaginatedPlexts

API Request:
`` `Json
{
"Params": {
"ClientBasket": {
"ClientBlob": *
},
"KnobSyncTimestamp": *,
"EnergyGlobGuids": [],
"PlayerLocation": value,
"Categories": -1,
"AscendingTimestampOrder": false,
"DesiredNumItems": 100,
"MaxTimestampMs": -1,
"MinTimestampMs": 1481900888021,
"CellsAsHex": [...]
"FactionOnly": false
}
}
`` ``

This parameter can refer to the Intel Map

API Response:

`` `Json
{
"Result": [
[Guid, time, {
"LocationE6": {
"LatE6": *,
"LngE6": *
},
"Plext": {
"Text": *,
"Team": "ALIENS",
"Markup": [
["SENDER", {
"Plain": agent_name,
"Guid": *,
"Team": "ALIENS"
}],
["TEXT", {
"Plain": ""
}],
["AT_PLAYER", {
"Plain": *,
"Guid": guid,
"Team": "ALIENS"
}],
["TEXT", {
"Plain": *
}]
],
"PlextType": "PLAYER_GENERATED",
"Categories": 1
}
}]
],
"GameBasket": {
"GameEntities": [],
"PlayerEntity": [...]
"ApGains": [],
"Inventory": [],
"DeletedEntityGuids": [],
"ServerBlob": *
}
}
`` ``

Omitted some of the duplication, where you can also refer to the Intel Map API


### getInventory

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getInventory
API Request:
`` `Json
{
"Params": {
"LastQueryTimestamp": timestamp
}
}
`` ``

API Response: (only return the updated items)

`` ``
{
"Result": "1481911112158",
"GameBasket": {
"GameEntities": [],
"Inventory": [
["56b24e20197240d2a2330e7fb60fa6a1.5", 1473951636802, {
"InInventory": {
"PlayerId": *,
"AcquisitionTimestampMs": *
},
"ModResource": {
"DisplayName": "Heat Sink",
"Stats": {
"REMOVAL_STICKINESS": "0",
"HACK_SPEED": "200000"
},
"Rarity": "COMMON",
"ResourceType": "HEATSINK"
},
"DisplayName": {
"DisplayName": "Heat Sink",
"DisplayDescription": "Mod that reduces cooldown time between Portal hacks."
}
}], ...,
],
"DeletedEntityGuids": []
}
}
`` ``
Inventory can see what the updated items, forced synchronization, this is everything inside, very large. The (After all, each item a list ...)

### registerForApn

API URL: https://m-dot-betaspike.appspot.com/rpc/emptyBasket/registerForApn
API Request: params: [key, "com.google.ingress", "2048"]
API Response: {}

Used to register token


### unregisterForApn

API URL: https://m-dot-betaspike.appspot.com/rpc/emptyBasket/unregisterForApn
API Request:
API Response: {}



### getInviteInfo

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getInviteInfo
API Request: params: []
API Response:

`` `Json
{
"Result": {
"NumAvailableInvites": 298,
"InviteeToStatusMap": {
Email: status,
...
},
"ApGainOnInviteAccepted": "3000"
},
"GameBasket": {
"GameEntities": [],
"Inventory": [
],
"DeletedEntityGuids": []
}
}
`` ``



This API is good, can see their invitation to everyone and the state, or even ACCEPTED_ANOTHER_PLAYERS_INVITE

### putBulkPlayerStorage

API URL: https: //m-dot-betaspike.appspot.com/rpc/playerUndecorated/putBulkPlayerStorage
API Request: prams: [{tutorial_complete_scanner_intro: value}]

The value is a string of `true: delim: 1481912306360: delim: true` format

API Response:
`` `Json
{
"Result": "SUCCESS",
"GameBasket": {
"GameEntities": [],
"Inventory": [],
"DeletedEntityGuids": []
}
}
`` ``


### getPlayerProfile2

This is used to get their own information

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getPlayerProfile2
API Request: params: [agent_name]
API Response: result: {team, avatar: {foregroundLayer: {id, imageURL, layer}, backgroundLayer: {id, imageUrl, layer}, avatarColorForeground, avatarColorBackground}, metrics: [{metricName, metricCategory, formattedValueAllTime, formattedValue30Days, formattedValue7Days} , Which is a symbol, description, group, tiers: [{value, badgeImageUrl, locked}, ...], timestampAwarded], firstAchievementContinuationToken, ap, verifiedLeve, achievementCounts: {SILVER, GOLD, BLACK, PLATINUM CodeBasket, inventory, deletedEntityGuids}, gameBasket:

Are self-explanatory

Metrics is the statistics, such as Unique Portals Visite

HighlightedAchievements is brand information

HighlightedMissionBadges is the task information


### getPaginatedMissionBadges

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getPaginatedMissionBadges
API Request: params: [{continuationToken, nickname}]

This continuationToken is the firstMissionBadgeContinuationToken returned by the API above or the continuationToken that it returns itself

API Response:
`` `Json
{
"Result": {
"MissionBadges": [{
"MissionGuid": guid,
"MissionTitle": *,
"MissionDescription": *,
"ImageUrl": *
}, ...],
"ContinuationToken": *
},
"GameBasket": {
"GameEntities": [],
"Inventory": [],
"DeletedEntityGuids": []
}
}
`` ``

This API is used to get the task icon and details

### getNearbyMissions

API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/getNearbyMissions
API Request:
`` `Json
{
"Params": {
"ClientBasket": {
"ClientBlob": *
},
"KnobSyncTimestamp": *,
"EnergyGlobGuids": [],
"Location": value,
"ContinuationToken": value
}
}
`` ``

The value here is also a comma separated by two hex

ContinueationToken is null that the first view of the task, then the value of the API itself to return the continuationToken

API Response:
`` ``
{
"Result": {
"MissionSnippets": [{
"Header": {
"Guid": guid,
"Title": *,
"LogoUrl": *,
"StartLocation": value,
"BadgeUrl": *,
"Stats": {
"RatingE6": "0",
"MedianCompletionTimeMs": "0",
"NumUniqueCompletedPlayers": "0"
},
"Status": "NOT_STARTED",
"AuthorNickname": agent_name,
"AuthorTeam": "ALIENS"
},
"Version": "2"
}, ...],
"ContinuationToken": value
},
"GameBasket": {
"GameEntities": [],
"PlayerEntity": [guid, 1481912305897, {
"Avatar": {
"Foreground": {
"LayerId": "foreground4",
"TintColor": 16249235,
"ImageUrl": *
},
"Background": {
"LayerId": "background3",
"TintColor": 481625,
"ImageUrl": *
}
},
"PlayerPersonal": {
"Ap": "14031060",
"Energy": 17500,
"AllowNicknameEdit": false,
"AllowFactionChoice": false,
"VerifiedLevel": 13,
"MediaHighWaterMarks": {
"General": 1809,
"RESISTANCE": 1328,
"ALIENS": 1327
},
"EnergyState": "XM_OK",
"NotificationSettings": {
"ShouldSendEmail": false,
"MaySendPromoEmail": false,
"ShouldPushNotifyForAtPlayer": true,
"ShouldPushNotifyForPortalAttacks": true,
"ShouldPushNotifyForInvitesAndFactionInfo": true,
"ShouldPushNotifyForNewsOfTheDay": true,
"ShouldPushNotifyForEventsAndOpportunities": true,
"Locale": "zh-Hans-CN"
},
"ProfileSettings": {
"AreMetricsPublic": false
},
"VerificationState": "UNVERIFIED",
"ExtraXmTankEnergy": 0
},
"ControllingTeam": {
"Team": "RESISTANCE"
}
}],
"ApGains": [],
"Inventory": [],
"DeletedEntityGuids": [],
"ServerBlob": *,
"RefreshEntityGuids": [guid, ...]
}
}
`` ``

### getNickNamesFromPlayerIds

API URL: https://m-dot-betaspike.appspot.com/rpc/playerUndecorated/getNickNamesFromPlayerIds
API Request: params: [agent_guid]
API Response: result: [agent_name], gameBasket: {gameEntities:, inventory:, deletedEntityGuids:}


### collectItemsOrGlyphsFromPortal


API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/collectItemsOrGlyphsFromPortal

#### Without Glyphs


API Request:
`` `Json
{
"Params": {
"GlyphGameRequested": false,
"SpewInfluenceGlyphSequence": null,
"UserInputGlyphSequence": null,
"ClientBasket": {
"ClientBlob": *
},
"KnobSyncTimestamp": *,
"EnergyGlobGuids": [],
"PortalGuid": guid,
"Location": value
}
}
`` ``

API Response:
`` `Json
{
"Result": {
"Items": {
"AddedGuids": [guid, ...],
"BaseHackMultiplier": "1"
}
},
"GameBasket": {
"PlayerDamages": [],
"GameEntities": [],
"PlayerEntity": [guid, 1481913704637, {
"PlayerPersonal": {
...
"EnergyState": "XM_OK",
"NotificationSettings": ...,
"ProfileSettings": {
"AreMetricsPublic": false
},
"VerificationState": "UNVERIFIED",
"ExtraXmTankEnergy": 0
},
"ControllingTeam": {
"Team": "RESISTANCE"
},
"Avatar": {
...
}
}],
"ApGains": [],
"Inventory": [
[Guid, 1481913704513, {
"ModResource": {
"DisplayName": "Portal Shield",
"Stats": {
"REMOVAL_STICKINESS": "0",
"MITIGATION": "30"
},
"Rarity": "COMMON",
"ResourceType": "RES_SHIELD"
},
"DisplayName": {
"DisplayName": "Portal Shield",
"Displaydescription": "Mod which shields Portal from attacks."
},
"InInventory": {
"PlayerId": agent_guid,
"AcquisitionTimestampMs": *
}
}],
...
],
"DeletedEntityGuids": [],
"ServerBlob": *,
"RefreshEntityGuids": [guid]
}
}
`` ``

#### With Glyphs

API Request:
`` `Json
{
"Params": {
"GlyphGameRequested": true,
"SpewInfluenceGlyphSequence": null,
"UserInputGlyphSequence": null,
"ClientBasket": {
"ClientBlob": *
},
"KnobSyncTimestamp": 1481912295011,
"EnergyGlobGuids": [],
"PortalGuid": guid,
"Location": value
}
}
`` `Json

API Response:

`` `Json
{
"Result": {
"Glyphs": {
"GlyphSequence": [{
"GlyphOrder": "ejgahic"
}, {
"GlyphOrder": "ga"
}, {
"GlyphOrder": "jkgh"
}, {
"GlyphOrder": "ejka"
}, {
"GlyphOrder": "gkhijd"
}],
"IsPrompted": false,
"MaxInputTimeMs": "15000",
"TimeToDemoMs": "500"
}
},
"GameBasket": {
*
}
}
`` ``

### collectItemsFromPortalWithGlyphResponse

API URL: https://m-dot-betaspike.appspot.com/rpc/gameplay/collectItemsFromPortalWithGlyphResponse
API Request:
`` `Json
{
"Params": {
"GlyphGameRequested": false,
"SpewInfluenceGlyphSequence": {
"InputTimeMs": 1233,
"Bypassed": false,
"GlyphSequence": [{
"GlyphOrder": "hgkj"
}, {
"GlyphOrder": "gkh"
}]
},
"UserInputGlyphSequence": {
"InputTimeMs": 8683,
"Bypassed": false,
"GlyphSequence": [{
"GlyphOrder": "fgah"
}, {
"GlyphOrder": "egjihc"
}, {
"GlyphOrder": "egahc"
}, {
"GlyphOrder": "ega"
}, {
"GlyphOrder": "efabhkjd"
}]
},
"ClientBasket": {
"ClientBlob": *
},
"KnobSyncTimestamp": 1481912295011,
"EnergyGlobGuids": [],
"PortalGuid": "3e2bcc15c58d486fae24e2ade2bf7327.16",
"Location": "01d553fe, 0631e166"
}
}
`` ``

	API Response:

```
{
	"result": {
		"addedGuids": ["a4f754d8e8f0448c88a6fe349f303ebc.5", "c07c542b6e0c4fea9b66b66fcc110c37.5", "c7995bba88c4442eac1ac2c8008d419c.5", "3264136a3f414efba8e88a401786bc25.5", "4d928a839c3546658abc025fb8170981.5", "65916b9be8944fe986abe47433ad4c75.5", "0e9b7f761b8f41d8b149447dfd10ca0d.5", "995fddb948bf41caab25ad1d0496e57a.5", "36ecac7a73ba43bfb21f6d4dbdd84a1f.5", "afe79b3bd18941b995557eb0d0bcef41.5"],
		"glyphResponse": {
			"glyphResponses": [true, true, true, true, true],
			"powerBoostInMillion": 1630000,
			"timeBonusBoostInMillion": 421133,
			"displayNames": ["PURSUE", "CONFLICT", "WAR", "ADVANCE", "CHAOS"],
			"bonusGuids": ["a4f754d8e8f0448c88a6fe349f303ebc.5", "c07c542b6e0c4fea9b66b66fcc110c37.5", "3264136a3f414efba8e88a401786bc25.5", "4d928a839c3546658abc025fb8170981.5", "65916b9be8944fe986abe47433ad4c75.5", "afe79b3bd18941b995557eb0d0bcef41.5"]
		},
		"baseHackMultiplier": "1"
	},
	"gameBasket": {
		"playerDamages": [],
		"gameEntities": [],
		"playerEntity": [*, 1481914255972, {
			"avatar": {
				*
			},
			"playerPersonal": {
				"ap": "14031565",
				"energy": 17100,
				"allowNicknameEdit": false,
				"allowFactionChoice": false,
				"verifiedLevel": 13,
				"mediaHighWaterMarks": {
					"General": 1809,
					"RESISTANCE": 1328,
					"ALIENS": 1327
				},
				"energyState": "XM_OK",
				"notificationSettings": {
					"shouldSendEmail": false,
					"maySendPromoEmail": false,
					"shouldPushNotifyForAtPlayer": true,
					"shouldPushNotifyForPortalAttacks": true,
					"shouldPushNotifyForInvitesAndFactionInfo": true,
					"shouldPushNotifyForNewsOfTheDay": true,
					"shouldPushNotifyForEventsAndOpportunities": true,
					"locale": "zh-Hans-CN"
				},
				"profileSettings": {
					"areMetricsPublic": false
				},
				"verificationState": "UNVERIFIED",
				"extraXmTankEnergy": 0
			},
			"controllingTeam": {
				"team": "RESISTANCE"
			}
		}],
		"apGains": [{
			"apTrigger": "GLYPH_HACK",
			"apGainAmount": "405"
		}],
		"inventory": [
			["a4f754d8e8f0448c88a6fe349f303ebc.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 8
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255794"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["c07c542b6e0c4fea9b66b66fcc110c37.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMP_BURSTER",
					"level": 8
				},
				"displayName": {
					"displayName": "Xmp Burster",
					"displayDescription": "Exotic Matter Pulse weapons which can destroy enemy Resonators and Mods and neutralize enemy Portals."
				},
				"empWeapon": {
					"level": 8,
					"ammo": 1
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255779"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["c7995bba88c4442eac1ac2c8008d419c.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 7
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255777"
				},
				"accessLevel": {
					"requiredLevel": 7,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 7
					}
				}
			}],
			["3264136a3f414efba8e88a401786bc25.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMP_BURSTER",
					"level": 7
				},
				"displayName": {
					"displayName": "Xmp Burster",
					"displayDescription": "Exotic Matter Pulse weapons which can destroy enemy Resonators and Mods and neutralize enemy Portals."
				},
				"empWeapon": {
					"level": 7,
					"ammo": 1
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255794"
				},
				"accessLevel": {
					"requiredLevel": 7,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 7
					}
				}
			}],
			["4d928a839c3546658abc025fb8170981.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 8
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255779"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["65916b9be8944fe986abe47433ad4c75.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 8
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255780"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["0e9b7f761b8f41d8b149447dfd10ca0d.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 8
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255778"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["995fddb948bf41caab25ad1d0496e57a.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "POWER_CUBE",
					"level": 8
				},
				"displayName": {
					"displayName": "Power Cube",
					"displayDescription": "Store of XM which can be used to recharge Scanner."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255778"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				},
				"powerCube": {
					"energy": 8000
				}
			}],
			["36ecac7a73ba43bfb21f6d4dbdd84a1f.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "EMITTER_A",
					"level": 8
				},
				"displayName": {
					"displayName": "Resonator",
					"displayDescription": "XM object used to power-up a Portal and align it to a Faction."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255778"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				}
			}],
			["afe79b3bd18941b995557eb0d0bcef41.5", 1481914255823, {
				"resourceWithLevels": {
					"resourceType": "POWER_CUBE",
					"level": 8
				},
				"displayName": {
					"displayName": "Power Cube",
					"displayDescription": "Store of XM which can be used to recharge Scanner."
				},
				"inInventory": {
					"playerId": *,
					"acquisitionTimestampMs": "1481914255779"
				},
				"accessLevel": {
					"requiredLevel": 8,
					"failure": {
						"isAllowed": false,
						"requiredLevel": 8
					}
				},
				"powerCube": {
					"energy": 8000
				}
			}]
		],
		"deletedEntityGuids": [],
		"serverBlob": *,
		"refreshEntityGuids": [guid]
	}
}
```

Here all the submitted and get the parameters are listed, explain the drawing of the letters on behalf of the location:

From top to bottom, from left to right

A
Fb
Gh
K
Ejic
D

That is, the ALL command from the top of the vertex is followed by abcd (the bottom) dfa; Strong command from the top left corner clockwise ghij; center is k

# Conclusion

Spent two nights counting the next API.
Originally intends to also write a Game API library, but because it can not generate the client blob and no meaning, later used to say, Intel Map has been ok.
In the course of the game, batch will trigger when moving
Get the map update is a regular polling: getInventory (after all, you can passcode to get items, all have to get get), getPaginatedPlexts, getObjectsInCells
