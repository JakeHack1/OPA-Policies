# Google BigQuery

Tuesday, August 9, 2022

12:49 PM

**Google BigQuery**

Kind: BigQueryDataset

**Require CMEK for all Datasets**

**KCC**

- Block events for resources of type 'kind: BigQueryDataset where spec values do not match as described in below table
- [BigQueryDataset  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerydataset)

spec:

defaultEncryptionConfiguration:

kmsKeyRef:

external: string

name: string

namespace: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryDataset | spec.defaultEncryptionConfiguration.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_dataset" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_bigquery\_dataset",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "default\_encryption\_configuration": [

                        {}

                    ],

                },

                "after\_unknown": {

                    "default\_encryption\_configuration": [

                        {

                            "kms\_key\_name": true

                        }

                    ],

                },

                "after\_sensitive": {

                    "access": [],

                    "default\_encryption\_configuration": [

                        {}

                    ]

                }

            }

        },

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

**Google BigQuery**

Kind: BigQueryDataset

**Restrict Datasets to 'US' Locations Only**

**KCC**

- Block events for resources of type 'kind: BigQueryDataset where spec values do not match as described in below table
- [BigQueryDataset  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerydataset)

spec:

location: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryDataset | spec.location | equals | US |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_dataset" where field 'resource -\> location' equals 'US'
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_bigquery\_dataset",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "location": "US",

                },

**Google BigQuery**

Kind: BigQueryTable

**Restrict Datasets to 'US' Locations Only**

**KCC**

- Block events for resources of type 'kind: BigQueryTable' where spec values do not match as described in below table
- [BigQueryTable  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerytable)

spec:

encryptionConfiguration:

kmsKeyRef:

external: string

name: string

namespace: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryTable | spec.defaultEncryptionConfiguration.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_table" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_bigquery\_dataset",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                },

                "after\_unknown": {

                },

                "after\_sensitive": {

                }

            }

        },

        {

            "type": "google\_bigquery\_table",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "encryption\_configuration": [

                        {

                            "kms\_key\_name": "CMEK"

                        }

                    ],
