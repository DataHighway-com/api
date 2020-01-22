
# Roaming Agreements

The RESTful API routes for Roaming Agreements include:

<aside class="notice">Roaming Access Logs are used for Billing & Charging.</aside>
<aside class="notice">Roaming Activation & Traffic Records are used for LoRaWAN Backend Specification Compliance.</aside>

<aside class="warning">Only details of some of the RESTful routes listed below are covered in detail (i.e. `GET` requests that involve specific NSId are not covered in detail) </aside>

**Profiles (End Device)**:

All ED:

* `GET /api/roaming/profiles/devices/base-profiles`
* `GET /api/roaming/profiles/devices/base-profiles/<ID>`

Specific ED:

* `GET /api/roaming/profiles/devices/<EDId>/base-profiles`
* `GET /api/roaming/profiles/devices/<EDId>/base-profiles/<ID>`
* `POST /api/roaming/profiles/devices/<EDId>/base-profiles/new`

**Profiles (Network Servers)**:

All NS:

* `GET /api/roaming/profiles/network-servers/base-profiles`
* `GET /api/roaming/profiles/network-servers/base-profiles/<ID>`
* `GET /api/roaming/profiles/network-servers/services`
* `GET /api/roaming/profiles/network-servers/services/<ID>`
* `GET /api/roaming/profiles/network-servers/routing`
* `GET /api/roaming/profiles/network-servers/routing/<ID>`

Specific NS:

* `GET /api/roaming/profiles/network-servers/<NSId>/base-profiles`
* `GET /api/roaming/profiles/network-servers/<NSId>/base-profiles/<ID>`
* `GET /api/roaming/profiles/network-servers/<NSId>/base-profiles/new`
* `GET /api/roaming/profiles/network-servers/<NSId>/services`
* `GET /api/roaming/profiles/network-servers/<NSId>/services/<ID>`
* `POST /api/roaming/profiles/network-servers/<NSId>/services/new`
* `GET /api/roaming/profiles/network-servers/<NSId>/routing`
* `GET /api/roaming/profiles/network-servers/<NSId>/routing/<ID>`
* `POST /api/roaming/profiles/network-servers/<NSId>/routing/new`

**Policies**:

All NS:

* `GET /api/roaming/policies/network-servers/agreements`
* `GET /api/roaming/policies/network-servers/agreements/<ID>`
* `GET /api/roaming/policies/network-servers/accounting`
* `GET /api/roaming/policies/network-servers/accounting/<ID>`
* `GET /api/roaming/policies/network-servers/activation-types`
* `GET /api/roaming/policies/network-servers/activation-types/<ID>`

Specific NS:

* `GET /api/roaming/policies/network-servers/<NSId>/agreements`
* `GET /api/roaming/policies/network-servers/<NSId>/agreements/<ID>`
* `POST /api/roaming/policies/network-servers/<NSId>/agreements/new`
* `GET /api/roaming/policies/network-servers/<NSId>/accounting`
* `GET /api/roaming/policies/network-servers/<NSId>/accounting/<ID>`
* `POST /api/roaming/policies/network-servers/<NSId>/accounting/new`
* `GET /api/roaming/policies/network-servers/<NSId>/activation-types`
* `GET /api/roaming/policies/network-servers/<NSId>/activation-types/<ID>`
* `POST /api/roaming/policies/network-servers/<NSId>/activation-types/new`

**Access Logs (Network Servers)**:

All NS:

* `GET /api/roaming/logs/network-servers/access`
* `GET /api/roaming/logs/network-servers/access/<ID>`

Specific NS:

* `GET /api/roaming/logs/network-servers/<NSId>/access`
* `GET /api/roaming/logs/network-servers/<NSId>/access/<ID>`

**Activation Records (Network Servers)**:

All NS:

* `GET /api/roaming/logs/network-servers/activations`
* `GET /api/roaming/logs/network-servers/activations/<ID>`

Specific NS:

* `GET /api/roaming/logs/network-servers/<NSId>/activations`
* `GET /api/roaming/logs/network-servers/<NSId>/activations/<ID>`

**Traffic Records (Network Servers)**:

All NS:

* `GET /api/roaming/logs/network-servers/traffic`
* `GET /api/roaming/logs/network-servers/traffic/<ID>`

Specific NS:

* `GET /api/roaming/logs/network-servers/<NSId>/traffic`
* `GET /api/roaming/logs/network-servers/<NSId>/traffic/<ID>`

## General Roaming Response JSON Structure

```json
[
  {
    // Roaming configuration per network
    "roamingConfig": {
      // Mapping between Operator ID and Network ID
      "networkOperators": [
        {
          "opId": 200,
          "netd": 15,
        },
        {
          "opId": 200,
          "netd": 21,
        }
      ],
      "networkHost": { // Offering their network servers to roaming visitors
        "netId": 15, // Network ID (i.e. Huawei)
        "opId": 200, // Operator ID (i.e. ThingPark, MXC)
        "allowRoamingDevices": true,
        "blacklistedHosts": [
          "badserver.com"
        ],
        // List of operators and associated networks that are parties to the roaming policy
        "whitelistedVisitors": [
          {
            "netId": 21,
            "opId": 200,
            "policy": {
              "id": 1, // Policy ID
              "expiry": "2020-04-23T18:25:43.511Z",
              "cases": {
                "allowed": {
                  "handover": true,
                  "passive": false
                }
              },
              "feeRate": {
                "perUplinkPacket": 10,
                "perDownlinkPacket": 1
              },
            },
            // Other hosts permitted to send data traffic to
            // Extract NwkID of received packet to lookup NetID
            "whitelistedHostAddresses": [
              "myserver.com",
              "https://10.0.0.1"
            ]
          },
        ],
        "roamingProfiles": [
          {
            "EDRoamingBaseProfileId": 123,
            "NSRoamingBaseProfileId": 123,
            "NSRoamingServiceProfileId": 123,
            "NSRoamingRoutingProfileId": 123,
          }
        ]
      },
    },
    // Roaming access logs
    "roamingAccessLogs": [
      {
        // End Device info
        "EDInfo": {
          // Used to determine the Net ID of the End Device
          "devAddr": "123",
          // Used to determine Join Server assocated with End Device
          "joinEUI": "456",
          // Network visitor info
          "homeNet": {
            "netId": 15,
            "opId": 200,
          },
        },
        // Network host info
        "visitedNetworkHost": {
          "netId": 15,
          "opId": 200,
          "originsUsed": [
            "myserver.com",
            "https://10.0.0.1"
          ]
        },
        // Refer to specific Policy ID configuration for further details
        "policyId": 1,
        "joinRequested": "2020-04-22T18:25:43.511Z",
        "joinAccepted": "2020-04-23T18:25:43.511Z",
        "joinRejected": "",
        // Calculate fee from packets processed in the configured policy fee rate
        "packetsProcessed": {
          "uplinkPackets": 200,
          "downlinkPackets": 400
        },
        "status": {
          "httpStatusCode": 4xx
        }
      }
    ]
  },
]
```

## End Device (ED) Roaming Base Profiles

### Get All ED Roaming Base Profiles

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profiles := api.roaming.profiles.devices.baseProfiles.Get()
```

```shell
curl "http://datahighway.com/api/roaming/profiles/devices/base-profiles"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profiles = api.roaming.profiles.devices.baseProfiles.get();
```

> The above command returns JSON structured like this:

```json
{
  "EDRoamingBaseProfiles": [
    {
      "id": 123, // Device Profile ID
      "EDId": 123,
       // Used to allow device owners to enable roaming for a period of time
      "expiry": "2020-04-23T18:25:43.511Z",
      "devEUI": "012",
      // Used to determine the Net ID of the End Device, and its Operator ID
      "devAddr": "123",
      // Used to determine Join Server associated with End Device
      "joinEUI": "456",
      "vendorId": "MatchX", // VSExtension vendor OUI
      // Network visitor info
      "homeNet": {
        "netId": 15,
        "opId": 200,
        "regionId": "ABC", // Radio frequency region
        "maxDutyCycle": 0.1
      },
    }
  ]
}
```

This endpoint retrieves all device profiles.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/devices/base-profiles`

### Get a Specific ED Roaming Base Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profile := api.roaming.profiles.devices.baseProfiles.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/profiles/devices/base-profiles/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profile = api.roaming.profiles.devices.baseProfiles.get(2);
```

> The above command returns JSON structured similar to retrieving all device profiles, but only containing the specific device profile that was requested.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/devices/base-profiles/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the device profile to retrieve

### Set a Specific ED Roaming Base Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type EDRoamingBaseProfileHomeNetConfig struct {
  NetID string
  OpID string
  RegionID string
  MaxDutyCycle float32
}

type EDRoamingBaseProfileConfig struct {
  EDId string
  Expiry datetime
  DevEUI string
  DevAddr string
  JoinEUI string
  VendorID string
  HomeNet EDRoamingBaseProfileHomeNetConfig
}

// Roaming Device Profile configuration
config := EDRoamingBaseProfileConfig {
  "EDId": "123"
  "expiry": "2020-04-23T18:25:43.511Z",
  "devEUI": "012",
  // Used to determine the Net ID of the End Device, and its Operator ID
  "devAddr": "123",
  // Used to determine Join Server associated with End Device
  "joinEUI": "456",
  "vendorId": "MatchX", // VSExtension vendor OUI
  // Network visitor info
  "homeNet": {
    "netId": 15,
    "opId": 200,
    "regionId": "ABC", // Radio frequency region
    "maxDutyCycle": 0.1
  }
}

// Create a Devices Profile
policy := api.roaming.profiles.devices.baseProfile.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/profiles/devices/<EDId>/base-profiles/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    <INSERT_ROAMING_DEVICES_PROFILE_CONFIG>
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Devices Profile configuration
let config = EDRoamingBaseProfileConfig {
  <INSERT_ROAMING_DEVICES_PROFILE_CONFIG>
}
let policy = api.roaming.profiles.devices(<EDId>).baseProfiles.new(config);
```

This endpoint creates a new profile.

#### HTTP Request

