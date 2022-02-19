---
title: "Get Started With Microsoft Graph SDK for Go"
date: 2022-02-11T15:58:29-08:00
draft: true
---


I want to get associate name list from `Report.xlsx` in my corporate OneDrive account by using Go program.

### How to sync files in working OneDrive account to my personal account?
For corporate account, I can't register my application because I don't have access to Azure Active Directory.
But I can register application in my personal OneDrive account. 

* Login to my work OneDrive account
* Select the folder `Work` in my working account and click `Share`
* Select `Anyone with the link can edit` and share it to my personal OneDrive account
* Login to my personal OneDrive account, open the email and accept the shared folder
* Click `Add to my OneDrive` will add the shared folder to my computer



### Use MSAL instead of ADAL
Starting June 30th, 2020 we will no longer add any new features to Azure Active Directory Authentication Library (ADAL) 
and Azure AD Graph. We will continue to provide technical support and security updates but we will no longer provide 
feature updates. Applications will need to be upgraded to Microsoft Authentication Library (MSAL) and Microsoft Graph.

## Useful Links
* [Sync files between work account and personal account](https://www.youtube.com/watch?v=YSdLYRcge0I)
* [Microsoft Graph SDK for Go](https://github.com/microsoftgraph/msgraph-beta-sdk-go)
* [Microsoft Graph Explorer](https://developer.microsoft.com/en-us/graph/graph-explorer)
* [App Registration](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps)
* See: https://www.youtube.com/watch?v=SInnmOEKcg0