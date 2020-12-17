---
title: 在 Reporting Services 資料來源中儲存認證 | Microsoft Docs
description: 了解如何為原生模式與 SharePoint 模式報表伺服器設定預存認證。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 05/24/2018
ms.openlocfilehash: 18d9a5ad8c4df17525bad04f0b056e7f9c4f05ae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478849"
---
# <a name="store-credentials-in-a-reporting-services-data-source"></a>Store Credentials in a Reporting Services Data Source

::: moniker range="=sql-server-2016"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-2016](../../includes/ssrs-appliesto-sharepoint-2013-2016.md)]

::: moniker-end

::: moniker range=">=sql-server-2017"

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]

::: moniker-end

您可以設定預存認證，讓 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 報表伺服器用來存取報表的外部資料。 如果報表會自動執行，便是使用預存認證 (例如以電子郵件形式發行報表的 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 訂閱)。 排定或觸發報表處理時，報表伺服器會擷取和使用認證。 本主題會逐步引導您完成為原生模式和 SharePoint 模式報表伺服器設定預存認證的程序。  
  
##  <a name="security-policy-requirements-for-stored-credentials"></a><a name="bkmk_top"></a> 預存認證的安全性原則需求  
 ![as_powerpivot_refresh_sss_set_key](/analysis-services/analysis-services/power-pivot-sharepoint/media/as-powerpivot-refresh-sss-set-key.gif "as_powerpivot_refresh_sss_set_key") 您必須在報表伺服器上，為預存認證所使用的帳戶設定下列其中一項安全性原則。 建議您為您的環境選取具備需要的最低層級權限的原則。  
  
