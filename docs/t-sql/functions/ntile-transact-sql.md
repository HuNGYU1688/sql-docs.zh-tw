---
description: NTILE (Transact-SQL)
title: NTILE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NTILE_TSQL
- NTILE
dev_langs:
- TSQL
helpviewer_keywords:
- distributing rows
- groups [SQL Server], row distribution
- row distribution [SQL Server]
- NTILE function
ms.assetid: 1c364511-d72a-4789-8efa-3cf2a1f6b791
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 140ec1e1d023612cc7f5e8812c55f6d6b0916fee
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472049"
---
# <a name="ntile-transact-sql"></a>NTILE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  將排序分割區中的資料列散發到指定數目的群組中。 這些群組從 1 開始編號。 對於每個資料列，NTILE 都會傳回資料列所屬群組的號碼。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
NTILE (integer_expression) OVER ( [ <partition_by_clause> ] < order_by_clause > )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *integer_expression*  
 用於指定每個分割區必須劃分之群組數的正整數運算式。 *integer_expression* 可為 **int** 類型或 **bigint**。  
  
 \<partition_by_clause>  
 將 [FROM](../../t-sql/queries/from-transact-sql.md) 子句產生的結果集分割成函式所要套用的分割區。 如需 PARTITION BY 的語法，請參閱 [OVER 子句 &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md)。  
  
 \<order_by_clause>  
 指定 NTILE 值指派給分割區中之資料列的順序。 在次序函數中使用 \<order_by_clause> 時，整數無法代表資料行。  
  
## <a name="return-types"></a>傳回型別  
 **bigint**  
  
## <a name="remarks"></a>備註  
 如果 *integer_expression* 無法整除分割區中的資料列數，兩個大小的群組會相差一個成員。 在 OVER 子句所指定的順序中，較大群組會在較小群組的前面。 例如，如果資料列總數是 53，群組數目是 5，前三個群組會有 11 個資料列，後兩個群組會有 10 個資料列。 如果群組數目可以整除資料列的總數，資料列就會平均分散在各個群組中。 例如，如果資料列總數是 50，有 5 個群組，每個貯體都會包含 10 個資料列。  
  
 NTILE 不具決定性。 如需詳細資訊，請參閱 [決定性與非決定性函數](../../relational-databases/user-defined-functions/deterministic-and-nondeterministic-functions.md)。  
  
## <a name="examples"></a>範例  
  
### <a name="a-dividing-rows-into-groups"></a>A. 將資料列分割成數個群組  
 在下列範例中，根據員工年初至今的銷售收益，將資料列歸類為四組員工。 由於群組數目無法整除資料列的總數，前兩組有四個資料列，其他組各有三個資料列。  
  
```sql  
USE AdventureWorks2012;   
GO  
SELECT p.FirstName, p.LastName  
    ,NTILE(4) OVER(ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    , a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
  
FirstName      LastName              Quartile  SalesYTD       PostalCode  
-------------  --------------------- --------- -------------- ----------  
Linda          Mitchell              1         4,251,368.55   98027  
Jae            Pak                   1         4,116,871.23   98055  
Michael        Blythe                1         3,763,178.18   98027  
Jillian        Carson                1         3,189,418.37   98027  
Ranjit         Varkey Chudukatil     2         3,121,616.32   98055  
José           Saraiva               2         2,604,540.72   98055  
Shu            Ito                   2         2,458,535.62   98055  
Tsvi           Reiter                2         2,315,185.61   98027  
Rachel         Valdez                3         1,827,066.71   98055  
Tete           Mensa-Annan           3         1,576,562.20   98055  
David          Campbell              3         1,573,012.94   98055  
Garrett        Vargas                4         1,453,719.47   98027  
Lynn           Tsoflias              4         1,421,810.92   98055  
Pamela         Ansman-Wolfe          4         1,352,577.13   98027  

(14 row(s) affected)  
```  
  
### <a name="b-dividing-the-result-set-by-using-partition-by"></a>B. 使用 PARTITION BY 分割結果集  
 下列範例會在範例 A 的程式碼中加入 `PARTITION BY` 引數。資料列會先由 `PostalCode` 進行資料分割，接著再依據每個 `PostalCode` 分成四個群組。 此範例也會宣告 `@NTILE_Var` 變數，並且使用該變數來指定 *integer_expression* 參數的值。  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @NTILE_Var INT = 4;  
  
SELECT p.FirstName, p.LastName  
    ,NTILE(@NTILE_Var) OVER(PARTITION BY PostalCode ORDER BY SalesYTD DESC) AS Quartile  
    ,CONVERT(NVARCHAR(20),s.SalesYTD,1) AS SalesYTD  
    ,a.PostalCode  
FROM Sales.SalesPerson AS s   
INNER JOIN Person.Person AS p   
    ON s.BusinessEntityID = p.BusinessEntityID  
INNER JOIN Person.Address AS a   
    ON a.AddressID = p.BusinessEntityID  
WHERE TerritoryID IS NOT NULL   
    AND SalesYTD <> 0;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
