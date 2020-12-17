---
title: 啟動 sqlcmd 公用程式
description: 了解如何啟動 sqlcmd 公用程式，其可供在 SQLCMD 模式下，或在指令碼和作業中輸入 Transact-SQL 陳述式、系統程序，以及指令碼檔案。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 00d57437-7a29-4da1-b639-ee990db055fb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c305ebb95a57f8686fb2dbc49e125e7ca09d844e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97408337"
---
# <a name="sqlcmd---start-the-utility"></a>sqlcmd - 啟動公用程式
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
   您可利用 [sqlcmd 公用程式](../../tools/sqlcmd-utility.md)在命令提示字元處、於 SQLCMD 模式的 [查詢編輯器] 中、在 Windows 指令檔中，或是在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 作業的作業系統 (Cmd.exe) 作業步驟中，輸入 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式、系統程序與指令檔。
> [!NOTE]  
>  **sqlcmd** 的預設驗證模式為 Windows 驗證。 若要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 驗證，必須使用 **-U** 和 **-P** 選項來指定使用者名稱及密碼。  
  
> [!NOTE]  
>  根據預設， [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 會安裝成具名執行個體 **sqlexpress**。  
  
### <a name="start-the-sqlcmd-utility-and-connect-to-a-default-instance-of-sql-server"></a>啟動 sqlcmd 公用程式和連接 SQL Server 的預設執行個體  
  
1.  在 [開始] 功能表上，按一下 [執行]。 在 [開啟] 方塊中，輸入 **cmd**，然後按一下 [確定] 開啟 [命令提示字元] 視窗。 (如果您從未連接過此 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的執行個體，可能必須設定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以接受連接。)  
  
2.  在命令提示字元下，輸入 **sqlcmd**。  
  
3.  按 ENTER 鍵。  
  
     現在，您已經有了信任連接，連接了電腦上所執行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的預設執行個體。  
  
     **1>** 是指定行號的 **sqlcmd** 提示字元。 每按一次 ENTER 鍵，這個號碼就會增加 1。  
  
4.  若要結束 **sqlcmd** 工作階段，請在 **sqlcmd** 提示字元中輸入 **EXIT** 。  
  
### <a name="start-the-sqlcmd-utility-and-connect-to-a-named-instance-of-sql-server"></a>啟動 sqlcmd 公用程式並連接到 SQL Server 的具名執行個體  
  
1.  開啟 [命令提示字元] 視窗，然後輸入 **sqlcmd -S**_myServer\instanceName_。 以要連接的電腦名稱和 *的執行個體來取代* myServer\instanceName [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
2.  按 ENTER 鍵。  
  
     **sqlcmd** 提示字元 (1>) 表示您已連接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的指定執行個體。  
  
    > [!NOTE]  
    >  輸入的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式會儲存在緩衝區。 當遇到 GO 命令時，這些陳述式會當做批次來執行。  
  
## <a name="see-also"></a>另請參閱  
 [使用 sqlcmd 執行 Transact-SQL 指令碼檔案](./sqlcmd-run-transact-sql-script-files.md)  
  
