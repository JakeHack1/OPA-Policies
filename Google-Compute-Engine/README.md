# Google Compute Engine

Monday, August 1, 2022

12:07 PM

## Image User Role Config

**KCC**
Kind: IAMPolicy
Kind: IAMPolicyMember

- Control the use of compute.imageUser role
- [IAMPolicyMember  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/iam/iampolicymember)

```yaml
spec:
  member: string
    role: compute.imageUser
```

**Terraform:**

- Allow 'create' events for resources of google_organization_iam_policy, google_organization_iam_binding, google_organization_iam_member, google_project_iam_policy, google_project_iam_binding, google_project_iam_member, google_folder_iam_policy, google_folder_iam_binding, google_folder_iam_member and field 'role' or 'binding -\> role' (depending on resource type) has roles (compute.imageUser) if the 'member' field has a 'group' in the allow list
- Block 'create' events for resources of google_organization_iam_policy, google_organization_iam_binding, google_organization_iam_member, google_project_iam_policy, google_project_iam_binding, google_project_iam_member, google_folder_iam_policy, google_folder_iam_binding, google_folder_iam_member and field 'role' or 'binding -\> role' (depending on resource type) has roles (compute.imageUser) if the 'member' field has a 'group' not in the allow list
- Do nothing for 'no-op' or 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

## Image Approval Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table
- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance

```yaml
spec:
 machineType: n1-standard-1
   zone: us-west1-a
   bootDisk:
     initializeParams:
       size: 24

      type: pd-ssd
       sourceImageRef:
         external: debian-cloud/debian-9
```

ComputeInstanceTemplate
```yaml
spec:
  disk:
  - sourceImageRef:
      name: instancetemplate-dep
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | spec.bootDisk.initializeParams.sourceImageRef.external | equals | predefined image list |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | spec.disk.sourceImageRef.name | equals | predefined image list |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance"and "google_compute_instance_template" where field 'boot_disk -\> initialize_params -\> image' is not in a predefined image list.
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "boot_disk": [
                {
                    "initialize_params": [
                        {
                            "image": "only-approved-images"
                        }
                    ],
                 }
             ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "disk": [
                {
                    "source_image": "Approved Images"
                }
            ],
```

## Prevent External IP Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in the table below
- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
networkInterface:
  -accessConfig:
    -natIpRef:
        external: string
        name: string
        namespace: string
```
ComputeInstanceTemplate
```yaml
networkInterface:
  - accessConfig:
    - natIpRef:
      external: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | spec.networkInterface.accessConfig.natIpRef.external | equals | null |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | networkInterface.accessConfig.natIpRef.external | equals | null |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'network_interface -\> access_config ' is populated (Omit to ensure that the instance is not accessible from the Internet)
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "network_interface": [
                {
                    "access_config": [], #Notice access_config is empty
                }
            ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "network_interface": [
                {
                    "access_config": [], # Access_config is empty
                }
            ],
```

## Prevent IP Forwarding Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance AND ComputeInstanceTemplate
```yaml
spec:
  canIpForward: false
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.canIpForward | equals | False, or ignore if VM instanr project is in exemption list |

**Terraform**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template"where field 'resource -\> can_ip_forward' equals 'true' unless VM instance or project is an exemption list
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "can_ip_forward": false,
        }
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "can_ip_forward": false,
        }
```

## Prevent Default SA Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
  serviceAccount:
    serviceAccountRef:
      external: gcp_serviceaccount_email
      name: object_name
      namespace: object_namespace
```

ComputeInstanceTemplate
```yaml
spec:
  serviceAccount:
    serviceAccountRef:
      name: instancetemplate-dep
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.serviceAccount.serviceAccountRef.name or external | equals | Any string value - null not permitting
 |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'service_account -\> email' field exists with a string value (when directly referenced)
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "service_account": [
                {
                    "email": "google_service_account.email", #Non default
                    "scopes": [
                        [https://www.googleapis.com/auth/cloud-platform](https://www.googleapis.com/auth/cloud-platform)
                    ] #separate rule dis-allowing cloud-platform scope
                }
            ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "service_account": [
                {
                    "email": "google_service_account.non-default.email",
                    "scopes": [
                        "non-cloud-platform"
                    ]
                }
            ],
```

