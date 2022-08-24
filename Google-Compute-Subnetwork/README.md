# Google Compute Subnetwork

Friday, August 12, 2022

11:25 AM

## Subnetwork should only be IPv4 CIDR blocks

**KCC**
Kind: ComputeSubnetwork

- Block events for resources of type 'kind: ComputeSubnetwork' where spec values do not match as described in below table
- [ComputeSubnetwork  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computesubnetwork)

```yaml
spec:
  description: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.description | equals | Non-null value |


## Subnetwork should only be created in allowed regions

**KCC**
Kind: ComputeSubnetwork

- Block events for resources of type 'kind: ComputeSubnetwork' where spec values do not match as described in below table
- [ComputeSubnetwork  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computesubnetwork)

```yaml
spec:
  region: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.region | equals | Allowed regions |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_subnetwork" where field 'resource -\> region' equals an allowed region
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_subnetwork",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "region": "us-central1",
        },
```

## Subnetwork should have flow logging enabled

**KCC**
Kind: ComputeSubnetwork

- Block events for resources of type 'kind: ComputeSubnetwork' where spec values do not match as described in below table
- [ComputeSubnetwork  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computesubnetwork)

```yaml
spec:
  logConfig:
    metadata: INCLUDE_ALL_METADATA
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.logConfig.metadata | equals | INCLUDE_ALL_METADATA |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_subnetwork" where field 'resource -\> log_config -\> metadata' equals "INCLUDE_ALL_METADATA"
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_subnetwork",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "log_config": [
                {
                    "metadata": "INCLUDE_ALL_METADATA",
                }
            ],
        },
```

## Subnetwork should have Private Google Access enabled

**KCC**
Kind: ComputeSubnetwork

- Block events for resources of type 'kind: ComputeSubnetwork' where spec values do not match as described in below table
- [ComputeSubnetwork  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computesubnetwork)

```yaml
spec:
  privateIpGoogleAccess: boolean
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.privateIpGoogleAcccess | equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_subnetwork" where field 'resource -\>private_ip_google_access' equals true
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_subnetwork",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "private_ip_google_access": true,
        },
```

# Google Compute Subnetwork IAM Control

Friday, August 12, 2022

11:47 AM

## Subnetwork should only be IPv4 CIDR blocks

**KCC**
Kind: IAMPolicy

- Block events for resources of type 'kind: IAMPolicy where spec values do not match as described in below table
- [IAMPolicy  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/iam/iampolicy)

```yaml
spec:
  description: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.description | equals | Non-null value |
