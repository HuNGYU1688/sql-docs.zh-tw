---
title: 將權限授與使用者及警示管理員 | Microsoft Docs
description: 了解如何在 SQL Server Reporting Services (SSRS) 中將權限授與使用者及警示管理員。
ms.date: 08/17/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 166808e1-ada7-48d2-bda8-8f7c017fb3aa
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 87c453d6b896a998d6a3229d57db905cf57cc0fb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472529"
---
# <a name="grant-permissions-to-users-and-alerting-administrators"></a>將權限授與使用者及警示管理員

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

使用者和警示系統管理員必須經過 SharePoint 權限的授與，才能建立、編輯、刪除及檢視資料警示。 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 資料警示功能沒有特殊權限可供使用，您需使用內建的 SharePoint 權限。

> [!NOTE]
> SQL Server 2016 後即不再提供 Reporting Services 與 SharePoint 的整合。

**資訊工作者** - 權限必須包含「建立警示」和「檢視項目」的 SharePoint 權限。 名稱為「設計」、「參與」、「讀取」及「僅檢視」的內建 SharePoint 權限等級包含「建立警示」和「檢視項目」的 SharePoint 權限。 您也可以使用支援使用者建立、編輯、執行及檢視資料警示所需的權限，建立自訂權限等級。

**警示系統管理員** - 權限必須包含「管理警示」的 SharePoint 權限。 根據預設，只有「完整控制權」權限層級包含使用「小組網站」網站範本建立的這項網站權限。 如果您使用其他網站範本，則會看見不同的預設 SharePoint 群組清單。 您可以使用支援警示系統管理員檢視及刪除資料警示所需的權限，將「管理警示」權限加入其中一個內建權限等級或建立自訂權限等級。

若要深入了解 SharePoint 權限，請參閱 [使用者權限與權限等級 (SharePoint Server 2010)](/SharePoint/sites/user-permissions-and-permission-levels)。

## <a name="grant-permissions"></a>授與權限
  
1.  移至您要授與權限的 SharePoint 網站。  
  
2.  在工具列上，按一下 **[網站動作]** ，然後按一下 **[網站權限]** 。  
  
     如果沒有看到此選項，表示您沒有足夠的權限以將權限授與給其他人。  
  
3.  按一下 [授與權限]。  
  
4.  在 [使用者/群組] 中，輸入要授與權限的使用者名稱、群組名稱或電子郵件地址。  
  
5.  選取 **[加入使用者至 SharePoint 群組]** 或 **[直接授與使用者權限]** 選項。 根據您選取的是 **[新增使用者至 SharePoint 群組]** 或是 **[直接授與使用者權限]** 而定，執行下列其中一項操作：  
  
    -   如果您選取了 [新增使用者至 SharePoint 群組]，請選取下拉式清單中的權限等級。  
  
    -   如果您選取了 **[直接授與使用者權限]** ，請選取權限等級。  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  

## <a name="see-also"></a>另請參閱

[設定 SharePoint 網站上報表伺服器項目的權限 &#40;SharePoint 整合模式的 Reporting Services&#41;](../reporting-services/security/set-permissions-for-report-server-items-on-a-sharepoint-site.md)   
[Reporting Services Data Alerts](../reporting-services/reporting-services-data-alerts.md)  

更多問題嗎？ [請嘗試詢問 Reporting Services 論壇](https://go.microsoft.com/fwlink/?LinkId=620231)