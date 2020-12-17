---
description: 警示屬性 - 新增警示 (一般頁面)
title: 警示屬性 - 新增警示 (一般頁面)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.general.f1
ms.assetid: f5c11610-62e3-44df-9800-a5dc35be4a09
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 18cbd660ed24526331aed570415eccd314127c73
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472439"
---
# <a name="alert-properties---new-alert-general-page"></a>警示屬性 - 新增警示 (一般頁面)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此頁面來檢視和修改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 警示的一般屬性。  

## <a name="options"></a>選項。  
**名稱**  
變更警示的名稱。  
  
**啟用**  
啟用警示。 若警示尚未啟用，警示中所指定的動作就不會發生。  
  
**型別**  
選取警示的類型：  
  
-   **SQL Server 事件警示** 會回應 [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows 事件記錄檔中的訊息。  
  
-   **SQL Server 效能條件警示** 會回應效能計數器中的特定條件。  
  
-   **WMI 事件警示** 會回應 Windows Management Instrumentation (WMI) 事件。  
  
## <a name="sql-server-event-alert-options"></a>SQL Server 事件警示選項  
**資料庫名稱**  
不論事件發生所在的資料庫為何，針對該事件指定一個資料庫或 **所有資料庫** ，來回應訊息。  
  
**錯誤號碼**  
指定此事件會回應錯誤，並指定錯誤號碼。  
  
**嚴重性**  
指定此事件會回應具特定嚴重性層級的任何訊息，並指定嚴重性層級。  
  
**訊息包含下列內容時引發警示**  
依特定字串來篩選事件。 若選取此選項，警示就只會回應包含特定字串的事件。  
  
**訊息文字**  
指定要用來篩選事件的字串。  
  
## <a name="sql-server-performance-condition-alerts"></a>SQL Server 效能條件警示  
**Object**  
指定要監視的效能物件。  
  
**計數器**  
指定要監視之效能物件內的計數器。  
  
**執行個體**  
指定要監視之計數器的執行個體。  
  
**發出警示的時機為計數器達**  
指定警示要回應的計數器行為。 例如，當 [Free space in tempdb (KB)] 計數器的值低於某個值時，您可能會希望警示針對如此的條件做出回應，或者當 [SQL Compilations/sec] 高於某個值時，您可能會希望警示針對如此的條件做出回應。  
  
**值**  
指定計數器的值。  
  
## <a name="wmi-event-alert-options"></a>WMI 事件警示選項  
**Namespace**  
指定針對 WMI 查詢語言 (WQL) 陳述式使用的命名空間。 僅支援執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 之電腦上的命名空間。  
  
**查詢**  
指定會識別警示所回應之事件的 WQL 陳述式。  
  
## <a name="see-also"></a>另請參閱  
[警示](../../ssms/agent/alerts.md)  
[搭配伺服器事件的 WMI 提供者使用 WQL](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md)  
[使用錯誤號碼建立警示](../../ssms/agent/create-an-alert-using-an-error-number.md)  
[Create an Alert Using Severity Level](../../ssms/agent/create-an-alert-using-severity-level.md)  
