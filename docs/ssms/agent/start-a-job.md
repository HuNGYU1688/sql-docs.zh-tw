---
description: 啟動作業
title: 啟動作業
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], starting
- SQL Server Agent jobs, starting
- starting jobs
ms.assetid: cec9f7f7-d0a7-4239-9dc5-a69c011ebaa0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 27183ba9d7429e7703e3fba011ad5471d6c192db
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97408828"
---
# <a name="start-a-job"></a>啟動作業
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主題描述如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL Server 管理物件，以在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中開始執行 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業。  
  
作業是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 執行的一系列指定動作。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業可以在本機伺服器或多個遠端伺服器上執行。  
  
-   **開始之前：**  
  
    [安全性](#Security)  
  
-   **若要使用下列項目啟動作業：**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [SQL Server 管理物件](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>開始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
如需詳細資訊，請參閱＜ [實作 SQL Server Agent 安全性](../../ssms/agent/implement-sql-server-agent-security.md)＞。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-start-a-job"></a>若要啟動作業  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的執行個體，然後展開該執行個體。  
  
2.  展開 **[SQL Server Agent]** ，再展開 **[作業]**。 請依照您所需要的作業啟動方式，執行下列其中一項作：  
  
    -   若要對單一伺服器或目標伺服器執行作業，或要在主要伺服器上執行本機伺服器作業，請在要啟動的作業上按一下滑鼠右鍵，然後按一下 [啟動作業]。  
  
    -   若要啟動多項作業，請在 [作業活動監視器]，然後按一下 [檢視作業活動]。 您可以在作業活動監視器中選取多項作業，然後在選取範圍上按一下滑鼠右鍵，再按一下 [啟動作業]。  
  
    -   若要對主要伺服器執行作業，並希望所有目標伺服器同時執行該作業，請在要啟動的作業上按一下滑鼠右鍵，然後按一下 [啟動作業]，再按一下 [在所有目標伺服器上啟動]。  
  
    -   若要對主要伺服器執行作業，並要指定執行該作業的目標伺服器，請在要啟動的作業上按一下滑鼠右鍵，然後按一下 [啟動作業]，再按一下 [在特定目標伺服器上啟動]。 在 **[公佈下載指示]** 對話方塊中選取 **[下列目標伺服器]** 核取方塊，然後選取應該執行此作業的每個目標伺服器。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-start-a-job"></a>若要啟動作業  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    -- starts a job named Weekly Sales Data Backup.    
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_start_job N'Weekly Sales Data Backup' ;  
    GO  
    ```  
  
如需詳細資訊，請參閱 [sp_start_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理物件  
**若要啟動作業**  
  
使用選取的程式語言 (例如 Visual Basic、Visual C# 或 PowerShell) 呼叫 **Job** 類別的 **Start** 方法。 如需詳細資訊，請參閱 [SQL Server 管理物件 (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md)。  
