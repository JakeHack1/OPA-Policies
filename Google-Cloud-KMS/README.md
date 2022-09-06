# Google Cloud KMS

Tuesday, September 9, 2022

10:48 AM

## KMS Keys/Keyrings Permissions Must not Include "allUsers" or "allAuthenticatedUsers"

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google \_kms\_crypto\_key\_iam\_policy" or "google\_kms\_crypto\_key\_iam\_binding" and "google\_kms\_crypto\_key\_iam\_member" and "google\_kms \_key\_ring\_iam\_policy" and "google\_kms \_key\_ring\_iam\_binding" and "google\_kms \_key\_ring\_iam\_member" where field 'resource -\> members' is not equal to 'allUsers' or 'allAuthenticatedUsers'
- Do nothing for 'delete' events

google\_kms\_crypto\_key\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy\_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyEncrypter\"}]}"

                },
```

google\_kms\_crypto\_key\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "members": [

                        "allAuthenticatedUsers",

                        "allUsers"

                    ],

                },
```

google\_kms\_crypto\_key\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "member": "allUsers", (or "allAuthenticatedUsers")

                },
```

google\_kms \_key\_ring\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy\_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/editor\"}]}"

                },
```

google\_kms \_key\_ring\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "members": [

                        "allAuthenticatedUsers",

                        "allUsers"

                    ],

                },
```

google\_kms \_key\_ring\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "member": "allUsers", (or allAuthenticatedUsers)

                },
```

## Automatic Rotation of GCP KMS Symmetric Keys Must be Set to 90 Day Rotation Period

**KCC**

Kind: KMSCryptoKey, KMSKeyRing

- Block events for resources of type 'kind: KMSCryptoKey/KMSKeyRing' where spec values do not match as described in below table
- [KMSCryptoKey  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmscryptokey)
- [KMSKeyRing  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmskeyring)

KMSCryptoKey
```yaml
spec:
  rotationPeriod: string
```

KMSKeyRing
```yaml
N/A
```

| **API** | **Kind** | **Key** | **Conditional** | **Value** |
| --- | --- | --- | --- | --- |
| kms.cnrm.cloud.google.com | KMSCryptoKey | spec.rotationPeriod | Equals | 90 days |

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google\_kms\_crypto\_key" or "google\_kms\_key\_ring" where field 'resource -\> rotation\_period' is equal to 90 days
- Do nothing for 'delete' events

google\_kms\_crypto\_key
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "rotation\_period": "7776000s",

                },
```

google\_kms\_crypto\_key
```json
N/A
```

## Access to KMS IAM cryptoKeyVersion.destroy, .restore, and .update permissions must be restricted to authorized Service Accounts and AD groups

**KCC**

Kind: KMSCryptoKey, KMSKeyRing

- Allow 'create' and 'update' for resources of 'kind: IAMPolicy/IAMPolicyMember' where permissions .restore, .destroy, and .update are only given to authorized service accounts and AD groups
- Do nothing for 'delete' events
- [KMSCryptoKey  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmscryptokey)
- [KMSKeyRing  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmskeyring)

**Terraform:**

- Allow 'create' or 'update' events for resources of type "google\_kms\_crypto\_key\_iam\_policy" or "google\_kms\_crypto\_key\_iam\_binding" and "google\_kms\_crypto\_key\_iam\_member" and "google\_kms \_key\_ring\_iam\_policy" and "google\_kms \_key\_ring\_iam\_binding" and "google\_kms \_key\_ring\_iam\_member" where permissions .restore, .destroy, and .update are only given to authorized service accounts and AD groups

- Do nothing for 'delete' or 'no-op' events

google\_kms\_crypto\_key\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy\_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google\_kms\_crypto\_key\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.update.restore"

                },
```

google\_kms\_crypto\_key\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.restore.update"

                },
```

google\_kms \_key\_ring\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy\_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google\_kms \_key\_ring\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.restore.update"

                },
```

google\_kms \_key\_ring\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.restore.update"

                },
```

## DEV and UT Access to PROD KMS Keys must be restricted to approved service accounts/groups

**KCC**

- Allow 'create' and 'update' for resources of 'kind: IAMPolicy/IAMPolicyMember' where DEV and UT access to PROD KMS Keys is only given to authorized service accounts and groups
- [KMSCryptoKey  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmscryptokey)
- [KMSKeyRing  |  Config Connector Documentation  |  Google Cloud](https://cloud.google.com/config-connector/docs/reference/resource-docs/kms/kmskeyring)

**Terraform:**

- Allow 'create' or 'update' events for resources of type "google\_kms\_crypto\_key\_iam\_policy" or "google\_kms\_crypto\_key\_iam\_binding" and "google\_kms\_crypto\_key\_iam\_member" and "google\_kms \_key\_ring\_iam\_policy" and "google\_kms \_key\_ring\_iam\_binding" and "google\_kms \_key\_ring\_iam\_member" where field for "resource -\> kms\_key\_name" is equal to a PROD KMS Key and the field "resource -\> member" is an approved service account/group

- Do nothing for 'delete' or 'no-op' events

google\_kms\_crypto\_key\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "name": "PROD KMS Key",

                    "policy\_data": "{\"bindings\":[{\"members\":[\"approvedADGroup@email.com\",\"approvedServiceAccount@email.com\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google\_kms\_crypto\_key\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "crypto\_key\_id": "PROD KMS Key",

                    "members": [

                        "approvedADGroup@email.com",

                        "approvedServiceAccount@email.com"

                    ],

                },
```

google\_kms\_crypto\_key\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_crypto\_key\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "crypto\_key\_id": "PROD KMS Key",

                    "member": "approvedAccounts/Groups",

                },
```

google\_kms \_key\_ring\_iam\_policy
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "name": "PROD KMS Key",

                    "policy\_data": "{\"bindings\":[{\"members\":[\"approvedADGroups\",\"approvedServiceAccounts\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google\_kms \_key\_ring\_iam\_binding
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "key\_ring\_id": "PROD KMS Key",

                    "members": [

                        "approvedADGroups",

                        "approvedServiceAccounts"

                    ],

                },
```

google\_kms \_key\_ring\_iam\_member
```json
    "resource\_changes": [

        {

            "type": "google\_kms\_key\_ring\_iam\_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "key\_ring\_id": "PROD KMS Key",

                    "member": "approvedServiceAccounts/ADGroups",

                },
```

## The constraints/gcp.restrictNonCmekServices org policy must be used for Services that support GCP KMS CMEK

**Terraform:**

- Allow 'create' or 'update' events for resources of type "google\_iam\_deny\_policy" where field "resource -\> rules -\> deny\_rule -\> denied\_permissions" is equal to 'constraints/gcp.restrictNonCmekServices'

- Do nothing for 'delete' or 'no-op' events

```json
    "resource\_changes": [

        {

            "type": "google\_iam\_deny\_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "rules": [

                        {

                            "deny\_rule": [

                                {

                                    "denied\_permissions": [

                                        "constraints/gcp.restrictNonCmekServices"

                                    ],

                                }

                            ],

                        }

                    ],

                },
```
