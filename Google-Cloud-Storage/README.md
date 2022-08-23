# Google Cloud Storage

Monday, August 8, 2022

12:37 PM

**Google Cloud Storage**

Kind: StorageBucket

**Require CMEK for GCS Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

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

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "encryption": [

                        {

                            "default\_kms\_key\_name": "Valid CMEK"

                        }

                    ],

**Google Cloud Storage**

Kind: StorageBucket

**Require Uniform Bucket Access Level for all GCS Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:

uniformBucketLevelAccess: true

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.uniformBucketLevelAccess | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> uniform\_bucket\_access\_level' equals true
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "uniform\_bucket\_level\_access": true,

                },

**Google Cloud Storage**

Kind: StorageBucket

**Require Multi-Regional Storage Class for all GCS Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:

storageClass: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.storageClass | equals | MULTI\_REGIONAL |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> lifecycle\_rule -\> storageClass' equals MULTI\_REGIONAL
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "lifecycle\_rule": [

                        {

                            "action": [

                                {

                                    "storage\_class": "MULTI\_REGIONAL",

                                    "type": "SetStorageClass"

                                }

                            ],

**Google Cloud Storage**

Kind: StorageBucket

**Require 'US' Location for all GCS Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:

location: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.location | equals | 'US' |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> location' equals US
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "location": "US",

                },

**Google Cloud Storage**

Kind: StorageBucket

**Require Versioning for all GCS Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:

versioning:

enabled: true

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.versioning.enabled | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> versioning -\> enabled' equals true
- Do nothing for 'delete' events

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

**Google Cloud Storage**

Kind: StorageBucket

**Require Retention Policy Enabled and Retention Period Set for all Buckets**

**KCC**

- Block events for resources of type 'kind: StorageBucket' where spec values do not match as described in below table
- [StorageBucket  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucket)

spec:

retentionPolicy:

retentionPeriod: integer

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucket | spec.retentionPolicy.retentionPeriod | equals | Non-null integer |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket" where field 'resource -\> retention\_policy -\> retention\_period' equals a non-null value
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "retention\_policy": [

                        {

                            "is\_locked": false,

                            "retention\_period": 18374

                        }

                    ],

**Google Cloud Storage**

Kind: StorageBucketAccessControl

**Require Private Access to GCS Buckets IAM Policies**

**KCC**

- Block events for resources of type 'kind: StorageBucketAccessControl' where spec values do not match as described in below table
- [StorageBucketAccessControl  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucketaccesscontrol)

spec:

entity: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucketAccessControl | spec.entity | equals | An allowed entity |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket\_access\_control" and "google\_storage\_bucket\_acl" and "google\_storage\_bucket\_iam\_policy" and "google\_storage\_bucket\_iam\_policy\_binding" where field 'resource -\> entity' equals allowed users
- Do nothing for 'delete' events

Google\_storage\_bucket\_access\_control

    "resource\_changes": [

        {

            "type": "google\_storage\_bucket",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "entity": "allowedUsers",

                },

Google\_storage\_bucket\_acl

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

Google\_storage\_bucket\_iam\_policy

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

Google\_storage\_bucket\_iam\_policy\_binding

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

**Google Cloud Storage**

Kind: StorageBucketAccessControl

**Block Owner Level Access to GCS Buckets IAM Policies**

**KCC**

- Block events for resources of type 'kind: StorageBucketAccessControl' where spec values do not match as described in below table
- [StorageBucketAccessControl  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/storage/storagebucketaccesscontrol)

spec:

role: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| storage.cnrm.cloud.google.com | StorageBucketAccessControl | spec.role | Not equal | OWNER |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_storage\_bucket\_access\_control" and "google\_storage\_bucket\_acl" and "google\_storage\_bucket\_iam\_policy" and "google\_storage\_bucket\_iam\_policy\_binding" where field 'resource -\> role' does not equal OWNER
- Do nothing for 'delete' events

Google\_storage\_bucket\_access\_control

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

Google\_storage\_bucket\_acl

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

Google\_storage\_bucket\_iam\_policy

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

Google\_storage\_bucket\_iam\_policy\_binding

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