`POST http://datahighway.com/api/roaming/profiles/devices/<EDId>/baseProfiles/new`

## Network Server (NS) Roaming Base Profiles

### Get All NS Roaming Base Profiles

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profiles := api.roaming.profiles.networkServers.baseProfiles.Get()
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/base-profiles"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profiles = api.roaming.profiles.networkServers.baseProfiles.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingBaseProfiles": [
    {
      "netId": 15, // Network ID (i.e. Huawei)
      "opId": 200, // Operator ID (i.e. ThinkPark, MXC)
      "allowRoamingEDs": true,
      "blacklistedHosts": [
        "badserver.com"
      ],
      // List of operators and associated networks that are parties to the roaming policy
      "whitelistedVisitors": [
        {
          "netId": 21,
          "opId": 200,
          "policyId": 1,
          // Other hosts permitted to send data traffic to
          // Extract NwkID of received packet to lookup NetID
          "whitelistedHostAddresses": [
            "myserver.com",
            "https://10.0.0.1"
          ]
        },
      ],
      // Use for ED Roaming DNS Lookup where EDRoamingBaseProfileConfig has a expiry
      "EDProfileIdsActivated": [123, 456],
      "GWRoamingProfileIdsActivated": [123, 456]
    }
  ]
}
```

This endpoint retrieves all device profiles.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/base-profiles`

### Get a Specific NS Roaming Base Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profile := api.roaming.profiles.networkServers.baseProfiles.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/base-profiles/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profile = api.roaming.profiles.networkServers.baseProfiles.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "netId": 15, // Network ID (i.e. Huawei)
  "opId": 200, // Operator ID (i.e. ThinkPark, MXC)
  "allowRoamingEDs": true,
  "blacklistedHosts": [
    "badserver.com"
  ],
  // List of operators and associated networks that are parties to the roaming policy
  "whitelistedVisitors": [
    {
      "netId": 21,
      "opId": 200,
      "policyId": 1,
      // Other hosts permitted to send data traffic to
      // Extract NwkID of received packet to lookup NetID
      "whitelistedHostAddresses": [
        "myserver.com",
        "https://10.0.0.1"
      ]
    },
  ],
  "EDRoamingProfileIdsActivated": [123, 456],
  "GWRoamingProfileIdsActivated": [123, 456]
}
```

This endpoint retrieves a specific profile.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/base-profiles/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the network server to retrieve

### Set a Specific NS Roaming Base Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingBaseProfileWhitelistedVisitorsConfig struct {
  NetID string
  OpID string
  PolicyID string
  WhitelistedHosts Array <String>
}

type NSRoamingBaseProfileConfig struct {
  NetID string
  OpID string
  AllowRoamingEDs bool
  BlacklistedHosts Array <String>
  WhitelistedHosts Array <NSRoamingBaseProfileWhitelistedVisitorsConfig>
  EDRoamingProfileIdsActivated Array <EDId>
  GWRoamingProfileIds Array <GWId>
}

// Roaming Profile configuration
config := NSRoamingBaseProfileConfig {
  <SAME_AS_RESPONSE_TO_GET_SPECIFIC_NS_ROAMING_BASE_PROFILE>
}

// Create a Roaming Base Profile
profile := api.roaming.profiles.networkServers.baseProfile.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/profiles/network-servers/base-profiles/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    <INSERT_ROAMING_PROFILE_CONFIG>
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Profile configuration
let config = NSRoamingBaseProfileConfig {
  <INSERT_ROAMING_PROFILE_CONFIG>
}
let profile = api.roaming.profiles.networkServers.baseProfiles.new(config);
```

This endpoint creates a new profile.

#### HTTP Request

`POST http://datahighway.com/api/roaming/profiles/network-servers/base-profiles/new`

## NS Roaming Service Profiles

### Get All NS Roaming Service Profiles

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profiles := api.roaming.profiles.networkServers.serviceProfiles.Get()
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/service-profiles"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profiles = api.roaming.profiles.networkServers.serviceProfiles.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingServiceProfiles": [
    {
      "id": 123, // Service Profile ID
      "EDRoamingProfileIds": [123, 456],
      "uplinkRate": 10,
      "uplinkBucketSize": 10,
      "uplinkRatePolicy": "Mark", // Mark or Drop
      "downlinkRate": 100,
      "downlinkBucketSize": 100,
      "downlinkRatePolicy": "Mark", // Mark or Drop
      "minDataRate": 10,
      "maxDataRate": 100
    }
  ]
}
```

This endpoint retrieves all profiles.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/service-profiles`

