# RStudio administrative dashboard<a name="rstudio-admin"></a>

 This topic shows how to access and use the RStudio administrative dashboard\. With the RStudio administrative dashboard, admins can manage users and RSessions, as well as view information about RStudio Server instance utilization and Amazon CloudWatch Logs\.

 

## Launch the RStudio administrative dashboard<a name="rstudio-admin-launch"></a>

The `R_STUDIO_ADMIN` authorization allows the user to access the RStudio administrative dashboard\. An `R_STUDIO_ADMIN` user can access the RStudio administrative dashboard by replacing `workspaces` with `admin` in their RStudio URL manually\. The following shows how to modify the URL to access the RStudio administrative dashboard\.

For example, the following RStudio URL: 

```
https://<DOMAIN-ID>.studio.us-east-2.sagemaker.aws/rstudio/default/s/<SESSION-ID>/workspaces
```

Can be converted to: 

```
https://<DOMAIN-ID>.studio.us-east-2.sagemaker.aws/rstudio/default/s/<SESSION-ID>/admin
```

## Dashboard tab<a name="rstudio-admin-dashboard"></a>

This tab gives an overview of your RStudio Server instance utilization, as well as information on the number of active RSessions\.

## Sessions tab<a name="rstudio-admin-sessions"></a>

This tab gives information on the active RSessions, such as the user that launched the RSessions, the time that the RSessions have been running, and their resource utilization\.

## Users tab<a name="rstudio-admin-users"></a>

This tab gives information on the RStudio authorized users in the Domain, such as the time that the last RSession was launched and their resource utilization\. The following procedure shows how to get information about the user's historical resource utilization\.

1. From the list of users, select the user that you want to view information for\. This opens a new page that is specific to the user\.

1. To view the user's historical resource utilization, select the **Stats** tab\. This tab gives information about the historical CPU and memory usage, as well as the number of active RSessions\.

1. To view Amazon CloudWatch Logs specific to the user, select the **Logs** tab\.

## Stats tab<a name="rstudio-admin-stats"></a>

This tab gives information on the historical utilization of your RStudio Server instance\.

## Logs tab<a name="rstudio-admin-logs"></a>

This tab displays Amazon CloudWatch Logs for the RStudio Server instance\. For more information about logging events with Amazon CloudWatch Logs, see [What is Amazon CloudWatch Logs?](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)\.