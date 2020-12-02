---
title: 線上還原：讀寫檔案 (完整復原模式)
description: 此範例會針對使用完整復原模式搭配多個檔案群組其資料庫來顯示讀寫檔案在 SQL Server 中的線上還原。
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- full recovery model [SQL Server], RESTORE example
- online restores [SQL Server], full recovery model
- restore sequences [SQL Server], online
ms.assetid: 0dbeda81-1464-44ba-9011-914900096368
author: cawrites
ms.author: chadam
ms.openlocfilehash: ce68e817070765a6f84a12c518e71de221734b34
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96126996"
---
# <a name="example-online-restore-of-a-read-write-file-full-recovery-model"></a>範例：線上還原讀取/寫入檔案 (完整復原模式)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主題是關於在完整復原模式下，包含多個檔案或檔案群組的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料庫。  
  
 這個範例當中，使用完整復原模式，名為 `adb`的資料庫包含三個檔案群組。 檔案群組 `A` 可讀取/寫入，而檔案群組 `B` 和檔案群組 `C` 則是唯讀的。 所有的檔案群組一開始都是在線上。  
  
 檔案群組 `a1` 內的 `A` 檔案已經損毀，資料庫管理員決定在資料庫仍然在線上時還原該檔案。  
  
> [!NOTE]  
>  在簡單復原模式下，不允許線上還原讀取/寫入資料。  
  
## <a name="restore-sequences"></a>還原順序  
  
> [!NOTE]  
>  線上還原順序的語法和離線還原順序的語法相同。  
  
1.  線上還原檔案 `a1`。  
  
    ```  
    RESTORE DATABASE adb FILE='a1' FROM backup   
    WITH NORECOVERY;  
    ```  
  
     此時，檔案 a1 處於 RESTORING 狀態，而檔案群組 A 已離線。  
  
2.  還原該檔案之後，資料庫管理員會進行新的記錄備份，以確保能擷取到檔案離線的時間點。  
  
    ```  
    BACKUP LOG adb TO log_backup3;   
    ```  
  
3.  線上還原記錄備份。  
  
     系統管理員會還原已還原之檔案備份後進行的所有記錄備份，並在最新的記錄備份 (步驟 2 所建立的 *log_backup3*) 處結束。 還原最後一個備份之後，資料庫就可以復原。  
  
    ```  
    RESTORE LOG adb FROM log_backup1 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup2 WITH NORECOVERY;  
    RESTORE LOG adb FROM log_backup3 WITH NORECOVERY;  
    RESTORE DATABASE adb WITH RECOVERY;  
    ```  
  
     檔案 `a1` 現在已經在線上。  
  
## <a name="additional-examples"></a>其他範例  
  
-   [範例：分次還原資料庫 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [範例：僅限於部分檔案群組的分次還原 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [範例：線上還原唯讀檔案 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-simple-recovery-model.md)  
  
-   [範例：分次還原資料庫 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [範例：僅限於部分檔案群組的分次還原 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [範例：線上還原唯讀檔案 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>另請參閱  
 [線上還原 &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [分次還原 &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)   
 [還原和復原概觀 &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [套用交易記錄備份 &#40;SQL Server&#41;](../../relational-databases/backup-restore/apply-transaction-log-backups-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