### Get a Specific NS Roaming Service Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profile := api.roaming.profiles.networkServers.servicesProfiles.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/service-profiles/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profile = api.roaming.profiles.networkServers.servicesProfiles.get(2);
```

> The above command returns JSON structured similar to retrieving all profiles, but only containing the specific profile that was requested.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/service-profiles/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the profile to retrieve

### Set a Specific NS Roaming Service Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingServiceProfileConfig struct {
  NSId string
  EDRoamingProfileIds array<string>
  UplinkRate int
  UplinkBucketSize int
  UplinkRatePolicy string
  DownlinkRate int
  DownlinkBucketSize int
  DownlinkRatePolicy string
  MinDataRate int
  MaxDataRate int
}

// Roaming Services Profile configuration
config := NSRoamingServiceProfileConfig {
  "NSId": "123",
  "EDRoamingProfileIds": ["123", "456"],
  "uplinkRate": 10,
  "uplinkBucketSize": 10,
  "uplinkRatePolicy": "Mark", // Mark or Drop
  "downlinkRate": 100,
  "downlinkBucketSize": 100,
  "downlinkRatePolicy": "Mark", // Mark or Drop
  "minDataRate": 10,
  "maxDataRate": 100
}

// Create a Services Profile
policy := api.roaming.profiles.networkServers(<NSId>).servicesProfiles.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/profiles/network-servers/<NSId>/service-profiles/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    <INSERT_ROAMING_SERVICES_PROFILE_CONFIG>
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Services Profile configuration
let config = NSRoamingServicesProfileConfig {
  <INSERT_ROAMING_SERVICES_PROFILE_CONFIG>
}
let profile = api.roaming.profiles.networkServers(<NSId>).servicesProfiles.new(config);
```

This endpoint creates a new profile.

#### HTTP Request

`POST http://datahighway.com/api/roaming/profiles/network-servers/<NSId>/service-profiles/new`

## NS Roaming Routing Profiles

### Get All NS Roaming Routing Profiles

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profiles := api.roaming.profiles.networkServers.routingProfiles.Get()
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/routing-profiles"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profiles = api.roaming.profiles.networkServers.routingProfiles.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingRoutingProfiles": [
    {
      "id": 123, // Routing Profile ID
      "EDRoamingProfileIds": [123, 456],
      "applicationServer": "myapplicationserver.com" // IP Address or DNS name
    }
  ]
}
```

This endpoint retrieves all profiles.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/routing-profiles`

### Get a Specific NS Roaming Routing Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
profile := api.roaming.profiles.networkServers.routingProfiles.Get(<ID>)
```

```shell
curl "http://datahighway.com/api/roaming/profiles/network-servers/routing-profiles/<ID>"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let profile = api.roaming.profiles.networkServers.routingProfiles.get(<ID>);
```

> The above command returns JSON structured similar to retrieving all profiles, but only containing the specific profile that was requested.

#### HTTP Request

`GET http://datahighway.com/api/roaming/profiles/network-servers/routing-profiles/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the profile to retrieve

### Set a Specific NS Roaming Routing Profile

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingRoutingProfileConfig struct {
  ApplicationServer string
}

// Roaming Routing Profile configuration
config := NSRoamingRoutingProfileConfig {
  "applicationServer": "myapplicationserver.com" // IP Address or DNS name
}

// Create a Routing Profile
profile := api.roaming.profiles.networkServers(<NSId>).routingProfiles.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/profiles/network-servers/<NSId>/routing-profiles/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    <INSERT_ROAMING_ROUTING_PROFILE_CONFIG>
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Routing Profile configuration
let config = NSRoamingRoutingProfileConfig {
  <INSERT_ROAMING_ROUTING_PROFILE_CONFIG>
}
let profile = api.roaming.profiles.networkServers(<NSId>).routingProfiles.new(config);
```

This endpoint creates a new profile.

#### HTTP Request

`POST http://datahighway.com/api/roaming/profiles/network-servers/<NSId>/routing-profiles/new`

## NS Roaming Agreement Policies

### Get All NS Roaming Agreement Policies

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policies := api.roaming.policies.networkServers.agreements.Get()
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/agreements"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policies = api.roaming.policies.networkServers.agreements.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingAgreementPolicies": [
    {
      "id": 1, // Agreement Policy ID
      "expiry": "2020-04-23T18:25:43.511Z",
      "roamingActivationTypeId": 1,
      "roamingAccountingFeeFactorId": 1
    }
  ]
}
```

This endpoint retrieves all policies.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/agreements`

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_expired | false | If set to true, the result will also include expired policies.
handover_only | true | If set to false, the result will include passive roaming only policies.

### Get a Specific NS Roaming Agreement Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policy := api.roaming.policies.networkServers.agreements.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/agreements/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policy = api.roaming.policies.networkServers.agreements.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 1, // Agreement Policy ID
  "expiry": "2020-04-23T18:25:43.511Z",
  "roamingActivationTypeId": 1,
  "roamingAccountingFeeFactorId": 1
}
```

This endpoint retrieves a specific policy.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/agreements/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the roaming agreement policy to retrieve

### Set a Specific NS Roaming Agreement Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingActivationTypesAvailable struct {
  Available map[string]bool
}

type NSRoamingAgreementPolicyConfig struct {
  Expiry string
  RoamingActivationTypes NSRoamingActivationTypesAvailable
  RoamingAccountingFeeFactorId int
}

// Roaming Agreement Policy configuration
config := NSRoamingAgreementPolicyConfig {
  "expiry": "2020-04-23T18:25:43.511Z",
  "roamingActivationTypeId": 1,
  "roamingAccountingFeeFactorId": 1
}

// Create a roaming agreement policy
policy := api.roaming.policies.networkServers(<NSId>).agreements.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/policies/network-servers/<NSId>/agreements/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    "expiry": "2020-04-23T18:25:43.511Z",
    "roamingActivationTypeId": 1,
    "roamingAccountingFeeFactorId": 1
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Agreement Policy configuration
let config = NSRoamingAgreementPolicyConfig {
  "expiry": "2020-04-23T18:25:43.511Z",
  "roamingActivationTypeId": 1,
  "roamingAccountingFeeFactorId": 1
}
let policy = api.roaming.policies.networkServers(<NSId>).agreements.new(config);
```

