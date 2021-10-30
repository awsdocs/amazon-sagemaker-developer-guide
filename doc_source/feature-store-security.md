# Security and Access Control<a name="feature-store-security"></a>

 Amazon SageMaker Feature Store enables you to create two types of stores: an online store or offline store\. The online store is used for low latency real\-time inference use cases whereas the offline store is used for training and batch inference use cases\. When you create a feature group for online or offline use you can provide a AWS Key Management Service customer managed key to encrypt all your data at rest\. In case you do not provide a AWS KMS key then we ensure that your data is encrypted on the server side using an AWS owned AWS KMS key or AWS managed AWS KMS key\. While creating a feature group, you can select storage type and optionally provide a AWS KMS key for encrypting data, then you can call various APIs for data management such as `PutRecord`, `GetRecord`, `DeleteRecord`\.

Feature Store allows you to grant or deny access to individuals at the feature group\-level and enables cross\-account access to Feature Store\. For example, you can set up developer accounts to access the offline store for model training and exploration that do not have write access to production accounts\. You can set up production accounts to access both online and offline stores\. Feature Store uses unique customer AWS KMS keys for offline and online store data at\-rest encryption\. Access control is enabled through both API and AWS KMS key access\. You can also create feature group\-level access control\. 

 For more information about customer managed key, see [customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys)\. For more information about AWS KMS, see [AWS KMS](https://aws.amazon.com/kms/)\. 

## Using AWS KMS Permissions for Amazon SageMaker Feature Store<a name="feature-store-kms-cmk-permissions"></a>

 Encryption at rest protects Feature Store under an AWS KMS customer managed key\. By default, it uses an [AWS owned customer managed key for OnlineStore and AWS managed customer managed key for OfflineStore](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk)\. Feature Store supports an option to encrypt your online or offline store under [customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk)\. You can select the customer managed key for Feature Store when you create your online or offline store, and they can be different for each store\. 

 Feature Store supports only [symmetric customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#symmetric-cmks)\. You cannot use an [asymmetric customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/symm-asymm-concepts.html#asymmetric-cmks) to encrypt your data in your online or offline store\. For help determining whether a customer managed key is symmetric or asymmetric, see [Identifying symmetric and asymmetric customer managed keys](https://docs.aws.amazon.com/kms/latest/developerguide/find-symm-asymm.html)\.

When you use a customer managed key, you can take advantage of the following features: 
+  You create and manage the customer managed key, including setting the [key policies](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html), [IAM policies](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html) and [grants](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html) to control access to the customer managed key\. You can [enable and disable](https://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html) the customer managed key, enable and disable [automatic key rotation](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html), and [delete the customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/deleting-keys.html) when it is no longer in use\. 
+  You can use a customer managed key with [imported key material](https://docs.aws.amazon.com/kms/latest/developerguide/importing-keys.html) or a customer managed key in a [custom key store](https://docs.aws.amazon.com/kms/latest/developerguide/custom-key-store-overview.html) that you own and manage\. 
+  You can audit the encryption and decryption of your online or offline store by examining the API calls to AWS KMS in [AWS CloudTrail logs](https://docs.aws.amazon.com/kms/latest/developerguide/services-dynamodb.html#dynamodb-cmk-trail)\. 

You do not pay a monthly fee for AWS owned customer managed keys\. Customer managed keys will [ incur a charge](https://aws.amazon.com/kms/pricing/) for each API call and AWS Key Management Service quotas apply to each customer managed key\.

## Authorizing Use of a Customer Managed Key for Your Online Store<a name="feature-store-authorizing-cmk-online-store"></a>

 If you use a [customer managed key ](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your online store, the policies on that customer managed key must give Feature Store permission to use it on your behalf\. You have full control over the policies and grants on a customer managed key\.

 Feature Store does not need additional authorization to use the default [AWS owned KMS key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#master_keys) to protect your online or offline stores in your AWS account\.

### Customer managed key policy<a name="feature-store-customer-managed-cmk-policy"></a>

 When you select a [customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your Online Store, Feature Store must have permission to use the customer managed key on behalf of the principal who makes the selection\. That principal, a user or role, must have the permissions on the customer managed key that Feature Store requires\. You can provide these permissions in a [key policy](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html), an [IAM policy](https://docs.aws.amazon.com/kms/latest/developerguide/iam-policies.html), or a [grant](https://docs.aws.amazon.com/kms/latest/developerguide/grants.html)\. At a minimum, Feature Store requires the following permissions on a customer managed key: 
+  "kms:Encrypt", "kms:Decrypt", "kms:DescribeKey", "kms:CreateGrant", "kms:RetireGrant", "kms:ReEncryptFrom", "kms:ReEncryptTo", "kms:GenerateDataKey", "kms:ListAliases", "kms:ListGrants", "kms:RevokeGrant" 

 For example, the following example key policy provides only the required permissions\. The policy has the following effects: 
+  Allows Feature Store to use the customer managed key in cryptographic operations and create grants, but only when it is acting on behalf of principals in the account who have permission to use your Feature Store\. If the principals specified in the policy statement don't have permission to use your Feature Store, the call fails, even when it comes from the Feature Store service\. 
+  The [kms:ViaService](https://docs.aws.amazon.com/kms/latest/developerguide/policy-conditions.html#conditions-kms-via-service) condition key allows the permissions only when the request comes from FeatureStore on behalf of the principals listed in the policy statement\. These principals can't call these operations directly\. The value for `kms:ViaService` should be `sagemaker.*.amazonaws.com`\. 
**Note**  
 The `kms:ViaService` condition key can only be used for the online store customer managed AWS KMS key, and cannot be used for the offline store\. If you add this special condition to your customer managed key, and use the same AWS KMS key for both the online and offline store, then it will fail the `CreateFeatureGroup` API operation\. 
+  Gives the customer managed key administrators read\-only access to the customer managed key and permission to revoke grants, including the grants that Feature Store uses to protect your data\. 

 Before using an example key policy, replace the example principals with actual principals from your AWS account\. 

```
{"Id": "key-policy-feature-store",
   "Version":"2012-10-17",
   "Statement": [
     {"Sid" : "Allow access through Amazon SageMaker Feature Store for all principals in the account that are authorized to use  Amazon SageMaker Feature Store",
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
       "Condition": {"StringLike": {"kms:ViaService" : "sagemaker.*.amazonaws.com"
          }
       }
     },
     {"Sid":  "Allow administrators to view the customer managed key and revoke grants",
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
     {"Sid": "Enable IAM User Permissions",
       "Effect": "Allow",
        "Principal": {"AWS": "arn:aws:iam::123456789:root"
        },
        "Action": "kms:*",
        "Resource": "*"
     }
   ]
 }
```

## Using Grants to Authorize Feature Store<a name="feature-store-using-grants-authorize"></a>

 In addition to key policies, Feature Store uses grants to set permissions on the customer managed key\. To view the grants on a customer managed key in your account, use the `[ListGrants](https://docs.aws.amazon.com/kms/latest/APIReference/API_ListGrants.html)` operation\. Feature Store does not need grants, or any additional permissions, to use the [AWS owned customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#aws-owned-cmk) to protect your online store\. 

 Feature Store uses the grant permissions when it performs background system maintenance and continuous data protection tasks\. 

 Each grant is specific to an online store\. If the account includes multiple stores encrypted under the same customer managed key, there will be unique grants per `FeatureGroup` using the same customer managed key\. 

 The key policy can also allow the account to [revoke the grant](https://docs.aws.amazon.com/kms/latest/APIReference/API_RevokeGrant.html) on the customer managed key\. However, if you revoke the grant on an active encrypted online store, Feature Store won't be able to protect and maintain the store\. 

## Monitoring Feature Store interaction with AWS KMS<a name="feature-store-monitoring-kms-interaction"></a>

 If you use a [customer managed key](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html#customer-cmk) to protect your online or offline store, you can use AWS CloudTrail logs to track the requests that Feature Store sends to AWS KMS on your behalf\.

## Accessing Data in Your Online Store<a name="feature-store-accessing-data-online-store"></a>

 The **caller \(either IAM user or IAM role\)** to **ALL DataPlane operations \(Put, Get, DeleteRecord\)** must have below permissions on the customer managed key: 

```
"kms:Decrypt"
```

## Authorizing Use of a Customer Managed Key for your Offline Store<a name="feature-store-authorizing-use-cmk-offline-store"></a>

 The **roleArn** that is passed as a parameter to `createFeatureGroup` must have below permissions to the OfflineStore KmsKeyId: 

```
"kms:GenerateDataKey"
```

**Note**  
The key policy for the online store also works for the offline store, only when the `kms:ViaService` condition is not specified\. 

**Important**  
You can specify a AWS KMS encryption key to encrypt the Amazon S3 location used for your offline feature store when you create a feature group\. If AWS KMS encryption key is not specified, by default we encrypt all data at rest using AWS KMS key\. By defining your [bucket\-level key](https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html) for SSE, you can reduce AWS KMS requests costs by up to 99 percent\. 