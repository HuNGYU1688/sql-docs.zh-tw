---
title: 效能中心
description: 尋找所需 SQL Server 資料庫引擎和 Azure SQL Database 中的效能資訊。
ms.custom: seo-dt-2019
ms.date: 12/11/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
f1_keywords:
- Performance (SQL Server)
- Performance (SQL Database)
helpviewer_keywords:
- SQL Server, performance
- performance (SQL Server)
- database performance (SQL Server)
- SQL Database (Performance)
- performance (SQL Database)
- database performance (SQL Database)
ms.assetid: 301204b2-140d-4495-98ed-021a9b5025f5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: a93a5ba8ecacf9080edba64b31d7760f6b359c48
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505127"
---
# <a name="performance-center-for-sql-server-database-engine-and-azure-sql-database"></a>SQL Server Database Engine 和 Azure SQL Database 的效能中心
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本頁提供的連結有助於您尋找所需之 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]中的效能相關資訊。  
  
 **圖例**  
  
 ![說明功能可用性圖示的圖例螢幕擷取畫面。](../../relational-databases/performance/media/security-center-legend.PNG "security-center-legend")  
  
## <a name="configuration-options-for-performance"></a>效能的組態選項  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可讓您透過許多組態選項來影響 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 層級的資料庫引擎效能。 使用 [!INCLUDE[ssSDSFull](../../includes/sssdsfull-md.md)]，Microsoft 會為您執行其中的大部分 (但非全部) 最大化。  
  
