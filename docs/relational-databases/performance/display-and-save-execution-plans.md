---
title: 顯示並儲存執行計畫 | Microsoft Docs
description: 了解如何顯示執行計畫，以及如何使用 SQL Server Management Studio 將執行計畫儲存至 XML 格式的檔案。
ms.custom: ''
ms.date: 08/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Showplan results
- execution plans [SQL Server]
- queries [SQL Server], tuning
- execution plans [SQL Server], how-to topics
- SQL Server Management Studio [SQL Server], execution plans
- tuning queries [SQL Server]
ms.assetid: bcd6f094-c613-4835-ae19-4caaadb4bb17
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f06128a425040fbd6542b12d0275276efa66d480
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505257"
---
# <a name="display-and-save-execution-plans"></a>顯示並儲存執行計畫
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
本節將說明如何顯示執行計畫，以及如何使用 Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]將執行計畫儲存到 XML 格式的檔案中。  
  
執行計畫會以圖形化的方式，顯示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查詢最佳化工具所選擇的資料擷取方法。 執行計畫會使用圖示來呈現 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中特定陳述式與查詢的執行成本，而不是使用 [SET SHOWPLAN_ALL](../../t-sql/statements/set-showplan-all-transact-sql.md) 或 [SET SHOWPLAN_TEXT](../../t-sql/statements/set-showplan-text-transact-sql.md) 陳述式所產生的表格呈現方式。 這種圖形式方法，對於了解查詢的效能特性很有幫助。  

雖然 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查詢最佳化工具只會產生一個執行計畫，但仍有 **估計** 執行計畫和 **實際** 執行計畫的概念。
-  [估計執行計劃](../../relational-databases/performance/display-the-estimated-execution-plan.md)會傳回查詢最佳化工具在編譯時期產生的執行計劃。 產生估計執行計畫不會實際執行查詢或 Batch，因此不包含任何執行階段資訊，例如實際的資源使用計量或執行階段警告。 
-  [實際執行計畫](../../relational-databases/performance/display-an-actual-execution-plan.md)傳回的執行計畫，如同查詢最佳化工具所產生、完成查詢或 Batch 執行之後的執行計畫。 這包括資源使用計量和任何執行階段警告的執行階段資訊。  

如需查詢執行計劃的詳細資訊，請參閱[查詢處理架構指南](../../relational-databases/query-processing-architecture-guide.md)。
  
## <a name="in-this-section"></a>本節內容  
  
-   [顯示估計的執行計畫](../../relational-databases/performance/display-the-estimated-execution-plan.md)  
  
-   [顯示實際執行計畫](../../relational-databases/performance/display-an-actual-execution-plan.md)  
  
-   [以 XML 格式儲存執行計畫](../../relational-databases/performance/save-an-execution-plan-in-xml-format.md)  
  
  
