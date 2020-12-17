---
title: 效能計數器 MSRS 2011 SharePoint 模式效能物件 | Microsoft Docs
description: 了解 MSRS 2011 Web 服務 SharePoint 模式和 MSRS 2011 Windows 服務 SharePoint 模式效能物件的效能計數器。
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- performance counters [Reporting Services]
- counters [Reporting Services]
- Report Server Windows service, performance counters
- RS Windows Service performance object
- Scheduling and Delivery Processor performance object [Reporting Services]
- performance [Reporting Services]
ms.assetid: 70bf6980-7845-4ab5-8b2a-ebf526d811a6
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: 95742efced8c5b5704863d2582c6ca80c5b227e7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440479"
---
# <a name="performance-counters-msrs-2011-sharepoint-mode-performance-objects"></a>效能計數器 MSRS 2011 SharePoint 模式效能物件
  本主題描述屬於 **SharePoint 模式部署一部分之** MSRS 2011 Web 服務 SharePoint 模式 **和** MSRS 2011 Windows 服務 SharePoint 模式 [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] 效能物件的效能計數器。  
  
> [!NOTE]  
>  此效能物件會監視本機報表伺服器的事件。 如果您是在向外延展部署中執行報表伺服器，則計數會套用到目前的伺服器，而非整個向外延展部署。  
  
 Windows 效能監視器 (**Perfmon.exe**) 中提供了效能物件。 如需詳細資訊，請參閱 Windows 文件集。 [執行階段分析](/dotnet/framework/debug-trace-profile/runtime-profiling) (https://msdn.microsoft.com/library/w4bz2147.aspx) 。  
  
 如需效能計數器和原生模式報表伺服器的資訊，請參閱 [MSRS 2011 Web Service 和 MSRS 2011 Windows 服務效能物件的效能計數器 &#40;原生模式&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md) 和 [MSRS 2011 Web Service SharePoint 模式和 MSRS 2011 Windows 服務 SharePoint 模式效能物件的效能計數器 (SharePoint 模式)](../../reporting-services/report-server/performance-counters-msrs-2011-sharepoint-mode-performance-objects.md)。  
  
 本主題內容：  
  
-   [MSRS 2011 Web 服務 SharePoint 模式效能計數器](#bkmk_webservice)  
  
-   [MSRS 2011 Windows 服務 SharePoint 模式效能計數器](#bkmk_windowsservice)  
  
-   [使用 PowerShell 指令程式傳回清單](#bkmk_powershell)  
  
##  <a name="msrs-2011-web-service-sharepoint-mode-performance-counters"></a><a name="bkmk_webservice"></a> MSRS 2011 Web 服務 SharePoint 模式效能計數器  
 **MSRS 2011 Web 服務 SharePoint 模式** 效能物件會監視報表伺服器效能。 此效能物件包含一組計數器集合，用來追蹤通常透過互動式報表檢視作業所起始的報表伺服器處理。 當您設定此計數器時，可以將此計數器套用至 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的所有執行個體，也可以選取特定的執行個體 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，便會重設這些計數器。  
  
 下表將列出 **MSRS 2011 Web 服務 SharePoint 模式** 效能物件所包含的計數器。  
  
|計數器|說明|  
|-------------|-----------------|  
|**Active Sessions**|使用中的工作階段數目。 此計數器提供從報表執行產生之所有瀏覽器工作階段的累積計數，不管這些工作階段是否仍然使用中。<br /><br /> 隨著工作階段記錄移除，此計數器會遞減。 根據預設，工作階段會在沒有活動的十分鐘後移除。|  
|**Cache Hits/Sec**|快取報表每秒的要求數目。 這些要求是針對重新轉譯的報表，而不是針對直接從快取處理的報表 (請參閱本主題稍後的 **Total Cache Hits** )。|  
|**Cache Hits/Sec (Semantic Models)**|快取模型每秒的要求數目。 這些要求是針對重新轉譯的報表，而不是針對直接從快取處理的報表|  
|**Cache Misses/Sec**|每秒無法從快取傳回報表的要求數目。 使用此計數器以了解用於快取 (磁碟或記憶體) 的資源是否足夠。|  
|**Cache Misses/Sec (Semantic Models)**|每秒無法從快取傳回模型的要求數目。 使用此計數器以了解用於快取 (磁碟或記憶體) 的資源是否足夠。|  
|**First Session Requests/Sec**|每秒從報表伺服器快取啟動的新使用者工作階段數目。|  
|**Memory Cache Hits/Sec**|每秒從記憶體快取中擷取報表的次數。 *「記憶體內部快取」* (In-Memory Cache) 是快取的一部分，它將報表儲存在 CPU 記憶體中。 使用記憶體內部快取時，報表伺服器不會查詢 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以取得快取內容。|  
|**Memory Cache Misses/Sec**|每秒無法從記憶體中快取擷取報表的次數。|  
|**Next Session Requests/Sec**|在現有的工作階段中開啟之報表 (例如，從工作階段快照集轉譯的報表) 的每秒要求數目。|  
|**Report Requests**|目前使用中和報表伺服器所處理的報表數目。|  
|**Reports Executed/Sec**|每秒成功執行報表的數目。 此計數器提供有關報表數量的統計資料。 您可以使用此計數器搭配 **Request/Sec** 來比較報表執行與可從快取傳回的報表要求。|  
|**Requests/Sec**|每秒對報表伺服器產生的要求數目。 此計數器追蹤報表伺服器所處理之所有類型的要求。|  
|**Total Cache Hits**|服務啟動之後，來自快取之報表要求的總數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。|  
|**Total Cache Hits (Semantic Models)**|服務啟動之後，來自快取之模型要求的總數。 每當 ASP.NET 停止報表伺服器 Web 服務時，此計數器就會重設。|  
|**Total Cache Misses**|服務啟動之後，無法從快取傳回報表的總次數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。 您可以使用此計數器來判斷磁碟空間與記憶體是否足夠。|  
|**Total Cache Misses (Semantic Models)**|服務啟動之後，無法從快取傳回模型的總次數。 每當 ASP.NET 停止報表伺服器 Web 服務時，此計數器就會重設。 您可以使用此計數器來判斷磁碟空間與記憶體是否足夠。|  
|**Total Memory Cache Hits**|服務啟動之後，從記憶體中快取傳回之快取報表的總數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。 *「記憶體內部快取」* (In-Memory Cache) 是快取的一部分，它將報表儲存在 CPU 記憶體中。 使用記憶體內部快取時，報表伺服器不會查詢 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以取得快取內容。|  
|**Total Memory Cache Misses**|啟動服務之後，針對記憶體中快取的遺漏總數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。|  
|**Total Processing Failures**|報表伺服器 Web 服務要求處理中的錯誤數。|  
|**Total Rejected Threads**|拒絕非同步處理之執行緒，以及後續在同一執行緒中當成同步處理之執行緒的總數。 每一個資料來源在一個執行緒上處理。 如果執行緒的數量超出容量，就會拒絕執行緒的非同步處理，然後以序列方式處理。|  
|**Total Reports Executed**|啟動服務之後，成功執行的報表總數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。|  
|**要求總數**|服務啟動之後，對報表伺服器進行之所有要求的總數。 每當 [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] 停止報表伺服器 Web 服務時，此計數器就會重設。|  
  
##  <a name="msrs-2011-windows-service-sharepoint-mode-performance-counters"></a><a name="bkmk_windowsservice"></a> MSRS 2011 Windows 服務 SharePoint 模式效能計數器  
 **MSRS 2011 Windows 服務 SharePoint 模式** 效能物件可用來監視報表伺服器 Windows 服務。 此效能物件包含一組計數器集合，用來追蹤透過已排程的作業所起始的報表處理。 排程的作業可能包括訂閱與傳遞、報表執行快照集和報表記錄。 當您設定此計數器時，可以將此計數器套用至 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的所有執行個體，也可以選取特定的執行個體  
  
 下表將列出 **MSRS 2011 Windows 服務 SharePoint 模式** 效能物件所包含的計數器。  
  
|計數器|說明|  
|-------------|-----------------|  
|**Active Sessions**|儲存在報表伺服器資料庫中之使用中工作階段的數目。 此計數器提供從報表訂閱產生之所有可使用瀏覽器工作階段的累積計數，不管這些工作階段是否仍然使用中。|  
|**警示：事件佇列長度**||  
|**警示：已處理的事件數 - CreateSchedule**||  
|**警示：已處理的事件數 - DeleteSchedule**||  
|**警示：已處理的事件數 - DeliverAlert**||  
|**警示：已處理的事件數 - FireAlert**||  
|**警示：已處理的事件數 - FireSchedule**||  
|**警示：已處理的事件數 - GenerateAlert**||  
|**警示：已處理的事件數 - UpdateSchedule**||  
|**Cache Flushes/Sec**|每秒快取排清的數目。|  
|**Cache Hits/Sec**|快取報表每秒的要求數目。 這些要求是針對重新轉譯的報表，而不是針對直接從快取處理的報表 (請參閱本主題稍後的 **Total Cache Hits** )。|  
|**Cache Hits/Sec (Semantic Models)**|快取模型每秒的要求數目。|  
|**Cache Misses/Sec**|每秒無法從快取傳回報表的要求數目。 使用此計數器以了解用於快取 (磁碟或記憶體) 的資源是否足夠。|  
|**Cache Misses/Sec (Semantic Models)**|每秒無法從快取傳回模型的要求數目。 使用此計數器以了解用於快取 (磁碟或記憶體) 的資源是否足夠。|  
|**Delivers/Sec**|每秒來自任何傳遞延伸模組的報表傳遞數目。|  
|**Events/Sec**|每秒處理的事件數目。 監視的事件包括 **SnapshotUpdated** 和 **TimedSubscription**。|  
|**First Session Requests/Sec**|每秒建立的新報表執行工作階段數目。|  
|**Memory Cache Hits/Sec**|每秒從記憶體快取中擷取報表的次數。 *「記憶體內部快取」* (In-Memory Cache) 是快取的一部分，它將報表儲存在 CPU 記憶體中。 使用記憶體內部快取時，報表伺服器不會查詢 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以取得快取內容。|  
|**Memory Cache Misses/Sec**|每秒無法從記憶體中快取擷取報表的次數。|  
|**Next Session Requests/Sec**|在現有的工作階段中開啟之報表 (例如，從工作階段快照集轉譯的報表) 的每秒要求數目。|  
|**Report Requests**|目前使用中和報表伺服器所處理的報表數目。 使用此計數器以評估快取策略。 要求的數目可能比產生的報表多很多。|  
|**Reports Executed/Sec**|每秒成功產生的報表數目。|  
|**Requests/Sec**|報表伺服器服務每秒處理的成功要求總數。|  
|**Snapshot Updates/Sec**|報表執行快照集每秒更新總數。|  
|**Total App Domain Recycles**|報表伺服器 Windows 服務啟動之後，應用程式定義域回收的總數。|  
|**Total Cache Flushes**|啟動服務之後，報表伺服器快取更新的總數。 當應用程式定義域回收時，這個計數器會重設。 請參閱 **Cache Flushes/Sec**。|  
|**Total Cache Hits**|報表伺服器 Windows 服務啟動之後，直接從快取處理報表的要求總數。 當應用程式定義域回收時，這個計數器會重設。 請參閱 **Cache Hits/Sec**。|  
|**Total Cache Hits (Semantic Models)**|報表伺服器 Windows 服務啟動之後，直接從快取處理的模型要求總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Cache Misses**|報表伺服器 Windows 服務啟動之後，無法從快取傳回報表的總次數。 當應用程式定義域回收時，這個計數器會重設。 請參閱 **Cache Misses/Sec**。|  
|**Total Cache Misses (Semantic Models)**|報表伺服器 Windows 服務啟動之後，無法從快取傳回模型的總次數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Deliveries**|排程與傳遞處理器針對所有傳遞延伸模組傳遞的報表總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**事件總數**|報表伺服器 Windows 服務啟動之後的事件總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Memory Cache Hits**|報表伺服器 Windows 服務啟動之後，從記憶體中快取傳回的快取報表總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Memory Cache Misses**|啟動服務之後，針對記憶體中快取的遺漏總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Processing Failures**|報表伺服器 Windows 服務的要求處理錯誤數。|  
|**Total Rejected Threads**|拒絕非同步處理之執行緒，以及後續在同一執行緒中當成同步處理之執行緒的總數。 在一般或重度負荷下，此計數器會穩定地增加。|  
|**Total Reports Executed**|執行的報表總數。|  
|**要求總數**|啟動服務之後，成功執行的報表總數。 當應用程式定義域回收時，這個計數器會重設。|  
|**Total Snapshot Updates**|報表執行快照集更新總數。|  
  
##  <a name="use-powershell-cmdlets-to-return-lists"></a><a name="bkmk_powershell"></a> 使用 PowerShell 指令程式傳回清單  
 ![PowerShell 相關內容](/analysis-services/analysis-services/instances/install-windows/media/rs-powershellicon.jpg "PowerShell 相關內容")下列 Windows PowerShell 指令碼會傳回 CounterSetName 開頭為 "msr" 的計數器集合  
  
```  
get-counter -listset msr*  
Returns a list with the following information  
CounterSetName     : MSRS 2011 Windows Service SharePoint Mode  
CounterSetName     : MSRS 2011 Web Service SharePoint Mode  
```  
  
 以下 Windows PowerShell 指令碼將會傳回 CounterSetName "MSRS 2011 Windows Service SharePoint Mode" 的效能計數器清單。  
  
```  
(get-counter -listset "MSRS 2011 Windows Service SharePoint Mode").paths  
```  
  
## <a name="see-also"></a>另請參閱  
 [監視報表伺服器效能](../../reporting-services/report-server/monitoring-report-server-performance.md)   
 [MSRS 2011 Web Service 和 MSRS 2011 Windows 服務效能物件的效能計數器 &#40;原生模式&#41;](../../reporting-services/report-server/performance-counters-msrs-2011-web-service-performance-objects.md)   
 [ReportServer:Service 和 ReportServerSharePoint:Service 效能物件的效能計數器](../../reporting-services/report-server/performance-counters-reportserver-service-performance-objects.md)  
  
