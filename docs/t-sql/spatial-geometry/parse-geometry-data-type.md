---
description: Parse (geometry 資料類型)
title: Parse (geometry 資料類型) | Microsoft Docs
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
- Parse (geometry Data Type)
ms.assetid: 6e080919-4b64-46cd-8dd2-254a9c232e53
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 2774f83d167ec61d0e1a2d68b391c2d5d3318535
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88416944"
---
# <a name="parse-geometry-data-type"></a>Parse (geometry 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

從開放地理空間協會 (OGC) 已知文字 (WKT) 表示傳回 **Geometry** 執行個體。 `Parse()` 相當於 [STGeomFromText()](../../t-sql/spatial-geometry/parse-geometry-data-type.md)，但是例外之處是它會採用空間參考識別碼 (SRID) 0 當做參數。 輸入可能會夾帶選擇性 Z (高度) 和 M (測量) 值。
  
## <a name="syntax"></a>語法  
  
```  
  
Parse ( 'geometry_tagged_text' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *geometry_tagged_text*  
 這是要傳回之 **geometry** 執行個體的 WKT 表示法。 *geometry_tagged_text* 是 **nvarchar** 陳述式。  
  
## <a name="return-types"></a>傳回型別  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**geometry**  
  
 CLR 傳回類型：**SqlGeometry**  
  
## <a name="remarks"></a>備註  
 `Parse()` 所傳回之 **geometry** 執行個體的 OGC 類型會設定為對應的 WKT 輸入。  
  
 'Null' 字串將會解譯為 Null **geometry** 執行個體。  
  
 如果輸入的格式不正確，這個方法將會擲回 **FormatException**。  
  
## <a name="examples"></a>範例  
 下列範例會使用 `Parse()` 建立 `geometry` 例項。  
  
```  
DECLARE @g geometry;   
SET @g = geometry::Parse('LINESTRING (100 100, 20 180, 180 180)');  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另請參閱  
 [STGeomFromText](../../t-sql/spatial-geometry/parse-geometry-data-type.md)   
 [擴充的靜態幾何方法](../../t-sql/spatial-geometry/extended-static-geometry-methods.md)  
  
  

