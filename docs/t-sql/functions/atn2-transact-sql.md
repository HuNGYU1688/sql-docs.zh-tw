---
description: ATN2 (Transact-SQL)
title: ATN2 (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ATN2
- ATN2_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- arctangent
- tangent
- ATN2 function
ms.assetid: 014b291e-7cd7-4c39-b20d-5db3a9f0505d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7dd9d89534be641da25c0cbf0d4f7e1e58d82dde
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96124961"
---
# <a name="atn2-transact-sql"></a>ATN2 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

以介於正數 X 軸和從原點到 (y, x) 點的切線之間的弧度來傳回角度，其中 x 與 y 是兩個指定浮點運算式的值。
  
![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>語法  
  
```syntaxsql
ATN2 ( float_expression , float_expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
*float_expression*  
資料型別為 **float** 的 [運算式](../../t-sql/language-elements/expressions-transact-sql.md)。
  
## <a name="return-types"></a>傳回類型
**float**
  
## <a name="examples"></a>範例  
下列範例會計算指定的 `ATN2` 和 `x` 元件的 `y`。
  
```sql
DECLARE @x FLOAT = 35.175643, @y FLOAT = 129.44;  
SELECT 'The ATN2 of the angle is: ' + CONVERT(VARCHAR, ATN2(@y, @x));  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
The ATN2 of the angle is: 1.30545                         
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>另請參閱
[CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[float 和 real &#40;Transact-SQL&#41;](../../t-sql/data-types/float-and-real-transact-sql.md)  
[數學函數 &#40;Transact-SQL&#41;](../../t-sql/functions/mathematical-functions-transact-sql.md)
  
  

