---
description: 從 JSON 輸出移除方括弧 - WITHOUT_ARRAY_WRAPPER 選項
title: 從 JSON 輸出移除方括弧 - WITHOUT_ARRAY_WRAPPER 選項
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- WITHOUT_ARRAY_WRAPPER
ms.assetid: aa86c2d1-458e-465f-abfa-75470137d054
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f4580a7c3a566ba14f5cf20e177760751a451791
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88490895"
---
# <a name="remove-square-brackets-from-json---without_array_wrapper-option"></a>從 JSON 輸出移除方括弧 - WITHOUT_ARRAY_WRAPPER 選項
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

若要移除預設圍住 **FOR JSON** 子句之 JSON 輸出的方括弧，請指定 **WITHOUT_ARRAY_WRAPPER** 選項。 將此選項用於單一資料列結果，以產生單一 JSON 物件作為輸出，而不是內含單一元素的陣列。

如果您將此選項用於多個資料列結果，由於有多個元素且遺漏方括號，因此產生的輸出不是有效的 JSON。  
  
## <a name="example-single-row-result"></a>範例 (單一資料列結果)  
下列範例顯示使用或不使用 **WITHOUT_ARRAY_WRAPPER** 選項的 **FOR JSON** 子句輸出。  
  
 **查詢**  
  
```sql  
SELECT 2015 as year, 12 as month, 15 as day  
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER 
```  

 使用 **結果** with the **結果**  
  
```json  
{
    "year": 2015,
    "month": 12,
    "day": 15
} 
```  
  
 不使用 **WITHOUT_ARRAY_WRAPPER** 選項的 **結果** (預設值)  
  
```json  
[{
    "year": 2015,
    "month": 12,
    "day": 15
}]
```  

## <a name="example-multiple-row-result"></a>範例 (多個資料列結果)
以下是使用 **FOR JSON** 選項的 **WITHOUT_ARRAY_WRAPPER** 選項。 這個範例會產生多個資料列結果。 由於有多個元素且遺漏方括號，因此輸出不是有效的 JSON。
  
 **查詢**  
  
```sql  
SELECT TOP 3 SalesOrderNumber, OrderDate, Status  
FROM Sales.SalesOrderHeader  
ORDER BY ModifiedDate  
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER 
```  
  
 使用 **結果** with the **結果**  
  
```json  
{
    "SalesOrderNumber": "SO43662",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43661",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43660",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
} 
```  
  
 不使用 **WITHOUT_ARRAY_WRAPPER** 選項的 **結果** (預設值)  
  
```json  
[{
    "SalesOrderNumber": "SO43662",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43661",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}, {
    "SalesOrderNumber": "SO43660",
    "OrderDate": "2011-05-31T00:00:00",
    "Status": 5
}]
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>深入了解 SQL Server 和 Azure SQL Database 中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 影片

如需 SQL Server 和 Azure SQL Database 中內建 JSON 支援的觀看式簡介，請參閱下列影片：

-   [SQL Server 2016 和 JSON 支援](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [使用 SQL Server 2016 和 Azure SQL Database 中的 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [NoSQL 與關聯式領域之間的橋樑 JSON](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
  
## <a name="see-also"></a>另請參閱  
 [FOR 子句 &#40;Transact-SQL&#41;](../../t-sql/queries/select-for-clause-transact-sql.md)  
  
  
