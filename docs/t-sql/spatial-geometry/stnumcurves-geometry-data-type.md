---
description: STNumCurves (geometry 資料類型)
title: STNumCurves (geometry 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- STNumCurves method (geometry)
ms.assetid: 20c2fa0b-656b-4519-b34c-cc8f094290d4
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: fe3418284131577051e48f8300bd89df2abc0ec1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88444952"
---
# <a name="stnumcurves-geometry-data-type"></a>STNumCurves (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

當 **geometry** 執行個體是一維空間資料類型時，這個方法會傳回此執行個體中的曲線數目。 一維空間資料類型包括 **LineString**、**CircularString** 及 **CompoundCurve**。 `STNumCurves`() 只能在簡單類型上運作；它不能與 **geometry** 集合 (例如 **MultiLineString**) 搭配運作。
  
## <a name="syntax"></a>語法  
  
```  
  
.STNumCurves()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**geometry**  
  
 CLR 傳回類型：**SqlGeometry**  
  
## <a name="remarks"></a>備註  
 空的一維 **geometry** 執行個體會傳回 0。 當 **geometry** 執行個體不是一維執行個體，或者是未初始化的執行個體時，就會傳回 **NULL**。  
  
## <a name="examples"></a>範例  
  
### <a name="a-using-stnumcurves-on-a-circularstring-instance"></a>A. 在 CircularString 執行個體上使用 STNumCurves()  
 下列範例會顯示如何取得 `CircularString` 執行個體中的曲線數目：  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0)');  
 SELECT @g.STNumCurves();
 ```  
  
### <a name="b-using-stnumcurves-on-a-compoundcurve-instance"></a>B. 在 CompoundCurve 執行個體上使用 STNumCurves()  
 下列範例會使用 `STNumCurves()` 傳回 `CompoundCurve` 執行個體中的曲線數目。  
  
```
 DECLARE @g geometry;  
 SET @g = geometry::Parse('COMPOUNDCURVE(CIRCULARSTRING(10 0, 0 10, -10 0, 0 -10, 10 0))');  
 SELECT @g.STNumCurves();
 ```  
  
## <a name="see-also"></a>另請參閱  
 [空間資料類型概觀](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [幾何例項上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

