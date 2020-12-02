---
description: 使用 Foreach 迴圈容器來執行 Excel 檔案和資料表迴圈
title: 使用 Foreach 迴圈容器來執行 Excel 檔案和資料表迴圈 | Microsoft Docs
ms.custom: ''
ms.date: 05/15/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- connections [Integration Services], Excel
- Excel [Integration Services]
- connection managers [Integration Services], Excel
ms.assetid: a5393c1a-cc37-491a-a260-7aad84dbff68
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c6e986f032f755f73db249f7ddeff539fca4a8c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "92197169"
---
# <a name="loop-through-excel-files-and-tables-with-a-foreach-loop-container"></a>使用 Foreach 迴圈容器來執行 Excel 檔案和資料表迴圈

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  此主題的程序描述如何使用「Foreach 迴圈」容器搭配適當列舉值，循環使用資料夾中的 Excel 活頁簿，或循環使用 Excel 活頁簿中的資料表。  

> [!IMPORTANT]
> 如需連接至 Excel 檔案，以及將資料從 Excel 檔案載入或載入至 Excel 檔案的限制與已知問題的詳細資訊，請參閱[使用 SQL Server Integration Services (SSIS) 將資料從 Excel 載入或載入至 Excel](../load-data-to-from-excel-with-ssis.md)。
 
## <a name="to-loop-through-excel-files-by-using-the-foreach-file-enumerator"></a>使用 Foreach 檔案列舉值循環使用 Excel 檔案  
  
1.  建立字串變數，用以在迴圈的每個反覆運算上接收目前的 Excel 路徑和檔案名稱 為了避免驗證問題，請指派有效的 Excel 路徑和檔案名稱當做變數的初始值 (此程序後面顯示的範例運算式會使用 `ExcelFile`變數名稱)。  
  
2.  (選擇性) 建立另一個字串變數，用以保存 Excel 連接字串之 [擴充屬性] 引數的值。 這個引數包含一系列的值，這些值指定 Excel 版本並決定第一個資料列是否包含資料行名稱，以及是否使用匯入模式。 (此程序後面顯示的範例運算式會使用變數名稱 `ExtProperties`，以及 "`Excel 12.0;HDR=Yes`" 的初始值)。  
  
     如果您沒有針對 [擴充屬性] 引數使用變數，就必須手動將它加入至包含連接字串的運算式。  
  
3.  將 Foreach 迴圈容器加入 [控制流程] 索引標籤。如需如何設定 Foreach 迴圈容器的資訊，請參閱 [設定 Foreach 迴圈容器](./foreach-loop-container.md)。  
  
4.  在 [Foreach 迴圈編輯器] 的 [集合] 頁面上，選取 Foreach 檔案列舉值、指定 Excel 活頁簿所在位置的資料夾，並指定檔案篩選 (通常是 *.xlsx)。  
  
5.  在 [變數對應] 頁面上，將 [索引 0] 對應至使用者自訂的字串變數，該變數將接收迴圈每個反覆運算上目前的 Excel 路徑和檔案名稱。 (此程序後面顯示的範例運算式會使用 `ExcelFile`變數名稱)。  
  
6.  關閉 [Foreach 迴圈編輯器]。  
  
7.  如 [加入、刪除或共用封裝中的連線管理員](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))中所述，將 Excel 連線管理員加入封裝。 選取現有的 Excel 活頁簿檔案做為要連接的檔案，以避免驗證錯誤。  
  
    > [!IMPORTANT]  
    >  為了避免在設定使用此 Excel 連線管理員的工作和資料流程元件時發生驗證錯誤，請在 [Excel 連線管理員編輯器] 中選取現有的 Excel 活頁簿。 在設定了 **ConnectionString** 屬性的運算式之後 (如下列步驟所述)，連線管理員就不會在執行階段使用這個活頁簿。 在您建立和設定封裝之後，就可以在 [屬性] 視窗中清除 **ConnectionString** 屬性的值。 不過，如果您要清除這個值，除非執行「ForEach 迴圈」，否則 Excel 連接管理員的連接字串屬性將不再有效。 因此，您必須將使用連線管理員的工作或封裝的 **DelayValidation** 屬性設為 [True]，以避免驗證錯誤。  
    >   
    >  您也必須將 [False] 的預設值用於 Excel 連線管理員的 **RetainSameConnection** 屬性。 如果您將此值變更為 [True]，迴圈的每個反覆運算都會繼續開啟第一個 Excel 活頁簿。  
  
