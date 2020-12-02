---
description: STInteriorRingN (geometry 資料類型)
title: STInteriorRingN (geometry 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STInteriorRingN_TSQL
- STInteriorRingN (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STInteriorRingN (geometry Data Type)
ms.assetid: 47310f9f-2cdb-41e0-a6da-7c3cfbf139ac
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 52d1f15affc8f253303bb636c4b94f280e8a3319
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88479262"
---
# <a name="stinteriorringn-geometry-data-type"></a>STInteriorRingN (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

傳回 **Polygongeometry** 執行個體的指定內環。
  
## <a name="syntax"></a>語法  
  
```  
  
.STInteriorRingN ( expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *expression*  
 這是介於 1 和 **geometry** 執行個體中內環數之間的 **int** 運算式。  
  
## <a name="return-types"></a>傳回型別  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**geometry**  
  
 CLR 傳回類型：**SqlGeometry**  
  
 開放地理空間協會 (OGC) 類型：**LineString**  
  
## <a name="remarks"></a>備註  
 如果 **geometry** 執行個體不是多邊形，這個方法會傳回 **null**。 如果此運算式大於環形數，此方法也會擲回 **ArgumentOutOfRangeException**。 可以使用 `STNumInteriorRing``()` 傳回環形數。  
  
## <a name="examples"></a>範例  
 下例範例會建立 `Polygon` 執行個體，並使用 `STInteriorRingN()` 傳回 **LineString** 形式之多邊形的內環。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('POLYGON((0 0, 3 0, 3 3, 0 3, 0 0),(2 2, 2 1, 1 1, 1 2, 2 2))', 0);  
SELECT @g.STInteriorRingN(1).ToString();  
```  
  
## <a name="see-also"></a>另請參閱  
 [幾何例項上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

