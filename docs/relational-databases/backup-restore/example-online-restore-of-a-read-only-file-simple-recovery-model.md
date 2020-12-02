---
title: 線上還原：唯讀檔案 (簡單復原模式)
description: 此範例會針對使用簡單復原模式搭配多個檔案群組其資料庫來顯示唯讀檔案在 SQL Server 中的線上還原。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
helpviewer_keywords:
- restore sequences [SQL Server], online
- online restores [SQL Server], simple recovery model
- simple recovery model [SQL Server], RESTORE examples
ms.assetid: 0c691fc6-9865-46a7-b055-8097424492d6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 58c4448ecde5bfed478ad19aa961dc74c1db59a5
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96130432"
---
# <a name="example-online-restore-of-a-read-only-file-simple-recovery-model"></a>範例：線上還原讀取/寫入檔案 (簡單復原模式)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  本主題是關於在簡單復原模式下，包含唯讀檔案群組的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料庫。 在簡單復原模式下，如果有檔案成為唯讀後保留的備份檔案，就可以線上還原唯讀檔案。  
  
 在此範例中，名為 `adb` 的資料庫包含三個檔案群組。 檔案群組 `A` 可讀取/寫入，而檔案群組 `B` 和 `C` 則是唯讀的。 所有的檔案群組一開始都是在線上。 在檔案群組 `B`中，必須還原的是唯讀檔案 `b1`。 資料庫管理員可以使用在檔案成為唯讀後保留的備份進行還原。 檔案群組 `B` 在還原期間會處於離線，但資料庫的其他檔案群組仍會維持線上工作。  
  
## <a name="restore-sequence"></a>還原順序  
  
> [!NOTE]  
>  線上還原順序的語法和離線還原順序的語法相同。  
  
 為還原檔案，資料庫管理員會使用下列還原順序：  
  
```  
RESTORE DATABASE adb FILE='b1' FROM filegroup_B_backup   
WITH RECOVERY  
```  
  
 檔案現在已經在線上。  
  
## <a name="additional-examples"></a>其他範例  
  
-   [範例：分次還原資料庫 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-simple-recovery-model.md)  
  
-   [範例：僅限於部分檔案群組的分次還原 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-simple-recovery-model.md)  
  
-   [範例：分次還原資料庫 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-database-full-recovery-model.md)  
  
-   [範例：僅限於部分檔案群組的分次還原 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-piecemeal-restore-of-only-some-filegroups-full-recovery-model.md)  
  
-   [範例：線上還原讀取/寫入檔案 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-write-file-full-recovery-model.md)  
  
-   [範例：線上還原唯讀檔案 &#40;完整復原模式&#41;](../../relational-databases/backup-restore/example-online-restore-of-a-read-only-file-full-recovery-model.md)  
  
## <a name="see-also"></a>另請參閱  
 [線上還原 &#40;SQL Server&#41;](../../relational-databases/backup-restore/online-restore-sql-server.md)   
 [分次還原 &#40;SQL Server&#41;](../../relational-databases/backup-restore/piecemeal-restores-sql-server.md)   
 [檔案還原 &#40;簡單復原模式&#41;](../../relational-databases/backup-restore/file-restores-simple-recovery-model.md)   
 [還原和復原概觀 &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md)   
 [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md)  
  
  