## Prevent OSLogin Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance and ComputeInstanceTemplate
```yaml
spec:
metadata:
- key: enable-os-login
value: true
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.metadata.key | Equals | enable-os-login does not exist, or when exists value = false
 |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'resource -\> metadata -\> enable-oslogin' is equal to true
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "metadata": {
                "enable-oslogin": "true",
            },
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "metadata": {
                "enable-oslogin": "true"
             },
```

## Enable vTPM, Secure Boot, Integrity Monitoring Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance AND ComputeInstanceTemplate
```yaml
spec:
  shieldedInstanceConfig:
    enableIntegrityMonitoring: true
    enableSecureBoot: true
    enableVtpm: true
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.shieldedInstanceConfig.enableIntegrityMonitoring | equals | true |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.shieldedInstanceConfig.enableSecureBoot | equals | true |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.shieldedInstanceConfig.enableVtpm | equals | true |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'shielded_instance_config -\> enable_secure_boot, enable_vtpm, enable_integrity_monitoring' fields are not equal to true
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "shielded_instance_config": [
                {
                    "enable_integrity_monitoring": true,
                    "enable_secure_boot": true,
                    "enable_vtpm": true
                }
            ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "shielded_instance_config": [
                {
                    "enable_integrity_monitoring": true,
                    "enable_secure_boot": true,
                    "enable_vtpm": true
                }
            ],
```

## CMEK for Persistent Disks Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
  bootDisk:
    kmsKeyRef:
      external: string
      name: string
      namespace: string
  attachedDisk:
    kmsKeyRef:
      external: string
      name: string
      namespace: string
```

ComputeInstanceTemplate
```yaml
spec:
  disk:
    diskEncryptionKey:
      kmsKeyRef:
        external: string
        name: string
        namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | bootDisk.kmsKeyRef | Must exist | External or name fields must exist to set CMEK key |
