---
description: RADIANS (Transact-SQL)
title: RADIANS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RADIANS
- RADIANS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- RADIANS function
ms.assetid: e9f69951-ecda-45d9-8909-dcb716b1b1c0
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c372955137f2a7aa1e99814aef2dbd0284d15a95
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "91380680"
---
# <a name="radians-transact-sql"></a>RADIANS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  當輸入數值運算式時，傳回弧度 (以角度為單位)。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
RADIANS ( numeric_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *numeric_expression*  
 這是精確數值或近似數值資料類型類別的 [運算式](../../t-sql/language-elements/expressions-transact-sql.md)，但 **bit** 資料類型除外。  
  
## <a name="return-types"></a>傳回型別  
 傳回與 *numeric_expression* 相同的類型。  
  
## <a name="examples"></a>範例  
  
### <a name="a-using-radians-to-show-00"></a>A. 利用 RADIANS 來顯示 0.0  
 下列範例會傳回 `0.0` 的結果，因為轉換成弧度的數值運算式對 `RADIANS` 函數而言太小。  
  
```sql  
SELECT RADIANS(1e-307)  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
-------------------   
0.0                        
(1 row(s) affected)  
```  
  
### <a name="b-using-radians-to-return-the-equivalent-angle-of-a-float-expression"></a>B. 利用 RADIANS 來傳回浮點運算式的對等角度  
 下列範例使用 `float` 運算式，且會傳回指定角度的 `RADIANS`。  
  
```sql  
-- First value is -45.01.  
DECLARE @angle FLOAT  
SET @angle = -45.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is -181.01.  
DECLARE @angle FLOAT  
SET @angle = -181.01  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.00.  
DECLARE @angle FLOAT  
SET @angle = 0.00  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Next value is 0.1472738.  
DECLARE @angle FLOAT  
SET @angle = 0.1472738  
SELECT 'The RADIANS of the angle is: ' +  
    CONVERT(VARCHAR, RADIANS(@angle))  
GO  
-- Last value is 197.1099392.  
DECLARE @angle FLOAT  
SET @angle = 197.1099392  
SELECT 'The RADIANS of the angle is: ' +  
   CONVERT(VARCHAR, RADIANS(@angle))  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
---------------------------------------   
The RADIANS of the angle is: -0.785573                        
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: -3.15922                         
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0                                
(1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 0.00257041                       
 (1 row(s) affected)  
---------------------------------------   
The RADIANS of the angle is: 3.44022                          
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另請參閱  
 [CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [decimal 和 numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md)   
 [float 和 real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)   
 [int、bigint、smallint 和 tinyint &#40;Transact-SQL&#41;](../../t-sql/data-types/int-bigint-smallint-and-tinyint-transact-sql.md)   
 [數學函數 &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)   
 [money 和 smallmoney &#40;Transact-SQL&#41;](../../t-sql/data-types/money-and-smallmoney-transact-sql.md)  
  
  

