---
description: 變更系統建立版本時態表的結構描述
title: 變更系統設定版本時態表的結構描述 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 9dbe5a21-9335-4f8b-85fd-9da83df79946
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4b57082f1ce4f76e191c0237e80f404199a9ac4a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462639"
---
# <a name="changing-the-schema-of-a-system-versioned-temporal-table"></a>變更系統建立版本時態表的結構描述


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

使用 **ALTER TABLE** 陳述式加入、改變或移除資料行。

## <a name="examples"></a>範例

以下是變更時態表結構描述的一些範例。

```sql
ALTER TABLE dbo.Department
    ALTER COLUMN DeptName varchar(100);

ALTER TABLE dbo.Department
    ADD WebAddress nvarchar(255) NOT NULL
    CONSTRAINT DF_WebAddress DEFAULT 'www.mycompany.com';

ALTER TABLE dbo.Department
    ADD TempColumn INT;

GO

ALTER TABLE dbo.Department
    DROP COLUMN TempColumn;

/* Setting IsHidden property for period columns.
Use ALTER COLUMN <period_column> DROP HIDDEN to clear IsHidden flag */

ALTER TABLE dbo.Department
    ALTER COLUMN SysStartTime ADD HIDDEN;

ALTER TABLE dbo.Department
    ALTER COLUMN SysEndTime ADD HIDDEN;
```

### <a name="important-remarks"></a>重要備註

- 變更時態表結構描述所需的目前和記錄資料表的 **CONTROL** 權限。
- 在 **ALTER TABLE** 作業期間，系統會保留這兩個資料表的結構描述鎖定。
- 指定的結構描述變更會以適當的方式 (視變更的類型而定) 傳播至記錄資料表。
- 如果您加入不可為 Null 資料行，或改變現有資料行使其成為不可為 Null，則必須指定現有資料列的預設值。 系統會產生具有相同值的其他預設值，並將它套用到記錄資料表。 將 **DEFAULT** 加入非空白資料表，是所有版本的資料作業大小 (為中繼資料作業的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition 除外)。
- 加入 varchar(max)、nvarchar(max)、varbinary(max) 或含有預設值的 XML 資料行，將是所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本的更新資料作業。
- 如果加入資料行之後的資料列大小超過資料列大小限制，則無法線上加入新的資料行。
- 使用新的 NOT NULL 資料行擴充資料表之後，請考慮卸除記錄資料表的預設條件約束，因為系統會自動填入該資料表中的所有資料行。
- 若為系統版本設定時態表，ONLINE 選項 (**WITH (ONLINE = ON**) 對 **ALTER TABLE ALTER COLUMN** 無效。 無論針對 ONLINE 選項指定何值，都不會以線上方式執行 ALTER 資料行。
- 您可以使用 **ALTER COLUMN** 變更期間資料行的 **IsHidden** 屬性。
- 您不能使用直接 **ALTER** 進行下列結構描述變更。 針對這些類型的變更，請設定 **SYSTEM_VERSIONING = OFF**。

  - 加入計算資料行
  - 加入 **IDENTITY** 資料行
  - 在記錄資料表設定為 **DATA_COMPRESSION = PAGE** 或 **DATA_COMPRESSION = ROW**(記錄資料表的預設值) 時加入 **SPARSE** 資料行或將現有資料行變更為 **SPARSE**。
  - 加入 **COLUMN_SET**
  - 加入 **ROWGUIDCOL** 資料行或將現有資料行變更為 **ROWGUIDCOL**

下列範例示範如何變更仍需要設定 **SYSTEM_VERSIONING = OFF** 的結構描述 (加入 **IDENTITY** 資料行)。 此範例會停用資料一致性檢查。 因為不能發生任何並行資料變更，所以在交易內進行結構描述變更時不需要這項檢查。

```sql
    BEGIN TRAN
        ALTER TABLE [dbo].[CompanyLocation] SET (SYSTEM_VERSIONING = OFF);
        ALTER TABLE [CompanyLocation] ADD Cntr INT IDENTITY (1,1);
        ALTER TABLE [dbo].[CompanyLocationHistory] ADD Cntr INT NOT NULL DEFAULT 0;
        ALTER TABLE [dbo].[CompanyLocation]
    SET
         (
            SYSTEM_VERSIONING = ON
           ( HISTORY_TABLE = [dbo].[CompanyLocationHistory])
         );
    COMMIT ;
```

## <a name="next-steps"></a>後續步驟

- [時態表](../../relational-databases/tables/temporal-tables.md)
 [開始使用系統建立版本的時態表](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [管理系統設定版本時態表中的歷程記錄資料保留](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系統版本設定時態表與記憶體最佳化資料表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
- [建立系統設定版本時態表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [修改系統建立版本時態表中的資料](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [查詢系統建立版本時態表中的資料](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [停止系統設定版本時態表上的系統版本設定功能](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)
