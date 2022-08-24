# Google PubSub

Tuesday, August 9, 2022

5:47 PM

## Require CMEK for all Datasets

**KCC**
Kind: PubSubTopic

- Block events for resources of type 'kind: PubSubTopic' where spec values do not match as described in below table
- [PubSubTopic  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/pubsub/pubsubtopic)

```yaml
spec:
  kmsKeyRef:
    external: string
    name: string
    namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| pubsub.cnrm.cloud.google.com | PubSubTopic | spec.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_pubsub_topic" and type "google_pubsub_lite_topic" where field 'resource -\> default_encryption_configuration' equals a valid CMEK
- Do nothing for 'delete' events

google_pubsub_topic
```json
"resource_changes": [
{
    "type": "google_kms_crypto_key",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "name": "example-key",
            "purpose": "ENCRYPT_DECRYPT",
        },
    }
},
{
    "type": "google_kms_key_ring",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "name": "example-keyring",
        },
    }
},
{
    "type": "google_pubsub_topic",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
        },
        "after_unknown": {
            "kms_key_name": true,
        },
```

google_pubsub_lite_topic
```json
non-existent kms_key_ref argument
```

## Allow specific regions to host PubSub message persistence

**KCC**
Kind: PubSubTopic

- Block events for resources of type 'kind: PubSubTopic' where spec values do not match as described in below table
- [PubSubTopic  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/pubsub/pubsubtopic)

```yaml
spec:
  messageStoragePolicy:
    allowedPersistenceRegions:
    - string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| pubsub.cnrm.cloud.google.com | PubSubTopic | spec.messageStoragePolicy.allowedPersistenceRegions | equals | Allowed Regions |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_pubsub_topic" and type "google_pubsub_lite_topic" where field 'resource -\> default_encryption_configuration' equals a valid CMEK
- Do nothing for 'delete' events

google_pubsub_topic
```json
"resource_changes": [
{
    "type": "google_pubsub_topic",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "message_storage_policy": [
                {
                    "allowed_persistence_regions": [
                        "us-central1"
                    ]
                }
            ],
        },
```

google_pubsub_lite_topic
```json
"resource_changes": [
{
    "type": "google_pubsub_lite_reservation",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "region": "us-central1",
        },
    }
},
```
