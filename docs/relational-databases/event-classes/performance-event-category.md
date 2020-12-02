---
description: Performance 事件類別目錄
title: Performance 事件類別目錄 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- SQL Server event classes, Performance event category
- Performance event category [SQL Server]
- event classes [SQL Server], Performance event category
ms.assetid: 708f3585-d8be-4980-bbff-672d7c59397e
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b652324f42375bd1de7161ac596f13be13891475
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88499416"
---
# <a name="performance-event-category"></a>Performance 事件類別目錄
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  使用 **Performance** 事件類別目錄，可以監視 **Showplan** 事件類別，以及經由 SQL 資料操作語言 (DML) 運算子的執行而產生的事件類別。  
  
## <a name="in-this-section"></a>本節內容  
  
|主題|描述|  
|-----------|-----------------|  
|[Auto Stats 事件類別](../../relational-databases/event-classes/auto-stats-event-class.md)|指出已自動更新索引和資料行統計資料。|  
|[Degree of Parallelism &#40;7.0 Insert&#41; 事件類別](../../relational-databases/event-classes/degree-of-parallelism-7-0-insert-event-class.md)|指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已經使用序列或平行計畫來執行 SELECT、INSERT、UPDATE 或 DELETE 陳述式。 此外，系統也會回報用來執行此作業的 CPU 數目。|  
|[Performance Statistics 事件類別](../../relational-databases/event-classes/performance-statistics-event-class.md)|監視執行中的查詢效能。|  
|[Showplan All 事件類別](../../relational-databases/event-classes/showplan-all-event-class.md)|識別 SQL 陳述式中的 **Showplan** 運算子。|  
|[Showplan All for Query Compile 事件類別](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md)|顯示 **Showplan** 運算子的編譯時間資料。|  
|[Showplan Statistics Profile 事件類別](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md)|顯示查詢的估計成本。|  
|[Showplan XML 事件類別](../../relational-databases/event-classes/showplan-xml-event-class.md)|識別 SQL 陳述式中的 **Showplan** 運算子。 事件類別會以定義完善的 XML 文件來儲存每一個事件。|  
|[Showplan XML For Query Compile 事件類別](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md)|以 XML 格式顯示 **Showplan** 運算子的編譯時間資料。|  
|[Showplan XML Statistics Profile 事件類別](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md)|識別與 SQL 陳述式相關聯的 **Showplan** 運算子。 輸出是 XML 文件。|  
|[SQL:FullTextQuery 事件類別](../../relational-databases/event-classes/sql-fulltextquery-event-class.md)|指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 已執行全文檢索查詢。|  
|[Plan Guide Successful 事件類別](../../relational-databases/event-classes/plan-guide-successful-event-class.md)|指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 成功針對包含計畫指南的查詢或批次產生了執行計畫。|  
|[Plan Guide Unsuccessful 事件類別](../../relational-databases/event-classes/plan-guide-unsuccessful-event-class.md)|指出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 無法針對包含計畫指南的查詢或批次產生執行計畫。|  
  
## <a name="see-also"></a>另請參閱  
 [擴充事件](../../relational-databases/extended-events/extended-events.md)  
  
  
