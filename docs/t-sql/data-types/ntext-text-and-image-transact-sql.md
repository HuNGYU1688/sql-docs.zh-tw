---
description: ntext、text 和 image (Transact-SQL)
title: ntext、text 和 image (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ntext_TSQL
- ntext
dev_langs:
- TSQL
helpviewer_keywords:
- text data type, about text data type
- text [SQL Server], data types
- ntext data type
- ntext data type, about ntext data type
- image data type, about image data type
ms.assetid: b0d8769c-7598-4f97-8162-ace5f182b5bc
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: b7b067831ca56d3dc5797aa68e4810a7e9969946
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88459953"
---
# <a name="ntext-text-and-image-transact-sql"></a>ntext、text 和 image (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

用來儲存非 Unicode 字元和 Unicode 字元及二進位資料的固定和可變長度資料類型。 Unicode 資料使用 UNICODE UCS-2 字元集。
  
>**重要！**  SQL Server 的未來版本將會移除 **ntext**、**text** 及 **image** 資料類型。 請避免在新的開發工作中使用這些資料類型，並規劃修改目前在使用這些資料類型的應用程式。 請改用 [nvarchar(max)](../../t-sql/data-types/nchar-and-nvarchar-transact-sql.md)、 [varchar(max)](../../t-sql/data-types/char-and-varchar-transact-sql.md)和 [varbinary(max)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md) 。  
  
## <a name="arguments"></a>引數
**ntext**  
最大字串長度為 2^30 - 1 (1,073,741,823) 位元組的可變長度 Unicode 資料。 儲存體大小 (以位元組為單位) 是輸入字串長度的兩倍。 **ntext** 的 ISO 同義字為 **national text**。
  
**text**  
在伺服器字碼頁中、最大字串長度為 2^31-1 (2,147,483,647) 的可變長度非 Unicode 資料。 當伺服器字碼頁使用雙位元組字元時，儲存體大小仍是 2,147,483,647 個位元組。 儲存體大小有可能少於 2,147,483,647 個位元組，這會隨著字元字串而不同。
  
**image**  
0 到 2^31-1 (2,147,483,647) 位元組的可變長度二進位資料。
  
## <a name="remarks"></a>備註  
您可以搭配 **ntext**、**text** 或 **image** 資料來使用下列函式和陳述式。
  
|函數|陳述式|  
|---|---|
|[DATALENGTH &#40;Transact-SQL&#41;](../../t-sql/functions/datalength-transact-sql.md)|[READTEXT &#40;Transact-SQL&#41;](../../t-sql/queries/readtext-transact-sql.md)|  
|[PATINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/patindex-transact-sql.md)|[SET TEXTSIZE &#40;Transact-SQL&#41;](../../t-sql/statements/set-textsize-transact-sql.md)|  
|[SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)|[UPDATETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/updatetext-transact-sql.md)|  
|[TEXTPTR &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textptr-transact-sql.md)|[WRITETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/writetext-transact-sql.md)|  
|[TEXTVALID &#40;Transact-SQL&#41;](../../t-sql/functions/text-and-image-functions-textvalid-transact-sql.md)||  
  
## <a name="see-also"></a>另請參閱
[CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[資料類型轉換 &#40;資料庫引擎&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)  
[資料類型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[定序與 Unicode 支援](../../relational-databases/collations/collation-and-unicode-support.md)

