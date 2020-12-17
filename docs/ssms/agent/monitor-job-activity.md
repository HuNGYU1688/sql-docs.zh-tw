---
description: 監視作業活動
title: 監視作業活動
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, monitoring
- jobs [SQL Server Agent], monitoring
- monitoring [SQL Server], jobs
- activity monitoring [SQL Server Agent]
- monitoring [SQL Server], SQL Server Agent
- monitoring [SQL Server Agent]
- SQL Server Agent Job Activity Monitor
- SQL Server Agent jobs, monitoring
- performance [SQL Server], jobs
- current activity
ms.assetid: 71cb432b-631d-4b8b-9965-e731b3d8266d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: d5cea1ad802ada770cd5e30c1b6c532741d78e61
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472329"
---
# <a name="monitor-job-activity"></a>監視作業活動
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

您可以使用「[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業活動監視器」，監視 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體上所有已定義作業的目前活動。  
  
## <a name="sql-server-agent-sessions"></a>SQL Server Agent 工作階段  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 每次服務啟動時，Agent 都會建立新的工作階段。 建立新的工作階段時， **msdb** 資料庫中的 **sysjobactivity** 資料表就會填入所有現有的已定義作業。 當 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 重新啟動時，這個資料表會保留作業的上一個活動。 每一個工作階段會記錄從作業開始到完成的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 一般作業活動。 這些工作階段的相關資訊儲存在 **msdb** 資料庫的 **syssessions** 資料表中。  
  
## <a name="job-activity-monitor"></a>作業活動監視器  
「作業活動監視器」可讓您使用 **檢視** sysjobactivity [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]資料表。 您可以檢視伺服器上的所有作業，或者您也可以定義篩選，來限制所顯示的作業數目。 您也可以按一下 [代理程式作業活動] 方格中的資料行標題，排序作業資訊。 例如，選取 [上次執行] 資料行標題時，可以按作業上次執行的順序來檢視作業； 再按一下資料行標題，可切換作業依上次執行日期的遞增或遞減順序來顯示。  
  
使用「作業活動監視器」，您可以執行下列工作：  
  
-   啟動和停止作業。  
  
-   檢視作業屬性。  
  
-   檢視特定作業的記錄。  
  
-   以手動方式重新整理 [代理程式作業活動] 方格中的資訊，或按一下 [檢視重新整理設定] 設定自動重新整理間隔。  
  
當您想要了解有哪些作業已排程執行、目前工作階段期間已執行作業的最後結果，以及找出哪些作業目前執行中或閒置時，便可使用「作業活動監視器」。 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務異常失敗，您可以查看「作業活動監視器」中的先前工作階段，判斷哪些作業原本正在執行中。  
  
若要開啟「作業活動監視器」，請展開「[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 物件總管」中的 [SQL Server Agent]、以滑鼠右鍵按一下 [作業活動監視器]，然後按一下 [檢視作業活動]。  
  
您也可以使用預存程序 **sp_help_jobactivity** 來檢視目前工作階段的作業活動。  
  
## <a name="related-tasks"></a>相關工作  
  
|描述|主題|  
|-|-|   
|描述如何檢視 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業的執行階段狀態。|[View Job Activity](../../ssms/agent/view-job-activity.md)|  
  
## <a name="see-also"></a>另請參閱  
[View Job Activity](../../ssms/agent/view-job-activity.md)  
[sysjobactivity (Transact-SQL)](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
[syssessions (Transact-SQL)](../../relational-databases/system-tables/dbo-syssessions-transact-sql.md)  
[sp_help_jobactivity (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-jobactivity-transact-sql.md)  
