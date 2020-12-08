---
title: SQL Server XTP 資料指標 | Microsoft Docs
description: 了解 SQL Server XTP 資料指標效能物件，包含與記憶體內 OLTP 引擎資料指標相關的計數器。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 84bf4654-3ef7-4d7f-a269-c8bb4ed4acad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 39c491efc7b0001b7f6b44bda127e7b128c31851
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505522"
---
# <a name="sql-server-xtp-cursors"></a>SQL Server XTP 資料指標
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  SQL Server XTP 資料指標效能物件包含與記憶體內 OLTP 引擎資料指標相關的計數器。 資料指標是記憶體內 OLTP 引擎用來處理 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查詢的低階建置組塊。 因此，您通常不會有這些指標的直接控制權。  
  
 下表描述 **SQL Server XTP Cursors** 計數器。  
  
|計數器|描述|  
|-------------|-----------------|  
|**資料指標刪除/秒**|(平均) 每秒資料指標刪除數目。|  
|**資料指標插入/秒**|(平均) 每秒資料指標插入數目。|  
|**啟動的資料指標掃描/秒**|(平均) 每秒啟動的資料指標掃描次數。|  
|**資料指標唯一違規/秒**|(平均) 每秒唯一約束違規數目。|  
|**資料指標更新/秒**|(平均) 每秒資料指標更新數目。|  
|**資料指標寫入衝突/秒**|(平均) 每秒寫至相同資料列版本的寫入 - 寫入衝突數目。|  
|**塵封角落掃描重試次數/秒 (由使用者發出)**|使用者完整資料表掃描發出的塵封角落掃掠期間，(平均) 每秒因為發生寫入衝突而進行掃描的重試次數。 這是層級非常低的計數器，非供客戶使用。|  
|**移除的過期資料列/秒**|(平均) 每秒由資料指標移除的過期資料列數。|  
|**接觸到的過期資料列/秒**|(平均) 每秒資料指標接觸到的過期資料列數。|  
|**傳回的資料列/秒**|(平均) 每秒由資料指標傳回的資料列數。|  
|**接觸到的資料列/秒**|(平均) 每秒資料指標接觸到的資料列數。|  
|**接觸到的暫訂刪除資料列/秒**|(平均) 每秒資料指標接觸到的將過期資料列數。 如果刪除資料列的交易仍為使用中 (也就是尚未認可或中止)，則資料列即將過期。|  
  
## <a name="see-also"></a>另請參閱  
 [SQL Server XTP &#40;記憶體內部 OLTP&#41; 效能計數器](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
