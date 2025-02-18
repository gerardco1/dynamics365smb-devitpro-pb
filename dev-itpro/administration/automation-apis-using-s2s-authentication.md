---
title: "Using Service to Service Authentication with Automation APIs"
description: Service-to-service authentication enables external services to connect as an application, without impersonating normal users.
author: henrikwh
ms.custom: na
ms.date: 06/14/2021
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.service: "dynamics365-business-central"
ms.author: jswymer
---

# Using Service-to-Service Authentication with Automation APIs

[!INCLUDE[2020_releasewave2.md](../includes/2020_releasewave2.md)] (Online)

Starting with [!INCLUDE[prod_short](../developer/includes/prod_short.md)] 2020 release wave 2, version 17, service-to-service authentication is enabled for Automation APIs. Service-to-service authentication enables external services to connect as an application, without impersonating normal users.  

## Usage and setup overview

Automation APIs provide capability for automating company setup through APIs. The automation APIs are used to hydrate tenants, that is, to bring them to an initial state. Service-to-service authentication is intended only for the hydration of companies.

The **D365 Automation** entitlements give access to APIs in the `/api/microsoft/automation` route by using the OAuth client credentials flow. An application token with the `Automation.ReadWrite.All` scope is needed for accessing [!INCLUDE[prod_short](../developer/includes/prod_short.md)] Automation APIs.

To enable service-to-service authentication, you'll have to do two things:

- Register an application in your Azure Active Directory tenant for authenticating API calls against [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

- Set up the Azure AD application in [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

These tasks are described in the sections that follow. 

## Task 1: Register an Azure AD application for authentication to Business Central

Complete these steps to register an application in your Azure AD tenant for service-to-service authentication.

1. Sign in to the [Azure portal](https://portal.azure.com).

2. Register an application for [!INCLUDE [prod_short](../developer/includes/prod_short.md)] in Azure Active Directory tenant.

    Follow the general guidelines at [Register your application with your Azure Active Directory tenant](/azure/active-directory/active-directory-app-registration).

    When you add an application to an Azure AD tenant, you must specify the following information:

    |Setting|Description|
    |-------|-----------|
    |Name|Specify a unique name for your application. |
    |Supported account types| Select either <strong>Accounts in this organizational directory only (Microsoft only - Single tenant)</strong> or <strong>Accounts in any organizational directory (Any Azure AD directory - Multitenant)</strong>.|
    |Redirect URI|Set the first box to **Web** to specify a web application. Enter the URL for your Business Central on-premises browser client, followed by *OAuthLanding.htm*, for example: `https://MyServer/BC180/OAuthLanding` or `https://cronus.onmicrosoft.com/BC180/OAuthLanding.htm`. This file is used to manage the exchange of data between Business Central on-premises and other services through Azure AD.<br> <br>**Important:** The URL must match the URL of Web client, as it appears in the browser address. For example, even though the actual URL might be `https://MyServer:443/BC180/OAuthLanding`, the browser typically removes the port number `:443`.|

    When completed, an **Overview** displays in the portal for the new application.

3. Create a client secret for the registered application as follows:

    1. Select **Certificates & secrets** > **New client secret**.
    2. Add a description, select a duration, and select **Add**.

    For the latest guidelines about adding client secrets in Azure AD, see [Add credentials ](/azure/active-directory/develop/quickstart-register-app#add-credentials) in the Azure documentation.

4. Grant the registered application **Automation.ReadWrite.All** permission to the **Dynamics 365 [!INCLUDE [prod_short](../developer/includes/prod_short.md)]** API as follows:

    1. Select **API permissions** > **Add a permission** > **Microsoft APIs**.
    2. Select **Dynamics 365 [!INCLUDE [prod_short](../developer/includes/prod_short.md)]**.
    3. Select **Application permissions**, select **Automation.ReadWrite.All**, then select **Add permissions**.

    When completed, the **API permissions** page will include the following entry:

    |API / Permission name|Type|Description|
    |---------------------|----|-----------|
    |Dynamics 365 Business Central / Automation.ReadWrite.All|Application|Full access to automation|

    For the latest guidelines about adding permissions in Azure AD, see [Add permissions to access your APIs](/azure/active-directory/develop/quickstart-configure-app-access-web-apis#add-permissions-to-access-your-web-api) in the Azure documentation.

## Task 2: Set up the Azure AD application in [!INCLUDE[prod_short](../developer/includes/prod_short.md)]

Complete these steps to set up the Azure AD application for service-to-service authentication in [!INCLUDE[prod_short](../developer/includes/prod_short.md)].

1. In the [!INCLUDE[prod_short](../developer/includes/prod_short.md)] client, search for **Azure Active Directory Applications**  and open the page.

2. Select **New**.

    The **Azure Active Directory Application Card** opens.

3. In the **Client ID** field, enter the **Application (Client) ID**  for the registered application in Azure AD from task 1. 

4. Fill in the **Description** field.

5. Set the **State** to **Enabled**.

6. Assign permissions to objects as needed.

   For more information, [Assign Permissions to Users and Groups](/dynamics365/business-central/ui-define-granular-permissions).

   > [!TIP]
   > The system permission sets and user groups called **D365 AUTOMATION** and **EXTEND. MGT. - ADMIN** provide access to most typical objects used with automation.

   > [!NOTE]  
   > The **EXTEND. MGT. - ADMIN** permission set was introduced in Business Central 2021 release wave 1 as a replacement for the **D365 EXTENSION MGT** permission set in earlier versions.


7. Select **Grant Consent** and follow the wizard to the complete the setup.

   > [!TIP]
   > Pre-consent can be done by adding the AAD application to the **Adminagents** group in the partner tenant.  For more information, see [Pre-consent your app for all your customers](/graph/auth-cloudsolutionprovider#pre-consent-your-app-for-all-your-customers) in the Graph documentation.


## See Also

[Automation company](resources/dynamics_automationcompany.md)  
[Company](resources/dynamics_company.md)  
[Configuration package](resources/dynamics_configurationpackage.md)  
[Extension](resources/dynamics_extension.md)  
[Extension deployment status](resources/dynamics_extensiondeploymentstatus.md)  
[Extension upload](resources/dynamics_extensionupload.md)  
[Permission set](resources/dynamics_permissionset.md)  
[Profile](resources/dynamics_profile.md)  
[Scheduled job](resources/dynamics_scheduledjob.md)  
[User](resources/dynamics_user.md)  
[User group](resources/dynamics_usergroup.md)  
[User group member](resources/dynamics_usergroupmember.md)  
[User group permission](resources/dynamics_usergrouppermission.md)  
[User permission](resources/dynamics_userpermission.md)  
