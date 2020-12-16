---
title: 只複製備份 | Microsoft Docs
description: 「只複製備份」是一種 SQL Server 備份，與 SQL Server 備份順序無關。 其不會影響往後還原備份的方式。
ms.custom: ''
ms.date: 01/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- copy-only backups [SQL Server]
- COPY_ONLY option [BACKUP statement]
- backups [SQL Server], copy-only backups
ms.assetid: f82d6918-a5a7-4af8-868e-4247f5b00c52
author: cawrites
ms.author: chadam
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: e10a888d9b9783b520063eddbe4d70f5ca02f9e2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465679"
---
# <a name="copy-only-backups"></a>只複製備份
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

「只複製備份」  是與傳統 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 備份順序無關的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 備份。 通常，進行備份會變更資料庫，而且會影響往後其他備份的還原方式。 不過，偶爾為了特殊目的在不影響資料庫整體備份及還原程序的情況下進行備份，相當有用。 只複製備份即是供此目的之用。
  
 只複製備份的類型如下所示：  
  
- 只複製完整備份 (所有復原模式)  
  
     只複製備份不能當做差異基底或差異備份，因此不會影響差異基底。  
  
     還原只複製完整備份與還原任何其他完整備份相同。  
  
- 只複製記錄備份 (僅完整復原模式和大量記錄復原模式)  

     只複製記錄備份會保留現有的記錄封存點，因此不會影響一般記錄備份的順序。 只複製記錄備份通常是沒有必要的。 您反倒可以建立新的例行記錄備份 (使用 WITH NORECOVERY)，且一併使用此備份與還原順序所需之任何先前的記錄備份。 但是，只複製記錄備份有時相當利於進行線上還原。 如需這類範例，請參閱[範例：線上還原讀取/寫入檔案 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)。  

     交易記錄永遠不會在只複製備份之後截斷。  
  
 只複製備份會記錄在 **backupset** 資料表的 [is_copy_only](../../relational-databases/system-tables/backupset-transact-sql.md) 資料行中。  
 
 > [!IMPORTANT]  
> 在 Azure SQL 受控執行個體中，無法為使用[服務管理的透明資料加密 (TDE)](/azure/sql-database/transparent-data-encryption-azure-sql?tabs=azure-portal#service-managed-transparent-data-encryption) \(部分機器翻譯\) 加密的資料庫建立僅複製備份。 服務管理的 TDE 使用內部金鑰加密資料，且該金鑰無法匯出，所以您無法將備份還原到其他位置。 請考慮改用[客戶管理的 TDE](/azure/sql-database/transparent-data-encryption-byok-azure-sql)，以便建立加密資料庫的僅複本備份，但請務必讓加密金鑰可供日後還原使用。
  
## <a name="to-create-a-copy-only-backup"></a>若要建立只複製備份  
 您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[tsql](../../includes/tsql-md.md)]或 PowerShell 建立只複製備份。  

### <a name="examples"></a>範例  
###  <a name="a-using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> A. 使用 SQL Server Management Studio  
在此範例中， `Sales` 資料庫的只複製備份將會備份至預設備份位置的磁碟。

1. 在物件總管  中，連接到 SQL Server Database Engine 的執行個體，然後展開該執行個體。

1. 展開 [資料庫]  ，以滑鼠右鍵按一下 `Sales`，指向 [工作]  ，然後按一下 [備份...]  。

1. 在 [一般]  頁面的 [來源]  區段中，核取 [只複製備份]  核取方塊。

1. 按一下 [確定]  。

###  <a name="b-using-transact-sql"></a><a name="TsqlProcedure"></a>B. 使用 TRANSACT-SQL  
此範例使用 COPY_ONLY 參數來建立 `Sales` 資料庫的只複製備份。  此外，也會擷取交易記錄的只複製備份。

```sql
BACKUP DATABASE Sales
TO DISK = 'E:\BAK\Sales_Copy.bak'
WITH COPY_ONLY;

BACKUP LOG Sales
TO DISK = 'E:\BAK\Sales_LogCopy.trn'
WITH COPY_ONLY;
```
  
> [!NOTE]  
> 指定 DIFFERENTIAL 選項時，COPY_ONLY 沒有任何作用。  

  
###  <a name="c-using-powershell"></a><a name="PowerShellProcedure"></a>C. 使用 PowerShell  
此範例使用 -CopyOnly 參數來建立 `Sales` 資料庫的只複製備份。  
```powershell
Backup-SqlDatabase -ServerInstance 'SalesServer' -Database 'Sales' -BackupFile 'E:\BAK\Sales_Copy.bak' -CopyOnly
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相關工作  
 **若要建立完整備份或記錄備份**  
  
- [建立完整資料庫備份 &#40;SQL Server&#41;](../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)  
  
- [備份交易記錄 &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  

 **檢視只複製備份**  
  
- [backupset &#40;Transact-SQL&#41;](../../relational-databases/system-tables/backupset-transact-sql.md)  
  
 **若要設定和使用 SQL Server PowerShell 提供者**  
  
- [SQL Server PowerShell 提供者](../../powershell/sql-server-powershell-provider.md)  

## <a name="see-also"></a>另請參閱  
 [備份概觀 &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-overview-sql-server.md)   
 [復原模式 &#40;SQL Server&#41;](../../relational-databases/backup-restore/recovery-models-sql-server.md)   
 [使用備份與還原複製資料庫](../../relational-databases/databases/copy-databases-with-backup-and-restore.md)   
 [還原和復原概觀 &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)  
[BACKUP (Transact-SQL)](../../t-sql/statements/backup-transact-sql.md)  
[Backup-SqlDatabase](/powershell/module/sqlserver/backup-sqldatabase)

