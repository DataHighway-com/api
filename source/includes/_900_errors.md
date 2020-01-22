# Errors

The Data Highway API uses the following error codes:

## Roaming API Status Codes

<aside class="notice">
The roaming API's specific HTTP status codes are 432-437.

Other LoRaWAN backend specification result codes are 452-469, obtained from
https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L68

`RejectedByNetworkVisitorHostSinceNotWhitelisted` is similar to `NoRoamingAgreement`.
</aside>

HTTP Status Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Your API key is wrong.
403 | Forbidden -- The requested resource is hidden for administrators only.
404 | Not Found -- The specified resource could not be found.
405 | Method Not Allowed -- You tried to access a resource with an invalid method.
406 | Not Acceptable -- You requested a format that isn't JSON.
410 | Gone -- The resource requested has been removed from our servers.
429 | Too Many Requests -- You're requesting too many resources! Slow down!
432 | Roaming Disabled -- The network has disabled roaming (`RejectedByNetworkSinceRoamingDisabled`).
433 | Roaming Connection Host Blacklisted -- The network host that is trying to connect has been blacklisted (`RejectedByNetworkSinceBlacklisted`).
434 | Roaming Period Expired -- The specified roaming agreement policy period has expired (`RejectedByNetworkVisitorHostSinceRoamingPeriodExpired`).
435 | Roaming Connection Host Not Whitelisted -- The Home Network Server of the Visiting End Device has not been whitelisted (`RejectedByNetworkVisitorHostSinceNotWhitelisted`).
436 | Roaming Operator Unknown -- There is no Operator ID corresponding to the specified Network ID (`UnknownOperatorForNetworkId`).
437 | Roaming Network Unknown -- There is no Network ID corresponding to the specified Operator ID (`UnknownNetworkForOperatorId`).
452 | MICFailed -- MIC verification has failed
453 | JoinReqFailed -- JS processing of the JoinReq has failed
454 | NoRoamingAgreement -- There is no roaming agreement between the operators
455 | DevRoamingDisallowed -- End-Device is not allowed to roam
456 | RoamingActDisallowed -- End-Device is not allowed to perform activation while roaming
457 | ActivationDisallowed -- End-Device is not allowed to perform activation
458 | UnknownDevEUI -- End-Device with a matching DevEUI is not found
459 | UnknownDevAddr -- End-Device with a matching DevAddr is not found
460 | UnknownSender -- SenderID is unknown
461 | UnkownReceiver -- ReceiverID is unknown
462 | Deferred -- Passive Roaming is not allowed for a period of time
463 | XmitFailed -- fNS failed to transmit DL packet
464 | InvalidFPort -- Invalid FPort for DL (e.g., FPort=0)
465 | InvalidProtocolVersion -- ProtocolVersion is not supported
466 | StaleDeviceProfile -- Device Profile is stale
467 | MalformedRequest -- JSON parsing failed (missing object or incorrect content)
468 | FrameSizeError -- Wrong size of PHYPayload or FRMPayload
469 | Other -- Used for encoding error cases that are not standardized yet
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
