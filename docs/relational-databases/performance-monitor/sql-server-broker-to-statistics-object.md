---
title: SQL Server 的 Broker TO Statistics 物件 | Microsoft Docs
description: 了解 SQLServer:Broker TO Statistics 效能物件，其會報告 Service Broker 要求傳輸物件的資訊。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Broker Transmission Object object
- 'SQL Server: Broker Transmission Object'
ms.assetid: b5bc5dde-e540-4848-8aa3-5735c51df2fb
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eabaa3854012b747e028babca44c749d1d51206e
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505828"
---
# <a name="sql-server-broker-to-statistics-object"></a>SQL Server 的 Broker TO Statistics 物件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  SQLServer:Broker TO Statistics 效能物件會報告 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 對話要求傳輸物件的次數以及傳輸物件寫入 **tempdb** 之頻率的相關資訊。  
  
 傳輸物件會記錄 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 對話之訊息傳輸的狀態。 這些會儲存在記憶體中。 為了釋出記憶體， [!INCLUDE[ssSB](../../includes/sssb-md.md)] 會定期將非使用中傳輸物件的批次寫入 **tempdb** 中的工作資料表。  
  
 下表列出這個物件包含的計數器。  
  
|SQL Server Broker TO Statistics 計數器|描述|  
|----------------------------------------------|-----------------|  
|**平均Length of Batched Writes**|儲存於批次中的傳輸物件平均數目。|  
|**平均Time To Write Batch (ms)**|儲存傳輸物件批次所需的平均毫秒數。|  
|**平均Time to Write Batch Base**|僅供內部使用。|
|**平均Time Between Batches (ms)**|傳輸物件批次寫入之間的平均毫秒數。|  
|**平均Time Between Batches Base**|僅供內部使用。| 
|**Tran Object Gets/sec**|對話每秒鐘要求傳輸物件的次數。|  
|**Tran Objects Marked Dirty/sec**|傳輸物件每秒鐘標示為中途的次數。 造成記憶體內部複本與儲存於 **tempdb** 中的複本不同的第一次修改將傳輸物件標示為中途。 當 [!INCLUDE[ssSB](../../includes/sssb-md.md)] 必須記錄對話之訊息傳輸的狀態變更時，會修改傳輸物件。|  
|**Tran Object Writes/sec**|傳輸物件批次每秒鐘寫入 **tempdb** 工作資料表的次數。 寫入次數很多時，可能表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 記憶體的負荷很大。|  
  
## <a name="see-also"></a>另請參閱  
 [SQL Server 的 Access Methods 物件](../../relational-databases/performance-monitor/sql-server-access-methods-object.md)   
 [SQL Server 的 Memory Manager 物件](../../relational-databases/performance-monitor/sql-server-memory-manager-object.md)   
 [監視資源使用狀況 &#40;System Monitor&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
