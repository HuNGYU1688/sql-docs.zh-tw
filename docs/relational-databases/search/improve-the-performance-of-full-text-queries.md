---
description: 改善全文檢索查詢的效能
title: 改善全文檢索查詢的效能 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 0658dc74-25eb-4486-bbd6-e85c1f92c272
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a47f4aab579410c147f4e6472a851cd6baa0dc0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479489"
---
# <a name="improve-the-performance-of-full-text-queries"></a>改善全文檢索查詢的效能
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  以下是有助於提升全文檢索查詢效能的建議事項清單。  
  
 全文檢索查詢的效能也會受到硬體資源的影響，例如記憶體、磁碟速度、CPU 速度和電腦架構。  
  
-   使用 [ALTER INDEX REORGANIZE](../../t-sql/statements/alter-index-transact-sql.md)來重組基底資料表的索引。  
  
-   使用 [ALTER FULLTEXT CATALOG REORGANIZE](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)來重新組織全文檢索目錄。 在進行效能測試之前，請確定完成此動作，因為執行此陳述式會造成該目錄中之全文檢索的主要合併。  
  
-   請將您選擇的全文檢索索引鍵資料行限制為小資料行。 雖然支援 900 位元組的資料行，但是我們建議您在全文檢索索引中使用較小的索引鍵資料行。 **int** 和 **bigint** 會提供最佳效能。  
  
-   使用整數全文檢索索引鍵可避免與 **docid** 對應資料表發生聯結。 因此，整數全文檢索索引鍵會依據重要性順序改善查詢效能並改善搜耙效能。 如果全文檢索索引鍵也是叢集索引索引鍵，可能會產生額外效能優勢。  
  
-   將多個 [CONTAINS](../../t-sql/queries/contains-transact-sql.md) 述詞結合為一個 CONTAINS 述詞。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，您可以在 CONTAINS 查詢中指定資料行清單。  
  
-   如果您只需要全文檢索索引鍵或次序資訊，請分別使用 [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) 或 [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md) ，而不要使用 CONTAINS 或 FREETEXT。  
  
-   若要限制結果並提升效能，請使用 FREETEXTTABLE 和 CONTAINSTABLE 函數的 *top_n_by_rank* 參數。 *top_n_by_rank* 可讓您僅重新叫用最相關的叫用。 只有當您的商務狀況不需要重新叫用所有可能的叫用 (亦即，不需要「全部重新叫用」) 時，才應該使用這個參數。  
  
    > [!NOTE]  
    >  全部重新叫用通常是法律狀況的必要項目，但是其重要性可能低於商務狀況的效能，例如電子商務。  
  
-   請檢查全文檢索查詢計畫，確定已選擇適當的聯結計畫。 必要的話，請使用聯結提示或查詢提示。 如果在全文檢索查詢中使用某個參數，該參數的首次值就會決定查詢計畫。 您可以使用 OPTIMIZE FOR [查詢提示](../../t-sql/queries/hints-transact-sql-query.md) ，強制查詢使用您想要的值進行編譯。 這有助於達成決定性的查詢計畫和更佳的效能。  
  
-   如果全文檢索索引包含過多全文檢索索引片段，可能會導致查詢效能大幅降低。 若要減少片段的數目，請使用 [ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)[!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式的 REORGANIZE 選項來重新組織全文檢索目錄。 這個陳述式基本上會將所有片段合併成較大的單一片段，然後從全文檢索索引中移除所有已過時的項目。  
  
-   在全文檢索搜尋中，CONTAINSTABLE (AND, OR) 中指定的邏輯運算子可以實作成 SQL 聯結或在全文檢索執行資料流資料表值函式 (STVF) 內部實作。 一般而言，只有一種邏輯運算子類型的查詢是完全由全文檢索執行實作，而混合使用邏輯運算子的查詢也會擁有 SQL 聯結。 在全文檢索執行 STVF 內部實作邏輯運算子會使用一些特殊索引屬性，讓它的速度比 SQL 聯結更快。 因此，我們建議您盡可能只使用單一邏輯運算子類型來設計查詢。  
  
-   若為包含選擇性關聯述詞的應用程式，當使用選擇性關聯式述詞與非選擇性全文檢索述詞的查詢撰寫成使用查詢最佳化工具時，這些查詢可能會具有最佳效能。 這樣做可讓查詢最佳化工具決定它是否可利用述詞或範圍下推來產生有效的查詢計畫。 這種方法比較簡單，而且通常會比將關聯式資料當做全文檢索資料進行索引更有效率。  
  
## <a name="related-resources"></a>相關資源  
 [SQL Server 2008 全文檢索搜尋：內部和增強功能](/previous-versions/sql/sql-server-2008/cc721269(v=sql.100))  
  
## <a name="see-also"></a>另請參閱  
 [sys.dm_fts_memory_buffers &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)   
 [sys.dm_fts_memory_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
  
