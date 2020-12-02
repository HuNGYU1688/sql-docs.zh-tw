---
description: STDimension (geography 資料類型)
title: STDimension (geography 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STDimension (geography Data Type)
- STDimension_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STDimension method
ms.assetid: 4368b0f6-0678-4ade-87dc-b43d8b2e8d92
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: e7cd207272093ab77aea443737f27bd99a35e2e5
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479291"
---
# <a name="stdimension-geography-data-type"></a>STDimension (geography 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回 **geography** 執行個體的最大維度。  
  
## <a name="syntax"></a>語法  
  
```  
  
.STDimension ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**int**  
  
 CLR 傳回類型：**SqlInt32**  
  
## <a name="remarks"></a>備註  
 如果 **geography** 執行個體是空的，STDimension() 會傳回 -1。  
  
## <a name="examples"></a>範例  
 下列範例會使用 `STDimension()` 建立資料表變數來保存 `geography` 執行個體，並插入 `Point`、`LineString` 和 `Polygon`。  
  
```  
DECLARE @temp table ([name] varchar(10), [geom] geography);  
  
INSERT INTO @temp values ('Point', geography::STGeomFromText('POINT(-122.34900 47.65100)', 4326));  
INSERT INTO @temp values ('LineString', geography::STGeomFromText('LINESTRING(-122.360 47.656, -122.343 47.656)', 4326));  
INSERT INTO @temp values ('Polygon', geography::STGeomFromText('POLYGON((-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653))', 4326));  
  
SELECT [name], [geom].STDimension() as [dim]  
FROM @temp;  
```  
  
 然後此範例會傳回每一個 `geography` 執行個體的維度。  
  
|名稱|dim|  
|----------|---------|  
|Point|0|  
|LineString|1|  
|Polygon|2|  
  
## <a name="see-also"></a>另請參閱  
 [地理位置例項上的 OGC 方法](../../t-sql/spatial-geography/ogc-methods-on-geography-instances.md)  
  
  
