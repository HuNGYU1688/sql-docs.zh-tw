---
description: 設定索引選項
title: 設定索引選項 | Microsoft Docs
ms.custom: ''
ms.date: 06/26/2019
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- ALLOW_ROW_LOCKS option
- SORT_IN_TEMPDB option
- DROP_EXISTING clause
- large data, indexes
- PAD_INDEX
- STATISTICS_NORECOMPUTE
- MAXDOP index option, setting
- index options [SQL Server]
- MAXDOP index option
- IGNORE_DUP_KEY option
- ALLOW_PAGE_LOCKS option
- OPTIMIZE_FOR_SEQUENTIAL_KEY option
- ONLINE
ms.assetid: 7969af33-e94c-41f7-ab89-9d9a2747cd5c
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1803a13e13f75bbb3cf38cbcd841fa7e2dae1508
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478189"
---
# <a name="set-index-options"></a>設定索引選項

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此主題描述如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中修改索引的屬性。

 **本文內容**

- **開始之前：**

   [限制事項](#Restrictions)

   [安全性](#Security)

- **使用下列方法修改索引的屬性：**

   [Transact-SQL](#SSMSProcedure)

   [Transact-SQL](#TsqlProcedure)

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制事項

- 透過使用 ALTER INDEX 陳述式的 SET 子句，會在索引立即套用下列選項：ALLOW_PAGE_LOCKS、ALLOW_ROW_LOCKS、OPTIMIZE_FOR_SEQUENTIAL_KEY、IGNORE_DUP_KEY 和 STATISTICS_NORECOMPUTE。
- 當您使用 ALTER INDEX REBUILD 或 CREATE INDEX WITH DROP_EXISTING 重建索引時，可以設定下列選項：PAD_INDEX、FILLFACTOR、SORT_IN_TEMPDB、IGNORE_DUP_KEY、STATISTICS_NORECOMPUTE、ONLINE、ALLOW_ROW_LOCKS、ALLOW_PAGE_LOCKS、MAXDOP 和 DROP_EXISTING (僅限 CREATE INDEX)。

### <a name="security"></a><a name="Security"></a> Security

#### <a name="permissions"></a><a name="Permissions"></a> 權限

需要資料表或檢視表的 ALTER 權限。

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio

### <a name="to-modify-the-properties-of-an-index-in-table-designer"></a>在資料表設計工具中修改索引的屬性

1. 在 [物件總管] 中，按一下加號展開資料庫，此資料庫包含您要修改索引屬性的資料表。
2. 按一下加號展開 **[資料表]** 資料夾。
3. 以滑鼠右鍵按一下要修改索引屬性的資料表，然後選取 [設計]。
4. 在 [資料表設計工具] 功能表上，按一下 [索引/索引鍵]。
5. 選取您要修改的索引。 其屬性會在主要方格中顯示。
6. 變更任何和所有屬性的設定，以自訂索引。
7. 按一下 [關閉] 。
8. 在 [檔案] 功能表上，選取 [儲存 _table_name_]。

### <a name="to-modify-the-properties-of-an-index-in-object-explorer"></a>在物件總管中修改索引的屬性

1. 在 [物件總管] 中，按一下加號展開資料庫，此資料庫包含您要修改索引屬性的資料表。
2. 按一下加號展開 **[資料表]** 資料夾。
3. 按一下加號展開要修改索引屬性的資料表。
4. 按一下加號展開 **[索引]** 資料夾。
5. 以滑鼠右鍵按一下要修改其屬性的索引，然後選取 [屬性]。
6. 在 **[選取頁面]** 底下，選取 **[選項]** 。
7. 變更任何和所有屬性的設定，以自訂索引。
8. 若要新增、移除或變更索引資料行的位置，請選取 [索引屬性 - _index_name_] 對話方塊中的 [一般] 頁面。 如需相關資訊，請參閱 [Index Properties F1 Help](../../relational-databases/indexes/index-properties-f1-help.md)

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL

### <a name="to-see-the-properties-of-all-the-indexes-in-a-table"></a>查看資料表中所有索引的屬性

下列範例會顯示 AdventureWorks 資料庫的某個資料表中所有索引的屬性。

```sql
SELECT i.name AS index_name
   , i.type_desc
   , i.is_unique
   , ds.type_desc AS filegroup_or_partition_scheme
   , ds.name AS filegroup_or_partition_scheme_name
   , i.ignore_dup_key
   , i.is_primary_key
   , i.is_unique_constraint
   , i.fill_factor
   , i.is_padded
   , i.is_disabled
   , i.allow_row_locks
   , i.allow_page_locks
   , i.has_filter
   , i.filter_definition
FROM sys.indexes AS i
   INNER JOIN sys.data_spaces AS ds
      ON i.data_space_id = ds.data_space_id
   WHERE is_hypothetical = 0 AND i.index_id <> 0
       AND i.object_id = OBJECT_ID('HumanResources.Employee')
;
```

#### <a name="to-set-the-properties-of-an-index"></a>設定索引的屬性

下列範例會設定 AdventureWorks 資料庫中索引的屬性。

[!code-sql[IndexDDL#AlterIndex4](../../relational-databases/indexes/codesnippet/tsql/set-index-options_1.sql)]

[!code-sql[IndexDDL#AlterIndex2](../../relational-databases/indexes/codesnippet/tsql/set-index-options_2.sql)]

如需詳細資訊，請參閱 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)。
