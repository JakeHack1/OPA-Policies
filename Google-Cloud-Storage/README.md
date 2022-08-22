# Google Cloud Storage

Monday, August 8, 2022

12:37 PM

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