This endpoint creates a new policy.

#### HTTP Request

`POST http://datahighway.com/api/roaming/policies/network-servers/<NSId>/agreements/new`

## NS Roaming Activation Type Policies

### Get All NS Roaming Activation Type Policies

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policies := api.roaming.policies.networkServers.activationTypes.Get()
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/activation-types"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policies = api.roaming.policies.networkServers.activationTypes.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingActivationTypePolicies": [
    {
      "id": 1, // Roaming Activation Type Policy ID
      "handoverAvailable": true,
      "passiveAvailable": false
    }
  ]
}
```

This endpoint retrieves all policies.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/activation-types`

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
handover_only | true | If set to false, the result will include passive roaming only policies.

### Get a Specific NS Roaming Activation Type Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policy := api.roaming.policies.networkServers.activationTypes.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/activation-types/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policy = api.roaming.policies.networkServers.activationTypes.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 1, // Roaming Activation Type Policy ID
  "handoverAvailable": true,
  "passiveAvailable": false
}
```

This endpoint retrieves a specific policy.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/activation-types/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the policy to retrieve


### Set a Specific NS Roaming Activation Type Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingActivationTypesPolicyConfig struct {
  HandoverAvailable bool
  PassiveAvailable bool
}

// Roaming Activation Types Policy configuration
config := NSRoamingActivationTypesPolicyConfig {
  "id": 1, // Roaming Activation Type Policy ID
  "handoverAvailable": true,
  "passiveAvailable": false
}

// Create a Roaming Activation Type Policy
policy := api.roaming.policies.networkServers(<NSId>).activationTypes.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/policies/network-servers/<NSId>/activation-types/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    "id": 1, // Roaming Activation Type Policy ID
    "handoverAvailable": true,
    "passiveAvailable": false
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Activation Types Policy configuration
let config := NSRoamingActivationTypesPolicyConfig {
  "id": 1, // Roaming Activation Type Policy ID
  "handoverAvailable": true,
  "passiveAvailable": false
}
let policy = api.roaming.policies.networkServers(<NSId>).activationTypes.new(config);
```

This endpoint creates a new policy.

#### HTTP Request

`POST http://datahighway.com/api/roaming/policies/network-servers/<NSId>/activation-types/new`

## NS Roaming Accounting Policies

### Get All NS Roaming Accounting Policies

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policies := api.roaming.policies.networkServers.accounting.Get()
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/accounting"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policies = api.roaming.policies.networkServers.accounting.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingAccountingPolicies": [
    {
      "id": 1, // Roaming Fee Factor Policy ID
      "uplinkAccountingFeeFactorPerPacketPolicy": 10,
      "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 20,
      "downlinkAccountingFeeFactorPerPacketPolicy": 1,
      "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 2
    }
  ]
}
```

This endpoint retrieves all policies.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/accounting`

### Get a Specific NS Roaming Accounting Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
policy := api.roaming.policies.networkServers.accounting.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/policies/network-servers/accounting/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let policy = api.roaming.policies.networkServers.accounting.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 1, // Roaming Fee Factor Policy ID
  "uplinkAccountingFeeFactorPerPacketPolicy": 10,
  "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 20,
  "downlinkAccountingFeeFactorPerPacketPolicy": 1,
  "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 2
}
```

This endpoint retrieves a specific policy.

#### HTTP Request

`GET http://datahighway.com/api/roaming/policies/network-servers/accounting/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the policy to retrieve

### Set a Specific NS Roaming Accounting Policy

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")

type NSRoamingAccountingPolicyConfig struct {
  Id int
  UplinkAccountingFeeFactorPerPacketPolicy int
  UplinkAccountingFeeFactorPerPacketOutProfilePolicy int
  DownlinkAccountingFeeFactorPerPacketPolicy int
  DownlinkAccountingFeeFactorPerPacketOutProfilePolicy int
}

// Roaming Accounting Policy configuration
config := NSRoamingAccountingPolicyConfig {
  "id": 1, // Roaming Accounting Policy ID
  "uplinkAccountingFeeFactorPerPacketPolicy": 10,
  "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 20,
  "downlinkAccountingFeeFactorPerPacketPolicy": 1,
  "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 2
}

