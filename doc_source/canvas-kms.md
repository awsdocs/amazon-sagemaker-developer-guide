# Give users the ability to import encrypted datasets<a name="canvas-kms"></a>

Your users might have datasets that have been encrypted with a AWS KMS key\. To give your users permissions to import encrypted datasets, add the following permissions to the IAM execution role that you've used for the user profile\.

```
     "kms:Encrypt",
     "kms:Decrypt",
     "kms:ReEncrypt*",
     "kms:GenerateDataKey*",
     "kms:DescribeKey"
```

For more information about AWS KMS keys, see [Key policies in AWS Key Management Service](https://docs.aws.amazon.com/kms/latest/developerguide/key-policies.html)\.