8.  選取新的 Excel 連線管理員，在 [屬性] 視窗中按一下 **Expressions** 屬性，然後按一下省略符號。  
  
9. 在 [屬性運算式編輯器] 中，選取 **ConnectionString** 屬性，然後按一下省略符號。  
  
10. 在「運算式產生器」中，輸入下列運算式：  
  
    ```  
    "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=" +  @[User::ExcelFile] + ";Extended Properties=\"" + @[User::ExtProperties] + "\""  
    ```  
  
     請注意，逸出字元 "\\" 是用來逸出括住 [擴充屬性] 引數之值的內部雙引號。  
  
     [擴充屬性] 引數不是選擇性的。 如果您沒有使用變數來包含其值，就必須手動將它新增至運算式，如下列範例所示：  
  
    ```  
    "Provider=Microsoft.ACE.OLEDB.12.0;Data Source=" +  @[User::ExcelFile] + ";Extended Properties=Excel 12.0"  
    ```  
  
11. 在「Foreach 迴圈」容器中建立工作，以便使用 Excel 連接管理員在符合指定之檔案位置和模式的每個 Excel 活頁簿上執行相同的作業。  
  
## <a name="to-loop-through-excel-tables-by-using-the-foreach-adonet-schema-rowset-enumerator"></a>使用 Foreach ADO.NET 結構描述資料列集列舉值來循環使用 Excel 資料表  
  
1.  建立會使用 Microsoft ACE OLE DB 提供者來連線到 Excel 活頁簿的 ADO.NET 連線管理員。 在 [連線管理員] 對話方塊的 [全部] 頁面上，確定您輸入 Excel 版本 - 在本案例中為 Excel 12.0 - 作為 Extended Properties 屬性的值。 如需詳細資訊，請參閱 [加入、刪除或共用封裝中的連線管理員](/previous-versions/sql/sql-server-2016/ms140237(v=sql.130))。  
  
2.  建立會接收迴圈每個反覆運算上目前資料表名稱的字串變數。  
  
3.  將 Foreach 迴圈容器加入 [控制流程] 索引標籤。如需如何設定Foreach 迴圈容器的資訊，請參閱 [設定 Foreach 迴圈容器](./foreach-loop-container.md)。  
  
4.  在 [Foreach 迴圈編輯器] 的 [集合] 頁面上，選取 [Foreach ADO.NET 結構描述資料列集] 列舉值。  
  
5.  選取您先前建立的 ADO.NET 連線管理員作為 [連接] 的值。  
  
6.  選取 [資料表] 作為 [結構描述] 的值。  
  
    > [!NOTE]  
    >  Excel 活頁簿中資料表清單包含活頁簿 (具有 $ 後置詞) 及具名範圍。 如果您必須只篩選清單中的活頁簿或具名範圍，必須在指令碼工作中寫入這個用途的自訂程式碼。 如需詳細資訊，請參閱 [以指令碼工作處理 Excel 檔案](../../integration-services/extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)。  
  
7.  在 [變數對應] 頁面上，將 [索引 2] 對應至先前建立的字串變數，以保留目前資料表的名稱。  
  
8.  關閉 [Foreach 迴圈編輯器]。  
  
9. 在「Foreach 迴圈」容器中建立工作，以便使用 Excel 連接管理員在指定之活頁簿中的每個 Excel 資料表上執行相同的作業。 如果您使用指令碼工作檢查列舉的資料表名稱，或者用來處理每個資料表，請記得將字串變數加入指令碼工作的 ReadOnlyVariables 屬性中。  
  
## <a name="see-also"></a>另請參閱  
 [使用 SQL Server Integration Services (SSIS) 將資料從 Excel 載入或載入至 Excel](../load-data-to-from-excel-with-ssis.md)  
 [設定 Foreach 迴圈容器](./foreach-loop-container.md)   
 [新增或變更屬性運算式](../../integration-services/expressions/add-or-change-a-property-expression.md)   
 [Excel 連線管理員](../../integration-services/connection-manager/excel-connection-manager.md)   
 [Excel 來源](../../integration-services/data-flow/excel-source.md)   
 [Excel 目的地](../../integration-services/data-flow/excel-destination.md)   
 [以指令碼工作處理 Excel 檔案](../../integration-services/extending-packages-scripting-task-examples/working-with-excel-files-with-the-script-task.md)  
  