// Create a Roaming Accounting Policy
policy := api.roaming.policies.networkServers(<NSId>).accounting.New(config)
```

```shell
curl -X POST "http://datahighway.com/api/roaming/policies/network-servers/<NSId>/accounting/new"
  -H "Authorization: INSERT_DH_API_KEY"
  -H "Content-Type: application/json"
  -d '{
    "id": 1, // Roaming Accounting Policy ID
    "uplinkAccountingFeeFactorPerPacketPolicy": 10,
    "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 20,
    "downlinkAccountingFeeFactorPerPacketPolicy": 1,
    "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 2
  }'
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
// Roaming Accounting Policy configuration
let config := NSRoamingAccountingPolicyConfig {
  "id": 1, // Roaming Accounting Policy ID
  "uplinkAccountingFeeFactorPerPacketPolicy": 10,
  "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 20,
  "downlinkAccountingFeeFactorPerPacketPolicy": 1,
  "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 2
}
let policy = api.roaming.policies.networkServers(<NSId>).accounting.new(config);
```

This endpoint creates a new policy.

#### HTTP Request

`POST http://datahighway.com/api/roaming/policies/network-servers/<NSId>/accounting/new`

## NS Roaming Access Logs

Retrieve access logs that have been stored on the Data Highway blockchain for End Devices that join Visited Network Servers.

### Get All NS Roaming Access Logs

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
accessLogs := api.roaming.logs.access.Get()
```

```shell
curl "http://datahighway.com/api/roaming/logs/access"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let accessLogs = api.roaming.logs.access.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingAccessLogs": [
    {
      "accessLogId": 1,
      "accessMetadata": {
        // Reference: https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L41
        "messageType": "JoinReq",
      },
      "policyMetadata": {
        // Refer to specific Roaming Agreement Policy ID configuration for further details
        "agreementPolicy": {
          "id": 1,
          "expiry": "2020-04-22T18:25:43.511Z",
        },
        "activationTypePolicy": {
          "id": 1,
          "activationTypePolicy": "handover",
          // Refer to Base Payload MessageType
          // Timestamp when message type was made and when it was accepted or rejected.
          "joinRequestedAt": "2020-04-22T18:25:43.511Z",
          "joinAcceptedAt": "2020-04-23T18:25:43.511Z",
          "joinRejectedAt": ""
        },
        "accountingPolicy": {
          "id": 1,
          "uplinkAccountingFeeFactorPerPacketPolicy": 1,
          "uplinkAccountingFeeFactorPerPacketOutProfilePolicy": 1,
          "downlinkAccountingFeeFactorPerPacketPolicy": 1,
          "downlinkAccountingFeeFactorPerPacketOutProfilePolicy": 1,
        }
      },
      "profileMetadata": {
        "EDRoamingBaseProfile": {
          "id": 1,
          "expiry": "2020-04-22T18:25:43.511Z",
          "devEUI": "012",
          "devAddr": "123",
          "joinEUI": "456",
          "vendorId": "MatchX",
          "homeNet": {
            "netId": 15,
            "opId": 200,
            "regionId": "ABC",
            "maxDutyCycle": 0.1
          },
        },
        "NSRoamingBaseProfile": {
          "id": 1,
          "netId": 15,
          "opId": 200,
          "visitedHosts": [
            {
              "netId": 21,
              "opId": 200,
              "policyId": 1,
              // Other hosts permitted to send data traffic to
              // Extract NwkID of received packet to lookup NetID
              "hostAddresses": [
                "myserver.com",
                "https://10.0.0.1"
              ]
            },
          ],
        },
        "NSRoamingRoutingProfile": {
          "id": 1,
          "applicationServersAccessed": [
            "myserver.com",
            "https://10.0.0.1"
          ]
        },
        "NSRoamingServiceProfile": {
          "id": 1,
          "uplinkRateAvg": 123,
          "uplinkBucketSizeAvg": 123,
          "downlinkRateAvg": 123,
          "downlinkBucketSizeAvg": 123,
          "downlinkRatePolicy": 123,
          "minDataRate": 123,
          "maxDataRate": 123
        }
      },
      "accountingMetadata": {
        // Calculate fee from packet counter in the configured policy fee rate
        // and the configured priority
        // i.e. fee = uplinkFee + downlinkFee
        //
        //      where,
        //
        //      uplinkFee = uplinkPacketCount * roamingUplinkAccountingFeeFactorPolicy * priorityLevelFactor +
        //            uplinkPacketCountOutProfile * roamingUplinkAccountingFeeFactorOutProfilePolicy * priorityLevelFactor * bytesFactor * bytesFactorOutProfile
        //      downlinkFee = downlinkPacketCount * roamingDownlinkAccountingFeeFactorOutProfilePolicy * priorityLevelFactor +
        //            downlinkPacketCountOutProfile * roamingDownlinkAccountingFeeFactorOutProfilePolicy * priorityLevelFactor * bytesFactor * bytesFactorOutProfile
        "priorityLevel": 3,
        "uplinks": [
          {
            "GWId": 123,
            "dataReceiveTimeStartAt": "2020-04-23T18:25:43.511Z",
            "dataReceiveTimeEndAt": "2020-04-23T18:25:43.511Z",
            "uplinkPacketCount": 200,
            "uplinkPacketCountOutProfile": 50,
            "uplinkBytes": 1000,
            "uplinkBytesOutProfile": 100,
          }
        ],
        "downlinks:": [
          {
            "GWId": 456,
            "dataReceiveTimeStartAt": "2020-04-23T18:25:43.511Z",
            "dataReceiveTimeEndAt": "2020-04-23T18:25:43.511Z",
            "downlinkPacketCount": 400,
            "downlinkPacketCountOutProfile": 50,
            "downlinkBytes": 100,
            "downlinkBytesOutProfile": 10
          }
        ]
      },
      // HTTP Status Codes that correspond to the respective roaming outcome
      "status": {
        "httpStatusCode": 200
      },
      // Reference: https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L68
      "resultCode": "Success",
      "confirmed": true,
      // Monthly Network Activation Record
      // https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L519
      "networkActivationRecordIds": [1, 2],
      // Monthly Network Traffic Record
      // https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L537
      "networkTrafficRecordIds": [1, 2]
    }
  ]
}
```

This endpoint retrieves all access logs.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/access`

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_empty_packets | false | If set to true, the result will also include requests where no uplink or downlink packets were transmitted.
join_accepted_only | false | If set to true, the result will include requests where the join request was successful only.
success_only | false | If set to true, the result will include requests with successful HTTP status codes only.
policies_only | NULL | If set to an array of Policy IDs, the result will include requests that correspond to specific Policy IDs only.
devices_only | NULL | If set to an array of DevAddr, the result will include requests from a specific end devices with that DevAddr only.
networks_only | NULL | If set to an array of NetIDs, the result will include requests from specific Network IDs only.
operators_only | NULL | If set to an array of OperatorIDs, the result will include requests from specific Operator IDs only.
error_only | false | If set to true, the result will include requests with erroneous HTTP status codes only.
error_roaming_only | false | If set to true, the result will include roaming-specific requests with erroneous HTTP status codes only.

