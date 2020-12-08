---
title: DMV - 檢視表的使用方式統計資料和效能
description: 了解如何使用動態管理檢視 (DMV) sys.dm_exec_query_optimizer_info、sys.views 與 sys.dmv_exec_cached_plans 來取得 SQL 查詢效能統計資料。
ms.custom: seo-dt-2019
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.date: 09/27/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.openlocfilehash: d900786f27280eb66ded8801843e9c657f2ceb5f
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504902"
---
# <a name="use-dmvs-to-determine-usage-statistics-and-performance-of-views"></a>使用 DMV 來判斷檢視表的使用方式統計資料和效能
本文涵蓋用來取得 **使用檢視表的查詢效能** 相關資訊的方法和指令碼。 這些指令碼的目的是提供資料庫內找到之各種檢視表的使用和效能指示器。 

## <a name="sysdm_exec_query_optimizer_info"></a>sys.dm_exec_query_optimizer_info
DMV [sys.dm_exec_query_optimizer_info](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-optimizer-info-transact-sql.md) 會公開 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 查詢最佳化工具所執行的最佳化統計資料。 這些值會累計，並且在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 啟動時開始錄製。 如需查詢最佳化工具的詳細資訊，請參閱[查詢處理架構指南](../../relational-databases/query-processing-architecture-guide.md)。   

下面 common_table_expression (CTE) 使用此 DMV 提供工作負載的資訊 (例如參考檢視的查詢百分比)。 此查詢所傳回的結果不表示本身有效能問題，但可以在結合使用者對緩慢執行查詢的抱怨時公開基礎問題。 

```sql
WITH CTE_QO AS
(
  SELECT
    occurrence
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    ([counter] = 'optimizations')
),
QOInfo AS
(
  SELECT
    [counter]
    ,[%] = CAST((occurrence * 100.00)/(SELECT occurrence FROM CTE_QO) AS DECIMAL(5, 2))
  FROM
    sys.dm_exec_query_optimizer_info
  WHERE
    [counter] IN ('optimizations'
                  ,'trivial plan'
                  ,'no plan'
                  ,'search 0'
                  ,'search 1'
                  ,'search 2'
                  ,'timeout'
                  ,'memory limit exceeded'
                  ,'insert stmt'
                  ,'delete stmt'
                  ,'update stmt'
                  ,'merge stmt'
                  ,'contains subquery'
                  ,'view reference'
                  ,'remote query'
                  ,'dynamic cursor request'
                  ,'fast forward cursor request'
             )
)
SELECT
  [optimizations] AS [optimizations %]
  ,[trivial plan] AS [trivial plan %]
  ,[no plan] AS [no plan %]
  ,[search 0] AS [search 0 %]
  ,[search 1] AS [search 1 %]
  ,[search 2] AS [search 2 %]
  ,[timeout] AS [timeout %]
  ,[memory limit exceeded] AS [memory limit exceeded %]
  ,[insert stmt] AS [insert stmt %]
  ,[delete stmt] AS [delete stmt]
  ,[update stmt] AS [update stmt]
  ,[merge stmt] AS [merge stmt]
  ,[contains subquery] AS [contains subquery %]
  ,[view reference] AS [view reference %]
  ,[remote query] AS [remote query %]
  ,[dynamic cursor request] AS [dynamic cursor request %]
  ,[fast forward cursor request] AS [fast forward cursor request %]
FROM
  QOInfo
PIVOT (MAX([%]) FOR [counter] 
  IN ([optimizations]
      ,[trivial plan]
      ,[no plan]
      ,[search 0]
      ,[search 1]
      ,[search 2]
      ,[timeout]
      ,[memory limit exceeded]
      ,[insert stmt]
      ,[delete stmt]
      ,[update stmt]
      ,[merge stmt]
      ,[contains subquery]
      ,[view reference]
      ,[remote query]
      ,[dynamic cursor request]
      ,[fast forward cursor request])) AS p;
GO
```

結合此查詢的結果與系統檢視表 [sys.views](../../relational-databases/system-catalog-views/sys-views-transact-sql.md) 的結果，來識別查詢統計資料、查詢文字和快取的執行計畫。 

## <a name="sysviews"></a>sys.views
下面 CTE 提供執行次數、總執行時間以及從記憶體讀取的頁面資訊。 結果可以用來識別可能為最佳化候選項目的查詢。 
  
> [!NOTE]
> 此查詢的結果會根據 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 版本而不同。  


```sql
WITH CTE_VW_STATS AS
(
  SELECT
    SCHEMA_NAME(vw.schema_id) AS schemaname
    ,vw.name AS viewname
    ,vw.object_id AS viewid
  FROM
    sys.views AS vw
  WHERE
    (vw.is_ms_shipped = 0)
  INTERSECT
  SELECT
    SCHEMA_NAME(o.schema_id) AS schemaname
    ,o.Name AS name
    ,st.objectid AS viewid
  FROM
    sys.dm_exec_cached_plans cp
  CROSS APPLY
    sys.dm_exec_sql_text(cp.plan_handle) st
  INNER JOIN
    sys.objects o ON st.[objectid] = o.[object_id]
  WHERE
    st.dbid = DB_ID()
)
SELECT
  vw.schemaname
  ,vw.viewname
  ,vw.viewid
  ,DB_NAME(t.databaseid) AS databasename
  ,t.databaseid
  ,t.*
FROM
  CTE_VW_STATS AS vw
CROSS APPLY
  (
    SELECT
      st.dbid AS databaseid
      ,st.text
      ,qp.query_plan
      ,qs.*
    FROM
      sys.dm_exec_query_stats AS qs
    CROSS APPLY
      sys.dm_exec_sql_text(qs.plan_handle) AS st
    CROSS APPLY
      sys.dm_exec_query_plan(qs.plan_handle) AS qp
    WHERE
      (CHARINDEX(vw.schemaname, st.text, 1) > 0)
      AND (st.dbid = DB_ID())
  ) AS t;
GO
```

## <a name="sysdmv_exec_cached_plans"></a>sys.dmv_exec_cached_plans
最終查詢使用 DMV [sys.dmv_exec_cached_plans](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cached-plans-transact-sql.md) 來提供未使用檢視表的資訊。 不過，執行計畫快取是動態的，而且結果可能會不同。 因此，使用此查詢一段時間，來判斷是否實際使用檢視表。 

```sql
SELECT
  SCHEMA_NAME(vw.schema_id) AS schemaname
  ,vw.name AS name
  ,vw.object_id AS viewid
FROM
  sys.views AS vw
WHERE
  (vw.is_ms_shipped = 0)
EXCEPT
SELECT
  SCHEMA_NAME(o.schema_id) AS schemaname
  ,o.name AS name
  ,st.objectid AS viewid
FROM
  sys.dm_exec_cached_plans cp
CROSS APPLY
  sys.dm_exec_sql_text(cp.plan_handle) st
INNER JOIN
  sys.objects o ON st.[objectid] = o.[object_id]
WHERE
  st.dbid = DB_ID();
GO
```

## <a name="see-also"></a>另請參閱
[動態管理檢視和函式](../../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) 
