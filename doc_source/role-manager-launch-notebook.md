# Launch Studio<a name="role-manager-launch-notebook"></a>

Use your persona\-focused roles to launch Studio\. If you are an administrator, you can give your users access to Studio and have them assume their persona role either directly through the AWS Management Console or through the AWS IAM Identity Center \(successor to AWS Single Sign\-On\)\.

## Launch Studio with AWS Management Console<a name="role-manager-launch-notebook-console"></a>

For data scientists or other users to assume their given persona through the AWS Management Console, they require a console role to get to the Studio environment\. 

You cannot use Amazon SageMaker Role Manager to create a role that grants permissions to the AWS Management Console\. However, after creating a service role in the role manager, you can go to the IAM console to edit the role and add a user access role\. The following is an example of a role that provides user access to the AWS Management Console:

```
{
    "Version": "2012-10-17",
    "Statement":
    [
        {
            "Sid": "DescribeCurrentDomain",
            "Effect": "Allow",
            "Action": "sagemaker:DescribeDomain",
            "Resource": "arn:aws:sagemaker:<REGION>:<ACCOUNT-ID>:domain/<STUDIO-DOMAIN-ID>"
        },
        {
            "Sid": "RemoveErrorMessagesFromConsole",
            "Effect": "Allow",
            "Action":
            [
                "servicecatalog:ListAcceptedPortfolioShares",
                "sagemaker:GetSagemakerServicecatalogPortfolioStatus",
                "sagemaker:ListModels",
                "sagemaker:ListTrainingJobs",
                "servicecatalog:ListPrincipalsForPortfolio",
                "sagemaker:ListNotebookInstances",
                "sagemaker:ListEndpoints"
            ],
            "Resource": "*"
        },
        {
            "Sid": "RequiredForAccess",
            "Effect": "Allow",
            "Action":
            [
                "sagemaker:ListDomains",
                "sagemaker:ListUserProfiles"
            ],
            "Resource": "*"
        },
        {
            "Sid": "CreatePresignedURLForAccessToDomain",
            "Effect": "Allow",
            "Action": "sagemaker:CreatePresignedDomainUrl",
            "Resource": "arn:aws:sagemaker:<REGION>:<ACCOUNT-ID>:user-profile/<STUDIO-DOMAIN-ID>/<PERSONA_NAME>"
        }
    ]
}
```

In the Studio control panel, choose **Add User** to create a new user\. In the **General Settings** section, give your user a name and set the **Default execution role** for the user to be the role that you created using Amazon SageMaker Role Manager\.

On the next screen, choose the appropriate Jupyter Lab version, and whether to turn on SageMaker Jumpstart and SageMaker Project templates\. Then choose **Next**\. On the SageMaker Canvas settings page, choose whether to turn on SageMaker Canvas support, and additionally whether to allow for timeseries forecasting in SageMaker Canvas\. Then choose **Submit**\.

Your new user should now be visible in the Studio control panel\. To test this user, choose **Studio** from the **Launch app** dropdown list in the same row as the user’s name\. 

## Launch Studio with IAM Identity Center<a name="role-manager-launch-notebook-iam-identity-center"></a>

To assign IAM Identity Center users to execution roles, the user must first exist in the IAM Identity Center directory\. For more information, see [Manage identities in IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/manage-your-identity-source-sso.html) in the *AWS IAM Identity Center \(successor to AWS Single Sign\-On\)*\. 

**Note**  
Your IAM Identity Center Authentication directory and Studio Domain must be in the same AWS Region\.

1. To assign IAM Identity Center users to your Studio Domain, choose **Assign users and Groups** in the Studio control panel\. On the **Assign users and groups** screen select your data scientist user, and then choose **Assign Users and Groups**\.

1. After the user is added to the Studio control panel, choose the user to open the user details screen\.

1. On the **User details** screen, choose **Edit**\.

1. On the **Edit user profile** screen, under **General settings**, modify the **Default execution role** to match the user execution role you’ve created for your data scientists\. 

1. Choose **Next** through the rest of the settings pages, and choose **Submit** to save your changes\.

When your data scientist or other user logs into the IAM Identity Center portal, they see a tile for this Studio Domain\. Choosing that tile logs them into Studio with their assigned user execution role\.