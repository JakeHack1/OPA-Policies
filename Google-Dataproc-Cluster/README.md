# Google Dataproc Cluster

Wednesday, August 31, 2022

5:04 PM

## Block Assigning Dataproc Role roles/Dataproc.worker to users or groups (Service Accounts exempted)

**KCC**
Kind: IAMPolicy or IAMPolicyMember

- Allow 'create' events for resources of 'kind: IAMPolicy' and field 'spec -\> bindings -\> role' or of 'kind: IAMPolicyMember' and field 'spec -\> bindings -\> role' if the user being added is a service account
- Block 'create' events for resources of 'kind: IAMPolicy' and field 'spec -\> bindings -\> role' or of 'kind: IAMPolicyMember' and field 'spec -\> bindings -\> role' if the user being added is a user or a group
- Do nothing for 'update' or 'delete' events
- See documentation here: [IAMPolicy  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/iam/iampolicy)

**Terraform**

- Allow 'create' events for resources of google\_dataproc\_cluster\_iam\_policy, google\_dataproc\_cluster\_iam\_binding, google\_dataproc\_cluster\_iam\_member and field 'role' or 'binding -\> role' where the user being added is a service account
- Block 'create' events for resources of google\_dataproc\_cluster\_iam\_policy, google\_dataproc\_cluster\_iam\_binding, google\_dataproc\_cluster\_iam\_member and field 'role' or 'binding -\> role' where the user being added is a user or a group
- Do nothing for 'no-op' or 'delete' events

## Block Assigning Dataproc Permissions Dataproc.viewer + monitoring.timeSeries.list in Custom Roles

**KCC**
Kind: IAMCustomRole

- Block events for resources of type 'kind: IAMCustomRole' where spec values do not match as described in below table
- [IAMCustomRole  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/iam/iamcustomrole)

```yaml
spec:
  permissions:
  - string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| iam.cnrm.cloud.google.com | IAMCustomRole | spec.permissions | not equals | Dataproc.viewer + monitoring.timeSeries.list |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_project\_iam\_custom\_role" or "google\_organization\_iam\_custom\_role" where field 'resource -\> permissions' is not equal to Dataproc.viewer + monitoring.timeSeries.list
- Do nothing for 'delete' events

google\_organization\_iam\_custom\_role
```json
    "resource\_changes": [

        {

            "type": "google\_organization\_iam\_custom\_role",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "permissions": [

                        "dataproc.viewer",

                        "monitoring.timeSeries.list"

                    ],

                },
```

google\_project\_iam\_custom\_role
```json
    "resource\_changes": [

        {

            "type": "google\_project\_iam\_custom\_role",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "permissions": [

                        "iam.roles.create",

                        "iam.roles.delete",

                        "iam.roles.list"

                    ],

                },
```

## Require Persistent History Server to be Enabled, which requires enable\_http\_port\_access set to true and specific properties set

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)
- Must also add one of the cluster properties from the following list:
  - [Cluster properties  |  Dataproc Documentation  |  Google Cloud](https://cloud.google.com/dataproc/docs/concepts/configuring-clusters/cluster-properties#dataproc_service_properties_table)

```yaml
spec:
  config:
    endpointConfig:
      enableHttpPortAccess: Boolean
    softwareConfig:
      properties:
        string: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.endpointConfig.enableHttpPortAccess | Equals | true |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.softwareConfig.properties.string | Equals | A valid property |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> endpoint\_config -\> enable\_http\_port\_access' is equal to true AND where field 'resource -\> cluster\_config -\> software\_config -\> override\_properties' is equal to a valid property
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "endpoint\_config": [

                                {

                                    "enable\_http\_port\_access": true

                                }

                            ],

                            "software\_config": [

                                {

                                    "override\_properties": {

                                        "dataproc:dataproc.allow.zero.workers": "true"

                                    }

                                }

                            ],
```

## Set job logging configuration per standard - driver\_log\_levels set to INFO AND cluster\_config, software\_config, override\_properties on cluster creation

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_job" where field 'resource -\> spark\_config -\> logging\_config -\> root' is equal to INFO AND where the cluster\_config, software\_config, and override\_properties are set as shown during cluster creation
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "endpoint\_config": [

                                {

                                    "enable\_http\_port\_access": true

                                }

                            ],

                            "software\_config": [

                                {

                                    "override\_properties": {

                                        "dataproc:dataproc.allow.zero.workers": "true"

                                    }

                                }

                            ],

                        }

                    ],

                },

        {

            "type": "google\_dataproc\_job",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "spark\_config": [

                        {

                            "logging\_config": [

                                {

                                    "driver\_log\_levels": {

                                        "root": "INFO"

                                    }

                                }

                            ],
```

## Require Hadoop secure mode which includes setting the enable\_kerberos flag in the kerberos\_config block

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    securityConfig:
      kerberosConfig:
        enableKerberos: boolean
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.securityConfig.kerberosConfig.enableKerberos | Equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> security\_config -\> kerberos\_config -\> enable\_kerberos' is equal to true
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "security\_config": [

                                {

                                    "kerberos\_config": [

                                        {

                                            "enable\_kerberos": true,

                                        }

                                    ]

                                }

                            ],
```

## Require metdata with block-project-ssh-keys=true and enable-oslogin=false

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    gceClusterConfig:
      metadata:
        string: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.metadata.block-project-ssh-keys | Equals | true |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.metadata.enable-oslogin | Equals | false |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> metadata -\> enable\_oslogin' is equal to false
- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> metadata -\> block\_project\_ssh\_keys' is equal to true
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "gce\_cluster\_config": [

                                {

                                    "metadata": {

                                        "block\_project\_ssh\_keys": "true",

                                        "enable\_oslogin": "false"

                                    },

                                }

                            ],
```

## Block deployment if cluster\_config, software\_config, override\_properties contains core:fs.gs.bucket.delete.enable=true

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    softwareConfig:
      properties:
        string: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.softwareConfig.properties.string | Not equal | Core:fs.gc.bucket.delete.enable = true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> software\_config -\> override\_properties' is not equal to Core:fs.gc.bucket.delete.enable = true
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "software\_config": [

                                {

                                    "override\_properties": {

                                        "Core:fs.gc.bucket.delete.enable": "true"

                                    }

                                }

                            ],

                        }

                    ],
