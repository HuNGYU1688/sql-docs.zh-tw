---
description: STArea (geometry 資料類型)
title: STArea (geometry 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STArea (geometry Data Type)
- STArea_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STArea (geometry Data Type)
ms.assetid: a7dd6083-c649-4ac3-885d-1234e0db62f1
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 2dec51b1ce17ed52ed7c6bb6d601b5977eaf2172
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88422282"
---
# <a name="starea-geometry-data-type"></a>STArea (geometry 資料類型)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  傳回 **geometry** 執行個體的總介面區。  
  
## <a name="syntax"></a>語法  
  
```  
  
.STArea ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**float**  
  
 CLR 傳回類型：**SqlDouble**  
  
## <a name="remarks"></a>備註  
 如果 **geometry** 執行個體只包含 0 維度和 1 維度的圖形，或它是空的，`STArea()` 會傳回 0。 如果 **geometry** 執行個體尚未初始化，`STArea()` 會傳回 **NULL**。  
  
## <a name="examples"></a>範例  
  
### <a name="a-computing-the-area-of-a-polygon-instance"></a>A. 計算 Polygon 執行個體的區域  
 下列範例會建立 `Polygon``geometry` 執行個體，並計算此多邊形的區域。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STArea();  
```  
  
### <a name="b-computing-the-area-of-a-curvepolygon-instance"></a>B. 計算 CurvePolygon 執行個體的區域  
 下列範例會計算 `CurvePolygon` 執行個體的區域。  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CURVEPOLYGON(CIRCULARSTRING(0 2, 2 0, 4 2, 4 2, 0 2))');  
 SELECT @g.STArea() AS Area;
 ```  
  
## <a name="see-also"></a>另請參閱  
 [幾何例項上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
