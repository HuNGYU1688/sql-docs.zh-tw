---
description: STAsBinary (geometry 資料類型)
title: STAsBinary (geometry 資料類型) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STAsBinary_TSQL
- STAsBinary (geometry Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STAsBinary (geometry Data Type)
ms.assetid: 65353777-e3e6-461c-9504-ea4d83312692
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 59416177b8b50855a0b6a0aa694f938f90479b6c
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88426900"
---
# <a name="stasbinary-geometry-data-type"></a>STAsBinary (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回幾何執行個體的開放式地理空間協會 (Open Geospatial Consortium，OGC) 已知的二進位 (well-known binary，WKB) 表示法。  
 
## <a name="syntax"></a>語法  
  
```  
  
.STAsBinary ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**varbinary(max)**  
  
 CLR 傳回類型：**SqlBytes**  
  
## <a name="examples"></a>範例  
 下列範例會從文字建立 (0,0) 到 (2,3) 的 `LineString` 幾何例項。 `STAsBinary()` 會在 WKB 中傳回結果。  
  
```  
DECLARE @g geometry;  
SET @g = geometry::STGeomFromText('LINESTRING(0 0, 2 3)', 0);  
SELECT @g.STAsBinary();  
```  
  
## <a name="see-also"></a>另請參閱  
 [幾何例項上的 OGC 方法](../../t-sql/spatial-geometry/ogc-methods-on-geometry-instances.md)  
  
  
