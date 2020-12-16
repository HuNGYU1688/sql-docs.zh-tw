---
description: 時態表系統一致性檢查
title: 時態表系統一致性檢查 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: ec081d42-57e4-43c7-9e1c-317ba8f23437
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 31aaceb646bf735731317177f579df638da1b5c9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97426576"
---
# <a name="temporal-table-system-consistency-checks"></a>時態表系統一致性檢查


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


使用時態表時，系統會執行多項一致性檢查，以確保結構描述符合 Temporal 需求，資料一致且保持一致。 此外，Temporal 檢查已加入 **DBCC CHECKCONSTRAINTS** 陳述式中。

## <a name="system-consistency-checks"></a>系統一致性檢查

在 **SYSTEM_VERSIONING** 設為 **ON** 之前，會在歷程記錄資料表和目前的資料表上執行一組檢查。 這些檢查會集結到結構描述檢查和資料檢查中 (如果記錄資料表不是空的)。 此外，系統也會執行執行階段一致性檢查。

### <a name="schema-check"></a>結構描述檢查

當建立或改變資料表使成為時態表時，系統會確認是否符合需求︰

1. 目前的資料表和記錄資料表中的資料行名稱和數目是相同的。
2. 目前的資料表和記錄資料表之間每個資料行的資料類型都相符。
3. 期間資料行設為 **NOT NULL**。
4. 目前的資料表有主索引鍵條件約束，記錄資料表沒有主索引鍵條件約束。
5. 記錄資料表中未定義任何 **IDENTITY** 資料行。
6. 記錄資料表中未定義任何觸發程序。
7. 記錄資料表中未定義任何外部索引鍵。
8. 記錄資料表中未定義任何資料表或資料行條件約束。 不過，記錄資料表上允許預設的資料行值。
9. 記錄資料表不是放在唯讀檔案群組中。
10. 記錄資料表不會設定處理變更追蹤或異動資料擷取。

### <a name="data-consistency-check"></a>資料一致性檢查

在 **SYSTEM_VERSIONING** 設為 **ON** 並成為任何 DML 作業的一部分之前，系統會執行下列檢查︰ **SysEndTime** ≥**SysStartTime**

建立現有記錄資料表的連結時，您可以選擇執行資料一致性檢查。 這項資料一致性檢查可確保現有的記錄沒有重疊，而且每一筆個別記錄都達到暫時需求。 預設執行資料一致性檢查。 一般而言，每當目前和記錄資料表之間的資料可能不同步時，即建議執行資料一致性作業，如納入已填入歷程記錄資料的現有記錄資料表時。

> [!WARNING]
> 手動變更系統時鐘會導致系統意外失敗，因為正在進行的避免重疊條件 (也就是記錄的結束時間不得早於其開始時間) 執行階段資料一致性檢查會失敗。

## <a name="dbcc-checkconstraints"></a>DBCC CHECKCONSTRAINTS

**DBCC CHECKCONSTRAINTS** 命令包含暫時資料一致性檢查。 如需詳細資訊，請參閱 [DBCC CHECKCONSTRAINTS &#40;TRANSACT-SQL&#41;](../../t-sql/database-console-commands/dbcc-checkconstraints-transact-sql.md)。

## <a name="next-steps"></a>後續步驟

- [時態表](../../relational-databases/tables/temporal-tables.md)
- [開始使用系統建立版本的時態表](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [對時態表進行資料分割](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [時態表考量與限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [時態表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理系統設定版本時態表中的歷程記錄資料保留](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系統版本設定時態表與記憶體最佳化資料表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [暫存資料表中繼資料檢視和函數](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)  
