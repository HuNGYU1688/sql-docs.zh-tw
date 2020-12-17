---
title: 在 SharePoint 中啟用報表伺服器和 Power View 整合功能 | Microsoft Docs
description: 適用於 SharePoint 的 SQL Server Reporting Services 增益集功能通常會自動啟用。 如果需要手動加以啟動，請遵循下列指示。
ms.date: 09/25/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 2f79af39e2ba871a0ee4acf9f33c23f54f2bfd3b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461459"
---
# <a name="activate-the-report-server-and-power-view-integration-features-in-sharepoint"></a>在 SharePoint 中啟用報表伺服器和 Power View 整合功能

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016i](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

  在安裝適用於 SharePoint 產品的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 增益集後，預設會啟動 Reporting Services 網站集合功能。 在某些情況下，您必須手動啟動這些功能。  

> [!NOTE]
> SQL Server 2016 後即不再提供 Reporting Services 與 SharePoint 的整合。

 如果您在安裝 SharePoint 產品之後安裝適用於 SharePoint 2010 產品的 Reporting Services 增益集，只會針對根網站集合啟用報表伺服器整合功能和 Power View 整合功能。 如果是其他網站集合，您必須手動啟用這些功能。 例如，如果您擁有 **https://[我的伺服器名稱]/sites/[網站集合名稱]** 網站集合，您需要手動啟用 Reporting Services 網站集合功能。  
  
 如果沒有根網站集合，則 Reporting Services 增益集會記錄與下面類似的訊息。  
  
 「SharePoint Web 應用程式 80 沒有根網站集合」  
  
 此訊息可在名為 "RS_SP_#.log" 的增益集安裝記錄中找到，其中 # 是遞增的數字。 此記錄檔可在目前使用者的 Temp 資料夾中找到，例如 C:\Users\\[使用者名稱]\AppData\Local\Temp。如需使用增益集之記錄選項的詳細資訊，請參閱 [安裝或解除安裝 SharePoint 的 Reporting Services 增益集](../../reporting-services/install-windows/install-or-uninstall-the-reporting-services-add-in-for-sharepoint.md)。  

## <a name="activate-the-report-server-and-power-view-integration-site-collection-features"></a>啟用報表伺服器和 Power View 整合網站集合功能
  
1.  開啟您想要啟用 Reporting Services 功能的網站瀏覽器。  
  
2.  按一下 **[網站動作]** 。  
  
3.  按一下 **[站台設定]** 。  
  
4.  在網站集合管理群組中，按一下 **[網站集合功能]** 。  
  
5.  在清單中尋找 **[報表伺服器整合功能]** 或 **[Power View 整合功能]** 。  
  
6.  按一下 [啟用]。  
  
 若要停用功能，您可以使用相同的程序，但是要按一下 **[停用]** ，而不是 **[啟用]** 。  
  
## <a name="activate-or-deactivate-reporting-services-central-administration-site-collection-feature"></a>啟用或停用 Reporting Services 管理中心的網站集合功能
  
1.  開啟瀏覽器，並移至 SharePoint 管理中心。  
  
2.  按一下 **[網站動作]** 。  
  
3.  按一下 **[站台設定]** 。  
  
4.  在網站集合管理群組中，按一下 **[網站集合功能]** 。  
  
5.  在清單中尋找 **[報表伺服器管理中心功能]** 。  
  
6.  按一下 [啟用]。  
  
 若要停用此功能，您可以使用相同的程序，但是要按一下 **[停用]** ，而不是 **[啟用]** 。  
  
## <a name="next-steps"></a>後續步驟

當此功能啟動之後，您就可以繼續伺服器整合。

更多問題嗎？ [請嘗試詢問 Reporting Services 論壇](https://go.microsoft.com/fwlink/?LinkId=620231)
