# Google BigQuery

Tuesday, August 9, 2022

12:49 PM

## Require CMEK for all Datasets

**KCC**

Kind: BigQueryDataset

- Block events for resources of type 'kind: BigQueryDataset where spec values do not match as described in below table
- [BigQueryDataset  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerydataset)

```yaml
spec:
  defaultEncryptionConfiguration:
    kmsKeyRef:
      external: string
      name: string
      namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryDataset | spec.defaultEncryptionConfiguration.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_dataset" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

```json
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
```

## Restrict Datasets to 'US' Locations Only

**KCC**

Kind: BigQueryDataset

- Block events for resources of type 'kind: BigQueryDataset where spec values do not match as described in below table
- [BigQueryDataset  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerydataset)

```yaml
spec:
  location: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryDataset | spec.location | equals | US |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_dataset" where field 'resource -\> location' equals 'US'
- Do nothing for 'delete' events

```json
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
```


## Restrict Datasets to 'US' Locations Only

**KCC**
Kind: BigQueryDataset

- Block events for resources of type 'kind: BigQueryTable' where spec values do not match as described in below table
- [BigQueryTable  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/bigquery/bigquerytable)

```yaml
spec:
  encryptionConfiguration:
    kmsKeyRef:
      external: string
      name: string
      namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| bigquery.cnrm.cloud.google.com | BigQueryTable | spec.defaultEncryptionConfiguration.kmsKeyRef.external/name/namespace | equals | Valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_bigquery\_table" where field 'resource -\> default\_encryption\_configuration' equals a valid CMEK
- Do nothing for 'delete' events

```json
"resource\_changes": [
{
    "type": "google\_bigquery\_dataset",
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
```
