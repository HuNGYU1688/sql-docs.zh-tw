---
title: 顯示估計的執行計畫 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio 來產生圖形化的估計執行計畫。 估計執行計畫不包含任何執行階段資訊。
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- zoom [SQL Server]
- estimated execution plan [SQL Server]
- displaying execution plans
- viewing execution plans
- execution plans [SQL Server], displaying
- customizing execution plan display [SQL Server]
- modifying execution plan display
- custom zoom [SQL Server]
ms.assetid: e94aa576-4c0c-4c54-ad05-6c3432cc615b
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1d9f9a764427d08c30a1e02890b7abf1378ea407
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97418053"
---
# <a name="display-the-estimated-execution-plan"></a>顯示估計的執行計畫
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  此主題描述如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]產生圖形化的估計執行計畫。 產生估計執行計畫時，不會執行 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查詢或批次。 因為這個緣故，估計執行計畫不包含任何執行階段資訊，如實際資源使用計量和執行階段警告等。 不過，如果真的執行查詢，所產生的執行計畫會顯示 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 最有可能使用的查詢執行計畫，並顯示流經計劃中數項作業的估計資料列。  
  
 若要使用這個功能，使用者必須具有適當的權限來執行要產生圖形執行計畫的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查詢，而且必須獲得查詢所參考的所有資料庫的 SHOWPLAN 權限。  
  
## <a name="to-display-the-estimated-execution-plan-for-a-query"></a>若要顯示查詢的評估執行計畫  
  
1.  在工具列上，按一下 [Database Engine 查詢]。 您也可以按一下 [開啟檔案] 工具列按鈕並找出現有的查詢，以開啟現有的查詢並顯示估計的執行計畫。  
  
2.  輸入您要顯示估計執行計畫的查詢。  
  
3.  在 [查詢] 功能表上，按一下 [顯示估計執行計畫]，或按一下 [顯示估計執行計畫] 工具列按鈕。 估計執行計畫會顯示在結果窗格中的 [執行計畫] 索引標籤。 

    ![工具列上的估計執行計畫按鈕](../../relational-databases/performance/media/estimatedexecplantoolbar.png "工具列上的估計執行計畫按鈕")    

    若要檢視其他資訊，請將滑鼠暫時放在邏輯與實體運算子的圖示上，即可在顯示的 [工具提示] 中檢視運算子的說明與屬性。 或者，您可以在 [屬性] 視窗中檢視運算子屬性。 如果沒有看到 [屬性] 視窗，請以滑鼠右鍵按一下運算子，然後按一下 [屬性]。 選取運算子以檢視其屬性。  

    ![以滑鼠右鍵按一下計畫運算子中的 [屬性]](../../relational-databases/performance/media/planproperties.png "以滑鼠右鍵按一下計畫運算子中的 [屬性]")    
  
4.  若要改變執行計劃的顯示，請以滑鼠右鍵按一下 [執行計畫]，然後選取 [放大]、[縮小]、[自訂顯示比例] 或 [縮放至適當比例]。 [放大] 與 [縮小] 可讓您以固定量放大或縮小執行計畫。 [自訂顯示比例] 可讓您定義自己的顯示倍率，例如縮放至百分之 80。 [縮放至適當比例] 會放大執行計畫，以符合結果窗格的大小。 或者，使用 CTRL 鍵加滑鼠滾輪，啟動 **動態縮放**。  

5.  若要在執行計劃的顯示內容中巡覽，請使用垂直和水平捲軸，或是 **在任何執行計劃的空白區域上按住不放**，然後 **拖曳您的滑鼠**。 或者，按住不放執行計劃視窗右下角的加號 (+) ，來顯示整個執行計劃的縮圖。
 
> [!NOTE] 
> 或者，使用 [SET STATISTICS XML](../../t-sql/statements/set-showplan-xml-transact-sql.md)，在不執行各個陳述式的情況下傳回其執行計畫資訊。 如果用於 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]，[結果] 索引標籤會有連結，以圖形格式開啟執行計畫。   
  
## <a name="see-also"></a>另請參閱  
 [執行計畫](../../relational-databases/performance/execution-plans.md)    
 [查詢處理架構指南](../../relational-databases/query-processing-architecture-guide.md)  
