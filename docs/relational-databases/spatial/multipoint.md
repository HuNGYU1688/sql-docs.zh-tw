---
description: MultiPoint
title: MultiPoint | Microsoft Docs
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- MultiPoint geometry subtype [SQL Server]
ms.assetid: 2aaab211-3aba-4dbd-90b7-095d997b1f62
author: MladjoA
ms.author: mlandzic
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea276d706d2c1e95f352755cb0b794861e45b42a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475419"
---
# <a name="multipoint"></a>MultiPoint
[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]
  **MultiPoint** 是零或多個點的集合。 **MultiPoint** 執行個體的界限是空的。  
  
## <a name="examples"></a>範例  

### <a name="example-a"></a>範例 A。
下列範例會建立具有 SRID 23 和兩個點的 `geometry MultiPoint` 執行個體：一個點的座標為 (2, 3)，另一個點的座標為 (7, 8)，而 Z 值為 9.5。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-b"></a>範例 B. 
下列範例使用 `STMPointFromText()` 來表示 `MultiPoint` 執行個體。  
  
```sql  
DECLARE @g geometry;  
SET @g = geometry::STMPointFromText('MULTIPOINT((2 3), (7 8 9.5))', 23);  
```  
  
### <a name="example-c"></a>範例 C。
下列範例使用 `STGeometryN()` 方法來擷取集合中第一個點的描述。  
  
```sql  
SELECT @g.STGeometryN(1).STAsText();  
```  
  
## <a name="see-also"></a>另請參閱  
 [點](../../relational-databases/spatial/point.md)   
 [空間資料 &#40;SQL Server&#41;](../../relational-databases/spatial/spatial-data-sql-server.md)  
  
  
