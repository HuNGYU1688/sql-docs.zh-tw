---
title: SQL Server 的 Memory Manager 物件 | Microsoft Docs
description: 了解 Memory Manager 物件，其所提供計數器可在 SQL Server 中監視整體的伺服器記憶體使用量。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Memory Manager
- Memory Manager object
ms.assetid: dbf49000-eeb0-4e9c-a361-5092363920dc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 179cd20f1006565ea4ca6924d568b6da80dbb154
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505628"
---
# <a name="sql-server-memory-manager-object"></a>SQL Server 的 Memory Manager 物件
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Microsoft **中的** Memory Manager [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 物件提供計數器，可監視整體的伺服器記憶體使用狀況。 監視整體的伺服器記憶體使用狀況以估計使用者活動和資源使用狀況，可協助您找出效能瓶頸。 監視 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體所使用的記憶體，有助於判定：  
  
-   是否因為實體記憶體不足，無法將經常存取的資料儲存在快取中，而產生瓶頸。 若記憶體不足， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 必須從磁碟擷取資料。  
  
-   是否可藉由增加更多記憶體，或讓資料快取或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 內部結構可使用更多記憶體，而改善查詢效能。  
  
## <a name="memory-manager-counters"></a>Memory Manager 計數器  
 下表描述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Memory Manager** 計數器。  
  
|SQL Server Memory Manager 計數器|描述|  
|----------------------------------------|-----------------|  
|**Connection Memory (KB)**|指定伺服器用來維護連線的動態記憶體總數。|  
|**Database Cache Memory (KB)**|指定伺服器目前用於資料庫頁面快取的記憶體數量。|  
|**External benefit of memory**| 將記憶體新增至特定快取之性能優勢的內部估計。 此為引擎用來平衡快取之間的記憶體使用量，並且在對具有非預期快取成長的情況下進行疑難排解時很有幫助。 該值根據內部計算呈現為整數。 | 
|**Free Memory (KB)**|指定伺服器目前不使用的已認可記憶體數量。|  
|**Granted Workspace Memory (KB)**|指定目前授權來執行雜湊、排序、大量複製及建立索引作業等類處理序的記憶體總數。|  
|**Lock Blocks**|指定目前伺服器上使用中的鎖定區塊數 (定期重新整理)。 鎖定區塊代表個別的鎖定資源，例如資料表、分頁或資料列。|  
|**Lock Blocks Allocated**|指定目前配置的鎖定區塊數。 當伺服器啟動時，所配置鎖定區塊數加上配置的鎖定擁有者區塊數，將取決於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Locks** 組態選項。 如需更多的鎖定區塊，此數值將會增加。|  
|**Lock Memory (KB)**|指定伺服器用於鎖定的動態記憶體總數。|  
|**Lock Owner Blocks**|指定目前用於伺服器的鎖定擁有者區塊數 (定期重新整理)。 鎖定擁有者區塊代表個別執行緒在物件上的鎖定擁有權。 因此若三個執行緒中的每一個在分頁上都有共用 (S) 鎖定，就會有三個鎖定擁有者區塊。|  
|**Lock Owner Blocks Allocated**|指定目前配置的鎖定擁有者區塊數。 當伺服器啟動時，所配置鎖定擁有者區塊數及配置的鎖定區塊數，將取決於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Locks** 組態選項。 如需更多的鎖定擁有者區塊，此數值將會動態地增加。|  
|**Log Pool Memory (KB)**|伺服器用於記錄檔集區的動態記憶體總數。| 
|**Maximum Workspace Memory (KB)**|指定可用來執行雜湊、排序、大量複製和建立索引作業等處理序的最大記憶體量。|  
|**Memory Grants Outstanding**|指定成功取得工作空間記憶體授權的處理序總數。|  
|**Memory Grants Pending**|指定等候工作空間記憶體授權的處理序總數。|  
|**Optimizer Memory (KB)**|指定伺服器用於查詢最佳化的動態記憶體總數。|  
|**Reserved Server Memory (KB)**|表示伺服器已保留供未來使用的記憶體數量。 這個計數器會顯示最初授與之記憶體 ( **Granted Workspace Memory (KB)** 中所示) 的目前未使用數量。|  
|**SQL Cache Memory (KB)**|指定伺服器用於動態 SQL 快取的動態記憶體總數。|  
|**Stolen Server Memory (KB)**|指定伺服器用於資料庫頁面以外用途的記憶體數量。|  
|**Target Server Memory (KB)**|指出伺服器可用的理想記憶體數量。|  
|**Total Server Memory (KB)**|指定伺服器已使用記憶體管理員認可的記憶體數量。|  
  
## <a name="see-also"></a>另請參閱  
 [監視資源使用量 &#40;系統監視器&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server 的 Buffer Manager 物件](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
[sys.dm_os_performance_counters (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
