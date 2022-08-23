# Google Compute Firewall

Thursday, August 11, 2022

2:17 PM

## Firewalls must have descriptions

**KCC**
Kind: ComputeFirewall

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

```yaml
spec:
  description: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.description | equals | Non-null valuex |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_firewall" where field 'resource -\> description' equals a non-null value
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_firewall",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "description": "Any non-null value"
        },
```

## Firewalls must have logging enabled

**KCC**
Kind: ComputeFirewall

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

```yaml
spec:
  logConfig:
    metadata: INCLUDE_ALL_METADATA
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.logConfig.metadata | equals | INLCUDE_ALL_METADATA |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_firewall" where field 'resource -\> log_config -\> metadata' equals "INCLUDE_ALL_METADATA"
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_firewall",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "log_config": [
                {
                    "metadata": "INCLUDE_ALL_METADATA"
                }
            ],
        },
```

## Firewalls should not be created with 0.0.0.0/0 CIDR block

**KCC**
Kind: ComputeFirewall

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

```yaml
spec:
  sourceRanges: string
    destinationRanges: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.sourceRanges | Not equal to | 0.0.0.0/0 |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.destinationRanges | Not equal to | 0.0.0.0/0 |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_firewall" where field 'resource -\> source_ranges' or 'resource -\> destination_ranges' is not equal "0.0.0.0/0"
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_firewall",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "source_ranges": [
                "0.0.0.0/0"
            ],
        },
```

## Firewalls should only allow specific ranges per network

**KCC**
Kind: ComputeFirewall

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

```yaml
spec:
  sourceRanges: string
  destinationRanges: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.sourceRanges | Equals | An allowed CIDR range |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.destinationRanges | Equals | An allowed CIDR range |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_compute_firewall" where field 'resource -\> source_ranges' or 'resource -\> destination_ranges' is equal to an allowed range
- Do nothing for 'delete' events

```json
"resource_changes": [
{
    "type": "google_compute_firewall",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "source_ranges": [
                "a pre-defined allowed range"
            ],
            
            "destination_ranges": [
                "a pre-defined allowed range"
            ],
        },
