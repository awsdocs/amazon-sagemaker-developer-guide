# Security and Access Control<a name="feature-store-security"></a>

 Amazon SageMaker Feature Store enables you to create two types of stores: an online store or offline store\. The online store is used for low latency real\-time inference use cases whereas the offline store is used for training and batch inference use cases\. When you create a feature group for online or offline use you can provide a KMS customer master key \(CMK\) to encrypt all your data at rest\. In case you do not provide a CMK then we ensure that your data is encrypted on the server side using an AWS owned CMK or AWS managed CMK\. While creating a feature group, you can select storage type and optionally provide a CMK for encrypting data, then you can call various APIs for data management such as `PutRecord`, `GetRecord`, `DeleteRecord`\.  

 For more information about CMKs, see [Customer Managed Keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys)\. For more information about KMS, please see [KMS](https://aws.amazon.com/kms/)\.  

## Using KMS Permissions for Amazon SageMaker Feature Store<a name="feature-store-kms-cmk-permissions"></a>

 Encryption at rest protects Feature Store under an AWS KMS customer master key \(CMK\)\. By default, it uses an [AWS owned CMK for OnlineStore and AWS managed CMK for OfflineStore](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk)\. Feature Store supports an option to encrypt your online or offline store under [customer managed CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk)\. You can select the CMK for Feature Store when you create your online or offline store, and they can be different for each store\.  

 Feature Store supports only [symmetric CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#symmetric-cmks)\. You cannot use an [asymmetric CMK](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#asymmetric-cmks) to encrypt your data in your online or offline store\. For help determining whether a CMK is symmetric or asymmetric, see [Identifying symmetric and asymmetric CMKs](https://docs.aws.amazon.com/kms/latest/developerguide/find-symm-asymm.html)\.

When you use a customer managed CMK, you can take advantage of the following features: 
+  You create and manage the CMK, including setting the [key policies](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html), [IAM policies](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) and [grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) to control access to the CMK\. You can [enable and disable](https://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html) the CMK, enable and disable [automatic key rotation](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html), and [delete the CMK](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html) when it is no longer in use\. 
+  You can use a customer managed CMK with [imported key material](https://docs.aws.amazon.com/kms/latest/developerguide/importing-keys.html) or a customer managed CMK in a [custom key store](https://docs.aws.amazon.com/kms/latest/developerguide/custom-key-store-overview.html) that you own and manage\. 
+  You can audit the encryption and decryption of your online or offline store by examining the API calls to AWS KMS in [AWS CloudTrail logs](https://docs.aws.amazon.com/kms/latest/developerguide/services-dynamodb.html#dynamodb-cmk-trail)\. 

You do not pay a monthly fee for AWS owned CMKs\. Customer managed CMKs [incur a charge](https://aws.amazon.com/kms/pricing/) for each API call and AWS KMS quotas apply to these CMKs\.  

## Authorizing Use of Your CMK for Your Online Store<a name="feature-store-authorizing-cmk-online-store"></a>

 If you use a [customer managed CMK ](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your online store, the policies on that CMK must give Feature Store permission to use it on your behalf\. You have full control over the policies and grants on a customer managed CMK\.  

 Feature Store does not need additional authorization to use the default [AWS owned CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) to protect your online or offline stores in your AWS account\.  

### Customer Managed CMK Key Policy<a name="feature-store-customer-managed-cmk-policy"></a>

 When you select a [customer managed CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your Online Store, Feature Store must have permission to use the CMK on behalf of the principal who makes the selection\. That principal, a user or role, must have the permissions on the CMK that Feature Store requires\. You can provide these permissions in a [key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html), an [IAM policy](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html), or a [grant](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html)\. At a minimum, Feature Store requires the following permissions on a customer managed CMK: 
+  "kms:Encrypt", "kms:Decrypt", "kms:DescribeKey", "kms:CreateGrant", "kms:RetireGrant", "kms:ReEncryptFrom", "kms:ReEncryptTo", "kms:GenerateDataKey", "kms:ListAliases", "kms:ListGrants", "kms:RevokeGrant" 

 For example, the following example key policy provides only the required permissions\. The policy has the following effects: 
+  Allows Feature Store to use the CMK in cryptographic operations and create grants, but only when it is acting on behalf of principals in the account who have permission to use your Feature Store\. If the principals specified in the policy statement don't have permission to use your Feature Store , the call fails, even when it comes from the Feature Store service\.  
+  The [kms:ViaService](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-via-service) condition key allows the permissions only when the request comes from FeatureStore on behalf of the principals listed in the policy statement\. These principals can't call these operations directly\. Note the `kms:ViaService` value, sagemaker\.\*\.`amazonaws.com.` 
+  Gives the CMK administrators read\-only access to the CMK and permission to revoke grants, including the grants that Feature Store uses to protect your data\.  
+  Gives Feature Store read\-only access to the CMK\. In this case, Feature Store can call these operations directly\. It does not have to act on behalf of an account principal\.  

 Before using an example key policy, replace the example principals with actual principals from your AWS account\. 

```
{"Id": "key-policy-feature-store",
  "Version":"2012-10-17",
  "Statement": [
    {"Sid" : "Allow access through Amazon SageMaker Feature Store for all principals in the account that are authorized to use Amazon SageMaker Feature Store",
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::111122223333:user/featurestore-user"},
      "Action": [
        "kms:Encrypt",
        "kms:Decrypt",
        "kms:DescribeKey",
        "kms:CreateGrant",
        "kms:RetireGrant",
        "kms:ReEncryptFrom",
        "kms:ReEncryptTo",
        "kms:GenerateDataKey",
        "kms:ListAliases",
        "kms:ListGrants"
      ],
      "Resource": "*",      
      "Condition": {"StringLike": {"kms:ViaService" : "sagemaker.*.amazonaws.com"
         }
      }
    },
    {"Sid":  "Allow administrators to view the CMK and revoke grants",
      "Effect": "Allow",
      "Principal": {"AWS": "arn:aws:iam::111122223333:role/featurestore-admin"
       },
      "Action": [
        "kms:Describe*",
        "kms:Get*",
        "kms:List*",
        "kms:RevokeGrant"
      ],
      "Resource": "*"
    },
    {"Sid":  "Allow Feature Store to get information about the CMK",
      "Effect": "Allow",
      "Principal": {"Service":["sagemaker.amazonaws.com"]
      },
      "Action": [
        "kms:Describe*",
        "kms:Get*",
        "kms:List*"
      ],
      "Resource": "*"
    }
  ]
}
```

## Using Grants to Authorize Feature Store<a name="feature-store-using-grants-authorize"></a>

 In addition to key policies, Feature Store uses grants to set permissions on the customer managed CMK\. To view the grants on a CMK in your account, use the [ListGrants](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListGrants.html) operation\. Feature Store does not need grants, or any additional permissions, to use the [AWS owned CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk) to protect your online store\.  

 Feature Store uses the grant permissions when it performs background system maintenance and continuous data protection tasks\.  

 Each grant is specific to an online store\. If the account includes multiple stores encrypted under the same CMK, there will be unique grants per `FeatureGroup` using the same CMK\. 

 The key policy can also allow the account to [revoke the grant](https://docs.aws.amazon.com/kms/latest/APIReference/API_RevokeGrant.html) on the CMK\. However, if you revoke the grant on an active encrypted online store, Feature Store won't be able to protect and maintain the store\.  

## Monitoring Feature Store interaction with AWS KMS<a name="feature-store-monitoring-kms-interaction"></a>

 If you use a [customer managed CMK](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your online or offline store, you can use AWS CloudTrail logs to track the requests that Feature Store sends to AWS KMS on your behalf\.  

## Accessing Data in Your Online Store<a name="feature-store-accessing-data-online-store"></a>

 The **caller \(either IAM user or IAM role\)** to **ALL DataPlane operations \(Put, Get, DeleteRecord\)** must have below permissions on the customer managed CMK: 

```
"kms:Decrypt"
```

## Authorizing Use of Your CMK for Offline Store<a name="feature-store-authorizing-use-cmk-offline-store"></a>

 The **roleArn** that is passed as a parameter to `createFeatureGroup` must have below permissions to the OfflineStore KmsKeyId: 

```
"kms:GenerateDataKey"
```