---
description: 'sys.pdw_health_component_status_mappings (Transact-sql) '
title: sys.pdw_health_component_status_mappings (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: conceptual
ms.assetid: 4272cfad-5ad7-493d-9edd-d9111619bda0
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: d0d5bfea5d43315fed6b74f8767cea6cb89904a9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466939"
---
# <a name="syspdw_health_component_status_mappings-transact-sql"></a>sys.pdw_health_component_status_mappings (Transact-sql) 
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  定義 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 元件狀態與製造商定義元件名稱之間的對應。  
  
|資料行名稱|資料類型|描述|範圍|  
|-----------------|---------------|-----------------|-----------|  
|property_id|**int**|屬性的唯一識別碼。<br /><br /> property_id、component_id 和 physical_name 會形成此視圖的索引鍵。|NOT NULL|  
|component_id|**int**|元件的識別碼。 請參閱 [sys.pdw_health_components &#40;transact-sql&#41;](../../relational-databases/system-catalog-views/sys-pdw-health-components-transact-sql.md)。<br /><br /> property_id、component_id 和 physical_name 會形成此視圖的索引鍵。|NOT NULL|  
|physical_name|**nvarchar(32)**|屬性名稱，如製造商所定義。<br /><br /> property_id、component_id 和 physical_name 會形成此視圖的索引鍵。|NOT NULL|  
|logical_name|**nvarchar(255)**|由定義的屬性名稱 [!INCLUDE[ssDW](../../includes/ssdw-md.md)] 。|NOT NULL<br /><br /> 0-裝置實例是唯一的。<br /><br /> 1-裝置實例不是唯一的。|  
  
## <a name="see-also"></a>另請參閱  
 [Azure Synapse Analytics 和平行處理資料倉儲目錄檢視](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
