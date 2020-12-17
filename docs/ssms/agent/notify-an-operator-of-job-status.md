---
description: 通知操作員作業狀態
title: 通知操作員作業狀態
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- status information [SQL Server], jobs
- jobs [SQL Server Agent], notification options
- SQL Server Agent jobs, status
- jobs [SQL Server Agent], status
- SQL Server Agent jobs, notification options
- notifications [SQL Server], job status
ms.assetid: e7399505-27ac-48d9-a637-73bf92b9df49
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 11ea9a4bff085843b571fbf69df5b79d9c3a9f8a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466449"
---
# <a name="notify-an-operator-of-job-status"></a>通知操作員作業狀態
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主題描述如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL Server 管理物件，以在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中設定通知選項來讓 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 將作業的相關通知傳送給操作員。  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>開始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
如需詳細資訊，請參閱＜ [實作 SQL Server Agent 安全性](../../ssms/agent/implement-sql-server-agent-security.md)＞。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-notify-an-operator-of-job-status"></a>若要通知操作員作業狀態  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的執行個體，然後展開該執行個體。  
  
2.  依序展開 [SQL Server Agent] 和 [作業]、以滑鼠右鍵按一下要編輯的作業，然後選取 [屬性]。  
  
3.  在 **[作業屬性]** 對話方塊中，選取 **[通知]** 頁面。  
  
4.  如果您想以電子郵件通知操作員，請核取 [電子郵件]、從清單選取操作員，然後選取下列其中一個選項：  
  
    -   **[當作業成功時]** 即可在作業順利完成時，通知操作員。  
  
    -   **[當作業失敗時]** 會在作業未順利完成時通知操作員。  
  
    -   **[作業完成時]** 可在無論完成狀態為何，都通知操作員。  
  
5.  如果您想以呼叫器來通知操作員，請核取 **[呼叫器]**、從清單選取操作員，然後選取下列其中一個選項：  
  
    -   **[當作業成功時]** 即可在作業順利完成時，通知操作員。  
  
    -   **[當作業失敗時]** 會在作業未順利完成時通知操作員。  
  
    -   **[作業完成時]** 可在無論完成狀態為何，都通知操作員。  
  
6.  如果您想以網路傳送的方式來通知操作員，請核取 **[網路傳送]**、從清單選取操作員，然後選取下列其中一個選項：  
  
    -   **[當作業成功時]** 即可在作業順利完成時，通知操作員。  
  
    -   **[當作業失敗時]** 會在作業未順利完成時通知操作員。  
  
    -   **[作業完成時]** 可在無論完成狀態為何，都通知操作員。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-notify-an-operator-of-job-status"></a>若要通知操作員作業狀態  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    -- adds an e-mail notification for the specified alert (Test Alert).  
    -- This example assumes that Test Alert already exists
    --  and that François Ajenstat is a valid operator name.  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_notification   
    @alert_name = N'Test Alert',   
    @operator_name = N'François Ajenstat',   
    @notification_method = 1 ;  
    GO  
    ```  
  
如需詳細資訊，請參閱 [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理物件  
**若要通知操作員作業狀態**  
  
透過所選的程式語言，例如 Visual Basic、Visual C# 或 PowerShell，使用 **Job** 類別。 如需詳細資訊，請參閱 [SQL Server 管理物件 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
