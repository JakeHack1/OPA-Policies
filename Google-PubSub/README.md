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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_pubsub\_topic" and type "google\_pubsub\_lite\_topic" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

google\_pubsub\_topic
```json
"resource\_changes": [
{
    "type": "google\_kms\_crypto\_key",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "name": "example-key",
            "purpose": "ENCRYPT\_DECRYPT",
        },
    }
},
{
    "type": "google\_kms\_key\_ring",
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
    "type": "google\_pubsub\_topic",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
        },
        "after\_unknown": {
            "kms\_key\_name": true,
        },
```

google\_pubsub\_lite\_topic
```json
non-existent kms\_key\_ref argument
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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_pubsub\_topic" and type "google\_pubsub\_lite\_topic" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

google\_pubsub\_topic
```json
"resource\_changes": [
{
    "type": "google\_pubsub\_topic",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "message\_storage\_policy": [
                {
                    "allowed\_persistence\_regions": [
                        "us-central1"
                    ]
                }
            ],
        },
```

google\_pubsub\_lite\_topic
```json
"resource\_changes": [
{
    "type": "google\_pubsub\_lite\_reservation",
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
