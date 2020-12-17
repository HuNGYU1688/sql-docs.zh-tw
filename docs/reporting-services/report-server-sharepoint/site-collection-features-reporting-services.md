---
title: Reporting Services 網站集合功能 | Microsoft Docs
description: 了解 SQL Server Reporting Services SharePoint 模式提供的 SharePoint 網站集合功能。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 75c1c49f70eb44fbffab50ef66a3785d97a3f9ec
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461449"
---
# <a name="reporting-services-site-collection-features"></a>Reporting Services 網站集合功能

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Reporting Services SharePoint 模式提供了三個 SharePoint 網站集合功能。 這些功能支援一般 Reporting Services SharePoint 模式報表環境、[!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)]、SQL Server 2016 Reporting Services 增益集功能，以及 SharePoint 管理中心的 Reporting Services 管理作業。

> [!NOTE]
> SQL Server 2016 後即不再提供 Reporting Services 與 SharePoint 的整合。
  
## <a name="site-collection-features"></a>網站集合功能

 下表描述 Reporting Services 網站集合功能。  
  
|功能|說明|  
|-------------|-----------------|  
|**報表伺服器管理中心功能**|啟用管理與 Reporting Services 報表伺服器整合的功能。 此功能只在 SharePoint 管理中心網站集合中安裝及使用。<br /><br /> 在安裝適用於 SharePoint 產品的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 增益集後，SharePoint 管理中心網站集合中會自動啟用報表伺服器整合功能。 在某些情況下，您必須手動啟動此功能。 若要啟用報表伺服器功能，請使用 SharePoint 管理中心內 [網站設定] 頁面中的 Reporting Services 頁面。<br /><br /> 適用於 SharePoint 產品的 [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] Reporting Services 版及以後版本的增益集，會在安裝增益集時，針對所有現有的網站集合啟用報表伺服器整合功能。 此外，新的網站集合會自動啟用這項功能。|  
|**報表伺服器整合功能**|使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Reporting Services 可擁有豐富的報表功能<br /><br /> 此功能預設為使用中。|  
|**Power View 整合功能**|針對 [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] 活頁簿與 Analysis Services 表格式資料庫啟用互動式資料瀏覽及視覺化簡報。<br /><br /> 此功能可透過下列資料來源的操作功能表存取：<br /><br /> **.rdlx**<br /><br /> **.rsds**<br /><br /> **.bism** 連線檔案<br /><br /> <br /><br /> 如果 [!INCLUDE[ssCrescent](../../includes/sscrescent-md.md)] 未出現在操作功能表中，請確認已啟用 [Power View 整合功能]。<br /><br /> 此功能預設為停用。|  

## <a name="next-steps"></a>後續步驟

[在 SharePoint 中啟用報表伺服器和 Power View 整合功能](../../reporting-services/report-server-sharepoint/site-collection-features-report-server-and-power-view.md)   
[Reporting Services 網站設定和網站功能 &#40;SharePoint 模式&#41;](../../reporting-services/report-server-sharepoint/site-settings-and-features-reporting-services.md)   
[在 SharePoint 管理中心啟動報表伺服器檔案同步處理功能](../../reporting-services/report-server-sharepoint/activate-the-report-server-file-sync-feature-in-sharepoint-ca.md)  

更多問題嗎？ [請嘗試詢問 Reporting Services 論壇](https://go.microsoft.com/fwlink/?LinkId=620231)
