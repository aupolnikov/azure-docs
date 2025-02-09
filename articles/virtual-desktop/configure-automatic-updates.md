---
title: Configure Microsoft Endpoint Configuration Manager - Azure
description: How to configure Microsoft Endpoint Configuration Manager to deploy software updates to Windows 10 Enterprise multi-session on Azure Virtual Desktop.
author: Heidilohr
ms.topic: how-to
ms.date: 06/12/2020
ms.author: helohr
ms.reviewer: v-cawood; clemr
manager: femila
---
# Configure Microsoft Endpoint Configuration Manager

This article explains how to configure Microsoft Endpoint Configuration Manager to automatically apply updates to a Azure Virtual Desktop host running Windows 10 Enterprise multi-session.

## Prerequisites

To configure this setting, you'll need the following things:

   - Make sure you've installed the Microsoft Endpoint Configuration Manager Agent on your virtual machines.
   - Make sure your version of Microsoft Endpoint Configuration Manager is at least on branch level 1906. For best results, use branch level 1910 or higher.

## Receiving updates for Windows 10 and 11 Enterprise multi-session

You can update Windows 10 Enterprise multi-session with the corresponding Windows 10 client updates. For example, you can update Windows 10 Enterprise multi-session, version 21H2 by installing the Windows 10, version 21H2 client updates.

> [!NOTE]
> Currently, you can't update Windows 10 Enterprise multi-session version 21H2 and Windows 11 Enterprise multi-session with their corresponding Windows client updates.

## Create a query-based collection

To create a collection of Windows 10 Enterprise multi-session virtual machines, a query-based collection can be used to identify the specific operating system SKU.

To create a collection:

1. Select **Assets and Compliance**.
2. Go to **Overview** > **Device Collections** and right-click **Device collections** and select **Create Device Collection** from the drop-down menu.
3. In the **General** tab of the menu that opens, enter a name that describes your collection in the **Name** field. In the **Comment** field, you can give additional information describing what the collection is. In **Limiting Collection**, define which machines you're including in the collection query.
4. In the **Membership Rules** tab, add a rule for your query by selecting **Add Rule**, then selecting **Query Rule**.
5. In **Query Rule Properties**, enter a name for your rule, then define the parameters of the rule by selecting **Edit Query Statement**.
6. Select **Show Query Statement**.
7. In the statement, enter the following string:

    ```syntax
    select
    SMS_R_SYSTEM.ResourceID,SMS_R_SYSTEM.ResourceType,SMS_R_SYSTEM.Name,SMS_R_SYSTEM.SMSUniqueIdentifier,SMS_R_SYSTEM.ResourceDomainORWorkgroup,SMS_R_SYSTEM.Client
    from SMS_R_System inner join SMS_G_System_OPERATING_SYSTEM on
    SMS_G_System_OPERATING_SYSTEM.ResourceId = SMS_R_System.ResourceId where
    SMS_G_System_OPERATING_SYSTEM.OperatingSystemSKU = 175
    ```

8. Select **OK** to create the collection.
9. To check if you successfully created the collection, go to **Assets and Compliance** > **Overview** > **Device Collections**.
