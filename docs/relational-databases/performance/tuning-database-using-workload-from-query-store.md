---
title: 使用查詢存放區的工作負載調整資料庫 | Microsoft 文件
description: Database Engine Tuning Advisor 支援使用查詢存放區自動選取適當工作負載以進行微調的選項。
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Database Engine Tuning Advisor, Query Store
ms.assetid: 17107549-5073-4fa2-8ee7-5ed33b38821e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 64c730f4559173f1ac8dcf1817c688dcb11e3ce4
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504942"
---
# <a name="tuning-database-using-workload-from-query-store"></a>使用查詢存放區的工作負載調整資料庫
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的[查詢存放區](../../relational-databases/performance/how-query-store-collects-data.md)功能可自動擷取查詢、計劃和執行階段統計資料的歷程記錄，並將這項資訊保存在資料庫中。 [Database Engine Tuning Advisor (DTA)](../../relational-databases/performance/database-engine-tuning-advisor.md) 支援新選項，可使用查詢存放區自動選取適當的工作負載以進行調整。 對於許多使用者而言，這可消除明確收集工作負載進行調整的需要。 只有已開啟「查詢存放區」功能的資料庫才能使用這項功能。 
  
此功能可在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **v16.4** 或更高版本中取得。 
  
## <a name="how-to-tune-a-workload-from-query-store-in-database-engine-tuning-advisor-gui"></a>如何在 Database Engine Tuning Advisor GUI 中調整查詢存放區的工作負載
從 DTA GUI 中，選取 [一般]窗格中的 [查詢存放區] 選項按鈕，即可啟用此功能 (請參閱下圖)。

![查詢存放區的 DTA 工作負載](../../relational-databases/performance/media/dta-workload-from-query-store.gif)
 
## <a name="how-to-tune-a-workload-from-query-store-in-dtaexe-command-line-utility"></a>如何在 dta.exe 命令列公用程式中調整查詢存放區的工作負載
從命令列 (dta.exe) 中，選擇 **-iq** 選項，以選取查詢存放區的工作負載。 

有另外兩個選項可透過命令列，協助您從查詢存放區中選取工作負載時，調整 DTA 的行為。 透過 GUI 無法使用這些選項：
  1. **要調整的工作負載事件數目**：使用 **-n** 命令列引數指定的這個選項，可讓使用者控制查詢存放區中調整的事件數目。 根據預設，DTA 會針對此選項使用 1000 這個值。 DTA 一律會選擇總持續期間最耗費資源的事件數。 
  
  2. **要調整的事件時間範圍**：因為查詢存放區可能包含很久以前執行的查詢，所以此選項可讓使用者指定過去的時間範圍 (以時數為單位)，只有在此時間範圍執行的查詢，DTA 才會考慮進行調整。 您可以使用 **-I** 命令列引數來指定此選項。 

如需詳細資訊，請參閱 [dta 公用程式](../../tools/dta/dta-utility.md)。

## <a name="difference-between-using-workload-from-query-store-and-plan-cache"></a>使用查詢存放區與計畫快取的工作負載之間的差異 
查詢存放區和計畫快取選項之間的差異在於，前者包含已針對資料庫執行之查詢的長期記錄，這些查詢會在伺服器重新啟動時持續保存。 另一方面，計劃快取只包含最近執行查詢 (其計劃已快取到記憶體中) 的子集。 當伺服器重新啟動時，就會捨棄計畫快取中的項目。

## <a name="see-also"></a>另請參閱  
[Database Engine Tuning Advisor](../../relational-databases/performance/database-engine-tuning-advisor.md)     
[教學課程：Database Engine Tuning Advisor](../../tools/dta/tutorial-database-engine-tuning-advisor.md)        
[查詢存放區如何收集資料](../../relational-databases/performance/how-query-store-collects-data.md)     
[查詢存放區的最佳做法](../../relational-databases/performance/best-practice-with-the-query-store.md)
