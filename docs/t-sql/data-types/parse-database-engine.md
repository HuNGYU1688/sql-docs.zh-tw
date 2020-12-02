---
description: 剖析 (Database Engine)
title: Parse (資料庫引擎) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Parse
- Parse_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Parse [Database Engine]
ms.assetid: b37e28b6-6e2e-470a-945b-ce5252da743a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 8ebfca7738f8310108aab9ba988e658e7a5c1e17
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "92038361"
---
# <a name="parse-database-engine"></a>剖析 (Database Engine)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Parse 會將 **hierarchyid** 的標準字串表示法轉換成 **hierarchyid** 值。 將字串類型轉換成 **hierarchyid** 時，就會隱含呼叫 Parse。 其作用與 [ToString](../../t-sql/data-types/tostring-database-engine.md) 相反。 Parse() 是一種靜態方法。
  
## <a name="syntax"></a>語法  
  
```sql
-- Transact-SQL syntax  
hierarchyid::Parse ( input )  
-- This is functionally equivalent to the following syntax   
-- which implicitly calls Parse():  
CAST ( input AS hierarchyid )  
```  
  
```csharp
-- CLR syntax  
static SqlHierarchyId Parse ( SqlString input )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
*input*  
[!INCLUDE[tsql](../../includes/tsql-md.md)]：正在轉換的字元資料類型值。
  
CLR：正在評估的字串值。
  
## <a name="return-types"></a>傳回型別  
**SQL Server 傳回型別：hierarchyid**
  
**CLR 傳回型別：SqlHierarchyId**
  
## <a name="remarks"></a>備註  
若 Parse 收到一個值，而這個值不是 **hierarchyid** 的有效字串表示法，則會引發例外狀況。 例如，若 **char** 資料類型包含尾端空白，則會引發例外狀況。
  
## <a name="examples"></a>範例  
  
### <a name="a-converting-transact-sql-values-without-a-table"></a>A. 轉換 Transact-SQL 值而不使用資料表  
下列程式碼範例會使用 `ToString` 將 **hierarchyid** 值轉換成字串，並使用 `Parse` 將字串值轉換成 **hierarchyid**。
  
```sql
DECLARE @StringValue AS NVARCHAR(4000), @hierarchyidValue AS hierarchyid  
SET @StringValue = '/1/1/3/'  
SET @hierarchyidValue = 0x5ADE  
  
SELECT hierarchyid::Parse(@StringValue) AS hierarchyidRepresentation,  
@hierarchyidValue.ToString() AS StringRepresentation ;
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```
hierarchyidRepresentation    StringRepresentation
-------------------------    -----------------------
0x5ADE                       /1/1/3/
```
  
### <a name="b-clr-example"></a>B. CLR 範例  
下列程式碼片段會呼叫 Parse() 方法：
  
```csharp
string input = "/1/2/";  
SqlHierarchyId.Parse(input);  
```  
  
## <a name="see-also"></a>另請參閱
[Hierarchyid 資料類型方法參考](./hierarchyid-data-type-method-reference.md)  
[階層式資料 &#40;SQL Server&#41;](../../relational-databases/hierarchical-data-sql-server.md)  
[hierarchyid &#40;Transact-SQL&#41;](../../t-sql/data-types/hierarchyid-data-type-method-reference.md)
  
