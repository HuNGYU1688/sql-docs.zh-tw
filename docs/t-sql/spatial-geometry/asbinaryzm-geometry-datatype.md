---
description: AsBinaryZM (geometry 資料類型)
title: AsBinaryZM (geometry 資料類型) | Microsoft Docs
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
- AsBinaryZM geometry
ms.assetid: 5eae2872-adca-4b8f-8b04-4ee91ced98f1
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 83f6f173213f81f9633d9593f73d115851789cce
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128198"
---
# <a name="asbinaryzm-geometry-datatype"></a>AsBinaryZM (geometry 資料類型)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

傳回開放地理空間協會 (Open Geospatial Consortium，OGC) 對於 **geometry** 執行個體的已知的二進位 (Well-Known Binary，WKB) 表示法，經由此執行個體夾帶的任何 **Z** (高度) 和 **M** (測量) 值來擴充。
  
## <a name="syntax"></a>語法  
  
```  
  
.AsBinaryZM()  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**varbinary(max)**  
  
 CLR 傳回類型：**SqlBytes**  
  
## <a name="remarks"></a>備註  
  
## <a name="examples"></a>範例  
  
```sql  
DECLARE @g1 GEOMETRY = 'Point(1 1 2 3)';  
  
SELECT @g1.STAsBinary();  
-- Returns: 0x0101000000000000000000F03F000000000000F03F  
  
SELECT @g1.AsBinaryZM();  
--Returns: 0x01B90B0000000000000000F03F000000000000F03F00000000000000400000000000000840  
```  
  
## <a name="see-also"></a>另請參閱  
 [幾何執行個體上擴充的方法](../../t-sql/spatial-geometry/extended-methods-on-geometry-instances.md)   
 [M &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/m-geometry-data-type.md)   
 [Z &#40;geometry 資料類型&#41;](../../t-sql/spatial-geometry/z-geometry-data-type.md)  
  
  

