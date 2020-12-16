---
title: 估計堆積的大小 | Microsoft Docs
description: 使用此程序估計在 SQL Server 的堆積中，儲存資料所需的空間量。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- disk space [SQL Server], indexes
- estimating heap size
- size [SQL Server], heap
- space [SQL Server], indexes
- heaps
ms.assetid: 81fd5ec9-ce0f-4c2c-8ba0-6c483cea6c75
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 31a157c04afc4890c8818a118c83f5a3c5458e84
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474109"
---
# <a name="estimate-the-size-of-a-heap"></a>估計堆積的大小
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  您可以使用下列步驟，估計儲存堆積中的資料所需的空間量：  
  
1.  指定資料表中將會有的資料列數目：  
  
     **_Num_Rows_**  = 資料表中的資料列數目  
  
2.  指定固定長度與可變長度資料行的數目，並計算儲存這些資料行所需的空間：  
  
     計算這兩組資料行在資料列內各佔多少空間。 資料行的大小取決於資料類型和長度規格。  
  
     **_Num_Cols_**  = 資料行 (固定長度和可變長度) 總數  
  
     **_Fixed_Data_Size_**  = 所有固定長度資料行的總位元組大小  
  
     **_Num_Variable_Cols_**  = 可變長度資料行的數目  
  
     **_Max_Var_Size_**  = 所有可變長度資料行的總位元組大小上限  
  
3.  資料列中有一部分 (稱為 Null 點陣圖) 是保留的空間，用來管理資料行的 Null 屬性。 計算它的大小：  
  
     **_Null_Bitmap_**  = 2 + ((**_Num_Cols_** + 7) / 8)  
  
     您應該僅使用這個運算式的整數部分。 請捨去任何餘數。  
  
4.  計算可變長度資料的大小：  
  
     如果在索引中有可變長度的資料行，請決定將資料行儲存到索引列中所需的空間大小。  
  
     **_Variable_Data_Size_**  = 2 + (**_Num_Variable_Cols_** x 2) + **_Max_Var_Size_**  
  
     新增到 **_Max_Var_Size_** 之位元組是用於追蹤每個可變長度的資料行。 這個公式假設所有可變長度的資料行是 100% 填滿的。 如果您預期可變長度資料行所佔儲存空間的百分比會比較低，您可以經由調整百分比所得的 **_Max_Var_Size_** 值，取得更精確的整體資料表大小估計值。  
  
    > [!NOTE]  
    >  您可以結合使定義的資料表總寬度超過 8,060 個位元組的 **varchar**、 **nvarchar**、 **varbinary** 或 **sql_variant** 資料行。 這些資料行的每個長度必須仍然在 **varchar**、**nvarchar、varbinary** 或 **sql_variant** 資料行的 8,000 個位元組限制內。 然而，結合的寬度可能超過資料表中 8,060 位元組的限制。  
  
     如果沒有可變長度資料行，請將 **_Variable_Data_Size_** 設為 0。  
  
5.  計算資料列總大小：  
  
     **_Row_Size_**  = **_Fixed_Data_Size_** + **_Variable_Data_Size_** + **_Null_Bitmap_** + 4  
  
     公式中 4 這個值是資料列的資料列標頭負擔。  
  
6.  計算每個分頁的資料列數目 (每個分頁包含 8096 個可用位元組)：  
  
     **_Rows_Per_Page_**  = 8096 / (**_Row_Size_** + 2)  
  
     因為資料列不能跨頁，每個分頁的資料列數目必須無條件捨去小數，而取最接近的整數資料列。 公式中的 2 這個值是給分頁位置陣列中的該資料列項目。  
  
7.  計算儲存所有資料列所需的分頁數目：  
  
     **_Num_Pages_**  = **_Num_Rows_** / **_Rows_Per_Page_**  
  
     估計的分頁數目應該要將小數進位到最接近的整分頁數。  
  
8.  計算儲存堆積內的資料所需的空間 (每個頁面共有 8192 個位元組)：  

     堆積大小 (位元組) = 8192 x **_Num_Pages_**  
  
 上述計算未考慮下列項目：  
  
-   資料分割  
  
     從資料分割而來的空間負擔非常小，但是計算起來很複雜。 不一定要包含它。  
  
-   配置頁面  
  
     至少有一個 IAM 頁面用以追蹤配置到堆積的頁面，但是空間負擔非常小，而且沒有演算法來決定性地計算要使用多少 IAM 頁面。  
  
-   大型物件 (LOB) 值  
  
     決定到底要使用多少空間來儲存 LOB 資料類型 **varchar(max)** 、 **varbinary(max)** 、 **nvarchar(max)** 、 **text**、 **ntextxml** 和 **image** 值的演算法是很複雜的。 只要加入預期的 LOB 值平均大小，並將此值加入堆積大小總計，這樣就已足夠。  
  
-   壓縮  
  
     您無法預先計算壓縮堆積的大小。  
  
-   疏鬆資料行  
  
     如需有關疏鬆資料行空間需求的詳細資訊，請參閱＜ [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md)＞。  
  
## <a name="see-also"></a>另請參閱  
 [堆積 &#40;無叢集索引的資料表&#41;](../../relational-databases/indexes/heaps-tables-without-clustered-indexes.md)   
 [叢集與非叢集索引說明](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [建立叢集索引](../../relational-databases/indexes/create-clustered-indexes.md)   
 [建立非叢集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [估計資料表的大小](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [估計叢集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [估計非叢集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-nonclustered-index.md)   
 [估計資料庫的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
