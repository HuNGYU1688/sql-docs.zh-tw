---
description: float 和 real (Transact-SQL)
title: float 和 real (Transact-SQL)
ms.custom: ''
ms.date: 09/10/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- float
- real_TSQL
- real
- float_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- numeric data type, floating point
- float data type
- floating point data [SQL Server]
- real data type
ms.assetid: 08ea66b7-624e-4d8b-86bc-750ff76cdfc5
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8fdbe0c896a5aba4cc68abf2b2c572a82ad9ca7b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482117"
---
# <a name="float-and-real-transact-sql"></a>float 和 real (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

用來搭配浮點數值資料使用的近似數值資料類型。 浮點數資料是近似的；因此，並非資料類型範圍內的所有值都能夠精確地表示。 **real** 的 ISO 同義字是 **float(24)** 。
  
![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>語法  
**float** [ **(** _n_ **)** ] 其中 *n* 是用來儲存科學記號標記法 **float** 數尾數的位元數目；因此，其規定有效位數和儲存體大小。 如果指定 *n*，則其值必須介於 **1** 與 **53** 之間。 *n* 的預設值為 **53**。
  
|*n* 值|Precision|儲存體大小|  
|---|---|---|
|**1-24**|7 位數|4 個位元組|  
|**25-53**|15 位數|8 個位元組|  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會將 *n* 當做兩個可能值的其中一個來處理。 如果 **1**<=n<=**24**，則將 *n* 當作 **24** 來處理。 如果 **25**<=n<=**53**，則將 *n* 當作 **53** 來處理。  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **float**[ **(n)** ] 資料類型從 *1* 到 **53** 的所有 **n** 值都符合 ISO 標準。 **double precision** 的同義字是 **float(53)** 。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>備註  
  
|資料類型|範圍|儲存體|  
|---|---|---|
|**float**|- 1.79E+308 到 -2.23E-308、0 及 2.23E-308 到 1.79E+308|這會隨著 *n* 的值而不同|  
|**real**|- 3.40E + 38 到 -1.18E - 38、0 及 1.18E - 38 到 3.40E + 38|4 個位元組|  
  
##  <a name="converting-float-and-real-data"></a>轉換 float 與 real 資料  
將 **float** 值轉換成任何整數類型時都會截斷。
  
當您想要從 **float** 或 **real** 轉換成字元資料時，使用 STR 字串函式比 CAST( ) 來得有用。 這是因為 STR 可以對格式有較多的控制。 如需詳細資訊，請參閱 [STR &#40;Transact-SQL&#41](../../t-sql/functions/str-transact-sql.md) 和 [函式 &#40;Transact-SQL&#41;](../../t-sql/functions/functions.md)。
  
在 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 之前，**float** 值轉換至 **decimal** 或 **numeric**，就會限制為只有 17 個有效位數的值。 任何小於 5E-18 (當設定使用 5E-18 科學記號標記法或 0.0000000000000000050000000000000005 十進位標記法時) 的 **float** 值都會捨去為 0。 這不再是 [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] 的限制。
  
## <a name="see-also"></a>另請參閱
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[資料類型轉換 &#40;資料庫引擎&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[資料類型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)
  
  
