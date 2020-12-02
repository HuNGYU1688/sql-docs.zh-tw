---
description: money 和 smallmoney (Transact-SQL)
title: money 和 smallmoney (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- money_TSQL
- money
- smallmoney
- smallmoney_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- money data type, about money data type
- money data type
- smallmoney data type
- values [SQL Server], monetary
- currency [SQL Server]
ms.assetid: 57861137-89ea-4b89-b361-390597d7bccc
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8218bf8b98cd072dd60f8458c75819761153b634
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "91111280"
---
# <a name="money-and-smallmoney-transact-sql"></a>money 和 smallmoney (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

代表金融或貨幣值的資料類型。
  
## <a name="remarks"></a>備註  
  
|資料類型|範圍|儲存體|  
|---|---|---|
|**money**|-922,337,203,685,477.5808 到 922,337,203,685,477.5807 (-922,337,203,685,477.58<br />到 922,337,203,685,477.58 (Informatica)。  Informatica 只支援兩個十進位數，而非四個。)|8 個位元組|  
|**smallmoney**|- 214、748.3648 到 214、748.3647|4 個位元組|  
  
**money** 和 **smallmoney** 資料類型的精確度可達它們所代表之金融單位的萬分之一。 針對 Informatica，**money** 和 **smallmoney** 資料類型的精確度可達它們所代表之金融單位的百分之一。
  
句點可用來分隔局部的貨幣單位 (如分，Cent) 與完整的貨幣單位。 例如，2.15 是指定 2 元又 15 分。
  
這些資料類型可以使用以下任一種貨幣符號。
  
![貨幣符號、十六進位值表格](../../t-sql/data-types/media/money01.gif "貨幣符號、十六進位值表格")
  
貨幣資料不必括在單引號 (') 中。 您必須記住，在指定前面有貨幣符號的貨幣值時，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不會儲存任何與符號相關聯的貨幣資訊，它只會儲存數值。
  
## <a name="converting-money-data"></a>轉換 money 資料
當您從任何整數資料類型轉換成 **money** 時，會假設單位是貨幣單位。 例如，整數值 4 轉換成 **money** 時相當於 4 個貨幣單位。
  
下列範例會分別將 **smallmoney** 和 **money** 值轉換成 **varchar** 和 **decimal** 資料類型。
  
```sql
DECLARE @mymoney_sm SMALLMONEY = 3148.29,  
        @mymoney    MONEY = 3148.29;  
SELECT  CAST(@mymoney_sm AS VARCHAR) AS 'SM_MONEY varchar',  
        CAST(@mymoney AS DECIMAL)    AS 'MONEY DECIMAL';  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
SM_MONEY VARCHAR               MONEY DECIMAL  
------------------------------ ----------------------  
3148.29                        3148    
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>請參閱
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)
[CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)
[資料類型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
[sys.types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-types-transact-sql.md)
  
  
