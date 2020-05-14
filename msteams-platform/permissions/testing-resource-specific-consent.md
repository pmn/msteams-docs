---
title: Testing resource specific consent in Teams
description: Details testing resource specific consent in Teams using Postman
localization_priority:  Normal
author: laujan
ms.author: lajanuar
ms.topic: How-to
keywords: teams authorization OAuth SSO AAD rsc Postman Graph
---

# Test resource specific consent permissions  in Teams

Resource specific consent (RSC) is a Microsoft Teams and Graph API integration that enables you to use API endpoints to manage specific team life cycles within your organization. Please *see*  [Resource specific consent (RSC) — Microsoft Teams Graph API](./resource-specific-consent.md). 

> [!NOTE]
>To test the RSC permissions, your Teams app manifest file must include the **webApplicationInfo** key populated with the `id` (your [Azure AD app id](././resource-specific-consent.md#register-your-app-using-the-azure-portal)), `resource`(any string), and the `application permissions` you want your app to have.  *See* [Update your Teams app manifest](./resource-specific-consent.md#update-your-teams-app-manifest).

```json
"webApplicationInfo": {

        "id": "XXxxXXXXX-XxXX-xXXX-XXxx-XXXXXXXxxxXX", 

"resource": "https://AnyString",

        "applicationPermissions": [

    "TeamSettings.Read.Group",

   "ChannelMessage.Read.Group",

  "TeamSettings.Edit.Group",

  "ChannelSettings.Edit.Group",

  "Channel.Create.Group",

  "Channel.Delete.Group",

  "TeamsApp.Read.Group",

  "TeamsTab.Read.Group",

  "TeamsTab.Create.Group",

  "TeamsTab.Edit.Group",

  "TeamsTab.Delete.Group",

  "Member.Read.Group",

  "Member.ReadWrite.Group",

  "File.Read.Group",

  "File.Create.Group",

  "File.Edit.Group",

  "File.Delete.Group"

        ]

    }
```

>[!IMPORTANT]
In your app manifest only include the RSC permissions that you want your app to have.

## Test added RSC permissions using Postman

To check whether the RSC permissions are being honored by the API request payload, you'll need to download the [Test RSC Postman Collection JSON](test-rsc-postman-collection.json) and update the following values:

1. `azureADAppId`  — your app's Azure AD app id.
1. `azureADAppSecret`  — your Azure AD app secret (password)
1. `teamGroupId` — you can get the team group id from the Teams client:

> [!div class="checklist"]
>
> * In the Teams client, select **Teams** from the far left nav bar .
> * Select the team where the app is installed from the dropdown menu.
> * Select the **More options** icon (&#8943;)
> * Select **Get link to team** 
> * Copy and save the **groupId** value from the string.

Execute the entire collection. The permissions that you specified in your app manifest should succeed while those not specified should fail with an HTTP 403 status code. Check all of the response status codes to confirm that the behavior of the RSC permissions in your app meet expectations. 

>[!NOTE]
>To test specific DELETE and READ API calls, please add those instance scenarios to the Postman collection JSON file.

## Test  revoked RSC permissions using Postman

> [!div class="checklist"]
>
> * Uninstall the app from the specific team.
> * Follow the steps above for [Test added RSC permissions using Postman](#test-added-rsc-permissions-using-postman).
> * Check all of the response status codes to confirm that the specific API calls that succeeded have failed with an HTTP 403 status code.

> [!div class="nextstepaction"]
>
> [Learn more about the Graph API and Teams](/graph/api/resources/teams-api-overview?view=graph-rest-1.0)