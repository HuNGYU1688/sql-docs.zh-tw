---
description: 教學課程：使用 OData 來源
title: 教學課程：使用 OData 來源 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 2c64cf8b-5edb-48df-8ffe-697096258f71
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e12fe80482b6b6ffd96fe6e56e6dfad1a92362fc
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88425670"
---
# <a name="tutorial-using-the-odata-source"></a>教學課程：使用 OData 來源

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  本教學課程將逐步引導您進行從範例 **Northwind** OData 服務 (https://services.odata.org/V3/Northwind/Northwind.svc/) 擷取 **Employees** 集合，然後將該集合載入一般檔案的程序。  
  
## <a name="1-create-an-integration-services-project"></a>1.建立 Integration Services 專案  
  
1.  啟動 **[SQL Server Data Tools]** 或 [ [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]]。  
  
2.  按一下 [檔案]  ，指向 [新增]  ，然後按一下 [專案]  。  
  
3.  在 **[新增專案]** 對話方塊中，依序展開 **[已安裝的]**、 **[範本]**、 **[Business Intelligence]**，然後按一下 **[Integration Services]**。  
  
4.  選取該專案類型的 **[Integration Services 專案]** 。  
  
5.  輸入專案的 **[名稱]** 並選取 **[位置]** ，然後按一下 **[確定]**。  
  
## <a name="2-add-and-configure-an-odata-source"></a>2.加入和設定 OData 來源 
  
1.  從 [SSIS 工具箱] 將 [資料流程工作] 拖放至 SSIS 封裝的控制流程設計介面。  
  
2.  按一下 [資料流程] 索引標籤或按兩下 [資料流程工作] 以開啟 [資料流程] 設計介面。  
  
3.  從 [SSIS 工具箱] 的 [Common] 群組中拖放 [OData 來源]。
  
4.  按兩下 [OData 來源] 元件以啟動 [OData 來源編輯器] 對話方塊。  
  
5.  按一下 [新增...] 以新增 OData 連接管理員。  
  
6.  輸入 **[服務文件位置]** 的 OData 服務 URL。 此 URL 可以是服務文件的 URL，或是特定摘要或實體的 URL。 針對本教學課程的用途，輸入服務文件的 URL：[https://services.odata.org/V3/Northwind/Northwind.svc/](https://services.odata.org/V3/Northwind/Northwind.svc/)。  
  
7.  確認已選取 **[Windows 驗證]** 做為用來存取 OData 服務的 **[驗證]** 。 **[Windows 驗證]** 預設為選取狀態。  
  
8.  按一下 [測試連接] 以測試連接，然後按一下 [確定] 以完成建立 OData 連線管理員的執行個體。  
  
9. 在 **[OData 來源編輯器]** 對話方塊中，確認已針對 **[在資源路徑上使用集合]** 選項選取 **[集合]** 。  
  
10. 從 [集合] 下拉式清單中選取 **Employees**。  
  
11. 針對 **[查詢選項]** 輸入任何其他 OData 查詢選項或篩選。 例如： `$orderby=CompanyName&$top=100` 。 針對本教學課程的用途，輸入 `$top=5`。  
  
12. 按一下 **[預覽]** 預覽資料。  
  
13. 按一下左側功能窗格中的 **[資料行]** ，切換至 **[資料行]** 頁面。  
  
14. 透過選取核取方塊的方式，從 **[可用的外部資料行]** 選取 **[EmployeeID]**、 **[FirstName]** 和 **[LastName]** 。  
  
15. 按一下 **[確定]** ，關閉 **[OData 來源編輯器]** 對話方塊。  
  
## <a name="3-add-and-configure-a-flat-file-destination"></a>3.加入和設定一般檔案目的地
  
1.  現在，從 [SSIS 工具箱] 將 [一般檔案目的地] 拖放至 [OData 來源] 元件下方的資料流程設計介面。  
  
2.  使用藍色箭頭連接 **[OData 來源]** 元件與 **[一般檔案目的地]** 元件。  
  
3.  按兩下 [一般檔案目的地]。 **[一般檔案目的地編輯器]** 對話方塊應會隨即出現。  
  
4.  在 **[一般檔案目的地編輯器]** 對話方塊中，按一下 **[新增]** 建立新的一般檔案連接管理員。  
  
5.  在 **[一般檔案格式]** 對話方塊中，選取 **[使用分隔符號]**。 [一般檔案連線管理員編輯器] 對話方塊則會隨即出現。  
  
6.  在 [一般檔案連線管理員編輯器] 對話方塊中，輸入 `c:\Employees.txt` 作為 [檔案名稱]。  
  
7.  在左側功能窗格中，按一下 **[資料行]**。 您可以在此頁面上預覽資料。  
  
8.  按一下 [確定]，關閉 **[一般檔案連接管理員編輯器]** 對話方塊。  
  
9. 在 **[一般檔案目的地編輯器]** 對話方塊中，按一下左邊功能窗格中的 **[對應]** 。 檢閱對應。  
  
10. 按一下 [確定]，關閉 **[一般檔案目的地編輯器]** 對話方塊。  

## <a name="4-run-the-package"></a>4.執行封裝
執行 SSIS 封裝。 確認建立的輸出檔案包含 OData 摘要中五位員工的 [識別碼]、[名字] 和 [姓氏]。
  
  