|||  
|-|-|  
|**磁碟組態選項**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [磁碟分割和 RAID](https://technet.microsoft.com/library/ms190764\(v=sql.105\).aspx)|  
|**資料和記錄檔組態選項**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [將資料檔和記錄檔放在不同的磁碟機上](../../relational-databases/policy-based-management/place-data-and-log-files-on-separate-drives.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [檢視或變更資料及記錄檔的預設位置 &#40;SQL Server Management Studio&#41;](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)|  
|**TempDB 組態選項**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [TempDB 中的效能改善](../databases/tempdb-database.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [資料庫引擎組態 - TempDB](../../database-engine/install-windows/install-sql-server.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [使用 Azure VM 中的 SSD 來儲存 SQL Server TempDB 和緩衝集區擴充](https://cloudblogs.microsoft.com/sqlserver/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [Azure 虛擬機器中 SQL Server 暫存磁碟的磁碟和效能最佳做法](/aspnet/core/performance/performance-best-practices)|  
|**伺服器設定選項**|**處理器組態選項**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [親和性遮罩伺服器組態選項](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [affinity Input-Output mask 伺服器組態選項](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [affinity64 mask 伺服器組態選項](../../database-engine/configure-windows/affinity64-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [affinity64 輸入輸出伺服器組態選項](../../database-engine/configure-windows/affinity64-input-output-mask-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定 max worker threads 伺服器組態選項](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)<br /><br />**記憶體組態選項**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [伺服器記憶體伺服器組態選項](../../database-engine/configure-windows/server-memory-server-configuration-options.md)<br /><br />**索引組態選項**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定 fill factor 伺服器組態選項](../../database-engine/configure-windows/configure-the-fill-factor-server-configuration-option.md)<br /><br />**查詢組態選項**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定每筆查詢的最小記憶體數伺服器組態選項](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定 query governor cost limit 伺服器組態選項](../../database-engine/configure-windows/configure-the-query-governor-cost-limit-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定 max degree of parallelism 伺服器組態選項](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [設定 cost threshold for parallelism 伺服器組態選項](../../database-engine/configure-windows/configure-the-cost-threshold-for-parallelism-server-configuration-option.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [針對特定工作負載最佳化伺服器組態選項](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md)<br /><br />**備份組態選項**<br /><br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [檢視或設定 backup compression default 伺服器組態選項](../../database-engine/configure-windows/view-or-configure-the-backup-compression-default-server-configuration-option.md)|  
|**資料庫組態最佳化選項**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [資料壓縮](../../relational-databases/data-compression/data-compression.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: [檢視或變更資料庫的相容性層級](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)|  
|**資料表組態最佳化**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [分割資料表與索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)|  
|**Azure 虛擬機器中的 Database Engine 效能**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [快速檢查清單](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [虛擬機器大小和儲存體帳戶考量](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [磁碟和效能考量](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [I/O 效能考量](/aspnet/core/performance/performance-best-practices)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [功能特定效能考量](/aspnet/core/performance/performance-best-practices)| 
|**Linux 上 SQL Server 的效能最佳做法和設定方針**|:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [SQL Server 設定](../../linux/sql-server-linux-performance-best-practices.md#sql-server-configuration)<br />:::image type="icon" source="../../relational-databases/performance/media/security-center-sqlserver.png"::: [Linux OS 設定](../../linux/sql-server-linux-performance-best-practices.md#linux-os-configuration)|

> [!IMPORTANT]
> 您可以從以下內容取得其他考量事項：    
> -  [適用於具備高效能工作負載 SQL Server 2012 及 SQL Server 2014 的建議更新及設定選項](https://support.microsoft.com/help/2964518)
> -  [適用於具備高效能工作負載 SQL Server 2017 及 SQL Server 2016 的建議更新及設定選項](https://support.microsoft.com/help/4465518)

## <a name="query-performance-options"></a>查詢效能選項  
  
|||  
|-|-|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[索引](../../relational-databases/indexes/indexes.md)**|[重新組織與重建索引](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)<br />[指定索引的填滿因素](../../relational-databases/indexes/specify-fill-factor-for-an-index.md)<br />[設定平行索引作業](../../relational-databases/indexes/configure-parallel-index-operations.md)<br />[索引的 SORT_IN_TEMPDB 選項](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)<br />[改善全文檢索索引的效能](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)<br />[設定每筆查詢的最小記憶體數伺服器組態選項](../../database-engine/configure-windows/configure-the-min-memory-per-query-server-configuration-option.md)<br />[設定 index create memory 伺服器組態選項](../../database-engine/configure-windows/configure-the-index-create-memory-server-configuration-option.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[資料分割資料表與索引](../../relational-databases/partitions/partitioned-tables-and-indexes.md)**|[資料分割的優點](../partitions/partitioned-tables-and-indexes.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[聯結](../../relational-databases/performance/joins.md)**|[聯結基本概念](../../relational-databases/performance/joins.md#fundamentals)<br />[巢狀迴圈聯結](../../relational-databases/performance/joins.md#nested_loops)<br />[合併聯結](../../relational-databases/performance/joins.md#merge)<br />[雜湊聯結](../../relational-databases/performance/joins.md#hash)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[子查詢](../../relational-databases/performance/subqueries.md)**|[子查詢基本概念](../../relational-databases/performance/subqueries.md#fundamentals)<br />[相互關聯的子查詢](../../relational-databases/performance/subqueries.md#correlated)<br />[子查詢類型](../../relational-databases/performance/subqueries.md#types)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[預存程序](../stored-procedures/stored-procedures-database-engine.md)**|[CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md#best-practices)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[使用者定義函式](../user-defined-functions/user-defined-functions.md)**|[CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md#best-practices)<br />[建立使用者定義函式 &#40;資料庫引擎&#41;](../user-defined-functions/create-user-defined-functions-database-engine.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **平行處理原則最佳化**|[設定 max worker threads 伺服器組態選項](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)<br />[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **查詢最佳化工具最佳化**|[ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)<br />[USE HINT 查詢提示](../../t-sql/queries/hints-transact-sql-query.md#use_hint)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png"::: **[統計資料](../../relational-databases/statistics/statistics.md)**|[何時更新統計資料](../statistics/statistics.md)<br />[更新統計資料](../../relational-databases/statistics/update-statistics.md)|  
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::  **[記憶體內部 OLTP &#40;記憶體內部最佳化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)**|[記憶體最佳化資料表](../in-memory-oltp/sample-database-for-in-memory-oltp.md)<br />[原生編譯的預存程序](../in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md)<br />[從原生編譯預存程序建立及存取 TempDB 中的資料表](../../relational-databases/in-memory-oltp/create-and-access-tables-in-tempdb-from-stored-procedures.md)<br />[針對記憶體最佳化雜湊索引常見效能問題進行疑難排解](/previous-versions/sql/sql-server-2016/dn589805(v=sql.130))<br />[示範：記憶體內部 OLTP 的效能改善](../../relational-databases/in-memory-oltp/demonstration-performance-improvement-of-in-memory-oltp.md)|
|:::image type="icon" source="../../relational-databases/performance/media/security-center-both.png":::  **[智慧查詢處理](../../relational-databases/performance/intelligent-query-processing.md)**|[智慧查詢處理](../../relational-databases/performance/intelligent-query-processing.md)|
  
## <a name="see-also"></a>另請參閱  
 [效能的監視與微調](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [相關檢視、函數與程序](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [單一資料庫的 Azure SQL Database 效能指引](/azure/azure-sql/database/performance-guidance)   
 [使用彈性集區最佳化 Azure SQL Database 效能](/azure/azure-sql/database/elastic-pool-overview)   
 [Azure SQL Database 的查詢效能深入解析](/azure/azure-sql/database/query-performance-insight-use)  
 [索引設計指南](../../relational-databases/sql-server-index-design-guide.md)  
 [記憶體管理架構指南](../../relational-databases/memory-management-architecture-guide.md)  
 [分頁與範圍架構指南](../../relational-databases/pages-and-extents-architecture-guide.md)  
 [移轉後驗證和最佳化指南](../../relational-databases/post-migration-validation-and-optimization-guide.md)  
 [查詢處理架構指南](../../relational-databases/query-processing-architecture-guide.md)  
 [SQL Server 交易鎖定與資料列版本設定指南](../sql-server-transaction-locking-and-row-versioning-guide.md)  
 [SQL Server 交易記錄架構與管理指南](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)  
 [執行緒和工作架構指南](../../relational-databases/thread-and-task-architecture-guide.md) 
