---
description: MakeValid (geography 資料類型)
title: MakeValid (geography 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- MakeValid method (geography)
ms.assetid: f67038e3-4f62-4465-994e-e95ac27d8ada
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 7a3ca43dea611fb3fb74167032bd72f045e39b41
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88422412"
---
# <a name="makevalid-geography-data-type"></a>MakeValid (geography 資料類型)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  將無效的 **geography** 執行個體轉換成具有有效開放地理空間協會 (Open Geospatial Consortium，OGC) 類型的有效 **geography** 執行個體。  
  
 如果 input 物件針對 STIsValid() 傳回 False，則 `MakeValid()` 會將無效的執行個體轉換為有效的執行個體。  
  
 這個 geography 資料類型方法可支援 **FullGlobe** 執行個體或大於半球的空間執行個體。  
  
## <a name="syntax"></a>語法  
  
```  
  
.MakeValid ()  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**geography**  
  
 CLR 傳回類型：**SqlGeography**  
  
## <a name="remarks"></a>備註  
 這個方法可變更 **geography** 執行個體的類型。 此外，**geography** 執行個體的點可能會稍微移動。 來自某些方法 (如 NumPoint()) 的結果可能會變更。  
  
 當無效的空間執行個體與赤道交集，且 EnvelopeAngle() = 180 時，將會傳回 **FullGlobe** 執行個體。 `MakeValid()`**geography** 資料類型方法將會盡可能地嘗試傳回有效執行個體，但無法保證其結果是否準確或精確。  
  
> [!NOTE]  
>  無效的物件可以儲存在資料庫中。 可以在無效執行個體 (也就是 STIsValid() 會傳回 False 的執行個體) 上執行的方法為用來檢查有效性或允許匯出的方法：STIsValid()、MakeValid()、STAsText()、STAsBinary()、ToString()、AsTextZM() 和 AsGml()。  
  
 這個方法並不精確。  
  
## <a name="examples"></a>範例  
 第一個範例會建立與自己重疊的無效 `LineString` 例項，並使用 `STIsValid()` 來確認它是無效的例項。 `STIsValid()` 會針對無效的例項傳回 0 的值。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromText('LINESTRING(0 2, 1 1, 1 0, 1 1, 2 2)', 4326);  
SELECT @g.STIsValid();  
```  
  
 第二個範例會使用 `MakeValid()` 讓此例項變成有效及測試看看此例項是否真正有效。 `STIsValid()` 會針對有效的例項傳回 1 的值。  
  
```  
SET @g = @g.MakeValid();  
SELECT @g.STIsValid();  
```  
  
 第三個範例會確認此例項已做了哪些變更而成為有效的例項。  
  
```  
SELECT @g.ToString();  
```  
  
 在此範例中，當選取 `LineString` 例項時，值會當做有效的 `MultiLineString` 例項傳回。  
  
```  
MULTILINESTRING ((0 2, 1 1, 2 2), (1 1, 1 0))  
```  
  
## <a name="see-also"></a>另請參閱  
 [STIsValid &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stisvalid-geometry-data-type.md)   
 [地理例項上擴充的方法](../../t-sql/spatial-geography/extended-methods-on-geography-instances.md)  
  
  
