---
title: 擴充事件工具
description: 這些工具會建立和管理 SQL Server 擴充事件工作階段。 除了所建立的工作階段以外，伺服器上也會有預設系統健康狀態工作階段。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
helpviewer_keywords:
- extended events [SQL Server], using
- extended events [SQL Server], options for using
ms.assetid: d312a9ff-50ba-4721-baef-50bfd3169d38
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b0695aed06d62e9dbe687983958ebc9b707ec37d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465559"
---
# <a name="extended-events-tools"></a>擴充事件工具

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  您可以使用下列工具來建立及管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 擴充事件工作階段：  
  
-   資料定義語言 (DDL) 陳述式。 這些可讓您建立及修改「擴充事件」工作階段。  
  
-   動態管理檢視、目錄檢視和系統資料表。 這些可讓您使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式取得工作階段資料和中繼資料。 系統資料表可幫助您判定 SQL 追蹤事件類別與資料行的現有「擴充事件」對等項目。  
  
-   [物件總管] 的 [擴充事件]  節點。 這可讓您啟動、停止或刪除工作階段，或是匯入及匯出工作階段範本。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] PowerShell 提供者。 這是一項功能強大的工具，可讓您用來建立、更改和管理「擴充事件」工作階段。 如需詳細資訊，請參閱 [針對擴充事件使用 PowerShell 提供者](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)。  
  
-   第 1 課：建立 Windows Azure 儲存體物件[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。 這可讓您建立及執行「擴充事件」主題中所提供的程式碼範例。 如需詳細資訊，請參閱 [物件總管](../../ssms/object/object-explorer.md)。  
  
 除了您所建立的工作階段以外，伺服器上也會有預設系統健康工作階段存在。 此工作階段會收集系統資料，讓您能夠用來協助排除效能問題。 如需詳細資訊，請參閱 [使用 system_health 工作階段](../../relational-databases/extended-events/use-the-system-health-session.md)。  
  
## <a name="ddl-statements"></a>DDL 陳述式  
 使用下列 DDL 陳述式，以建立、變更和卸除「擴充事件」工作階段。  
  
|名稱|描述|  
|----------|-----------------|  
|[CREATE EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/create-event-session-transact-sql.md)|建立「擴充事件」工作階段物件，此物件可識別事件的來源、事件工作階段目標及事件工作階段參數。|  
|[ALTER EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-event-session-transact-sql.md)|啟動或停止事件工作階段，或是變更事件工作階段組態。|  
|[DROP EVENT SESSION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-event-session-transact-sql.md)|卸除事件工作階段。|  
  
## <a name="catalog-views"></a>目錄檢視  
 使用下列目錄檢視，以取得當您建立事件工作階段時所建立的中繼資料。  
  
|名稱|描述|  
|----------|-----------------|  
|[sys.server_event_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-sessions-transact-sql.md)|列出所有事件工作階段定義。|  
|[sys.server_event_session_actions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-actions-transact-sql.md)|針對事件工作階段之每個事件的每個動作傳回資料列。|  
|[sys.server_event_session_events &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-events-transact-sql.md)|針對事件工作階段中的每個事件傳回資料列。|  
|[sys.server_event_session_fields &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-fields-transact-sql.md)|針對事件和目標上明確設定的每一個可自訂資料行傳回資料列。|  
|[sys.server_event_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-event-session-targets-transact-sql.md)|傳回事件工作階段中每一個事件目標的資料列。|  
  
## <a name="dynamic-management-views"></a>動態管理檢視  
 使用下列動態管理檢視來取得工作階段中繼資料和工作階段資料。 中繼資料是從目錄檢視中取得，而當您啟動和執行事件工作階段時，會建立工作階段資料。  
  
> [!NOTE]  
>  這些檢視要等到工作階段啟動之後，才會包含工作階段資料。  
  
|名稱|描述|  
|----------|-----------------|  
|[sys.dm_os_dispatcher_pools &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-dispatcher-pools-transact-sql.md)|傳回有關工作階段發送器集區的資訊。|  
|[sys.dm_xe_objects &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-objects-transact-sql.md)|針對事件封裝所公開的每個物件，各傳回一個資料列。|  
|[sys.dm_xe_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-object-columns-transact-sql.md)|傳回所有物件的結構描述資訊。|  
|[sys.dm_xe_packages &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-packages-transact-sql.md)|列出已向「擴充事件」引擎註冊的所有封裝。|  
|[sys.dm_xe_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-sessions-transact-sql.md)|傳回使用中「擴充事件」工作階段的相關資訊。|  
|[sys.dm_xe_session_targets &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-targets-transact-sql.md)|會傳回工作階段目標的相關資訊。|  
|[sys.dm_xe_session_events &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-events-transact-sql.md)|傳回有關工作階段事件的資訊。|  
|[sys.dm_xe_session_event_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-event-actions-transact-sql.md)|傳回有關事件工作階段動作的資訊。|  
|[sys.dm_xe_map_values &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-map-values-transact-sql.md)|提供內部數值索引鍵與人們可讀取之文字的對應。|  
|[sys.dm_xe_session_object_columns &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-xe-session-object-columns-transact-sql.md)|顯示繫結至工作階段之物件的組態值。|  
  
## <a name="system-tables"></a>系統資料表  
 使用下列系統資料表，取得有關 SQL 追蹤事件類別與資料行之「擴充事件」對等項目的資訊。  
  
|名稱|描述|  
|----------|-----------------|  
|[trace_xe_event_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-event-map.md)|針對對應至 SQL 追蹤事件類別的每個「擴充事件」事件包含一個資料列。|  
|[trace_xe_action_map &#40;Transact-SQL&#41;](../../relational-databases/system-tables/extended-events-tables-trace-xe-action-map.md)|針對對應到 SQL 追蹤資料行識別碼的每個擴充事件動作包含一個資料列。|  
  
## <a name="see-also"></a>另請參閱  
 [動態管理檢視與函數 &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [SQL Server 擴充的事件目錄資料表 &#40;Transact-SQL&#41;](../system-tables/system-tables-transact-sql.md)   
 [使用 system_health 工作階段](../../relational-databases/extended-events/use-the-system-health-session.md)   
 [針對擴充事件使用 PowerShell 提供者](../../relational-databases/extended-events/use-the-powershell-provider-for-extended-events.md)  
  