| compute.cnrm.cloud.google.com | ComputeInstance | attachedDisk.kmsKeyRef | Must exist | External or name fields must exist to set CMEK key |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | disk.diskEncryption.kmsKeyRef | Must exist | External or name fields must exist to set CMEK key |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'boot_disk -\> kms_key_self_link, or attached_disk -\> kms_key_self_link' fields don't use CMEK for persistent disks (boot_disk required, attached_disk field may not exist but CMEK is required if it does exist)
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "attached_disk": [
                {
                    "kms_key_self_link": "CMEK Key",
                    "source": "Approved Image"
                }
            ],
            "boot_disk": [
                {
                    "initialize_params": [
                        {
                            "image": "debian-cloud/debian-9"
                        }
                    ], 
                    "kms_key_self_link": "CMEK Key",
                }
            ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "disk": [
                {
                    "disk_encryption_key": [
                        {
                            "kms_key_self_link": "CMEK Key"
                        }
                    ],
                }
            ],
```

## Block Cloud-Platform Access Scopes Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance AND ComputeInstanceTemplate
```yaml
spec
  serviceAccount:
    scopes:
    - string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance, ComputeInstanceTemplate | spec.serviceAccount.scopes | Not equal | 'cloud-platform'
 |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'resource -\> service_account -\> scopes' is equal to 'cloud-platform'
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "service_account": [
                {
                    "email": "google_service_account.email",
                    "scopes": [
                        "cloud-platform"
                    ]
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "service_account": [
                {
                    "email": "google_service_account.non-default.email",
                    "scopes": [
                        "cloud-platform"
                    ]
                }
            ],
```

## Disallow use of Local SSD Config

"Scratch Disks" – aka Local SSD's – do not support KMS encryption keys.

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
  scratchDisk:
  - interface:
```

ComputeInstanceTemplate
```yaml
spec:
  disk:
    diskType: local-ssd
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | spec.scratchDisk.interface | Not Exist | Should not be attached so field should not exist
 |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | spec.disk.diskType | Not equal | local-ssd |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'resource -\> scratch_disk' exists (no Local SSD's attached)
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
``json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "scratch_disk": [
                {
                    "interface": "SCSI"
                }
            ],
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "disk": [
                {
                    "interface": "SCSI"
                }
            ],
```

## Require 'block-project-ssh-keys' Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
  metadata:
  - key: block-project-ssh-keys
    value: true
```

ComputeInstanceTemplate
```yaml
spec:
  metadata:
  - key: block-project-ssh-keys
    value: true
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | spec.metadata.key / .value | equals | block-project-ssh-keys / true |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | spec.metadata.key / .value | equals | block-project-ssh-keys / true |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'resource -\> metadata -\> block_project_ssh_keys' is not equal to "true"
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "metadata": {
                "block_project_ssh_keys": "true",
            },
```

ComputeInstanceTemplate
```json
"resource_changes": [
{
    "type": "google_compute_instance_template",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "metadata": {
                "block_project_ssh_keys": "true"
            },
```

## Block Virtual Displays Config

**KCC**
Kind: ComputeInstance
Kind: ComputeInstanceTemplate

- Block events for resources of type 'kind: ComputeInstance' and 'kind: ComputeInstanceTemplate' where spec values do not match as described in below table

- [ComputeInstance  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstance)
- [ComputeInstanceTemplate  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computeinstancetemplate#custom_resource_definition_properties)

ComputeInstance
```yaml
spec:
  enableDisplay: false
```

ComputeInstanceTemplate
```yaml
spec:
  enableDisplay: false
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstance | spec.enableDisplay | equal | false
 |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeInstanceTemplate | spec.enableDisplay | equal | false |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_instance" and "google_compute_instance_template" where field 'resource -\> enable_display' is not equal to "false"
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeInstance
```json
"resource_changes": [
{
    "type": "google_compute_instance",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "enable_display": false,
        },
```

## CMEK for Persistent Disks Config

**KCC**
Kind: ComputeDisk

- Block events for resources of type 'kind: ComputeDisk' where spec values do not match as described in below table
- [ComputeDisk  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computedisk)

ComputeDisk
```yaml
spec:
  diskEncryptionKey:
    kmsKeyRef:
      external: string
      name: string
      namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeDisk | spec.diskEncryptionKey.kmsKeyRef.external/name/namespace | equal | External or name must exist |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_disk" where field 'resource -\> disk_encryption_key -\> kms_key_self_link' is not populated
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeDisk
```json
"resource_changes": [
{
    "type": "google_compute_disk",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "disk_encryption_key": [
                {
                    "kms_key_self_link": "CMEK Key",
                    "kms_key_service_account": null,
                    "raw_key": null
                }
            ],
```

## Approved Imaged for Persistent Disks Config

**KCC**
Kind: ComputeDisk

- Block events for resources of type 'kind: ComputeDisk' where spec values do not match as described in below table
- [ComputeDisk  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computedisk)

ComputeDisk
```yaml
spec:
  imageRef:
    external: string
    name: string
    namespace: string
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeDisk | spec.imageRef.external/name/namespace | equal | Approved images |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_disk" where field 'resource -\> image' is equal to an approved image
- Do nothing for 'delete' events
- Ignore the allow list if the 'project' is in the exemption list -\> i.e., do not enforce the rule if the 'project' is in an exemption list.

ComputeDisk
```json
"resource_changes": [
{
    "type": "google_compute_disk",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "image": "Approved Images",
        },
```

## Disallow use of Local SSD Config

**KCC**
Kind: ComputeDisk

- Block events for resources of type 'kind: ComputeDisk' where spec values do not match as described in below table
- [ComputeDisk  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/compute/computedisk)

ComputeDisk
```yaml
spec:
  type: pd-ssd
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| compute.cnrm.cloud.google.com | ComputeDisk | spec.type | Not equal | local-ssd |

**Terraform:**

- Block 'create' or 'no-op' or 'update' events for resources of type "google_compute_disk" where field 'resource -\> type' is equal to "local-ssd"
- Do nothing for 'delete' events

ComputeDisk
```json
"resource_changes": [
{
    "type": "google_compute_disk",
    "change": {
        "actions": [
            "create"
        ],
        "after": {
            "type": "local-ssd", #block local-ssd
        },
