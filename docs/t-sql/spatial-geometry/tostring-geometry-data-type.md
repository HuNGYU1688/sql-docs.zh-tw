---
description: ToString (geometry 資料類型)
title: ToString (geometry 資料型別) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ToString (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- ToString (geometry Data Type)
ms.assetid: 2e55fa98-aa22-4baa-a516-7c233a33e212
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: d5b8e9d4c027dd364c9aaf84b4b8e56680548d0c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88305536"
---
# <a name="tostring-geometry-data-type"></a>ToString (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

傳回開放式地理空間協會 (Open Geospatial Consortium，OGC) 對於幾何例項的已知的文字 (Well-Known Text，WKT) 表示法，經由此例項夾帶的任何 Z (高度) 和 M (測量) 值來擴充。
  
## <a name="syntax"></a>語法  
  
```  
  
.ToString ()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**nvarchar(max)**  
  
 CLR 傳回類型：**SqlString**  
  
## <a name="remarks"></a>備註  
 這個方法在 Null 例項上呼叫時，將會傳回 "Null" 字串。  
  
 在非 Null 例項上，此方法相當於使用 `AsTextZM().`  
  
## <a name="examples"></a>範例  
 下列範例會建立 `LineString` 執行個體，並使用 `ToString()` 來提取此例項的文字描述。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 0 1, 1 0)', 0);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另請參閱  
 [STAsText &#40;geometry 資料型別&#41;](../../t-sql/spatial-geometry/stastext-geometry-data-type.md)   
 [幾何例項上擴充的方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)  
  
  