FirstName    LastName             Quartile SalesYTD      PostalCode  
------------ -------------------- -------- ------------  ----------  
Linda        Mitchell             1        4,251,368.55  98027  
Michael      Blythe               1        3,763,178.18  98027  
Jillian      Carson               2        3,189,418.37  98027  
Tsvi         Reiter               2        2,315,185.61  98027  
Garrett      Vargas               3        1,453,719.47  98027  
Pamela       Ansman-Wolfe         4        1,352,577.13  98027  
Jae          Pak                  1        4,116,871.23  98055  
Ranjit       Varkey Chudukatil    1        3,121,616.32  98055  
José         Saraiva              2        2,604,540.72  98055  
Shu          Ito                  2        2,458,535.62  98055  
Rachel       Valdez               3        1,827,066.71  98055  
Tete         Mensa-Annan          3        1,576,562.20  98055  
David        Campbell             4        1,573,012.94  98055  
Lynn         Tsoflias             4        1,421,810.92  98055  
  
(14 row(s) affected)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>範例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-dividing-rows-into-groups"></a>C. 將資料列分割成數個群組  
 下列範例會根據 2003 年他們受指派的銷售配額，使用 NTILE 函式將一組銷售人員分割成四個群組。 由於群組數目無法整除資料列的總數，所以第一個群組會有五個資料列，其餘群組會有四個資料列。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(4) OVER(ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
WHERE sq.CalendarYear = 2003  
    AND SalesTerritoryKey IS NOT NULL AND SalesAmountQuota <> 0  
GROUP BY e.LastName  
ORDER BY Quartile, e.LastName;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD  
----------------- -------- ------------`  
Blythe            1        4,716,000.00  
Carson            1        4,350,000.00  
Mitchell          1        4,682,000.00  
Pak               1        5,142,000.00  
Varkey Chudukatil 1        2,940,000.00  
Ito               2        2,644,000.00  
Saraiva           2        2,293,000.00  
Vargas            2        1,617,000.00  
Ansman-Wolfe      3        1,183,000.00  
Campbell          3        1,438,000.00  
Mensa-Annan       3        1,481,000.00  
Valdez            3        1,294,000.00  
Abbas             4          172,000.00  
Albert            4          651,000.00  
Jiang             4          544,000.00  
Tsoflias          4          867,000.00
```  
  
### <a name="d-dividing-the-result-set-by-using-partition-by"></a>D. 使用 PARTITION BY 分割結果集  
 下列範例會將 PARTITION BY 引數新增至範例 A 的程式碼中。資料列會先由 `SalesTerritoryCountry` 進行分割，接著依據每個 `SalesTerritoryCountry` 分成兩個群組。 請注意，OVER 子句中的 ORDER BY 會排序 NTILE，而 SELECT 陳述式的 ORDER BY 會排序結果集。  
  
```sql  
-- Uses AdventureWorks  
  
SELECT e.LastName, NTILE(2) OVER(PARTITION BY e.SalesTerritoryKey ORDER BY SUM(SalesAmountQuota) DESC) AS Quartile,  
       CONVERT (VARCHAR(13), SUM(SalesAmountQuota), 1) AS SalesQuota  
   ,st.SalesTerritoryCountry  
FROM dbo.DimEmployee AS e   
INNER JOIN dbo.FactSalesQuota AS sq   
    ON e.EmployeeKey = sq.EmployeeKey  
INNER JOIN dbo.DimSalesTerritory AS st  
    ON e.SalesTerritoryKey = st.SalesTerritoryKey  
WHERE sq.CalendarYear = 2003  
GROUP BY e.LastName,e.SalesTerritoryKey,st.SalesTerritoryCountry  
ORDER BY st.SalesTerritoryCountry, Quartile;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
LastName          Quartile SalesYTD       SalesTerritoryCountry  
----------------- -------- -------------- ------------------  
Tsoflias          1         867,000.00     Australia  
Saraiva           1        2,293,000.00    Canada  
Varkey Chudukatil 1        2,940,000.00    France  
Valdez            1        1,294,000.00    Germany  
Alberts           1          651,000.00    NA  
Jiang             1          544,000.00    NA  
Pak               1        5,142,000.00    United Kingdom  
Mensa-Annan       1        1,481,000.00    United States  
Campbell          1        1,438,000.00    United States  
Reiter            1        2,768,000.00    United States  
Blythe            1        4,716,000.00    United States  
Carson            1        4,350,000.00     United States  
Mitchell          1        4,682,000.00     United States  
Vargas            2        1,617,000.00     Canada  
Abbas             2          172,000.00     NA  
Ito               2        2,644,000.00     United States  
Ansman-Wolfe      2        1,183,000.00     United States
```  
  
## <a name="see-also"></a>另請參閱  
 [RANK &#40;Transact-SQL&#41;](../../t-sql/functions/rank-transact-sql.md)   
 [DENSE_RANK &#40;Transact-SQL&#41;](../../t-sql/functions/dense-rank-transact-sql.md)   
 [ROW_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/row-number-transact-sql.md)   
 [次序函式 &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md)   
 [內建函數 &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)  
  
  


