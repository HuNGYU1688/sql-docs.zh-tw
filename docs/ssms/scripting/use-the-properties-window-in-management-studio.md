---
title: 在 Management Studio 中使用屬性視窗
description: 了解如何使用 [屬性] 視窗查看 SQL Server Management Studio 項目 (例如連線) 與資料庫物件的相關資訊。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- viewing properties
- Properties window [SQL Server Management Studio]
- complex properties [SQL Server Management Studio]
ms.assetid: 903d4aca-f57c-43d9-a893-702eceaa7004
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9f18cec0f6ac8754fc5826e75325c810e965b692
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480579"
---
# <a name="use-the-properties-window-in-management-studio"></a>在 Management Studio 中使用屬性視窗
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [屬性] 視窗描述 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的項目狀態，如連接或執行程序表運算子，以及資料庫物件的詳細資訊，如資料表、檢視表及設計師。  
  
 您可以利用 [屬性] 視窗來檢視目前連接的屬性。 許多屬性在 [屬性] 視窗中都是唯讀的，但在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]中的其他位置是可改變的。 例如，查詢的 [資料庫] 屬性在 [屬性] 視窗是唯讀的，但在工具列中是可改變的。  
  
### <a name="to-view-properties-using-the-properties-window"></a>利用 [屬性] 視窗檢視屬性  
  
1.  如果看不到 [屬性] 視窗，請按一下 [檢視] 功能表中的 [屬性視窗]，或按 F4。  
  
2.  將焦點設在您要檢視的物件上。  
  
3.  在 [屬性] 視窗中，尋找特定屬性。  
  
### <a name="to-view-connection-properties-of-a-query-window"></a>檢視查詢視窗的連接屬性  
  
1.  如果看不到 [屬性] 視窗，請按一下 [檢視] 功能表中的 [屬性視窗]，或按 F4。  
  
2.  在 [屬性] 視窗中，您可以見到所有連接屬性。  
  
### <a name="to-view-the-properties-of-a-showplan-operator"></a>檢視執行程序表運算子的屬性  
  
1.  在 [查詢] 功能表上，按一下 [包括實際執行計畫]。  
  
2.  在 [SQL 查詢編輯器] 中，輸入和執行查詢。  
  
3.  如果看不到 [屬性] 視窗，請按一下 [檢視] 功能表中的 [屬性視窗]，或按 F4。  
  
4.  在 [SQL 查詢編輯器] 的 [執行計畫] 索引標籤中，按一下運算子的圖示來檢視 [屬性] 視窗中之運算子的相關資訊。  
  
## <a name="see-also"></a>另請參閱  
 [屬性視窗 &#40;Management Studio&#41;](../properties-window-management-studio.md)  
  
