---
title: 存取外部資料：MongoDB - PolyBase
description: 本文說明如何在 SQL Server 執行個體上使用 PolyBase 查詢位於 MongoDB 中的外部資料。 建立外部資料表以參考外部資料。
ms.date: 12/13/2019
ms.metadata: seo-lt-2019
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: fdbc2112467cadc2aaec390a667e3d189786b3ef
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467489"
---
# <a name="configure-polybase-to-access-external-data-in-mongodb"></a>設定 PolyBase 存取 MongoDB 中的外部資料

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

本文說明如何在 SQL Server 執行個體上使用 PolyBase 查詢位於 MongoDB 中的外部資料。

## <a name="prerequisites"></a>必要條件

如果您尚未安裝 PolyBase，請參閱 [PolyBase 安裝](polybase-installation.md)。

在建立資料庫範圍認證之前，必須建立[主要金鑰](../../t-sql/statements/create-master-key-transact-sql.md)。 
    

## <a name="configure-a-mongodb-external-data-source"></a>設定 MongoDB 外部資料來源

若要查詢來自 MongoDB 資料來源的資料，您必須建立外部資料表來參考外部資料。 本節提供建立這些外部資料表的範例程式碼。

本節中使用下列 Transact-SQL 命令：

- [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)
- [CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md) 
- [CREATE STATISTICS (Transact-SQL)](../../t-sql/statements/create-statistics-transact-sql.md)

1. 建立資料庫範圍認證以存取 MongoDB 資料來源。

    ```sql
    /*  specify credentials to external data source
    *  IDENTITY: user name for external source. 
    *  SECRET: password for external source.
    */
    CREATE DATABASE SCOPED CREDENTIAL credential_name WITH IDENTITY = 'username', Secret = 'password';
    ```
    
   > [!IMPORTANT] 
   > 適用於 PolyBase 的 MongoDB ODBC 連接器僅支援基本驗證，不支援 Kerberos 驗證。    
    
1. 使用 [CREATE EXTERNAL DATA SOURCE](../../t-sql/statements/create-external-data-source-transact-sql.md) 建立外部資料來源。

    ```sql
    /*  LOCATION: Location string should be of format '<type>://<server>[:<port>]'.
    *  PUSHDOWN: specify whether computation should be pushed down to the source. ON by default.
    *CONNECTION_OPTIONS: Specify driver location
    *  CREDENTIAL: the database scoped credential, created above.
    */
    CREATE EXTERNAL DATA SOURCE external_data_source_name
    WITH (LOCATION = 'mongodb://<server>[:<port>]',
    -- PUSHDOWN = ON | OFF,
    CREDENTIAL = credential_name);
    ```

1. **選擇性：** 在外部資料表上建立統計資料。

    我們建議在外部資料表資料行上建立統計資料 (尤其是用於聯結、篩選和彙總的資料行)，以取得最佳查詢效能。

    ```sql
    CREATE STATISTICS statistics_name ON customer (C_CUSTKEY) WITH FULLSCAN; 
    ```

>[!IMPORTANT] 
>當您建立外部資料來源之後，可以使用 [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md) 命令透過該來源建立可查詢的資料表。
>
>如需範例，請參閱[建立 MongoDB 的外部資料表](../../t-sql/statements/create-external-table-transact-sql.md#k-create-an-external-table-for-mongodb)。

## <a name="flattening"></a>壓平合併
針對來自 MongoDB 文件集合的巢狀及重複資料會啟用壓平合併。 使用者必須啟用 `create an external table`，並明確指定可能具有巢狀和/或重複資料之 MongoDB 文件集合的關聯式結構描述。 JSON 巢狀/重複資料類型會以下列方式壓平合併

* 物件：未排序索引鍵/值集合會包圍在大括弧中 (巢狀)

   - SQl Server 會為每個物件機碼建立資料表資料行

     * 資料行名稱：objectname_keyname

* 陣列：排序值，以逗號分隔，並包圍在方括弧內 (重複)

   - SQL Server 會為每個陣列項目新增新的資料表資料列

   - SQL Server 會為每個陣列建立資料行，以儲存陣列項目索引

     * 資料行名稱：arrayname_index

     * 資料類型：bigint

此技術有數個潛在問題，其中兩個是：

* 空白重複欄位會有效遮罩包含在相同記錄一般欄位中的資料

* 多個重複欄位可能會導致所產生資料列的數量暴增

例如，假設 SQL Server 評估一個儲存在非關聯式 JSON 格式中的 MongoDB 範例資料集餐廳集合。 每間餐廳都有巢狀地址欄位，以及在不同天所獲指派的等級。 下圖說明具有巢狀地址和巢狀重複等級的典型餐廳。

![MongoDB 壓平合併](../../relational-databases/polybase/media/mongo-flattening.png "MongoDB 餐廳壓平合併")

物件地址會以下列順序壓平合併：

* 巢狀欄位 restaurant.address.building becomes restaurant.address_building
* 巢狀欄位 restaurant.address.coord becomes restaurant.address_coord
* 巢狀欄位 restaurant.address.street becomes restaurant.address_street
* 巢狀欄位 restaurant.address.zipcode becomes restaurant.address_zipcode

陣列等級會以下列順序壓平合併：

| grades_date | grades_grade  | games_score | 
| ------------- | ------------------------- | -------------- |
|1393804800000 |A |2|
|1378857600000|A |6|
|135898560000 |A |10|
|1322006400000|A |9|
|1299715200000 |B |14|

## <a name="cosmos-db-connection"></a>Cosmos DB 連線

使用 Cosmos DB Mongo API 和 Mongo DB PolyBase 連接器時，您可以建立 **Cosmos DB 執行個體** 的外部資料表。 依照上面所列的相同步驟，即可完成此操作。 請確認資料庫範圍認證、伺服器位址、連接埠及位置字串皆反映 Cosmos DB 伺服器的對應設定。 

## <a name="next-steps"></a>後續步驟

若要深入了解 PolyBase，請參閱 [SQL Server PolyBase 概觀](polybase-guide.md)。
