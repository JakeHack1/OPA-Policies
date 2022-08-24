# Google MemoryStore

Thursday, August 4, 2022

12:37 PM

## Requires use of CMEK

**KCC**
Kind: RedisInstance

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

```yaml
spec:
  transitEncryptionMode: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.transitEncryptionMode | equals | CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_redis_instance" where field 'resource -\> customer_managed_key' equals a valid CMEK key
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_kms_crypto_key",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "customer_managed_key": {
                "references": [
                    "google_kms_crypto_key.redis_key.id",
                    "google_kms_crypto_key.redis_key"
                ]
```

## Only Allowed Regions, Locations, and Alternative-Locations

**KCC**
Kind: RedisInstance

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

```yaml
spec:
  alternativeLocationId: string
  locationId: string
  region: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.alternativeLocationId/locationId/region | equals | Approved locations |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_redis_instance" where field 'resource -\> customer_managed_key' equals a valid CMEK key
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_redis_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "alternative_location_id": "Approved Alt Location id",
            "location_id": "Approved Location id",
            "region": "Approved Region",
        },
```

## Private Service Access Required

**KCC**
Kind: RedisInstance

- Allow create events for resources of type 'kind: RedisInstance' where users have the permissions below (UI and gcloud permissions for creating Private Service Access Connection):
  - Serviceusage.services.enable
  - Compute.addresses.create
  - Compute.addresses.list
  - Servicenetworking.services.addPeering
- Allow create events for resources of type 'kind: RedisInstance' where spec values match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance#spec)
- [Networking  |  Memorystore for Redis  |  Google Cloud](https://cloud.google.com/memorystore/docs/redis/networking#required_permissions_to_establish_a_private_services_access_connection)

```yaml
spec:
  connectMode: 'PRIVATE_SERVICE_ACCESS'
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.connectMode | equals | PRIVATE_SERVICE_ACCESS |

**Terraform:**

- Allow create events for resources of type 'kind: RedisInstance' where users have the permissions below (UI and gcloud permissions for creating Private Service Access Connection):
  - Serviceusage.services.enable
  - Compute.addresses.create
  - Compute.addresses.list
  - Servicenetworking.services.addPeering
- Block 'create' events for resources of type "google_redis_instance" where field 'resource -\> connect_mode' is not equal to "PRIVATE_SERVICE_ACCESS"

- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_redis_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "connect_mode": "PRIVATE_SERVICE_ACCESS",
        },
```

## TLS Connections Required

**KCC**
Kind: RedisInstance

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

```yaml
spec:
  transitEncryptionMode: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.transitEncryptionMode | equals | TLS mode or any non null value |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_redis_instance" where field 'resource -\> transit_encryption_mode' does not equal "SERVER_AUTHENTICATION"
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_redis_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "transit_encryption_mode": "SERVER_AUTHENTICATION"
        },
```

## Redis AUTH Required

**KCC**
Kind: RedisInstance

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)
- [AUTH feature overview  |  Memorystore for Redis  |  Google Cloud](https://cloud.google.com/memorystore/docs/redis/auth-overview)

```yaml
spec:
  authEnabled: true
  authString: string (auto generated when authEnabled = true)
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.authEnabled | equals | true |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_redis_instance" where field 'resource -\> auth_enabled' does not equal "true"
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_redis_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "auth_enabled": true,
        },
        "after_unknown": {
            "auth_string": true,
        },
```

## Minimum Version Required

**KCC**
Kind: RedisInstance

- Block events for resources of type 'kind: RedisInstance' where spec values do not match as described in below table
- [RedisInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/redis/redisinstance)

```yaml
spec:
  redisVersion: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| redis.cnrm.cloud.google.com | RedisInstance | spec.redisVersion | Greater than or equal to | Lowest approved version or null |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_redis_instance" where field 'resource -\> redis_version' is lower than the lowest approved version of redis
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_redis_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "redis_version": "REDIS_6_X",
        },
```
