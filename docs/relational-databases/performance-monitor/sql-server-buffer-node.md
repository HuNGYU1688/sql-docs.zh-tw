---
title: SQL Server:Buffer Node | Microsoft Docs
description: 了解 Buffer Node 物件，其提供計數器來監視每個 NUMA 節點的 SQL Server 緩衝集區頁面散發。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Buffer Node
- Buffer Node object
ms.assetid: fd3f9f0f-7c38-4cfd-a0c5-ee93dd52d9a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: eb86d3734d7e2cf7799940837f6d3677fb53c89e
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505827"
---
# <a name="sql-serverbuffer-node"></a>SQL Server:Buffer Node
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  **Buffer Node** 物件提供用來補充 **Buffer Manager** 物件所提供之計數器的計數器。 它可讓您監視每個非統一記憶體存取 (NUMA) 節點的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 緩衝集區頁面散發。 每個 NUMA 節點都會使用 **Buffer Node** 物件執行個體。 在非 NUMA 架構上，會有單一 **Buffer Node** 物件的執行個體。  
  
## <a name="buffer-node-performance-objects"></a>Buffer Node 效能物件  
 下表描述 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **Buffer Node** 效能物件。  
  
|SQL Server Buffer Node 計數器|描述|  
|-------------------------------------|-----------------|  
|**Database pages**|指出在這個具有資料庫內容之節點上的緩衝集區頁面數。|  
|**Page life expectancy**|指出頁面停留在這個沒有參考之節點的緩衝集區中的秒數下限。|  
|**Local Node page lookups/sec**|指出來自這個節點而且由這個節點滿足的查閱要求數目。|  
|**Remote Note page lookups/sec**|指出來自這個節點但是由其他節點滿足的查閱要求數目。|  
  
 如果 SQL Server 是在非 NUMA 硬體上執行，則 **Buffer Node** 和 **Buffer Manager** 物件的計數器應該相符。  
  
 在 NUMA 硬體上，所有 **Buffer Nodes** 之對應計數器的總和應符合它們與 **Buffer Manager** 的對應計數器總和。  
  
> [!NOTE]  
>  因為受限於計數器的動態本質和取樣精確度，所以計數器值和總和可能不會完全相符。  
  
## <a name="see-also"></a>另請參閱  
 [SQL Server 的 Buffer Manager 物件](../../relational-databases/performance-monitor/sql-server-buffer-manager-object.md)   
 [伺服器記憶體伺服器組態選項](../../database-engine/configure-windows/server-memory-server-configuration-options.md)   
 [監視資源使用量 &#40;系統監視器&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [sys.dm_os_performance_counters &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-performance-counters-transact-sql.md)  
  
  
