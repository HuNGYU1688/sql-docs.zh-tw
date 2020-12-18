---
title: 估計非叢集索引的大小 | Microsoft Docs
description: 使用此程序以估計在 SQL Server 中儲存非叢集索引所需的空間量。
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.reviewer: ''
ms.prod_service: database-engine, sql-database
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- space allocation [SQL Server], index size
- size [SQL Server], tables
- predicting table size [SQL Server]
- table size [SQL Server]
- estimating table size
- clustered indexes, table size
- designing databases [SQL Server], estimating size
- calculating table size
ms.assetid: c183b0e4-ef4c-4bfc-8575-5ac219c25b0a
author: stevestein
ms.author: sstein
monikerRange: = azuresqldb-current || >= sql-server-2016
ms.openlocfilehash: 9c234bb8371d99025bd97cfb86f5d85472d34ee4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474079"
---
# <a name="estimate-the-size-of-a-nonclustered-index"></a>估計非叢集索引的大小

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  請遵循下列步驟來估計儲存非叢集索引所需的空間量：  
  
1.  計算步驟 2 和 3 中所使用的變數。  
  
2.  計算在非叢集索引的分葉層級中儲存索引資訊的空間。  
  
3.  計算在非叢集索引的非分葉層級中儲存索引資訊的空間。  
  
4.  加總計算的值。  
  
## <a name="step-1-calculate-variables-for-use-in-steps-2-and-3"></a>步驟 1： 計算步驟 2 和 3 中所使用的變數  
 您可以使用下列步驟來計算變數，以便用於估計儲存索引較高層級所需的空間量。  
  
1.  指定資料表中將會有的資料列數目：  
  
     ***Num_Rows** _  = 資料表中的資料列數目  
  
2.  指定索引鍵中固定長度和可變長度資料行的數目，並計算儲存它們所需的空間：  
  
     索引的索引鍵資料行可以包含固定長度和可變長度的資料行。 若要估計內部層級的索引資料列大小，請計算這些資料行群組在索引資料列中佔用的空間。 資料行的大小取決於資料類型和長度規格。  
  
     _*_Num_Key_Cols_*_  = 索引鍵資料行 (固定長度和可變長度) 總數  
  
     _*_Fixed_Key_Size_*_  = 所有固定長度索引鍵資料行的總位元組大小  
  
     _*_Num_Variable_Key_Cols_*_  = 可變長度索引鍵資料行的數目  
  
     _*_Max_Var_Key_Size_*_  = 所有可變長度索引鍵資料行的最大位元組大小  
  
3.  如果索引不是唯一的，就需說明資料列定位器：  
  
     如果非叢集索引不是唯一的，資料列定位器就會結合非叢集索引鍵，以便針對每個資料列產生唯一的索引鍵值。  
  
     如果非叢集索引是在堆積上，資料列定位器就是堆積 RID。 這是 8 位元組的大小。  
  
     _*_Num_Key_Cols_*_  = _*_Num_Key_Cols_*_ + 1  
  
     _*_Num_Variable_Key_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 1  
  
     _*_Max_Var_Key_Size_*_  = _*_Max_Var_Key_Size_*_ + 8  
  
     如果非叢集索引是在叢集索引上，資料列定位器就是叢集索引鍵。 必須與非叢集索引鍵結合的資料行是那些在叢集索引鍵中的資料行，這些資料行並未存在於非叢集索引鍵資料行集中。  
  
     _*_Num_Key_Cols_*_  = _*_Num_Key_Cols_*_ + 叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的數目 (如果叢集索引不是唯一的，則 + 1)  
  
     _*_Fixed_Key_Size_*_  = _*_Fixed_Key_Size_*_ + 固定長度叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的總位元組大小  
  
     _*_Num_Variable_Key_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 可變長度叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的數目 (如果叢集索引不是唯一的，則 + 1)  
  
     _*_Max_Var_Key_Size_*_  = _*_Max_Var_Key_Size_*_ + 可變長度叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的最大位元組大小 (如果叢集索引不是唯一的，則 + 4)  
  
4.  資料列的一部分，又稱為 Null 點陣圖，可用以管理資料行的 Null 屬性。 計算它的大小：  
  
     如果索引鍵中有可為 Null 資料行，包含步驟 1、3 中所描述的任何所需叢集索引鍵資料行，一部分的索引資料列將保留做為 Null 點陣圖。  
  
     _*_Index_Null_Bitmap_*_  = 2 + ((索引資料列中的資料行數目 + 7) / 8)  
  
     您應該僅使用先前運算式的整數部分。 請捨去任何餘數。  
  
     如果沒有可為 Null 的索引鍵資料行，請將 _*_Index_Null_Bitmap_*_ 設成 0。  
  
