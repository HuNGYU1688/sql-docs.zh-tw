---
description: STIsRing (geometry 資料類型)
title: STIsRing (geometry 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 08/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STIsRing_TSQL
- STIsRing (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STIsRing (geometry Data Type)
ms.assetid: ea0063be-1c74-4cc4-ac6f-b65321ddfa54
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 06702731a47683c44d6096b354c3ec2f773aefaf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88472470"
---
# <a name="stisring-geometry-data-type"></a>STIsRing (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

如果 **geometry** 執行個體滿足下列需求，便傳回 1：
-   它是 **LineString** 執行個體。  
-   它為封閉式。  
-   它為簡單式。  
-   如果 **LineString** 執行個體不符合這些需求，則傳回 0。  

 **geometry** 執行個體若要是封閉且簡單的執行個體，則在此執行個體上叫用 [STIsClosed()](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md) 和 [STIsSimple()](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md) 時，兩者必須都傳回 1。 若要判斷 **geometry** 的執行個體類型，請使用 [STGeometryType()](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)。  
  
## <a name="syntax"></a>語法  
  
```  
  
.STIsRing ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**bit**  
  
 CLR 傳回類型：**SqlBoolean**  
  
## <a name="remarks"></a>備註  
 如果執行個體不是 **LineString**，這個方法會傳回 Null。  
  
## <a name="examples"></a>範例  
 下列範例會建立 `LineString` 例項，並使用 `STIsRing()` 來測試看看此例項是不是環形。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 2, 1 0, 0 0)', 0);  
SELECT @g.STIsRing();  
```  
  
## <a name="see-also"></a>另請參閱  
 [STIsClosed &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stisclosed-geometry-data-type.md)   
 [STGeometryType &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stgeometrytype-geometry-data-type.md)   
 [STIsSimple &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/stissimple-geometry-data-type.md)   
 [幾何例項上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  

