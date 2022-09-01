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

- Allow 'create' events for resources of google_dataproc_cluster_iam_policy, google_dataproc_cluster_iam_binding, google_dataproc_cluster_iam_member and field 'role' or 'binding -\> role' where the user being added is a service account
- Block 'create' events for resources of google_dataproc_cluster_iam_policy, google_dataproc_cluster_iam_binding, google_dataproc_cluster_iam_member and field 'role' or 'binding -\> role' where the user being added is a user or a group
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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google _project_iam_custom_role" or "google_organization_iam_custom_role" where field 'resource -\> permissions' is not equal to Dataproc.viewer + monitoring.timeSeries.list
- Do nothing for 'delete' events

google_organization_iam_custom_role
```json
    "resource_changes": [

        {

            "type": "google_organization_iam_custom_role",

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

google_project_iam_custom_role
```json
    "resource_changes": [

        {

            "type": "google_project_iam_custom_role",

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

## Require Persistent History Server to be Enabled, which requires enable_http_port_access set to true and specific properties set

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> endpoint_config -\> enable_http_port_access' is equal to true AND where field 'resource -\> cluster_config -\> software_config -\> override_properties' is equal to a valid property
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "endpoint_config": [

                                {

                                    "enable_http_port_access": true

                                }

                            ],

                            "software_config": [

                                {

                                    "override_properties": {

                                        "dataproc:dataproc.allow.zero.workers": "true"

                                    }

                                }

                            ],
```

## Set job logging configuration per standard - driver_log_levels set to INFO AND cluster_config, software_config, override_properties on cluster creation

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_job" where field 'resource -\> spark_config -\> logging_config -\> root' is equal to INFO AND where the cluster_config, software_config, and override_properties are set as shown during cluster creation
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "endpoint_config": [

                                {

                                    "enable_http_port_access": true

                                }

                            ],

                            "software_config": [

                                {

                                    "override_properties": {

                                        "dataproc:dataproc.allow.zero.workers": "true"

                                    }

                                }

                            ],

                        }

                    ],

                },

        {

            "type": "google_dataproc_job",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "spark_config": [

                        {

                            "logging_config": [

                                {

                                    "driver_log_levels": {

                                        "root": "INFO"

                                    }

                                }

                            ],
```

## Require Hadoop secure mode which includes setting the enable_kerberos flag in the kerberos_config block

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> security_config -\> kerberos_config -\> enable_kerberos' is equal to true
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "security_config": [

                                {

                                    "kerberos_config": [

                                        {

                                            "enable_kerberos": true,

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> metadata -\> enable_oslogin' is equal to false
- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> metadata -\> block_project_ssh_keys' is equal to true
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "gce_cluster_config": [

                                {

                                    "metadata": {

                                        "block_project_ssh_keys": "true",

                                        "enable_oslogin": "false"

                                    },

                                }

                            ],
```

## Block deployment if cluster_config, software_config, override_properties contains core:fs.gs.bucket.delete.enable=true

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> software_config -\> override_properties' is not equal to Core:fs.gc.bucket.delete.enable = true
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "software_config": [

                                {

                                    "override_properties": {

                                        "Core:fs.gc.bucket.delete.enable": "true"

                                    }

                                }

                            ],

                        }

                    ],
```

## Require subnet specified in gce_cluster_config

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> subnetwork' is equal to a non null value
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "gce_cluster_config": [

                                {

                                    "subnetwork": "custom subnetwork specified",

                                }

                            ],
```

## Supplied 'region' is in allowed GCP regions only (allow_list)

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
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.location | Equals | A region on the 'allow_list' |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> region' is equal to a region in the 'allow_list'
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "region": "us-central1",

                },
```

## Require CMEK for all node instances - kms_key_name supplied in encryption_config

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> encryption_config -\> kms_key_name' is equal to a valid CMEK
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

\                            "encryption_config": [

                                {

                                    "kms_key_name": "Valid CMEK"

                                }

                            ],
```

## Require Service Account specified in gce_cluster_config

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> service_account' is equal to a valid service account
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "gce_cluster_config": [

                                {

                                    "service_account": "A valid service account",

                                }

                            ],
```

## Require internal ip only is true specified in gce_cluster_config

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> internal_ip_only' is equal to true
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "gce_cluster_config": [

                                {

                                    "internal_ip_only": true,

                                }

                            ],
```

## Required Shielded VM specified in gce_cluster_config with all three Shielded VM options configured

**KCC**
Kind: DataprocCluster

- Block events for resources of type 'kind: DataprocCluster' where spec values do not match as described in below table
- [DataprocCluster  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/dataproc/dataproccluster)

Cannot find anything for shielded VM options in KCC doc's

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| dataproc.cnrm.cloud.google.com | DataprocCluster | spec.config.gceClusterConfig.internalIPOnly | Equals | true |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_dataproc_cluster" where field 'resource -\> cluster_config -\> gce_cluster_config -\> shielded_instance_config -\> enable_integrity_monitoring/enable_secure_boot/enable_vtpm' is equal to true
- Do nothing for 'delete' events

```json
    "resource_changes": [

        {

            "type": "google_dataproc_cluster",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "cluster_config": [

                        {

                            "gce_cluster_config": [

                                {

                                    "shielded_instance_config": [

                                        {

                                            "enable_integrity_monitoring": true,

                                            "enable_secure_boot": true,

                                            "enable_vtpm": true

                                        }

                                    ],

                                }

                            ],
```
