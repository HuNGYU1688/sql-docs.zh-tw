---
description: Write (Database Engine)
title: Write (資料庫引擎) | Microsoft Docs
ms.custom: ''
ms.date: 07/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Write_TSQL
- Write
dev_langs:
- TSQL
helpviewer_keywords:
- Write [Database Engine]
ms.assetid: 7c554334-d2d9-4eae-a4ae-097aa4020e1a
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: eb08d6f4d10a5cdf0047022185e24828b3ae37b5
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "92037449"
---
# <a name="write-database-engine"></a>Write (Database Engine)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Write 會寫出 **SqlHierarchyId** 的二進位表示法到傳入的 **BinaryWriter**。 Write 無法使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 呼叫。 請改用 CAST 或 CONVERT。
  
## <a name="syntax"></a>語法  
  
```csharp
void Write( BinaryWriter w )
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
*w*  
此 **hierarchyid** 節點的二進位表示法寫出的 **BinaryWriter** 物件。
  
## <a name="return-types"></a>傳回型別  
**CLR 傳回型別：void**
  
## <a name="remarks"></a>備註  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會在需要的時候於內部使用 Write，例如從 **hierarchyid** 資料行載入資料時。 在 **hierarchyid** 和 **varbinary** 之間進行轉換時，也會在內部呼叫 Write。
  
## <a name="examples"></a>範例  
  
```csharp
MemoryStream stream = new MemoryStream();  
BinaryWriter bw = new BinaryWriter(stream);  
hid.Write(bw);  
byte[] encoding = stream.ToArray();  
```  
  
## <a name="see-also"></a>另請參閱
[Read &#40;資料庫引擎&#41;](../../t-sql/data-types/read-database-engine.md)  
[ToString &#40;資料庫引擎&#41;](../../t-sql/data-types/tostring-database-engine.md)  
[CAST 和 CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[Hierarchyid 資料類型方法參考](./hierarchyid-data-type-method-reference.md)
  
