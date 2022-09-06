# Google Cloud KMS

Tuesday, September 9, 2022

10:48 AM

## KMS Keys/Keyrings Permissions Must not Include "allUsers" or "allAuthenticatedUsers"

**Terraform:**

- Allow 'create' or 'no-op' or 'update' events for resources of type "google _kms_crypto_key_iam_policy" or "google_kms_crypto_key_iam_binding" and "google_kms_crypto_key_iam_member" and "google_kms _key_ring_iam_policy" and "google_kms _key_ring_iam_binding" and "google_kms _key_ring_iam_member" where field 'resource -\> members' is not equal to 'allUsers' or 'allAuthenticatedUsers'
- Do nothing for 'delete' events

google_kms_crypto_key_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyEncrypter\"}]}"

                },
```

google_kms_crypto_key_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_binding",

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

google_kms_crypto_key_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "member": "allUsers", (or "allAuthenticatedUsers")

                },
```

google_kms _key_ring_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/editor\"}]}"

                },
```

google_kms _key_ring_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_binding",

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

google_kms _key_ring_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_member",

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

- Allow 'create' or 'no-op' or 'update' events for resources of type "google_kms_crypto_key" or "google_kms_key_ring" where field 'resource -\> rotation_period' is equal to 90 days
- Do nothing for 'delete' events

google_kms_crypto_key
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "rotation_period": "7776000s",

                },
```

google_kms_crypto_key
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

- Allow 'create' or 'update' events for resources of type "google_kms_crypto_key_iam_policy" or "google_kms_crypto_key_iam_binding" and "google_kms_crypto_key_iam_member" and "google_kms _key_ring_iam_policy" and "google_kms _key_ring_iam_binding" and "google_kms _key_ring_iam_member" where permissions .restore, .destroy, and .update are only given to authorized service accounts and AD groups

- Do nothing for 'delete' or 'no-op' events

google_kms_crypto_key_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google_kms_crypto_key_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.update.restore"

                },
```

google_kms_crypto_key_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.restore.update"

                },
```

google_kms _key_ring_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "policy_data": "{\"bindings\":[{\"members\":[\"allAuthenticatedUsers\",\"allUsers\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google_kms _key_ring_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "role": "roles/cloudkms.cryptoKeyVersion.destroy.restore.update"

                },
```

google_kms _key_ring_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_member",

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

- Allow 'create' or 'update' events for resources of type "google_kms_crypto_key_iam_policy" or "google_kms_crypto_key_iam_binding" and "google_kms_crypto_key_iam_member" and "google_kms _key_ring_iam_policy" and "google_kms _key_ring_iam_binding" and "google_kms _key_ring_iam_member" where field for "resource -\> kms_key_name" is equal to a PROD KMS Key and the field "resource -\> member" is an approved service account/group

- Do nothing for 'delete' or 'no-op' events

google_kms_crypto_key_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "name": "PROD KMS Key",

                    "policy_data": "{\"bindings\":[{\"members\":[\"approvedADGroup@email.com\",\"approvedServiceAccount@email.com\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google_kms_crypto_key_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "crypto_key_id": "PROD KMS Key",

                    "members": [

                        "approvedADGroup@email.com",

                        "approvedServiceAccount@email.com"

                    ],

                },
```

google_kms_crypto_key_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_crypto_key_iam_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "crypto_key_id": "PROD KMS Key",

                    "member": "approvedAccounts/Groups",

                },
```

google_kms _key_ring_iam_policy
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "name": "PROD KMS Key",

                    "policy_data": "{\"bindings\":[{\"members\":[\"approvedADGroups\",\"approvedServiceAccounts\"],\"role\":\"roles/cloudkms.cryptoKeyVersion.destroy.restore.update\"}]}"

                },
```

google_kms _key_ring_iam_binding
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_binding",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "key_ring_id": "PROD KMS Key",

                    "members": [

                        "approvedADGroups",

                        "approvedServiceAccounts"

                    ],

                },
```

google_kms _key_ring_iam_member
```json
    "resource_changes": [

        {

            "type": "google_kms_key_ring_iam_member",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "key_ring_id": "PROD KMS Key",

                    "member": "approvedServiceAccounts/ADGroups",

                },
```

## The constraints/gcp.restrictNonCmekServices org policy must be used for Services that support GCP KMS CMEK

**Terraform:**

- Allow 'create' or 'update' events for resources of type "google_iam_deny_policy" where field "resource -\> rules -\> deny_rule -\> denied_permissions" is equal to 'constraints/gcp.restrictNonCmekServices'

- Do nothing for 'delete' or 'no-op' events

```json
    "resource_changes": [

        {

            "type": "google_iam_deny_policy",

            "change": {

                "actions": [

                    "create"

                ],

                "after": {

                    "rules": [

                        {

                            "deny_rule": [

                                {

                                    "denied_permissions": [

                                        "constraints/gcp.restrictNonCmekServices"

                                    ],

                                }

                            ],

                        }

                    ],

                },
```
