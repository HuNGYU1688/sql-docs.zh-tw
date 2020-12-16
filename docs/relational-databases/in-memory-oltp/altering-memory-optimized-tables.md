---
title: 改變記憶體最佳化資料表 | Microsoft Docs
description: 了解 ALTER TABLE 陳述式會在記憶體最佳化資料表上執行結構描述與索引變更。 在單一陳述式中結合 ADD、DROP 與 ALTER 作業。
ms.custom: ''
ms.date: 10/22/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 690b70b7-5be1-4014-af97-54e531997839
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: da12d83efd43c7ca6113348adee8e3bde14d9472
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465469"
---
# <a name="altering-memory-optimized-tables"></a>改變記憶體最佳化資料表

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

使用 ALTER TABLE 陳述式，可以在記憶體最佳化資料表上執行結構描述與索引變更。 在 SQL Server 2016 和 Azure SQL Database 中，記憶體最佳化資料表的 ALTER TABLE 作業是「離線」，表示當作業正在進行時，資料表不提供查詢。 資料庫應用程式可以繼續執行，但會封鎖任何存取資料表的作業，直到改變程序完成。 單一的 ALTER TABLE 陳述式中可以合併多個 ADD、DROP 或 ALTER 作業。

> [!IMPORTANT]
> Azure SQL 受控執行個體不支援一般用途服務層級中經記憶體最佳化的資料表。
  
## <a name="alter-table"></a>ALTER TABLE  

ALTER TABLE 語法用於變更資料表的結構描述，以及加入、刪除和重建索引。 索引視為資料表定義的一部分︰  
  
- 語法 ALTER TABLE ...只有記憶體最佳化資料表支援 ADD/DROP/ALTER INDEX。  
  
- 在未使用 ALTER TABLE 陳述式的情況下，記憶體最佳化資料表上的索引就不支援 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)、[DROP INDEX](../../t-sql/statements/drop-index-transact-sql.md)、[ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md) 與 [PAD_INDEX](../../t-sql/statements/alter-table-index-option-transact-sql.md) 陳述式。  
  
支援下列類型變更：  
  
- 變更值區計數  
  
- 新增及移除索引  
  
- 變更、新增和移除資料行  
  
- 新增和移除條件約束  
  
 如需 ALTER TABLE 功能和完整語法的詳細資訊，請參閱 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
## <a name="schema-bound-dependency"></a>結構描述繫結的相依性

 原生編譯的預存程序必須為結構描述繫結，也就是說這些預存程序會有所存取之記憶體最佳化資料表，以及所參考之資料行的結構描述繫結相依性。 結構描述繫結的相依性是兩個實體間的關聯性，只要參考實體存在，就可以防止受參考的實體遭到卸除或以不相容的方式修改。  
  
 例如，如果結構描述繫結的原生編譯預存程序參考了資料表 *mytable* 中的資料行 *c1*，則無法卸除資料行 *c1* 。 同樣地，如果搭配 INSERT 陳述式的這類程序沒有資料行清單 (例如 `INSERT INTO dbo.mytable VALUES (...)`)，則無法卸除資料表中的資料行。  

## <a name="logging-of-alter-table-on-memory-optimized-tables"></a>記憶體最佳化資料表上的 ALTER TABLE 記錄

在記憶體最佳化資料表上，大部分的 ALTER TABLE 案例現在會以平行方式執行，並導致對交易記錄的寫入最佳化。 只有將中繼資料變更記錄到交易記錄檔才能達到最佳化。 不過，下列 ALTER TABLE 作業會以單一執行緒執行，而且不會進行記錄檔最佳化。

本例中的單一執行緒作業會將已改變的資料表完整內容記錄到交易記錄檔。 單一執行緒的作業清單如下︰

- 改變或新增資料行以使用大型物件 (LOB) 類型︰nvarchar(max)、varchar(max) 或 varbinary(max)。

- 加入或卸除資料行存放區索引。

- 幾乎是任何會影響 [off-row 資料行](../../relational-databases/in-memory-oltp/supported-data-types-for-in-memory-oltp.md)的項目。

  - 導致 on-row 資料行移到 off-row。
  - 導致 off-row 資料行移到 on-row。
  - 建立新的 off-row 資料行。
  - *例外狀況︰* 會以最佳化方式記錄使已經 off-row 的資料行變長的情況。
  
## <a name="examples"></a>範例

下例會變更現有雜湊索引的值區計數。 這會以新的值區計數重建雜湊索引，但雜湊索引的其他屬性保持不變。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem
       ALTER INDEX imPK_SalesOrderDetail_SalesOrderID_SalesOrderDetailID  
              REBUILD WITH (BUCKET_COUNT=67108864);  
GO
```

下例會加入含有 NOT NULL 條件約束和 DEFAULT 定義的資料行，並使用 WITH VALUES 為資料表中現有的每一資料列提供值。 如果沒有使用 WITH VALUES，每一個資料列的新資料行中都會有 NULL 值。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD Comment NVARCHAR(100) NOT NULL DEFAULT N'' WITH VALUES;  
GO
```

下例會將主索引鍵條件約束加入現有的資料行中。  

```sql
CREATE TABLE dbo.UserSession (
   SessionId int not null,
   UserId int not null,
   CreatedDate datetime2 not null,
   ShoppingCartId int,
   index ix_UserId nonclustered hash (UserId) with (bucket_count=400000)
)
WITH (MEMORY_OPTIMIZED=ON, DURABILITY=SCHEMA_ONLY) ;  
GO  
  
ALTER TABLE dbo.UserSession  
       ADD CONSTRAINT PK_UserSession PRIMARY KEY NONCLUSTERED (SessionId);  
GO
```

下例會移除索引。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       DROP INDEX ix_ModifiedDate;  
GO
```  

下例會加入索引。  

```sql  
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD INDEX ix_ModifiedDate (ModifiedDate);  
GO  
```  

下例會加入具有索引和條件約束的多個資料行。  

```sql
ALTER TABLE Sales.SalesOrderDetail_inmem  
       ADD    CustomerID int NOT NULL DEFAULT -1 WITH VALUES,  
              ShipMethodID int NOT NULL DEFAULT -1 WITH VALUES,  
              INDEX ix_Customer (CustomerID);  
GO  
```

<a name="logging-of-alter-table-on-memory-optimized-tables-124"></a>

## <a name="see-also"></a>另請參閱  

[記憶體最佳化資料表](./sample-database-for-in-memory-oltp.md)