# Google MemoryStore

Thursday, August 4, 2022

12:37 PM

**Google MemoryStore**

Kind: RedisInstance

**Requires use of CMEK**

**KCC**

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

spec:

transitEncryptionMode: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.transitEncryptionMode | equals | CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_redis\_instance" where field 'resource -\> customer\_managed\_key' equals a valid CMEK key
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                        "customer\_managed\_key": {

                            "references": [

                                "google\_kms\_crypto\_key.redis\_key.id",

                                "google\_kms\_crypto\_key.redis\_key"

                            ]

**Google MemoryStore**

Kind: RedisInstance

**Only Allowed Regions, Locations, and Alternative-Locations**

**KCC**

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

spec:

alternativeLocationId: string

locationId: string

region: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.alternativeLocationId/locationId/region | equals | Approved locations |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google\_redis\_instance" where field 'resource -\> customer\_managed\_key' equals a valid CMEK key
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_redis\_instance",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "alternative\_location\_id": "Approved Alt Location id",

                    "location\_id": "Approved Location id",

                    "region": "Approved Region",

                },

**Google MemoryStore**

Kind: RedisInstance

**Private Service Access Required**

**KCC**

- Allow create events for resources of type 'kind: RedisInstance' where users have the permissions below (UI and gcloud permissions for creating Private Service Access Connection):
  - Serviceusage.services.enable
  - Compute.addresses.create
  - Compute.addresses.list
  - Servicenetworking.services.addPeering
- Allow create events for resources of type 'kind: RedisInstance' where spec values match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance#spec)
- [Networking  |  Memorystore for Redis  |  Google Cloud](https://cloud.google.com/memorystore/docs/redis/networking#required_permissions_to_establish_a_private_services_access_connection)

spec:

connectMode: 'PRIVATE\_SERVICE\_ACCESS'

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.connectMode | equals | PRIVATE\_SERVICE\_ACCESS |

**Terraform:**

- Allow create events for resources of type 'kind: RedisInstance' where users have the permissions below (UI and gcloud permissions for creating Private Service Access Connection):
  - Serviceusage.services.enable
  - Compute.addresses.create
  - Compute.addresses.list
  - Servicenetworking.services.addPeering
- Block 'create' events for resources of type "google\_redis\_instance" where field 'resource -\> connect\_mode' is not equal to "PRIVATE\_SERVICE\_ACCESS"

- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_redis\_instance",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "connect\_mode": "PRIVATE\_SERVICE\_ACCESS",

                },

**Google MemoryStore**

Kind: RedisInstance

**TLS Connections Required**

**KCC**

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

spec:

transitEncryptionMode: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.transitEncryptionMode | equals | TLS mode or any non null value |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google\_redis\_instance" where field 'resource -\> transit\_encryption\_mode' does not equal "SERVER\_AUTHENTICATION"
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_redis\_instance",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "transit\_encryption\_mode": "SERVER\_AUTHENTICATION"

                },

**Google MemoryStore**

Kind: RedisInstance

**Redis AUTH Required**

**KCC**

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)
- [AUTH feature overview  |  Memorystore for Redis  |  Google Cloud](https://cloud.google.com/memorystore/docs/redis/auth-overview)

spec:

authEnabled: true

authString: string (auto generated when authEnabled = true)

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.authEnabled | equals | true |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google\_redis\_instance" where field 'resource -\> auth\_enabled' does not equal "true"
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_redis\_instance",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "auth\_enabled": true,

                },

                "after\_unknown": {

                    "auth\_string": true,

                },

**Google MemoryStore**

Kind: RedisInstance

**Minimum Version Required**

**KCC**

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

spec:

redisVersion: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.redisVersion | Greater than or equal to | Lowest approved version or null |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google\_redis\_instance" where field 'resource -\> redis\_version' is lower than the lowest approved version of redis
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_redis\_instance",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "redis\_version": "REDIS\_6\_X",

                },
