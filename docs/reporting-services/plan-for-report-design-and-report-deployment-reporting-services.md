---
title: 規劃報表設計與報表部署 | Reporting Services | Microsoft Docs
description: 了解如何使用 Reporting Services 以針對一起運作的報表撰寫和報表伺服器環境進行規劃。
ms.date: 09/12/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: 1c1e265e-52a2-4de3-96fd-ca4abae01c02
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: cdaaaa55756860e4b62f90cb7e01c88c63ad4b61
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477059"
---
# <a name="plan-for-report-design-and-report-deployment--reporting-services"></a>規劃報表設計與報表部署 | Reporting Services
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 提供數種撰寫與部署分頁報表的方法。 了解如何針對一起運作的報表撰寫和報表伺服器環境進行規劃。

本主題為 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 元件支援之報表定義的概觀。 報表定義是以報表定義語言 (RDL) 或用戶端報表定義語言 (RDLC) 撰寫的 XML 檔案。 每個報表定義都符合列於檔案開頭的特定結構描述版本。  
  
 RDL 檔案是利用報表產生器於 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 專案的報表設計師中撰寫。 RDLC 檔案則是使用包含在 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)]中的 ReportViewer 控制項撰寫。
  
##  <a name="rdl-schema-versions"></a><a name="bkmk_rdl_schema_versions"></a> RDL 結構描述版本  
 下表列出本主題其餘部分所使用的每個可用結構描述版本與縮寫：  
  
|縮寫|結構描述版本|  
|------------------|--------------------|  
|2016 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2016/01/reportdefinition`|
|2010 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2010/01/reportdefinition`|  
|2008 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2008/01/reportdefinition`|  
|2005 RDL<br /><br /> 2005 RDLC|`https://schemas.microsoft.com/sqlserver/reporting/2005/01/reportdefinition`|  
|2000 RDL|`https://schemas.microsoft.com/sqlserver/reporting/2003/10/reportdefinition`|  
  
 如需有關 RDL 和 RDL 結構描述的詳細資訊，請參閱下列主題：  
  
-   [Microsoft SQL Server XML 結構描述](https://go.microsoft.com/fwlink/?LinkId=31850)  
  
-   [報表定義語言規格](https://go.microsoft.com/fwlink/?linkid=116865)  
  
-   [報表定義語言 &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
 如需 ReportViewer 控制項的詳細資訊，請參閱 [ReportViewer 控制項 (Visual Studio)](https://msdn.microsoft.com/library/ms251671.aspx)。  
  
##  <a name="report-server-and-rdl-schema-support"></a><a name="bkmk_report_server_rdl_schema_support"></a> 報表伺服器與 RDL 結構描述支援  
 報表定義檔案可以透過下列方式部署至 [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] 報表伺服器：  
  
-   **報表設計師：** 從報表設計師將報表部署在 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 中。  
  
-   **報表產生器：** 從報表產生器將報表儲存至報表伺服器。  
  
-   **入口網站：** 從 [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] 將報表上傳至原生模式報表伺服器。  

::: moniker range="=sql-server-2016"
  
-   **SharePoint：** 將報表上傳至以 SharePoint 模式報表伺服器設定的 SharePoint 網站。  

::: moniker-end
  
-   **以程式設計方式：** 使用 SOAP API 介面，以程式設計方式將報表發佈至報表伺服器。 如需詳細資訊，請參閱 [Report Server Web Service](../reporting-services/report-server-web-service/report-server-web-service.md)。  
  
 下表依照報表伺服器的版本列出支援的 rdl 結構描述版本。  
  
|報表伺服器版本|RDL 結構描述版本|  
|---------------------------|------------------------|  
|SQL Server 2016|2016 RDL<br /><br />2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|2010 RDL<br /><br /> 2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|2008 RDL<br /><br /> 2005 RDL<br /><br /> 2000 RDL|  
  
 當您將報表定義上傳至報表伺服器，或升級包含現有報表的報表伺服器時，報表伺服器會以原始格式保留報表定義。 **第一次使用時**，報表伺服器會將報表伺服器資料庫中的報表升級為二進位格式，而這個格式在後續檢視時都會保留著。 報表定義 (.rdl) 本身不會升級。  
  
 您可以從報表伺服器擷取報表定義檔案 (.rdl) 的唯讀複本。 在原生模式報表伺服器上，瀏覽至[!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]，然後選取報表並按一下 [下載]。 

::: moniker range="=sql-server-2016"

在 SharePoint 模式部署中，瀏覽至文件庫，然後選取報表並按一下 **[下載複本]** 。  

::: moniker-end
  
 若要升級報表定義，您必須在報表撰寫環境 (例如 SQL Server Data Tools 或報表產生器) 下開啟報表，然後儲存報表。  
  
 如需所支援之報表升級與結構描述版本的詳細資訊，請參閱 [升級報表](../reporting-services/install-windows/upgrade-reports.md)。  
  
##  <a name="report-authoring-and-deployment-support"></a><a name="bkmk_report_authoring_and_deployment"></a> 報表撰寫與報表部署支援  
 報表撰寫環境是指 [!INCLUDE[ss_dtbi](../includes/ss-dtbi-md.md)] 專案中的報表設計師，以及報表產生器。 報表撰寫環境為報表升級、報表設計、本機模式下的報表預覽、報表伺服器上的報表預覽，以及報表部署提供各種支援。  
  
 下表摘要說明撰寫與部署不同結構描述版本之報表定義的支援：  
  
|撰寫環境|撰寫的 RDL 版本|部署 RDL 版本|部署到報表伺服器版本|  
|---------------------------|--------------------------|------------------------|--------------------------------------|  
|SQL Server 2016 報表產生器|撰寫 2016 RDL<br /><br /> 將舊版 RDL 升級為 2016 RDL|2016 RDL|SQL Server 2016|
|SQL Server 2016 Data Tools 中的報表設計師 - Business Intelligence for Microsoft Visual Studio 2015|撰寫 2016 RDL<br /><br /> 將舊版 RDL 升級為 2016 RDL|2016 RDL|SQL Server 2016|
|SQL Server 2014 Data Tools 中的報表設計師 - Business Intelligence for Microsoft Visual Studio 2012<br /><br /> Or<br /><br /> SQL Server 2012 Data Tools 中的報表設計師 - Business Intelligence for Microsoft Visual Studio 2012<br /><br /> Or<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] Data Tools 中的報表設計師，包含在 [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]中。|撰寫 2010 RDL<br /><br /> 將舊版 RDL 升級為 2010 RDL|2010 RDL|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]<br /><br /> [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]<br /><br /> [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)] Business Intelligence Development Studio 中的報表設計師|撰寫 2010 RDL<br /><br /> 將舊版 RDL 升級為 2010 RDL|2010 RDL|[!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]|  
|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] Business Intelligence Development Studio 中的報表設計師|撰寫 2008 RDL<br /><br /> 將舊版 RDL 升級為 2008 RDL|2008 RDL|[!INCLUDE[ssKatmai](../includes/sskatmai-md.md)]|
  
 如需 SQL Server Data Tools (SSDT) 的詳細資訊，請參閱下列主題：  
  
