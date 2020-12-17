---
title: 使用 URL 存取權存取報表伺服器項目 | Microsoft Docs
description: 了解如何使用 rs:Command=Value，以存取報表伺服器資料庫或 SharePoint 網站中不同類型的目錄項目。
ms.date: 05/08/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- referencing URL items for report server access
- URL access [Reporting Services], report servers
ms.assetid: a58b4ca6-129d-45e9-95c7-e9169fe5bba4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1dda91d82f56fc8e8ff46904eaa9fdd163ce56c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474499"
---
# <a name="access-report-server-items-using-url-access"></a>使用 URL 存取權存取報表伺服器項目
  本主題描述如何使用 *rs:Command*=*Value*，以存取報表伺服器資料庫或 SharePoint 網站中不同類型的目錄項目。 這不需要實際加入此參數字串。 如果您省略此參數字串，報表伺服器會評估項目類型並自動選取適當的參數值。 不過，在 URL 中使用 *rs:Command*=*Value* 字串可改善報表伺服器的效能。  
  
 請注意下列範例中的 `_vti_bin` Proxy 語法。 如需使用 Proxy 語法的詳細資訊，請參閱 [URL 存取參數參考](../reporting-services/url-access-parameter-reference.md)。  

> [!NOTE]
> SQL Server 2016 後即不再提供 Reporting Services 與 SharePoint 的整合。
  
## <a name="access-a-report"></a>存取報表  
 若要在瀏覽器中檢視報表，請使用 *rs:Command*=*Render* 參數。 例如：  
  
 - **原生** `https://myrshost/reportserver?/Sales/YearlySalesByCategory&rs:Command=Render`  

::: moniker range="=sql-server-2016"

 - **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/YearlySalesByCategory&rs:Command=Render`  
  
> [!TIP]  
>  請務必讓 URL 包含 `_vti_bin` Proxy 語法，以便透過 SharePoint 和 [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] HTTP Proxy 路由傳送要求。 此 Proxy 會將某些內容加入至 HTTP 要求，也就是確保針對 SharePoint 模式報表伺服器正確執行報表所需的內容。  

::: moniker-end
  
## <a name="access-a-resource"></a>存取資源  
 若要存取資源，請使用 *rs:Command*=*GetResourceContents* 參數。如果資源與瀏覽器相容，例如影像，即可在瀏覽器中開啟。 否則，系統會提示您開啟檔案或資源，或是將其儲存至磁碟。  
  
 **原生** `https://myrshost/reportserver?/Sales/StorePicture&rs:Command=GetResourceContents`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/StorePicture.jpg&rs:Command=GetResourceContents`  

::: moniker-end
  
## <a name="access-a-data-source"></a>存取資料來源  
 若要存取資料來源，請使用 *rs:Command*=*GetDataSourceContents* 參數。 如果您的瀏覽器支援 XML，而且如果您是有資料來源 **Read Contents** 權限的已驗證使用者，就會顯示資料來源定義。 例如：  
  
 **原生** `https://myrshost/reportserver?/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales/AdventureWorks2012&rs:Command=GetDataSourceContents`  
  
 XML 結構可能類似於下列範例：  

::: moniker-end
  
```  
<DataSourceDefinition>  
   <Extension>SQL</Extension>  
   <ConnectString>Provider=SQLOLEDB.1;Integrated Security=SSPI;Persist Security Info=False;Initial Catalog=AdventureWorks2012;Data Source=MYSERVER1;</ConnectString>  
   <CredentialRetrieval>Integrated</CredentialRetrieval>  
   <WindowsCredentials>False</WindowsCredentials>  
   <ImpersonateUser>False</ImpersonateUser>  
   <Prompt />  
   <Enabled>True</Enabled>  
</DataSourceDefinition>  
```  
  
 根據報表伺服器的 **SecureConnectionLevel** 設定傳回連接字串。 如需 **SecureConnectionLevel** 設定的詳細資訊，請參閱 [使用安全的 Web 服務方法](../reporting-services/report-server-web-service/net-framework/using-secure-web-service-methods.md)。  
  
## <a name="access-the-contents-of-a-folder"></a>存取資料夾的內容  
 若要存取資料夾的內容，請使用 *rs:Command*=*GetChildren* 參數。 隨即會傳回一般資料夾巡覽頁面，其中包含在要求資料夾中所含的子資料夾、報表、資料來源與資源的連結。 例如：  
  
 **原生** `https://myrshost/reportserver?/Sales&rs:Command=GetChildren`  

::: moniker range="=sql-server-2016"
  
 **SharePoint** `https://myspsite/subsite/_vti_bin/reportserver?https://myspsite/subsite/Sales&rs:Command=GetChildren`  

::: moniker-end
  
 您看到的使用者介面類似於 [!INCLUDE[msCoName](../includes/msconame-md.md)] Internet Information Server (IIS) 所使用的目錄瀏覽模式。 報表伺服器的版本號碼 (包括組建編號) 也會顯示在資料夾清單下面。  
  
## <a name="see-also"></a>另請參閱  
 [URL 存取 &#40;SSRS&#41;](../reporting-services/url-access-ssrs.md)   
 [URL 存取參數參考](../reporting-services/url-access-parameter-reference.md) 
