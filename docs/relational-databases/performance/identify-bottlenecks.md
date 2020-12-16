---
title: 找出瓶頸 | Microsoft Docs
description: 了解瓶頸的原因，例如 SQL Server 中的資源不足，以及資源無法正常運作/設定不正確。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- resource bottlenecks [SQL Server]
- database monitoring [SQL Server], bottlenecks
- performance [SQL Server], bottlenecks
- tuning databases [SQL Server], bottlenecks
- monitoring server performance [SQL Server], bottlenecks
- monitoring performance [SQL Server], bottlenecks
- database performance [SQL Server], bottlenecks
- server performance [SQL Server], bottlenecks
- bottlenecks [SQL Server]
- identifying bottlenecks [SQL Server]
ms.assetid: db079e65-ee80-4105-aec9-f8230d0d6635
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 66a29131862d0b7cb3972fd8a52b5fa21ab79ef3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463269"
---
# <a name="identify-bottlenecks"></a>找出瓶頸
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  同時存取共用資源會產生瓶頸。 通常，每個軟體系統中都會有瓶頸存在，而且這是無法避免的。 但是，過量要求共用資源會讓回應時間變差，這時就必須找出問題並進行微調。  
  
 導致瓶頸的原因包括：  
  
-   需要額外或升級元件的資源不足。  
  
-   相同類型的資源，其工作負載分配不均 (例如，某個磁碟被獨占)。  
  
-   資源的功能有問題。  
  
-   資源的組態設定不正確。  
  
## <a name="analyzing-bottlenecks"></a>分析瓶頸  
 各種事件的持續時間過長，是發生瓶頸的指標，這種情況便可進行微調。  
  
 例如：  
  
-   可能是其他元件防止負載到達這個元件，因此增加完成負載的時間。  
  
-   可能是網路壅塞使得用戶端的要求更為耗時。  
  
 在追蹤伺服器效能以找出瓶頸時，有下列五個主要區域需要監視。  
  
|可能瓶頸區|對伺服器的影響|  
|------------------------------|---------------------------|  
|記憶體使用量|如果配置到 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的記憶體或其可用記憶體不足，會使效能降低。 這時必須從磁碟讀取資料，而不是直接從資料快取讀取。 當需要分頁時，Microsoft Windows 作業系統需與磁碟交換資料，因此會進行過度的分頁動作。|  
|CPU 使用率|CPU 使用率若長期偏高，表示 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查詢需要進行微調，或需要升級 CPU。|  
|磁碟輸入/輸出 (I/O)|[!INCLUDE[tsql](../../includes/tsql-md.md)] 可以微調查詢，以減少不必要的 I/O；例如利用索引。|  
|使用者連線|過多的使用者同時存取伺服器，會造成效能降低。|  
|封鎖的鎖定|設計錯誤的應用程式會造成鎖定與阻礙同時發生，而導致回應時間變長，以及交易輸送速度變慢。|  
  
## <a name="see-also"></a>另請參閱  
 [監視 CPU 使用量](../../relational-databases/performance-monitor/monitor-cpu-usage.md)   
 [監視磁碟使用量](../../relational-databases/performance-monitor/monitor-disk-usage.md)   
 [監視記憶體使用量](../../relational-databases/performance-monitor/monitor-memory-usage.md)   
 [SQL Server 的 General Statistics 物件](../../relational-databases/performance-monitor/sql-server-general-statistics-object.md)   
 [SQL Server 的 Locks 物件](../../relational-databases/performance-monitor/sql-server-locks-object.md)  
  
  