5.  計算可變長度的資料大小：  
  
     如果在索引鍵中有可變長度的資料行，包含任何所需的叢集索引鍵資料行，請決定在索引資料列中儲存資料行所需的空間。  
  
     _*_Variable_Key_Size_*_  = 2 + (_*_Num_Variable_Key_Cols_*_ x 2) + _*_Max_Var_Key_Size_*_  
  
     加入 _*_Max_Var_Key_Size_*_ 的位元組是用於追蹤每個變數資料行。這個公式假設所有可變長度的資料行都是 100% 填滿的。 如果您預期可變長度資料行所使用的儲存空間百分比會比較小，您可以經由調整百分比所得的 _*_Max_Var_Key_Size_*_ 值，產生更精確的整體資料表大小估計值。  
  
     如果沒有可變長度資料行，請將 _*_Variable_Key_Size_*_ 設成 0。  
  
6.  計算索引資料列的大小：  
  
     _*_Index_Row_Size_*_  = _*_Fixed_Key_Size_*_ + _*_Variable_Key_Size_*_ + _*_Index_Null_Bitmap_*_ + 1 (索引資料列的資料列標頭上方) + 6 (子頁面識別碼指標)  
  
7.  計算每個分頁的索引資料列數目 (每個分頁包含 8096 個可用位元組)：  
  
     _*_Index_Rows_Per_Page_*_  = 8096 / (_*_Index_Row_Size_*_ + 2)  
  
     由於索引資料列並不會跨越分頁，因此每個分頁索引資料列的數目應該捨去小數，而取最接近的整數列數。 公式中的 2 是給分頁位置陣列中的該資料列項目。  
  
## <a name="step-2-calculate-the-space-used-to-store-index-information-in-the-leaf-level"></a>步驟 2： 計算在分葉層級中儲存索引資訊所用的空間  
 您可以使用下列步驟來估計儲存索引分葉層級所需的空間量。 您將需要步驟 1 中所保留的值以計算此步驟。  
  
