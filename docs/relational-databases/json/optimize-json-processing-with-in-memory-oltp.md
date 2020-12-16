---
description: 使用記憶體內部 OLTP 最佳化 JSON 處理
title: 使用記憶體內部 OLTP 最佳化 JSON 處理
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
ms.assetid: d9c5adb1-3209-4186-bc10-8e41a26f5e57
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1854ba39c5131484079cccfd3f38e04743254106
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483400"
---
# <a name="optimize-json-processing-with-in-memory-oltp"></a>使用記憶體內部 OLTP 最佳化 JSON 處理
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2017-asdb.md)]

SQL Server 和 Azure SQL Database 可讓您使用格式化為 JSON 的文字。 若要提升處理 JSON 資料之查詢的效能，您可以使用標準字串資料行 (NVARCHAR 類型)，將 JSON 文件儲存到經記憶體最佳化的資料表中。 將 JSON 資料儲存在記憶體最佳化資料表時，透過利用無鎖定的記憶體內部資料存取，即可提高查詢效能。

## <a name="store-json-in-memory-optimized-tables"></a>將 JSON 儲存到記憶體最佳化資料表
下列範例示範具有 `Tags` 和 `Data` 這兩個 JSON 資料行的記憶體最佳化 `Product` 資料表。

```sql
CREATE SCHEMA xtp;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED, --standard column
    Name nvarchar(400) NOT NULL, --standard column
    Price float, --standard column

    Tags nvarchar(400),--json stored in string column
    Data nvarchar(4000) --json stored in string column

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="optimize-json-processing-with-additional-in-memory-features"></a>使用其他記憶體內部功能最佳化 JSON 處理
SQL Server 和 Azure SQL Database 中提供的功能可讓您完全整合 JSON 功能與現有記憶體內部 OLTP 技術。 例如，您可以執行下列動作：
 - 使用原生編譯 CHECK 條件約束，來[驗證經記憶體最佳化的資料表中所儲存之 JSON 文件的結構](#validate)。
 - 使用計算資料行來[公開 JSON 文件中所儲存的值，並將其設為強型別](#computedcol)。
 - 使用記憶體最佳化索引來[編製 JSON 文件中值的索引](#index)。
 - [原生編譯 SQL 查詢](#compile)，以使用 JSON 文件中的值，或將結果格式化為 JSON 文字。

## <a name="validate-json-columns"></a><a name="validate"></a> 驗證 JSON 資料行
SQL Server 和 Azure SQL Database 可讓您新增原生編譯 CHECK 條件約束，來驗證字串資料行中所儲存的 JSON 文件內容。 您可以使用原生編譯 JSON CHECK 條件約束，確保記憶體最佳化資料表中所儲存的 JSON 文字格式正確。

下列範例會建立一份含有 JSON 資料行 `Tags` 的 `Product` 資料表。 `Tags` 資料行具有 CHECK 條件約束，使用 `ISJSON` 函式來驗證資料行中的 JSON 文字。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Tags nvarchar(400)
            CONSTRAINT [Tags should be formatted as JSON]
                CHECK (ISJSON(Tags)=1),
    Data nvarchar(4000)

) WITH (MEMORY_OPTIMIZED=ON);
```

您也可以將原生編譯 CHECK 條件約束新增至包含 JSON 資料行的現有資料表。

```sql
ALTER TABLE xtp.Product
    ADD CONSTRAINT [Data should be JSON]
        CHECK (ISJSON(Data)=1)
```

## <a name="expose-json-values-using-computed-columns"></a><a name="computedcol"></a> 使用計算資料行公開 JSON 值
計算資料行可讓您公開 JSON 文字中的值，以及存取這些值，而不需要重新擷取 JSON 文字中的值，也不需要重新剖析 JSON 結構。 公開的值是強型別，並且會實際保存於計算資料行中。 使用保存的計算資料行存取 JSON 值，會比直接存取 JSON 文件中的值還要快。

下列範例示範如何公開下列來自 JSON `Data` 資料行的兩個值：
-   產品的製作國家/地區。
-   產品製造成本。

在此範例中，每次 `Data` 資料行中所儲存的 JSON 文件變更時，都會更新計算資料行 `MadeIn` 和 `Cost`。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float)

) WITH (MEMORY_OPTIMIZED=ON);
```

## <a name="index-values-in-json-columns"></a><a name="index"></a> 編製 JSON 資料行中值的索引
SQL Server 和 Azure SQL Database 可讓您使用記憶體最佳化索引來編製 JSON 資料行中值的索引。 必須公開要編製索引的 JSON 值，並使用計算資料行將其設為強型別，如以上範例所述。

您可以使用標準 NONCLUSTERED 和 HASH 索引來編製 JSON 資料行中值的索引。
-   NONCLUSTERED 索引最佳化查詢的方式是依據某個 JSON 值來選取某範圍的資料列，或依 JSON 值來排序結果。
-   HASH 索引最佳化查詢的方式是藉由指定要尋找的確切值來選取一或多個資料列。

下列範例會建立一個資料表，其使用兩個計算資料行來公開 JSON 值。 該範例會在一個 JSON 值上建立 NONCLUSTERED 索引，並在另一個值上建立 HASH 索引。

```sql
DROP TABLE IF EXISTS xtp.Product;
GO
CREATE TABLE xtp.Product(
    ProductID int PRIMARY KEY NONCLUSTERED,
    Name nvarchar(400) NOT NULL,
    Price float,

    Data nvarchar(4000),

    MadeIn AS CAST(JSON_VALUE(Data, '$.MadeIn') as NVARCHAR(50)) PERSISTED,
    Cost   AS CAST(JSON_VALUE(Data, '$.ManufacturingCost') as float) PERSISTED,

    INDEX [idx_Product_MadeIn] NONCLUSTERED (MadeIn)

) WITH (MEMORY_OPTIMIZED=ON)

ALTER TABLE Product
    ADD INDEX [idx_Product_Cost] NONCLUSTERED HASH(Cost)
        WITH (BUCKET_COUNT=20000)
```

## <a name="native-compilation-of-json-queries"></a><a name="compile"></a> JSON 查詢的原生編譯
如果您的程序、函式和觸發程序包含使用內建 JSON 函式的查詢，原生編譯可提升這些查詢的效能，並減少執行查詢所需的 CPU 週期數目。

下列範例示範使用下列數個 JSON 函式的原生編譯程序：**JSON_VALUE**、**OPENJSON** 和 **JSON_MODIFY**。

```sql
CREATE PROCEDURE xtp.ProductList(@ProductIds nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    SELECT ProductID,Name,Price,Data,Tags, JSON_VALUE(data,'$.MadeIn') AS MadeIn
    FROM xtp.Product
        JOIN OPENJSON(@ProductIds)
            ON ProductID = value
END;

CREATE PROCEDURE xtp.UpdateProductData(@ProductId int, @Property nvarchar(100), @Value nvarchar(100))
WITH SCHEMABINDING, NATIVE_COMPILATION
AS BEGIN
    ATOMIC WITH (transaction isolation level = snapshot,  language = N'English')

    UPDATE xtp.Product
    SET Data = JSON_MODIFY(Data, @Property, @Value)
    WHERE ProductID = @ProductId;
END
```

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>深入了解 SQL Server 和 Azure SQL Database 中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 影片

如需 SQL Server 和 Azure SQL Database 中內建 JSON 支援的觀看式簡介，請參閱下列影片：

-   [SQL Server 2016 和 JSON 支援](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [使用 SQL Server 2016 和 Azure SQL Database 中的 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [NoSQL 與關聯式領域之間的橋樑 JSON](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
