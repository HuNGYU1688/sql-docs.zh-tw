---
title: Reporting Services 和 Power View 的瀏覽器支援 | Microsoft Docs
description: 了解支援管理和檢視 SQL Server Reporting Services、ReportViewer 控制項和 Power View 的瀏覽器版本。
ms.date: 07/02/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- displaying reports
- scripts [Reporting Services], requirements
- viewing reports
- browsers [Reporting Services]
- Web browsers [Reporting Services], about browser support
- browsing reports [Reporting Services]
- components [Reporting Services], browsers
- Web browsers [Reporting Services]
ms.assetid: 48a75bbb-0029-4c43-891d-dc8f4fc0ebe1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: aa8a9fbbcadfcdd3c266de4b1453ea7e5402e1af
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466649"
---
# <a name="browser-support-for-reporting-services-and-power-view"></a>Reporting Services 和 Power View 的瀏覽器支援

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

了解支援管理和檢視 SQL Server Reporting Services、ReportViewer 控制項和 Power View 的瀏覽器版本。

> [!NOTE]
> SQL Server 2016 後即不再提供 Reporting Services 與 SharePoint 的整合。

## <a name="browser-requirements-for-the-web-portal"></a>入口網站的瀏覽器需求

以下是入口網站目前支援的瀏覽器清單。

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*
- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple iOS**  
*iPhone 和 iPad (含 iOS 9)*

- Apple Safari (+)

**Google Android**  
*電話和平板電腦 (含 Android 4.4 (KitKat) 或更新版本)*

- Google Chrome (+)

 **(+)** 最新的公開發行版本

## <a name="browser-requirements-for-the-reportviewer-web-control-2015"></a>ReportViewer Web 控制項 (2015) 的瀏覽器需求

 以下是 ReportViewer Web 控制項 (2015) 目前支援的瀏覽器清單。 報表檢視器支援從 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] Web 入口網站和 SharePoint 文件庫檢視報表。  

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** 最新的公開發行版本

::: moniker range="=sql-server-2016"

 如果您使用整合於 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]中的 SharePoint 產品，請參閱  [Plan browser support in SharePoint 2016](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx)(在 SharePoint 2016 中規劃瀏覽器支援)。

::: moniker-end

### <a name="authentication-requirements"></a>驗證需求

 瀏覽器可支援必須由報表伺服器處理的特定驗證配置，用戶端要求才會成功。 下表將識別 Windows 作業系統上執行之每個瀏覽器所支援的預設驗證類型。

|**瀏覽器類型**|**支援**|**瀏覽器預設值**|**伺服器預設值**|
|----------------------|------------------|-------------------------|------------------------|
|**Microsoft Edge** (+)|交涉, Kerberos, NTLM, 基本|交涉|是。 預設驗證設定適用於 Edge。|
|**Microsoft Internet Explorer**|交涉, Kerberos, NTLM, 基本|交涉|是。 搭配 Internet Explorer 使用預設驗證設定。|
|**Google Chrome**(+)|交涉, NTLM, 基本|交涉|是。 預設驗證設定適用於 Chrome。|
|**Mozilla Firefox**(+)|NTLM，基本|NTLM|是。 搭配 Firefox 使用預設驗證設定。|
|**Apple Safari**(+)|NTLM，基本|基本|是。 搭配 Safari 使用預設驗證設定。|

 **(+)** 最新的公開發行版本

### <a name="script-requirements-for-viewing-reports"></a>檢視報表的指令碼需求

 若要使用報表檢視器，請設定瀏覽器來執行指令碼。

 如果指令碼功能未啟用，當您開啟報表時，會看到類似下列的錯誤訊息：

- **您的瀏覽器不支援指令碼，或已設定為不允許執行指令碼。按一下此處，檢視不含指令碼的報表**。

 如果您選擇檢視不含指令碼支援的報表，報表會以 HTML 轉譯，且不含報表檢視器功能 (例如，報表工具列和文件引導模式)。

> [!NOTE]
> 報表工具列屬於 HTML 檢視器元件的一部分。 根據預設，此工具列會出現在瀏覽器視窗中轉譯的每個報表上方。 報表檢視器會提供一些功能，包括搜尋報表中的資訊、捲動至特定頁面以及調整頁面大小以供檢視等功能。 如需有關報表工具列或 HTML 檢視器的詳細資訊，請參閱＜ [HTML Viewer and the Report Toolbar](../reporting-services/html-viewer-and-the-report-toolbar.md)＞。

## <a name="browser-support-for-reportviewer-web-server-controls-in-visual-studio"></a>Visual Studio 中 ReportViewer Web 伺服器控制項的瀏覽器支援

 ReportViewer Web 伺服器控制項是用來將報表功能內嵌在 ASP.NET Web 應用程式內。 這些控制項隨附在 Visual Studio 中，而且支援與本主題所述之其他元件不同的瀏覽器和瀏覽器版本。 用來檢視應用程式的瀏覽器類型會決定您可以在應用程式中提供的 ReportViewer 功能種類。 請使用本主題提供的資料表來判斷哪些支援的瀏覽器受限於報表功能限制，以及支援的平台。  

 請使用已啟用指令碼支援的瀏覽器。 如果瀏覽器無法執行指令碼，您便無法檢視報表。

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 或 11
- Google Chrome (+)
- Mozilla Firefox (+)

 **(+)** 最新的公開發行版本

## <a name="power-view-browser-support"></a>Power View 瀏覽器支援

**Microsoft Windows**  
*Windows 7、8.1、10；Windows Server 2008 R2、2012、2012 R2*

- Microsoft Internet Explorer 10 或 11
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** 最新的公開發行版本

::: moniker range="=sql-server-2016"

 如需 SharePoint 2016 瀏覽器支援的詳細資訊，請參閱 [Plan browser support in SharePoint 2013](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx)(在 SharePoint 2013 中規劃瀏覽器支援)。

::: moniker-end

## <a name="next-steps"></a>後續步驟

[在入口網站中尋找和檢視報表](report-builder/finding-and-viewing-reports-in-the-web-portal-report-builder-and-ssrs.md)  
[Reporting Services 工具](../reporting-services/tools/reporting-services-tools.md)  
[入口網站 (SSRS 原生模式)](./web-portal-ssrs-native-mode.md)  
[HTML 檢視器和報表工具列](../reporting-services/html-viewer-and-the-report-toolbar.md)  
[URL 存取參數參考](../reporting-services/url-access-parameter-reference.md)  

更多問題嗎？ [請嘗試詢問 Reporting Services 論壇](https://go.microsoft.com/fwlink/?LinkId=620231)