---
title: 別名
description: Azure Synapse Analytics 和平行處理資料倉儲的別名。
titleSuffix: Azure Synapse Analytics
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
ms.assetid: 7b3a5c74-05cf-4385-8ee6-6176d003cb8a
author: shkale-msft
ms.author: shkale
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: a70959afb4e61f2049e19cfd1de96f0434abcb09
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476669"
---
# <a name="aliasing-azure-synapse-analytics-parallel-data-warehouse"></a>別名 (Azure Synapse Analytics、平行處理資料倉儲)

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

別名功能可允許暫時以簡短且易記的字串取代 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]或[!INCLUDE[ssPDW](../../includes/sspdw-md.md)][!INCLUDE[DWsql](../../includes/dwsql-md.md)] 查詢中的資料表或資料行名稱。 JOIN 查詢中經常使用資料表別名，因為在參考資料行時，JOIN 語法需要完整的物件名稱。  

別名必須是符合物件命名規則的單字。 如需詳細資訊，請參閱[!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]中的＜Object Naming Rules＞(物件命名規則)。 物件不可包含空格，且不可以單引號或雙引號括住。  

## <a name="syntax"></a>語法

```tsql
object_source [ AS ] alias
```

## <a name="arguments"></a>引數

*object_source*  
來源資料表或資料行的名稱。  

AS  
選擇性的別名前置詞。 使用範圍變數別名功能時，禁止使用 AS 關鍵字。  

*alias* 所需的資料表或資料行暫時參考名稱。 可以使用任何有效的物件名稱。 如需詳細資訊，請參閱[!INCLUDE[pdw-product-documentation](../../includes/pdw-product-documentation-md.md)]中的＜Object Naming Rules＞(物件命名規則)。  

## <a name="examples-sssdw-and-sspdw"></a>範例：[!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

下列範例示範含有多個聯結的查詢。 此範例同時示範資料表和資料行別名功能。  

- 資料行別名功能：在此範例中，資料行及涉及選取清單中資料行的運算式都採用別名。 `SalesTerritoryRegion AS SalesTR` 示範一個簡單的資料行別名。 `Sum(SalesAmountQuota) AS TotalSales` 示範  

- 資料表別名功能：`dbo.DimSalesTerritory AS st` 示範如何為 `dbo.DimSalesTerritory` 資料表建立 `st` 別名。  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) AS TotalSales, SalesTerritoryRegion AS SalesTR,  
    RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) AS RankResult  
FROM dbo.DimEmployee AS e  
INNER JOIN dbo.FactSalesQuota AS sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

如下所示，您可以不包含 AS 關鍵字，但通常會包含此關鍵字以便於閱讀。  

```tsql
-- Uses AdventureWorks

SELECT LastName, SUM(SalesAmountQuota) TotalSales, SalesTerritoryRegion SalesTR,  
RANK() OVER (PARTITION BY SalesTerritoryRegion ORDER BY SUM(SalesAmountQuota) DESC ) RankResult  
FROM dbo.DimEmployee e  
INNER JOIN dbo.FactSalesQuota sq ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory st ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE SalesPersonFlag = 1 AND SalesTerritoryRegion != N'NA'  
GROUP BY LastName, SalesTerritoryRegion;  
```

## <a name="next-steps"></a>後續步驟

- [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)
- [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)
- [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)