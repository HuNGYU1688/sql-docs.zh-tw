---
description: Set Up the Job History Log
title: Set Up the Job History Log
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], history
- logs [SQL Server], jobs
- SQL Server Agent jobs, history
- historical information [SQL Server], jobs
ms.assetid: 018e5c49-d3a0-4504-851a-f70996a34bb7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: d00c7cb8ee961dca15a7fcdfe0950963e7dec7b2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402484"
---
# <a name="set-up-the-job-history-log"></a>Set Up the Job History Log
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

本主題描述如何設定 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 的作業記錄。  
  
-   **開始之前：** [安全性](#Security)  
  
-   **若要使用下列項目設定作業記錄：** [SQL Server Management Studio](#SSMS)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>開始之前  
  
### <a name="security"></a><a name="Security"></a>安全性  
如需詳細資訊，請參閱＜ [實作 SQL Server Agent 安全性](../../ssms/agent/implement-sql-server-agent-security.md)＞。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>使用 SQL Server Management Studio  
**若要設定作業記錄**  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]的執行個體，然後展開該執行個體。  
  
2.  以滑鼠右鍵按一下 [SQL Server Agent]，然後按一下 [屬性]。  
  
3.  在 **[SQL Server Agent 屬性]** 對話方塊中，選取 **[記錄]** 頁面。  
  
4.  選擇下列選項：  
  
    1.  核取 **[限制作業記錄大小]**，然後輸入作業記錄的最大資料列數，以及每一個作業的最大資料列數。  
  
    2.  核取 **[自動移除代理程式記錄]**，並指定時間週期，比此週期要舊的記錄將會從記錄清除。  
  
## <a name="see-also"></a>另請參閱  
[實作作業](../../ssms/agent/implement-jobs.md)  
[監視作業活動](../../ssms/agent/monitor-job-activity.md)  
[建立作業](../../ssms/agent/create-jobs.md)  
