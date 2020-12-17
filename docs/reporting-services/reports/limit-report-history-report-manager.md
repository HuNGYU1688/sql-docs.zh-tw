---
title: 限制報表記錄 - Reporting Services | Microsoft Docs
description: 了解如何設定報表伺服器的報表記錄。 同時了解如何設定特定報表的報表記錄。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- viewing report history
- report history [Reporting Services], viewing history
- report history [Reporting Services], configuring history
- historical data [Reporting Services]
- displaying report history
ms.assetid: 8e255792-d9ef-496f-a26c-9e969c1209a0
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d01a008e00d2effccb109555799bbe6d55baf1e3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466569"
---
# <a name="limit-report-history---reporting-services"></a>限制報表記錄 - Reporting Services
  報表記錄是您在經過一段時間後建立之報表快照集的集合。 您可以視需要建立報表記錄，或是排程在報表記錄中建立和加入快照集的頻率。  
  
 報表記錄會儲存在報表伺服器資料庫中。 若報表快照集包含大量資料，建議您考慮限制報表記錄，以將快照集保留對資料庫大小的影響降到最低。  

::: moniker range="=sql-server-2016"
  
## <a name="to-configure-report-history-for-a-report-server"></a>若要設定報表伺服器的報表記錄  
  
1.  在報表管理員中，按一下全域工具列上的 [網站設定]  。  
  
2.  若要無限地保存所有報表記錄，請選取 [不限制報表記錄中的快照集數目]  。 否則，請選取 [限制報表記錄的副本]  ，指定可以為任一給定報表保存的快照集最大數目。  
  
3.  按一下 [套用]  。  
  
## <a name="to-configure-report-history-for-a-specific-report"></a>若要設定特定報表的報表記錄  
  
1.  在報表管理員中，導覽至您要設定記錄的報表，然後按一下該報表來開啟它。  
  
2.  按一下 [屬性]  索引標籤。  
  
3.  按一下 [記錄]  索引標籤。  
  
4.  選取報表的選項，然後按一下 [套用]  。 如需每個選項的詳細資訊，請參閱[快照集選項屬性頁 &#40;報表管理員&#41;](/previous-versions/sql/sql-server-2016/ms189952(v=sql.130))。  
  
## <a name="see-also"></a>另請參閱  
 [將快照集加入報表記錄 &#40;報表管理員&#41;](../../reporting-services/report-server/add-a-snapshot-to-report-history-report-manager.md)   
 [報表管理員 &#40;SSRS 原生模式&#41;](../web-portal-ssrs-native-mode.md)  

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="to-configure-report-history-for-a-report-server"></a>若要設定報表伺服器的報表記錄  
  
1.  在入口網站中，按一下全域工具列上的 [網站設定]  。  
  
2.  若要無限地保存所有報表記錄，請選取 [不限制報表記錄中的快照集數目]  。 否則，請選取 [限制報表記錄的副本]  ，指定可以為任一給定報表保存的快照集最大數目。  
  
3.  按一下 [套用]  。  
  
## <a name="to-configure-report-history-for-a-specific-report"></a>若要設定特定報表的報表記錄  
  
1.  在入口網站中，導覽至您要設定記錄的報表，然後按一下該報表來開啟它。  
  
2.  按一下 [屬性]  索引標籤。  
  
3.  按一下 [記錄]  索引標籤。  
  
4.  選取報表的選項，然後按一下 [套用]  。 如需每個選項的詳細資訊，請參閱[快照集選項屬性頁](/previous-versions/sql/sql-server-2016/ms189952(v=sql.130))。  
  
## <a name="see-also"></a>另請參閱  
 [將快照集加入至報表記錄](../../reporting-services/report-server/add-a-snapshot-to-report-history-report-manager.md)   

::: moniker-end
