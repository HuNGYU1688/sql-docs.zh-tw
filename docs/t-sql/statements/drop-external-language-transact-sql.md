---
description: DROP EXTERNAL LANGUAGE (Transact-SQL) - SQL Server
title: DROP EXTERNAL LANGUAGE (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/08/2019
ms.prod: sql
ms.technology: language-extensions
ms.topic: language-reference
author: nelgson
ms.author: negust
ms.reviewer: dphansen
manager: cgronlun
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: 785aaff078085fd5c202e7b3c043ab87e273472f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481799"
---
# <a name="drop-external-language-transact-sql"></a>DROP EXTERNAL LANGUAGE (Transact-SQL)  
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

刪除現有的外部語言。

## <a name="syntax"></a>語法

```syntaxsql
DROP EXTERNAL LANGUAGE <language_name>
```

### <a name="arguments"></a>引數

**language_name**

語言是資料庫範圍的物件。 語言名稱在資料庫內必須是唯一的。

## <a name="permissions"></a>權限

若要刪除語言，則需要 ALTER ANY EXTERNAL LANGUAGE 權限。 根據預設，任何資料庫擁有者或物件的擁有者，也可刪除外部語言。

> [!NOTE]
> 請注意，移除外部語言之前，您需要移除參考外部語言的外部程式庫。

### <a name="return-values"></a>傳回值

如果陳述式執行成功，會傳回參考訊息。

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>備註

需要先刪除指定語言的所有外部程式庫，才能刪除外部語言。

## <a name="examples"></a>範例

建立外部語言 **Java**：

```sql
CREATE EXTERNAL LANGUAGE Java 
FROM (CONTENT = N'<path-to-zip>', FILE_NAME = 'javaextension.dll');
GO
```

置放外部語言：

```sql
DROP EXTERNAL LANGUAGE Java;
```

## <a name="see-also"></a>另請參閱

[CREATE EXTERNAL LANGUAGE (Transact-SQL)](create-external-language-transact-sql.md)  
[ALTER EXTERNAL LANGUAGE (Transact-SQL)](alter-external-language-transact-sql.md)  
[sys.external_languages](../../relational-databases/system-catalog-views/sys-external-languages-transact-sql.md)  
[sys.external_language_files](../../relational-databases/system-catalog-views/sys-external-language-files-transact-sql.md)  
