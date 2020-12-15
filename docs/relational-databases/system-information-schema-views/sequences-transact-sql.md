---
description: " (Transact-sql) 的順序"
title: " (Transact-sql) 的順序 |Microsoft Docs"
ms.custom: ''
ms.date: 12/30/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- SEQUENCES
- SEQUENCES_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SEQUENCES view
- INFORMATION_SCHEMA.SEQUENCES view
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 365b91660769cbb189b23366671486271177e6e4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440589"
---
# <a name="sequences-transact-sql"></a> (Transact-sql) 的順序

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

針對目前資料庫中目前使用者所能存取的每個序列，各傳回一個資料列。

若要從這些視圖中取出資訊，請指定 **INFORMATION_SCHEMA**_.view_name_ 的完整名稱。

|資料行名稱|資料類型|描述|
|-----------------|---------------|-----------------|
|**SEQUENCE_CATALOG**|**nvarchar(128)**|順序辨識符號|
|**SEQUENCE_SCHEMA**|**Nvarchar (** 128) * *|包含順序的架構名稱|
|**SEQUENCE_NAME**|**nvarchar(128)**|順序名稱|
|**DATA_TYPE**|**Nvarchar (** 128 **)**|Sequence 資料類型|
|**NUMERIC_PRECISION**|**tinyint**|順序的有效位數|
|**NUMERIC_PRECISION_RADIX**|**smallint**|近似數值資料、精確數值資料、整數資料或貨幣資料的有效位數基數。 否則，就傳回 NULL。|
|**NUMERIC_SCALE**|**int**|近似數值資料、精確數值資料、整數資料或貨幣資料的小數位數。 否則，就傳回 NULL。|
|**START_VALUE**|**int**|順序物件會傳回的第一個值。|
|**MINIMUM_VALUE**|**int**|順序物件的界限。 新序列物件的預設最小值是序列物件之資料類型的最小值。 如果是 tinyint 資料類型，這是零，如果是所有其他資料類型，則為負數。|
|**MAXIMUM_VALUE**|**int**|順序物件的界限。 新序列物件的預設最大值是序列物件之資料類型的最大值。|
|**增量**|**int**|每次呼叫 NEXT VALUE FOR 函數時，用來遞增順序物件值的值 (如果是負數則遞減)。 如果增量是負值，則會遞減順序物件，否則會遞增。 增量不能為 0。 新順序物件的預設增量為 1。
|**CYCLE_OPTION**|**int**|屬性，指定當超出其最小值或最大值時，順序物件應該從最小值 (或是遞減順序物件的最大值) 重新啟動，還是擲回例外狀況。 新順序物件的預設循環選項是 NO CYCLE。
|**DECLARED_DATA_TYPE**|**int**|使用者定義資料類型的資料類型。|
|**DECLARED_DATA_PRECISION**|**int**|使用者定義資料類型的有效位數。|
|**DECLARED_NUJMERIC_SCALE**|**int**|使用者定義資料類型的數值小數位數。|

**範例** 下列範例會傳回測試資料庫中架構的相關資訊：

```sql
SELECT * FROM test.INFORMATION_SCHEMA.SEQUENCES;
```

## <a name="see-also"></a>另請參閱

- [&#40;Transact-sql&#41;的資訊架構視圖 ](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)
- [sys.sequences &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)