-   [SQL Server Data Tools 中的部署和版本支援 &#40;SSRS&#41;](../reporting-services/tools/deployment-and-version-support-in-sql-server-data-tools-ssrs.md)  
  
-   [SQL Server Data Tools for Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md)。  
  
##  <a name="reportviewer-controls"></a><a name="bkmk_reportviewer"></a> ReportViewer 控制項  
 [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] ReportViewer 控制項可採用本機預覽模式或遠端模式顯示 .rdlc 報表，此控制項可以顯示 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] 報表伺服器上裝載的 .rdl 檔案。 下表提供 ReportViewer 控制項所支援、用於本機處理 (.rdlc) 的 RDL 版本清單。 伺服器端 RDL 支援的摘要在此節中 [報表伺服器與 RDL 結構描述支援](#bkmk_report_server_rdl_schema_support)。  
  
|產品中的 ReportViewer 控制項|用於本機預覽的 RDL 版本|  
|-------------------------------------|--------------------------------------|  
|[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2015 <br/><br/>Or<br/><br/>[!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2013<br /><br /> Or<br /><br /> [!INCLUDE[vsprvs](../includes/vsprvs-md.md)] 2012<br /><br /> Or<br /><br /> [!INCLUDE[vs_dev10_long](../includes/vs-dev10-long-md.md)]|2008 RDL|  
|[!INCLUDE[vsprvslong](../includes/vsprvslong-md.md)]<br /><br /> Or<br /><br /> [!INCLUDE[vsOrcas](../includes/vsorcas-md.md)]|2005 RDL|  
  
 如需詳細資訊，請參閱下列：  
  
-   [將 RDLC 檔案轉換成 RDL 檔案](https://msdn.microsoft.com/library/ms252109.aspx)  
  
-   [ReportViewer 控制項 (Visual Studio)](https://msdn.microsoft.com/library/ms251671.aspx)  
  
-   [加入和設定 ReportViewer 控制項](https://msdn.microsoft.com/library/ms252104.aspx)  
  
## <a name="see-also"></a>另請參閱  
 [報表、報表組件和報表定義 &#40;報表產生器及 SSRS&#41;](../reporting-services/report-design/reports-report-parts-and-report-definitions-report-builder-and-ssrs.md)   
 [Reporting Services 工具](../reporting-services/tools/reporting-services-tools.md)   
 [報表定義語言 &#40;SSRS&#41;](../reporting-services/reports/report-definition-language-ssrs.md)  
  
  
