# Google Cloud Storage

Monday, August 8, 2022

12:37 PM

## Require CMEK for GCS Buckets

**KCC**
Kind: StorageBucket


- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  encryption:
    kmsKeyRef:
      external: string
      name: string
      namespace: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.encryption.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> encryption -\> kms\_key\_ref -\> external/name/namespace' equals valid CMEK
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "encryption": [
                {
                    "default_kms_key_name": "Valid CMEK"
                }
            ],
```

## Require Uniform Bucket Access Level for all GCS Buckets**

**KCC**
Kind: StorageBucket

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  uniformBucketLevelAccess: true
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.uniformBucketLevelAccess | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> uniform\_bucket\_access\_level' equals true
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "uniform_bucket_level_access": true,
        },
```

## Require Multi-Regional Storage Class for all GCS Buckets

**KCC**
Kind: StorageBucket

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  storageClass: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.storageClass | equals | MULTI\_REGIONAL |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> lifecycle\_rule -\> storageClass' equals MULTI\_REGIONAL
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "lifecycle_rule": [
                {
                    "action": [
                        {
                            "storage_class": "MULTI_REGIONAL",
                            "type": "SetStorageClass"
                        }
                    ],
```

## Require 'US' Location for all GCS Buckets

**KCC**
Kind: StorageBucket

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  location: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.location | equals | 'US' |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> location' equals US
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "location": "US",
        },
```

## Require Versioning for all GCS Buckets

**KCC**
Kind: StorageBucket

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

```yaml
spec:
  versioning:
    enabled: true
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.versioning.enabled | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> versioning -\> enabled' equals true
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
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

```yaml
spec:
  retentionPolicy:
    retentionPeriod: integer
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.retentionPolicy.retentionPeriod | equals | Non-null integer |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> retention\_policy -\> retention\_period' equals a non-null value
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_storage_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "retention_policy": [
                {
                    "is_locked": false,
                    "retention_period": 18374
                }
            ],
```

## Require Private Access to GCS Buckets IAM Policies

**KCC**
Kind: StorageBucketAccessControl

- Block events for resources of type 'kind: StorageBucketAccessControl' where spec values do not match as described in below table
- [StorageBucketAccessControl  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucketaccesscontrol)

```yaml
spec:
  entity: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucketAccessControl | spec.entity | equals | An allowed entity |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket\_access\_control" and "google\_storage\_bucket\_acl" and "google\_storage\_bucket\_iam\_policy" and "google\_storage\_bucket\_iam\_policy\_binding" where field 'resource -\> entity' equals allowed users
- Do nothing for 'delete' events


Google_storage_bucket_access_control
```json
"resource_changes": [
{
    "type": "google\_storage\_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "entity": "allowedUsers",
        },
```

Google_storage_bucket_acl
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "role\_entity": [
                "OWNER:allowed\_owner@gmail.com",
                "READER:allowed\_group-mygroup"
            ]
        },
```

Google_storage_bucket_iam_policy
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket\_iam\_policy",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "bucket": "bucket",
            "policy\_data": "{\"bindings\":[{\"members\":[\"user:allowed\_user@example.com\"],\"role\":\"roles/storage.admin\"}]}"
        },
```

Google_storage_bucket_iam_policy_binding
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket\_iam\_binding",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "members": [
                "user:allowed\_user@example.com"
            ],
            "role": "roles/storage.admin"
        },
```


## Block Owner Level Access to GCS Buckets IAM Policies

**KCC**
Kind: StorageBucketAccessControl

- Block events for resources of type 'kind: StorageBucketAccessControl' where spec values do not match as described in below table
- [StorageBucketAccessControl  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucketaccesscontrol)

```yaml
spec:
  role: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucketAccessControl | spec.role | Not equal | OWNER |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket\_access\_control" and "google\_storage\_bucket\_acl" and "google\_storage\_bucket\_iam\_policy" and "google\_storage\_bucket\_iam\_policy\_binding" where field 'resource -\> role' does not equal OWNER
- Do nothing for 'delete' events

Google_storage_bucket_access_control
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "role": "OWNER",
        },
```

Google_storage_bucket_acl
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "role\_entity": [
                "OWNER:allowed\_owner@gmail.com",
                "READER:allowed\_group-mygroup"
            ]
        },
```

Google_storage_bucket_iam_policy
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket\_iam\_policy",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "bucket": "bucket",
            "policy\_data": "{\"bindings\":[{\"members\":[\"user:allowed\_user@example.com\"],\"role\":\"roles/storage.owner\"}]}"
        },
```

Google_storage_bucket_iam_policy_binding
```json
"resource\_changes": [
{
    "type": "google\_storage\_bucket\_iam\_binding",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "role": "roles/storage.owner"
        },
```