### Get a Specific NS Access Log

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
accessLog := api.roaming.logs.access.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/logs/access/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let accessLog = api.roaming.logs.access.get(2);
```

> The above command returns JSON structured similar to retrieving all access logs, but only containing the specific access log that was requested.

This endpoint retrieves a specific access log.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/access/<ACCESS_LOG_ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the roaming access log to retrieve

## NS Roaming Activation Records

Reference: https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L519

### Get All NS Roaming Activation Records

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
logs := api.roaming.logs.activations.Get()
```

```shell
curl "http://datahighway.com/api/roaming/logs/activations"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let logs = api.roaming.logs.activations.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingActivationRecordLogs": [
    {
      "id": 123, // Network Server Activation Record ID
      "netId": 123,
      "NSRoamingServiceProfileId": 1,
      "EDRoamingProfileId": 123,
      "individualRecord": true,
      "totalActiveDevices": 1,
      "activationTime": "2020-04-23T18:25:43.511Z",
      "deactivationTime": ""
    }
  ]
}
```

This endpoint retrieves all logs.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/activations`

### Get a Specific NS Roaming Activation Record

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
log := api.roaming.logs.activations.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/logs/activations/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let log = api.roaming.logs.activations.get(2);
```

> The above command returns JSON structured similar to retrieving all logs, but only containing the specific log that was requested.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/activations/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the log to retrieve

## NS Roaming Traffic Records

Reference: https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L519

### Get All NS Roaming Traffic Records

https://github.com/brocaar/lorawan/blob/master/backend/backend.go#L537

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
logs := api.roaming.logs.traffic.Get()
```

```shell
curl "http://datahighway.com/api/roaming/logs/traffic"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let logs = api.roaming.logs.traffic.get();
```

> The above command returns JSON structured like this:

```json
{
  "NSRoamingTrafficRecordLogs": [
    {
      "id": 123, // Network Traffic Record ID
      "netId": 123, // NetID of the roaming partner NS
      "NSRoamingServiceProfileId": 1, // Service Profile ID
      "EDRoamingProfileId": 123,
      "roamingType": "Handover", // Passive Roaming or Handover Roaming
      "totalUplinkPackets": 1, // Number of uplink packets
      "totalDownlinkPackets": 1, // Number of downlink packets
      "totalOutProfileUplinkPackets": 1, // Number of uplink packets that exceeded ULRate but forwarded anyways per ULRatePolicy
      "totalOutProfileDownlinkPackets": 1, // Number of downlink packets that exceeded DLRate but forwarded anyways per DLRatePolicy
      "totalUplinkBytes": 1, // Total amount of uplink bytes
      "totalDownlinkBytes": 1, // Total amount of downlink bytes
      "totalOutProfileUplinkBytes": 1, // Total amount of uplink bytes that falls outside the Service Profile
      "totalOutProfileDownlinkBytes": 1, // Total amount of downlink bytes that falls outside the Service Profile
    }
  ]
}
```

This endpoint retrieves all logs.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/traffic`

### Get a Specific NS Roaming Traffic Record

```go
import (
  "os"
  dh "github.com/MXCFoundation/dh-api"
)

api := dh.APIClient.Authorize(os.Getenv("DH_API_KEY")
log := api.roaming.logs.traffic.Get(2)
```

```shell
curl "http://datahighway.com/api/roaming/logs/traffic/2"
  -H "Authorization: INSERT_DH_API_KEY"
```

```javascript
require('dotenv').config();
const dh = require('dh-api');

let api = dh.authorize(process.env.DH_API_KEY);
let log = api.roaming.logs.traffic.get(2);
```

> The above command returns JSON structured similar to retrieving all logs, but only containing the specific log that was requested.

#### HTTP Request

`GET http://datahighway.com/api/roaming/logs/traffic/<ID>`

#### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the log to retrieve

# Roaming Billing

TODO - Add class diagram

Billing and Charging processes are decoupled in the Data Highway.

Billing is the process of preparing amounts to be invoiced for charging.
Billing aggregation occurs monthly.
Adjustment may be required where an operator requires billing in fiat instead of DHX before charging occurs, or where a billing or charging dispute is raised and agreed upon. The adjustment would be made after a Balance transfer in DHX resolved the dispute and is applied to records associated with the Charging ID of the previous month.

Billing is calculated in DHX and is associated with uplinks and downlinks of an End Device when roaming. It is calculated based on the Roaming Agreement Policy and monthly Network Traffic Record associated with each End Device. The priority, amount of packets, size in bytes of each packet, the fee factor applied to uplink and downlink packets, and whether the Service Profile limits were exceeded (out of profile) are considered.

Charging is the process of either crediting or debiting DHX from an account balance.
Charging occurs automatically based on the billing information.

Charging and Adjustments should update balances to incorporate roaming charges.

The RESTful API routes for Roaming Billing reconcilliation include:

**Policies (Network)**:

All Networks:

* `GET /api/roaming/billing/networks/policies`
* `GET /api/roaming/billing/networks/policies/<ID>`

Specific Network:

* `GET /api/roaming/billing/networks/<NetId>/policies`
* `GET /api/roaming/billing/networks/<NetId>/policies/<ID>`
* `POST /api/roaming/billing/networks/<NetId>/policies/new`

**Logs (Network)**:

All Networks:

* `GET /api/roaming/billing/networks/logs`
* `GET /api/roaming/billing/networks/logs/<ID>`

Specific Network:

* `GET /api/roaming/billing/networks/<NetId>/logs`
* `GET /api/roaming/billing/networks/<NetId>/logs/<ID>`
* `POST /api/roaming/billing/networks/<NetId>/logs/new`

# Roaming Charging

The RESTful API routes for Roaming Charging include:

**Policies (Network)**:

All Networks:

* `GET /api/roaming/charging/networks/policies`
* `GET /api/roaming/charging/networks/policies/<ID>`

Specific Network:

* `GET /api/roaming/charging/networks/<NetId>/policies`
* `GET /api/roaming/charging/networks/<NetId>/policies/<ID>`
* `POST /api/roaming/charging/networks/<NetId>/policies/new`

**Logs (Network)**:

All Networks:

* `GET /api/roaming/charging/networks/logs`
* `GET /api/roaming/charging/networks/logs/<ID>`

Specific Network:

* `GET /api/roaming/charging/networks/<NetId>/logs`
* `GET /api/roaming/charging/networks/<NetId>/logs/<ID>`
* `POST /api/roaming/charging/networks/<NetId>/logs/new`

# Roaming Disputes

The RESTful API routes for Roaming Disputes include:

**Policies (Network)**:

All Networks:

* `GET /api/roaming/disputes/networks/policies`
* `GET /api/roaming/disputes/networks/policies/<ID>`

Specific Network:

* `GET /api/roaming/disputes/networks/<NetId>/policies`
* `GET /api/roaming/disputes/networks/<NetId>/policies/<ID>`
* `POST /api/roaming/disputes/networks/<NetId>/policies/new`

**Logs (Network)**:

All Networks:

* `GET /api/roaming/disputes/networks/logs`
* `GET /api/roaming/disputes/networks/logs/<ID>`

Specific Network:

* `GET /api/roaming/disputes/networks/<NetId>/logs`
* `GET /api/roaming/disputes/networks/<NetId>/logs/<ID>`
* `POST /api/roaming/disputes/networks/<NetId>/logs/new`

# Roaming Adjustments

The RESTful API routes for Roaming Adjustments include:

**Policies (Network)**:

All Networks:

* `GET /api/roaming/adjustments/networks/policies`
* `GET /api/roaming/adjustments/networks/policies/<ID>`

Specific Network:

* `GET /api/roaming/adjustments/networks/<NetId>/policies`
* `GET /api/roaming/adjustments/networks/<NetId>/policies/<ID>`
* `POST /api/roaming/adjustments/networks/<NetId>/policies/new`

**Logs (Network)**:

All Networks:

* `GET /api/roaming/adjustments/networks/logs`
* `GET /api/roaming/adjustments/networks/logs/<ID>`

Specific Network:

* `GET /api/roaming/adjustments/networks/<NetId>/logs`
* `GET /api/roaming/adjustments/networks/<NetId>/logs/<ID>`
* `POST /api/roaming/adjustments/networks/<NetId>/logs/new`