1.  **允許本機登入**。 如需詳細資訊，請參閱 [允許本機登入](https://technet.microsoft.com/library/cc756809\(v=WS.10\).aspx)。  
  
2.  **以批次工作登入**。 如需詳細資訊，請參閱 [以批次工作登入](https://technet.microsoft.com/library/cc755659\(v=ws.10\).aspx)。  
  
3.  如需原則的一般資訊，請參閱 [編輯群組原則物件的安全性設定](https://technet.microsoft.com/library/cc736516\(v=ws.10\).aspx)。  
  
##  <a name="configure-stored-credentials-for-a-report-specific-data-source-native-mode"></a><a name="bkmk_stored_credentials_data_source_native"></a> 為報表特定的資料來源設定預存認證 (原生模式)  
  
1.  在入口網站中，瀏覽至包含報表的資料夾。 按一下報表磚右上角的省略符號 (...)。  
  
2.  按一下 [管理]  ，然後按一下 [資料來源] 。  
  
3.  選取 **[自訂資料來源]** 。  
  
4.  在 [資料來源類型]  清單中，選取用來處理資料來源中之資料的資料處理延伸模組。  
  
5.  針對 **[連接字串]** ，請指定報表伺服器用於連接到資料來源的連接字串。 下列範例說明用來連線到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫的連接字串：  
  
    ```  
    data source=<servername>;initial catalog=AdventureWorks2012  
    ```  
  
6.  針對 [連接方式] ，選取 [安全地儲存在報表伺服器中的認證] 。  
  
7.  輸入使用者名稱和密碼。  
  
    -   若帳戶是 Windows 網域使用者帳戶，請使用下列格式來指定該帳戶：\<domain>\\<帳戶\>，然後選取 [連線到資料來源時作為 Windows 認證]。  
  
    -   如果使用者名稱和密碼是資料庫認證，請勿選取 **[連接到資料來源時做為 Windows 認證]** 。 如果資料庫伺服器支援模擬或委派，您就可以選取 **[連接到資料來源後，模擬已驗證的使用者]**。  
  
8.  按一下 [套用]。  
  
     ![搭配 [回到頁首] 連結使用的箭頭圖示](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "與 [回到頁首] 連結搭配使用的箭頭圖示") [預存認證的安全性原則需求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-report-specific-data-source-sharepoint-mode"></a><a name="bkmk_stored_credentials_data_source_sharepoint"></a> 為報表特定的資料來源設定預存認證 (SharePoint 模式)  
  
1.  瀏覽至包含報表的文件庫，然後按一下開啟的功能表 ![適用於 SSRS 項目的文件庫操作功能表](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "適用於 SSRS 項目的文件庫操作功能表")。  
  
2.  按一下第二個開啟的功能表 ![適用於 SSRS 項目的文件庫操作功能表](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "適用於 SSRS 項目的文件庫操作功能表")，然後按一下 [管理資料來源]。  
  
3.  按一下您要設定預存認證的 [自訂]  資料來源的名稱。  
  
4.  在 [資料來源類型]  清單中，選取用來處理資料來源中之資料的資料處理延伸模組。  
  
5.  針對 **[連接字串]** ，請指定報表伺服器用於連接到資料來源的連接字串。 下列範例說明用來連線到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫的連接字串：  
  
    ```  
    data source=<servername>;initial catalog=AdventureWorks2012  
    ```  
  
6.  針對 [認證] 選取 [預存認證] 。  
  
7.  輸入 [使用者名稱]  和 [密碼] 。  
  
    -   若帳戶是 Windows 網域使用者帳戶，請使用下列格式來指定該帳戶：\<domain>\\<帳戶\>，然後選取 [連線到資料來源時作為 Windows 認證]。  
  
    -   如果使用者名稱和密碼是資料庫認證，請勿選取 [當做 Windows 認證使用] 。 如果資料庫伺服器支援模擬或委派，您可以選取 **[設定執行內容到這個帳戶]** 。  
  
8.  按一下 [確定]。  
  
     ![搭配 [回到頁首] 連結使用的箭頭圖示](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "與 [回到頁首] 連結搭配使用的箭頭圖示") [預存認證的安全性原則需求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-shared-data-source-native-mode"></a><a name="bkmk_stored_credentials_shared_data_source_native"></a> 為共用資料來源設定預存認證 (原生模式)  
  
1.  在入口網站中，瀏覽至共用資料來源項目。 
  
2.  按一下報表磚右上角的省略符號 (...) > [管理]。 
  
3.  在 [類型] 清單中，指定用來處理資料來源中之資料的資料處理延伸模組。  
  
4.  針對 **[連接字串]** ，請指定報表伺服器用於連接到資料來源的連接字串。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建議您不要在連接字串中指定認證。  
  
     下列範例說明用來連線到本機 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫的連接字串：  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
5.  輸入使用者名稱和密碼。  
  
    -   若帳戶是 Windows 網域使用者帳戶，請使用下列格式來指定該帳戶：\<domain>\\<帳戶\>，然後選取 [連線到資料來源時作為 Windows 認證]。  
  
    -   如果使用者名稱和密碼是資料庫認證，請勿選取 **[連接到資料來源時做為 Windows 認證]** 。 如果資料庫伺服器支援模擬或委派，您就可以選取 **[連接到資料來源後，模擬已驗證的使用者]**。  
  
6.  按一下 [套用]。  
  
     ![搭配 [回到頁首] 連結使用的箭頭圖示](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "與 [回到頁首] 連結搭配使用的箭頭圖示") [預存認證的安全性原則需求](#bkmk_top)  
  
##  <a name="configure-stored-credentials-for-a-shared-data-source-sharepoint-mode"></a><a name="bkmk_stored_credentials_shared_data_source_sharepoint"></a> 為共用資料來源設定預存認證 (SharePoint 模式)  
  
1.  在文件庫中，瀏覽至共用資料來源項目。![共用資料來源圖示](../../reporting-services/report-data/media/hlp-16datasource.png "共用資料來源圖示")  
  
2.  按一下操作功能表 ![適用於 SSRS 項目的文件庫操作功能表](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "適用於 SSRS 項目的文件庫操作功能表")，然後按一下第二個操作功能表 ![適用於 SSRS 項目的文件庫操作功能表](../../reporting-services/report-data/media/ssrs-sharepoint-item-context-menu.png "適用於 SSRS 項目的文件庫操作功能表")。  
  
3.  按一下 [編輯資料來源定義] 。  
  
4.  在 [資料來源類型]  清單中，指定用來處理資料來源中之資料的資料處理延伸模組。  
  
5.  針對 **[連接字串]** ，請指定報表伺服器用於連接到資料來源的連接字串。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建議您不要在連接字串中指定認證。  
  
     下列範例說明用來連線到本機 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫的連接字串：  
  
    ```  
    data source=<localservername>; initial catalog=AdventureWorks2012  
    ```  
  
6.  輸入使用者名稱和密碼。  
  
    -   若帳戶是 Windows 網域使用者帳戶，請使用下列格式來指定該帳戶：\<domain>\\<帳戶\>，然後選取 [當作 Windows 認證使用]。  
  
    -   如果使用者名稱和密碼是資料庫認證，請勿選取 [當做 Windows 認證使用] 。 如果資料庫伺服器支援模擬或委派，您可以選取 **Set Execution context to this account**。  
  
7.  按一下 [確定] 。  
  
     ![搭配 [回到頁首] 連結使用的箭頭圖示](/analysis-services/analysis-services/instances/media/uparrow16x16.gif "與 [回到頁首] 連結搭配使用的箭頭圖示") [預存認證的安全性原則需求](#bkmk_top)  
  
## <a name="see-also"></a>另請參閱  
 [指定報表資料來源的認證及連線資訊](../../reporting-services/report-data/specify-credential-and-connection-information-for-report-data-sources.md)   