```

## Require subnet specified in gce\_cluster\_config

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    gceClusterConfig:
      subnetworkRef:
        external: string
        name: string
        namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.subnetworkRef.external/name/namespace | Equal | Non-null |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> subnetwork' is equal to a non null value
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "gce\_cluster\_config": [

                                {

                                    "subnetwork": "custom subnetwork specified",

                                }

                            ],
```

## Supplied 'region' is in allowed GCP regions only (allow\_list)

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  location: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.location | Equals | A region on the 'allow\_list' |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> region' is equal to a region in the 'allow\_list'
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "region": "us-central1",

                },
```

## Require CMEK for all node instances - kms\_key\_name supplied in encryption\_config

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    encryptionConfig:
      gcePdKmsKeyRef:
        external: string
        name: string
        namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.encryptionConfig.gcePdKmsKeyRef.external/name/namespace | Equals | A valid CMEK |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> encryption\_config -\> kms\_key\_name' is equal to a valid CMEK
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

\                            "encryption\_config": [

                                {

                                    "kms\_key\_name": "Valid CMEK"

                                }

                            ],
```

## Require Service Account specified in gce\_cluster\_config

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    gceClusterConfig:
      serviceAccountRef:
        external: string
        name: string
        namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.serviceAccountRef.external/name/namespace | Equals | A non null value |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> service\_account' is equal to a valid service account
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "gce\_cluster\_config": [

                                {

                                    "service\_account": "A valid service account",

                                }

                            ],
```

## Require internal ip only is true specified in gce\_cluster\_config

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

```yaml
spec:
  config:
    gceClusterConfig:
      internalIPOnly: boolean
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.internalIPOnly | Equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> internal\_ip\_only' is equal to true
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "gce\_cluster\_config": [

                                {

                                    "internal\_ip\_only": true,

                                }

                            ],
```

## Required Shielded VM specified in gce\_cluster\_config with all three Shielded VM options configured

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

Cannot find anything for shielded VM options in KCC doc's

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.internalIPOnly | Equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_dataproc\_cluster" where field 'resource -\> cluster\_config -\> gce\_cluster\_config -\> shielded\_instance\_config -\> enable\_integrity\_monitoring/enable\_secure\_boot/enable\_vtpm' is equal to true
- Do nothing for 'delete' events

```json
    "resource\_changes": [

        {

            "type": "google\_dataproc\_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster\_config": [

                        {

                            "gce\_cluster\_config": [

                                {

                                    "shielded\_instance\_config": [

                                        {

                                            "enable\_integrity\_monitoring": true,

                                            "enable\_secure\_boot": true,

                                            "enable\_vtpm": true

                                        }

                                    ],

                                }

                            ],
```
