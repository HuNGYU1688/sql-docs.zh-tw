---
description: Create an ActiveX Script Job Step
title: Create an ActiveX Script Job Step
ms.custom: seo-lt-2019
ms.date: 10/06/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- ActiveX scripting jobs [SQL Server]
- job steps [Analysis Services]
ms.assetid: e6c46c6b-2d61-4571-bc8e-a831cd6e6302
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: <= sql-server-2016
ms.openlocfilehash: 5c4e73f37930c9c2bfc7458b5ec035739c67bf08
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464399"
---
# <a name="create-an-activex-script-job-step"></a>建立 ActiveX 指令碼作業步驟

[!INCLUDE [sqlserver](../../includes/applies-to-version/sqlserver.md)]

從 SQL Server 2016 開始，已不再支援 ActiveX 子系統。 將使用 ActiveX Script 的任何現有作業步驟轉換成 [PowerShell 指令碼作業步驟](create-a-powershell-script-job-step.md)。 使用 PowerShell 進行任何未來的開發。

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱[來自 SQL Server 的 Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) \(部分機器翻譯\)。

此主題描述如何使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、[!INCLUDE[tsql](../../includes/tsql-md.md)] 或 SQL Server 管理物件，在 SQL Server 2014 與更舊版本中建立並定義執行 ActiveX 指令碼的 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業步驟。  

## <a name="before-you-begin"></a>開始之前  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>限制事項  

[!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  

  
### <a name="security"></a><a name="Security"></a>安全性  

如需詳細資訊，請參閱＜ [實作 SQL Server Agent 安全性](../../ssms/agent/implement-sql-server-agent-security.md)＞。  
  
## <a name="use-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
  
#### <a name="to-create-an-activex-script-job-step"></a>若要建立 ActiveX 指令碼作業步驟  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的執行個體，然後展開該執行個體。  
  
2.  展開 **SQL Server Agent**，建立新作業或以滑鼠右鍵按一下現有作業，然後按一下 [屬性]  。 如需建立作業的詳細資訊，請參閱＜ [建立作業](../../ssms/agent/create-jobs.md)＞。  
  
3.  在 **[作業屬性]** 方塊中，按一下 **[步驟]** 頁面，然後按一下 **[新增]** 。  
  
4.  在 **[新增作業步驟]** 對話方塊中，輸入一個作業 **步驟名稱**。  
  
5.  在 **[類型]** 清單中，按一下 **[ActiveX Script]**。  
  
6.  在 **[執行身分]** 清單中，選取具有作業將會使用之認證的 Proxy 帳戶。  
  
7.  選取當初撰寫指令碼所用的 **[語言]** 。 或者，按一下 **[其他]** ，然後輸入將用來撰寫指令碼的 [!INCLUDE[msCoName](../../includes/msconame_md.md)] ActiveX 指令碼語言的名稱。  
  
8.  在 **[命令]** 方塊中，輸入將為作業步驟執行的指令碼語法。 或者，請按一下 **[開啟舊檔]** ，然後選取包含指令碼語法的檔案。  
  
9. 按一下 **[進階]** 頁面，設定下列作業步驟選項：作業步驟成功或失敗時要採取什麼動作、 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 應該嘗試執行作業步驟多少次，以及應該多久重試一次。  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>使用 Transact-SQL  
  
#### <a name="to-create-an-activex-script-job-step"></a>若要建立 ActiveX Script 作業步驟  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde_md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    -- create an ActiveX Script job step written in VBScript that creates a restore point  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Create a restore point',  
        @subsystem = N'ACTIVESCRIPTING',  
        @command = N'Const RESTORE_POINT = 20  
  
    strComputer = "."  
    Set objWMIService = GetObject("winmgmts:" _  
        & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\default")  
  
    Set objItem = objWMIService.Get("SystemRestore")  
    errResults = objItem.Restore(RESTORE_POINT)',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    ```  
  
如需詳細資訊，請參閱 [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)。  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>使用 SQL Server 管理物件  
**若要建立 ActiveX 指令碼作業步驟**  
  
透過所選的程式語言，例如 Visual Basic、Visual C# 或 PowerShell，使用 **JobStep** 類別。  
