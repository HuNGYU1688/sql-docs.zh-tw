---
description: CircularString
title: CircularString | Microsoft 文件
ms.custom: ''
ms.date: 07/16/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 9fe06b03-d98c-4337-9f89-54da98f49f9f
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06527821c8f3b500f0b44fac711860a74565ba6c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473269"
---
# <a name="circularstring"></a>CircularString
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  **CircularString** 是零個或多個連續圓弧線段的集合。 圓弧線段是指由二維平面中三個點所定義的弧形線段。第一個點不得與第三個點相同。 如果圓弧線段的三個點都是共線，此圓弧線段就會被視為直線線段。  
  
## <a name="circularstring-instances"></a>CircularString 執行個體  
 下圖顯示有效的 **CircularString** 執行個體：  
  
 ![CircularString 範例](../../relational-databases/spatial/media/5ff17e34-b578-4873-9d33-79500940d0bc.gif)
  
### <a name="accepted-instances"></a>已接受的執行個體  
 如果 **CircularString** 執行個體是空的或包含奇數點 n (其中 n > 1)，則會接受此執行個體。 下面是已接受的 **CircularString** 執行個體。  
  
```sql  
DECLARE @g1 geometry = 'CIRCULARSTRING EMPTY';  
DECLARE @g2 geometry = 'CIRCULARSTRING(1 1, 2 0, -1 1)';  
DECLARE @g3 geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 2 0, 1 1)';  
```  
  
 `@g3` 顯示 **CircularString** 執行個體可被系統接受但卻無效。 系統無法接受下列 CircularString 執行個體宣告。 這個宣告會擲回 `System.FormatException`。  
  
```sql  
DECLARE @g geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 1 1)';  
```  
  
### <a name="valid-instances"></a>有效的執行個體  
 有效的 **CircularString** 執行個體必須是空的或具有下列屬性：  
  
-   它至少必須包含一個圓弧弧線 (亦即，至少有三個點)。  
-   序列中每個圓弧線段的最後一個端點 (最後一個線段除外) 必須是序列中下一個線段的第一個端點。  
-   它必須有奇數點。  
-   它本身不得在間隔上重疊。  
-   雖然 **CircularString** 執行個體可包含直線線段，不過這些線段必須由三個共線點定義。  
  
下列範例會顯示有效的 **CircularString** 執行個體。  
  
```sql  
DECLARE @g1 geometry = 'CIRCULARSTRING EMPTY';  
DECLARE @g2 geometry = 'CIRCULARSTRING(1 1, 2 0, -1 1)';  
DECLARE @g3 geometry = 'CIRCULARSTRING(1 1, 2 0, 2 0, 1 1, 0 1)';  
DECLARE @g4 geometry = 'CIRCULARSTRING(1 1, 2 2, 2 2)';  
SELECT @g1.STIsValid(), @g2.STIsValid(), @g3.STIsValid(),@g4.STIsValid();  
```  
  
**CircularString** 執行個體至少必須包含兩個圓弧線段，才能定義完整的圓形。 **CircularString** 執行個體無法使用單一圓弧線段 (例如 (1 1, 3 1, 1 1)) 來定義完整的圓形。 您可以使用 (1 1, 2 2, 3 1, 2 0, 1 1) 來定義圓形。  
  
下列範例會顯示無效的 CircularString 執行個體。  
  
```sql  
DECLARE @g1 geometry = 'CIRCULARSTRING(1 1, 2 0, 1 1)';  
DECLARE @g2 geometry = 'CIRCULARSTRING(0 0, 0 0, 0 0)';  
SELECT @g1.STIsValid(), @g2.STIsValid();  
```  
  
### <a name="instances-with-collinear-points"></a>具有共線點的執行個體  
在下列情況中，圓弧線段會被視為直線線段：  
  
-   當三個點都是共線 (例如 (1 3, 4 4, 7 5)) 時。  
-   當第一個和中間點相同，但是第三個點不同 (例如 (1 3, 1 3, 7 5)) 時。  
-   當中間和最後一個點相同，但是第一個點不同 (例如 (1 3, 4 4, 4 4)) 時。  
  
## <a name="examples"></a>範例  
  
