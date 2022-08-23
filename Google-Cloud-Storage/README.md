# Google Cloud Storage

Monday, August 8, 2022

12:37 PM

## Require Versioning for all GCS Buckets

**KCC**
Kind: StorageBucket

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  versioning:
    enabled: true

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.versioning.enabled | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> versioning -\> enabled' equals true
- Do nothing for 'delete' events

```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "versioning": [
                {
                    "enabled": true
                }
            ],
        },
```

## Require Retention Policy Enabled and Retention Period Set for all Buckets

**KCC**
Kind: StorageBucket


- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:
