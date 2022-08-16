# Google Compute Firewall

Thursday, August 11, 2022

2:17 PM

**Google Compute Firewall**

Kind: ComputeFirewall

**Firewalls must have descriptions**

**KCC**

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

spec:

description: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.description
 | equals | Non-null valuex |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_compute\_firewall" where field 'resource -\> description' equals a non-null value
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_compute\_firewall",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "description": "Any non-null value"

                },

**Google Compute Firewall**

Kind: ComputeFirewall

**Firewalls must have logging enabled**

**KCC**

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

spec:

logConfig:

metadata: INCLUDE\_ALL\_METADATA

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.logConfig.metadata
 | equals | INLCUDE\_ALL\_METADATA |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_compute\_firewall" where field 'resource -\> log\_config -\> metadata' equals "INCLUDE\_ALL\_METADATA"
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_compute\_firewall",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "log\_config": [

                        {

                            "metadata": "INCLUDE\_ALL\_METADATA"

                        }

                    ],

                },

**Google Compute Firewall**

Kind: ComputeFirewall

**Firewalls should not be created with 0.0.0.0/0 CIDR block**

**KCC**

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

spec:

sourceRanges: string

destinationRanges: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.sourceRanges
 | Not equal to | 0.0.0.0/0 |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.destinationRanges
 | Not equal to | 0.0.0.0/0 |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_compute\_firewall" where field 'resource -\> source\_ranges' or 'resource -\> destination\_ranges' is not equal "0.0.0.0/0"
- Do nothing for 'delete' events

    "resource\_changes": [

        {

            "type": "google\_compute\_firewall",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "source\_ranges": [

                        "0.0.0.0/0"

                    ],

                },

**Google Compute Firewall**

Kind: ComputeFirewall

**Firewalls should only allow specific ranges per network**

**KCC**

- Block events for resources of type 'kind: ComputeFirewall' where spec values do not match as described in below table
- [ComputeFirewall  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computefirewall)

spec:

sourceRanges: string

destinationRanges: string

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.sourceRanges
 | Equals | An allowed CIDR range |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeFirewall | spec.destinationRanges
 | Equals | An allowed CIDR range |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_compute\_firewall" where field 'resource -\> source\_ranges' or 'resource -\> destination\_ranges' is equal to an allowed range
- Do nothing for 'delete' events

"resource\_changes": [

        {

            "type": "google\_compute\_firewall",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "source\_ranges": [

                        "a pre-defined allowed range"

                    ],

"destination\_ranges": [

                        "a pre-defined allowed range"

                    ],

                },
