---
title: 記憶體內部 OLTP 記憶體回收 | Microsoft Docs
description: 了解 SQL Server 中記憶體內部 OLTP 的記憶體回收。 記憶體回收的主旨是讓不再使用的交易刪除資料列。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 940140a7-4785-46fc-8bf4-151435dccd3c
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b5b9060049965e9a826d3f9f22a1e6eb341ca0dc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485290"
---
# <a name="in-memory-oltp-garbage-collection"></a>記憶體中的 OLTP 記憶體回收
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  如果某個不再使用的交易刪除資料列，則該資料列視為過時。 過時的資料列適合進行記憶體回收。 以下是 [!INCLUDE[hek_2](../../includes/hek-2-md.md)]之記憶體回收的特性：  
  
-   非鎖定。 記憶體回收會隨著時間散發，藉此將對工作負載的影響降到最低。  
  
-   合作式。 使用者交易會參與記憶體回收與主要記憶體回收執行緒。  
  
-   高效率。 使用者交易針對所使用之存取路徑 (索引) 中的過時資料列，會取消其連結。 這會減少最後移除資料列時所需進行的工作。  
  
-   主動回應。 記憶體不足的壓力會導致記憶體回收變得積極。  
  
-   可擴充。 認可後，使用者交易會執行一部分的記憶體回收工作。 交易活動越多，就有越多交易取消過時資料列的連結。  
  
 記憶體回收是由主要記憶體回收執行緒所控制。 主要記憶體回收執行緒會每分鐘執行一次，或在認可的交易數目超出內部臨界值時執行。 記憶體回收行程的工作是：  
  
-   識別已刪除或更新一組資料列並在最舊的使用中交易之前認可的交易。  
  
-   識別這些舊交易所建立的資料列版本。  
  
-   將這些舊資料列分組為一個或多個單位，每個單位包含 16 個資料列。 這樣做的目的在於將記憶體回收行程的工作分散到較小的單位。  
  
-   將這些工作單位逐一移至每個排程器的記憶體回收佇列中。 如需詳細資料，請參閱下列記憶體回收行程 DMV：[sys.dm_xtp_gc_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-stats-transact-sql.md)、[sys.dm_db_xtp_gc_cycle_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-xtp-gc-cycle-stats-transact-sql.md) 及 [sys.dm_xtp_gc_queue_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xtp-gc-queue-stats-transact-sql.md)。  
  
 使用者交易在認可之後，會識別與其執行所在之排程器相關聯的所有佇列項目，然後釋出記憶體。 如果排程器上的記憶體回收佇列是空的，則它會搜尋目前 NUMA 節點中所有非空白的佇列。 如果發現交易活動較少且有記憶體不足的壓力，則主要記憶體回收執行緒可從任何佇列進行資料列的記憶體回收。 例如，如果刪除大量資料列之後沒有交易活動，而且沒有記憶體不足的壓力，則在交易活動繼續或發生記憶體不足的壓力之前，將不會對已刪除的資料列進行記憶體回收。  
  
## <a name="see-also"></a>另請參閱  
 [為記憶體內部 OLTP 管理記憶體](/previous-versions/sql/sql-server-2016/dn465872(v=sql.130))  
  