1.  指定在分葉層級的固定長度和可變長度的資料行數目，並計算儲存它們所需的空間：  
  
    > [!NOTE]  
    >  除了索引鍵資料行之外，包含非索引鍵資料行將可擴充非叢集索引。 這些額外的資料行只會儲存在非叢集索引的分葉層級。 如需詳細資訊，請參閱 [建立內含資料行的索引](../../relational-databases/indexes/create-indexes-with-included-columns.md)。  
  
    > [!NOTE]  
    >  您可以結合使定義的資料表總寬度超過 8,060 個位元組的 _*varchar**、**nvarchar**、**varbinary** 或 **sql_variant** 資料行。 這些資料行的每個長度必須仍然在 **varchar**、 **varbinary** 或 **sql_variant** 資料行的 8,000 個位元組限制內，以及 **nvarchar** 資料行的 4,000 個位元組限制。 然而，結合的寬度可能超過資料表中 8,060 位元組的限制。 這也適用於已包含資料行的非叢集索引分葉資料列。  
  
     如果非叢集索引沒有任何內含的資料行，請使用步驟 1 的值，包含步驟 1、3 中所決定的任何修改：  
  
     **_Num_Leaf_Cols_* _  = _*_Num_Key_Cols_*_  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Key_Size_*_  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Key_Cols_*_  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Key_Size_*_  
  
     如果非叢集索引沒有任何內含的資料行，請加入適當的值至步驟 1 的值中，包含在步驟 1、3 中所做的任何修改。 資料行的大小取決於資料類型和長度規格。 如需詳細資訊，請參閱[資料類型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Key_Cols_*_ + 內含資料行的數目  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Key_Size_*_ + 固定長度內含資料行的總位元組大小  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Key_Cols_*_ + 可變長度內含資料行的數目  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Key_Size_*_ + 可變長度內含資料行的最大位元組大小  
  
2.  說明資料列定位器：  
  
     如果非叢集索引不是唯一的，由於資料列定位器的負擔在步驟 1、3 中已經考慮進去，因此不需要再做其他的修改。 到下一個步驟。  
  
     如果非叢集索引是唯一的，在分葉層級的所有資料列中必須說明資料列定位器。  
  
     如果非叢集索引是在堆積上，資料列定位器就是堆積 RID (大小是 8 位元組)。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Leaf_Cols_*_ + 1  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Leaf_Cols_*_ + 1  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Leaf_Size_*_ + 8  
  
     如果非叢集索引是在叢集索引上，資料列定位器就是叢集索引鍵。 必須與非叢集索引鍵結合的資料行是那些在叢集索引鍵中的資料行，這些資料行並未存在於非叢集索引鍵資料行集中。  
  
     _*_Num_Leaf_Cols_*_  = _*_Num_Leaf_Cols_*_ + 叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的數目 (如果叢集索引不是唯一的，則 + 1)  
  
     _*_Fixed_Leaf_Size_*_  = _*_Fixed_Leaf_Size_*_ + 固定長度叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的數目  
  
     _*_Num_Variable_Leaf_Cols_*_  = _*_Num_Variable_Leaf_Cols_*_ + 叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的數目 (如果叢集索引不是唯一的，則 + 1)  
  
     _*_Max_Var_Leaf_Size_*_  = _*_Max_Var_Leaf_Size_*_ + 可變長度叢集索引鍵資料行 (不是位於非叢集索引鍵資料行集中) 的位元組大小 (如果叢集索引不是唯一的，則 + 4)  
  
3.  計算 Null 點陣圖大小：  
  
     _*_Leaf_Null_Bitmap_*_  = 2 + ((_*_Num_Leaf_Cols_*_ + 7) / 8)  
  
     您應該僅使用先前運算式的整數部分。 請捨去任何餘數。  
  
4.  計算可變長度的資料大小：  
  
     如果有可變長度的資料行 (索引鍵資料行或已包含)，包含先前步驟 2.2 中所描述的任何所需叢集索引鍵資料行，請決定在索引資料列中儲存資料行所需的空間：  
  
     _*_Variable_Leaf_Size_*_  = 2 + (_*_Num_Variable_Leaf_Cols_*_ x 2) + _*_Max_Var_Leaf_Size_*_  
  
     加入 _*_Max_Var_Key_Size_*_ 的位元組是用於追蹤每個變數資料行。這個公式假設所有可變長度的資料行都是 100% 填滿的。 如果您預期可變長度資料行所使用的儲存空間百分比會比較小，您可以經由調整百分比所得的 _*_Max_Var_Leaf_Size_*_ 值，產生更精確的整體資料表大小估計值。  
  
     如果沒有可變長度的資料行 (索引鍵資料行或包含)，請將 _*_Variable_Leaf_Size_*_ 設成 0。  
  
5.  計算索引資料列的大小：  
  
     _*_Leaf_Row_Size_*_  = _*_Fixed_Leaf_Size_*_ + _*_Variable_Leaf_Size_*_ + _*_Leaf_Null_Bitmap_*_ + 1 (索引資料列的資料列標頭上方)  
  
6.  計算每個分頁的索引資料列數目 (每個分頁包含 8096 個可用位元組)：  
  
     _*_Leaf_Rows_Per_Page_*_  = 8096 / (_*_Leaf_Row_Size_*_ + 2)  
  
     由於索引資料列並不會跨越分頁，因此每個分頁索引資料列的數目應該捨去小數，而取最接近的整數列數。 公式中的 2 是給分頁位置陣列中的該資料列項目。  
  
7.  根據指定的 [填滿因數](../../relational-databases/indexes/specify-fill-factor-for-an-index.md) ，計算每個分頁所保留的可用資料列數目：  
  
     _*_Free_Rows_Per_Page_*_  = 8096 x ((100 - _*_Fill_Factor_*_) / 100) / (_*_Leaf_Row_Size_*_ + 2)  
  
     計算中所使用的填滿因數是整數值而不是百分比。 因為資料列不能跨頁，每個分頁的資料列數目必須無條件捨去小數，而取最接近的整數資料列。 當填滿因數變大時，每個分頁上會儲存更多資料，分頁也會比較少。 公式中的 2 是給分頁位置陣列中的該資料列項目。  
  
8.  計算儲存所有資料列所需的分頁數目：  
  
     _*_Num_Leaf_Pages_*_  = _*_Num_Rows_*_ / (_*_Leaf_Rows_Per_Page_*_ - _*_Free_Rows_Per_Page_*_ )  
  
     估計的分頁數目應該要將小數進位到最接近的整分頁數。  
  
9. 計算索引的大小 (每個分頁共有 8192 個位元組)：  
  
     _*_Leaf_Space_Used_*_  = 8192 x _*_Num_Leaf_Pages_*_  
  
## <a name="step-3-calculate-the-space-used-to-store-index-information-in-the-non-leaf-levels"></a>步驟 3： 計算在非分葉層級中儲存索引資訊所用的空間  
 請遵循下列步驟來估計儲存索引之中繼和根層級所需的空間量。 您將需要步驟 2 和 3 中所保留的值來完成此步驟。  
  
1.  計算索引中的非分葉層級數目：  
  
     _*_Non-leaf Levels_*_  = 1 + log( Index_Rows_Per_Page) (_*_Num_Leaf_Pages_*_ / _*_Index_Rows_Per_Page_*_)  
  
     將這個值無條件向上進位到最近的整數。 此值不包含非叢集索引的分葉層級。  
  
2.  計算索引中的非分葉頁面數目：  
  
     _*_Num_Index_Pages_*_  = ∑Level (_*_Num_Leaf_Pages/Index_Rows_Per_Page_*_^Level)，其中 1 <= Level <= _*_Levels_*_  
  
     將每個被加數無條件向上進位到最近的整數。 簡單舉例說明，假設有個索引，其中 _*_Num_Leaf_Pages_*_ = 1000 且 _*_Index_Rows_Per_Page_*_ = 25。 分葉層級上方的第一個索引層級會儲存 1000 個索引資料列，這是每個分葉頁面的一個索引資料列，而且每個頁面可以容納 25 個索引資料列。 這表示儲存這 1000 個索引資料列需要 40 頁。 索引的下一個層級必須儲存 40 個資料列， 這表示它需要 2 頁。 索引的最後一個層級必須儲存 2 個資料列。 這表示它需要 1 頁。 這會產生 43 個非分葉索引頁面。 在上述公式中使用這些數字時，結果如下：  
  
     _*_Non-leaf_Levels_*_  = 1 + log(25) (1000 / 25) = 3  
  
     _*_Num_Index_Pages_*_ = 1000/(25^3)+ 1000/(25^2) + 1000/(25^1) = 1 + 2 + 40 = 43，這是範例中所描述的頁數。  
  
3.  計算索引的大小 (每個分頁共有 8192 個位元組)：  
  
     _*_Index_Space_Used_*_  = 8192 x _*_Num_Index_Pages_*_  
  
## <a name="step-4-total-the-calculated-values"></a>步驟 4： 加總計算的值  
 將前兩個步驟所取得的值加總：  
  
 非叢集索引大小 (位元組) = _*_Leaf_Space_Used_*_ + _*_Index_Space_used_*_  
  
 上述計算未考慮下列項目：  
  
-   資料分割  
  
     從資料分割而來的空間負擔非常小，但是計算起來很複雜。 不一定要包含它。  
  
-   配置頁面  
  
     至少有一個 IAM 頁面用以追蹤配置到堆積的頁面，但是空間負擔非常小，而且沒有演算法來決定性地計算要使用多少 IAM 頁面。  
  
-   大型物件 (LOB) 值  
  
     決定到底要使用多少空間以儲存 LOB 資料類型 _*varchar(max)**、**varbinary(max)** 、**nvarchar(max)** 、**text**、**ntext**、**xml** 和 **image** 值的演算法是很複雜的。 只要加上預期的 LOB 值平均大小，乘以 **_Num_Rows_**，再將此值加上非叢集索引總大小，這就足夠。  
  
-   壓縮  
  
     您無法預先計算壓縮索引的大小。  
  
-   疏鬆資料行  
  
     如需有關疏鬆資料行空間需求的詳細資訊，請參閱＜ [Use Sparse Columns](../../relational-databases/tables/use-sparse-columns.md)＞。  
  
## <a name="see-also"></a>另請參閱  
 [叢集與非叢集索引說明](../../relational-databases/indexes/clustered-and-nonclustered-indexes-described.md)   
 [建立非叢集索引](../../relational-databases/indexes/create-nonclustered-indexes.md)   
 [建立叢集索引](../../relational-databases/indexes/create-clustered-indexes.md)   
 [估計資料表的大小](../../relational-databases/databases/estimate-the-size-of-a-table.md)   
 [估計叢集索引的大小](../../relational-databases/databases/estimate-the-size-of-a-clustered-index.md)   
 [估計堆積的大小](../../relational-databases/databases/estimate-the-size-of-a-heap.md)   
 [估計資料庫的大小](../../relational-databases/databases/estimate-the-size-of-a-database.md)  
  
  
