---
description: STGeomFromWKB (geography 資料類型)
title: STGeomFromWKB (geography 資料型別) | Microsoft Docs
ms.custom: ''
ms.date: 07/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- STGeomFromWKB_TSQL
- STGeomFromWKB (geography Data Type)
dev_langs:
- TSQL
helpviewer_keywords:
- STGeomFromWKB method
ms.assetid: 79d39d88-5440-49a7-9247-190eafce3f4f
author: MladjoA
ms.author: mlandzic
ms.openlocfilehash: 3f31d1af11761ca358a1cbbb844733b9fc32c985
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88467450"
---
# <a name="stgeomfromwkb-geography-data-type"></a>STGeomFromWKB (geography 資料類型)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

從開放地理空間協會 (OGC) 已知的二進位 (WKB) 表示法傳回 **geography** 執行個體。
  
這個 **geography** 資料類型方法可支援 **FullGlobe** 執行個體或大於半球的空間執行個體。
  
## <a name="syntax"></a>語法  
  
```  
  
STGeomFromWKB ( 'WKB_geography' , SRID )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *WKB_geography*  
 傳回 WKB 表示法的 **geography** 執行個體。 *WKB_geography* 是 **varbinary(max)** 運算式。  
  
 *SRID*  
 這是 **int** 運算式，表示要傳回之 **geography** 執行個體的空間參考識別碼 (SRID)。  
  
## <a name="return-types"></a>傳回型別  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳回類型：**geography**  
  
 CLR 傳回類型：**SqlGeography**  
  
## <a name="remarks"></a>備註  
 `STGeomFromText()` 傳回之 **geography** 執行個體的 OGC 型別會設定為對應的 WKB 輸入。  
  
 如果輸入的格式不正確，這個方法將會擲回 **FormatException**。  
  
 如果輸入包含對蹠邊緣，這個方法會擲回 **ArgumentException**。  
  
## <a name="examples"></a>範例  
 下列範例會使用 `STGeomFromWKB()` 建立 `geography` 例項。  
  
```  
DECLARE @g geography;  
SET @g = geography::STGeomFromWKB(0x010200000002000000D7A3703D0A975EC08716D9CEF7D34740CBA145B6F3955EC08716D9CEF7D34740, 4326);  
SELECT @g.ToString();  
```  
  
## <a name="see-also"></a>另請參閱  
 [OGC 靜態地理方法](../../t-sql/spatial-geography/ogc-static-geography-methods.md)  
  
  