### <a name="a-instantiating-a-geometry-instance-with-an-empty-circularstring"></a>A. 使用空的 CircularString 來具現化 Geometry 執行個體  
 這個範例會示範如何建立空的 **CircularString** 執行個體：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CIRCULARSTRING EMPTY');  
```  
  
### <a name="b-instantiating-a-geometry-instance-using-a-circularstring-with-one-circular-arc-segment"></a>B. 使用 CircularString 搭配一個圓弧線段來具現化 Geometry 執行個體  
 下列範例會示範如何建立具有單一圓弧線段 (半圓形) 的 **CircularString** 執行個體：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry:: STGeomFromText('CIRCULARSTRING(2 0, 1 1, 0 0)', 0);  
SELECT @g.ToString();  
```  
  
### <a name="c-instantiating-a-geometry-instance-using-a-circularstring-with-multiple-circular-arc-segments"></a>C. 使用 CircularString 搭配多個圓弧線段來具現化 Geometry 執行個體  
 下列範例會示範如何建立具有多個圓弧線段 (完整圓形) 的 **CircularString** 執行個體：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::Parse('CIRCULARSTRING(2 1, 1 2, 0 1, 1 0, 2 1)');  
SELECT 'Circumference = ' + CAST(@g.STLength() AS NVARCHAR(10));    
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]
  
```  
Circumference = 6.28319  
```  
  
當您使用 **LineString** 而非 **CircularString** 時，請比較輸出：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(2 1, 1 2, 0 1, 1 0, 2 1)', 0);  
SELECT 'Perimeter = ' + CAST(@g.STLength() AS NVARCHAR(10));  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]

```  
Perimeter = 5.65685  
```  
  
請注意， **CircularString** 範例的值會接近 2∏，這是圓形的實際圓周。  
  
### <a name="d-declaring-and-instantiating-a-geometry-instance-with-a-circularstring-in-the-same-statement"></a>D. 在相同的陳述式中使用 CircularString 來宣告和具現化 Geometry 執行個體  
 這個程式碼片段會示範如何在相同的陳述式中使用 **CircularString** 來宣告和具現化 **geometry** 執行個體：  
  
```sql  
DECLARE @g geometry = 'CIRCULARSTRING(0 0, 1 2.1082, 3 6.3246, 0 7, -3 6.3246, -1 2.1082, 0 0)';  
```  
  
### <a name="e-instantiating-a-geography-instance-with-a-circularstring"></a>E. 使用 CircularString 來具現化 Geography 執行個體  
 下列範例會示範如何使用 **CircularString** 來宣告和具現化 **geography** 執行個體：  
  
```sql  
DECLARE @g geography = 'CIRCULARSTRING(-122.358 47.653, -122.348 47.649, -122.348 47.658, -122.358 47.658, -122.358 47.653)';  
```  
  
### <a name="f-instantiating-a-geometry-instance-with-a-circularstring-that-is-a-straight-line"></a>F. 使用直線的 CircularString 來具現化 Geometry 執行個體  
 下列範例會示範如何建立直線的 **CircularString** 執行個體：  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('CIRCULARSTRING(0 0, 1 2, 2 4)', 0);  
```  
  
## <a name="see-also"></a>另請參閱  
 [空間資料類型概觀](../../relational-databases/spatial/spatial-data-types-overview.md)   
 [CompoundCurve](../../relational-databases/spatial/compoundcurve.md)   
 [MakeValid &#40;geography 資料類型&#41;](../../t-sql/spatial-geography/makevalid-geography-data-type.md)   
 [MakeValid &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/makevalid-geometry-data-type.md)   
 [STIsValid &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [STIsValid &#40;geography 資料類型&#41;](../../t-sql/spatial-geography/stisvalid-geography-data-type.md)   
 [STLength &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stlength-geometry-data-type.md)   
 [STStartPoint &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/ststartpoint-geometry-data-type.md)   
 [STEndpoint &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stendpoint-geometry-data-type.md)   
 [STPointN &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stpointn-geometry-data-type.md)   
 [STNumPoints &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stnumpoints-geometry-data-type.md)   
 [STIsRing &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stisring-geometry-data-type.md)   
 [STIsClosed &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STPointOnSurface &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stpointonsurface-geometry-data-type.md)   
 [LineString](../../relational-databases/spatial/linestring.md)  
  
  
