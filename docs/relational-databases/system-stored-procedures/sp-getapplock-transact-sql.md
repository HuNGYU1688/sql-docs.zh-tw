---
description: sp_getapplock (Transact-SQL)
title: sp_getapplock (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_getapplock_TSQL
- sp_getapplock
dev_langs:
- TSQL
helpviewer_keywords:
- application locks
- sp_getapplock
ms.assetid: e1e85908-9f31-47cf-8af6-88c77e6f24c9
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 16a51a2578718db33e27a5c0c027f607eb4d37fc
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474639"
---
# <a name="sp_getapplock-transact-sql"></a>sp_getapplock (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  設定應用程式資源的鎖定。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_getapplock [ @Resource = ] 'resource_name' ,  
     [ @LockMode = ] 'lock_mode'   
     [ , [ @LockOwner = ] 'lock_owner' ]   
     [ , [ @LockTimeout = ] 'value' ]  
     [ , [ @DbPrincipal = ] 'database_principal' ]  
[ ; ]  
```  
  
## <a name="arguments"></a>引數  
 [ @Resource =] '*resource_name*'  
 這是一個字串，指定用來識別鎖定資源的名稱。 應用程式必須確定資源名稱是唯一的。 指定的名稱會在內部雜湊成可儲存在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 鎖定管理員中的值。 *resource_name* 是 **Nvarchar (255)** 沒有預設值。 如果資源字串的長度超過 **Nvarchar (255)**，它將會被截斷為 **Nvarchar (255)**。  
  
 *resource_name* 是以二進位來比較，因此不論目前資料庫的定序設定為何，都會區分大小寫。  
  
> [!NOTE]  
>  取得應用程式鎖定之後，只會擷取純文字中的前 32 個字元，其餘部分會予以雜湊。  
  
 [ @LockMode =] '*lock_mode*'  
 這是要取得的特定資源鎖定模式。 *lock_mode* 是 **Varchar (32)** 且沒有預設值。 值可以是下列任何一項： **Shared**、 **Update**、 **IntentShared**、 **IntentExclusive** 或 **Exclusive**。 如需詳細資訊，請參閱 [鎖定模式](../sql-server-transaction-locking-and-row-versioning-guide.md#lock_modes)。
  
 [ @LockOwner =] '*lock_owner*'  
 為鎖定的擁有者，也就是要求鎖定時的 *lock_owner* 值。 *lock_owner* 是 **Varchar (32)**。 這個值可以是 **Transaction** (預設值) 或 **Session**。 當 *lock_owner* 值為 **transaction** 時，根據預設或明確地指定，必須從交易內執行 sp_getapplock。  
  
 [ @LockTimeout =] '*value*'  
 這是鎖定逾時值 (以毫秒為單位)。 預設值與 @ 所傳回的值相同 @LOCK_TIMEOUT 。 若要指出當要求無法立即授與時，鎖定要求應該傳回-1 的傳回碼，而不是等候鎖定，請指定0。  
  
 [ @DbPrincipal =] '*database_principal*'  
 這是擁有資料庫中物件權限的使用者、角色或應用程式角色。 函數的呼叫端必須是 *database_principal*、dbo 或 db_owner 固定資料庫角色的成員，才能成功呼叫函式。 預設值是 public。  
  
## <a name="return-code-values"></a>傳回碼值  
 \>= 0 (成功) ，或 < 0 (失敗)   
  
|值|結果|  
|-----------|------------|  
|0|同步授與鎖定成功。|  
|1|在等待其他不相容的鎖定釋出之後，授與鎖定成功。|  
|-1|鎖定要求逾時。|  
|-2|已取消鎖定要求。|  
|-3|已將鎖定要求選為死結犧牲者。|  
|-999|表示參數驗證或其他呼叫錯誤。|  
  
## <a name="remarks"></a>備註  
 資源所設定的鎖定與目前交易或目前工作階段相關聯。 當認可或回復交易時，便會釋放與目前交易相關聯的鎖定。 會話登出時，就會釋放與會話相關聯的鎖定。當伺服器因為任何原因而關閉時，就會釋放所有鎖定。  
  
 sp_getapplock 所建立的鎖定資源建立在工作階段的目前資料庫中。 每項鎖定資源都是用下列各項的合計值來識別的：  
  
-   包含鎖定資源之資料庫的資料庫識別碼。  
  
-   @DbPrincipal 參數中指定的資料庫主體。  
  
-   @Resource 參數中指定的鎖定名稱。  
  
 只有 @DbPrincipal 參數中指定的資料庫主體成員，才能夠取得指定這個主體的應用程式鎖定。 dbo 和 db_owner 角色的成員會隱含地被視為所有角色的成員。  
  
 您可以利用 sp_releaseapplock 來明確釋放鎖定。 當應用程式針對相同鎖定資源來重複呼叫 sp_getapplock 時，也必須呼叫 sp_releaseapplock 相同次數，以便釋出鎖定。  當鎖定擁有者開啟鎖定時 `Transaction` ，就會在認可或回復交易時釋放該鎖定。
  
 如果針對相同鎖定資源呼叫了 sp_getapplock 許多次，但有任何要求所指定的鎖定模式與現有的模式不同，資源便會受到兩個鎖定模式的聯合影響。 在大部分情況下，這表示鎖定模式會升級成較強的鎖定模式、現有的模式，或新要求的模式。 這個較強的鎖定模式會保留到最後釋出鎖定為止，即使鎖定釋出呼叫先發生，也是如此。 例如，在下列呼叫順序中，資源會保留 `Exclusive` 模式，而不是 `Shared` 模式。  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
EXEC @result = sp_releaseapplock @Resource = 'Form1';  
COMMIT TRANSACTION;  
GO  
```  
  
 含應用程式鎖定的死結不會回復要求應用程式鎖定的交易。 當因為傳回值而可能需要進行任何回復時，您必須手動回復。 因此，建議您在程式碼中併入錯誤檢查，以便在傳回某些值 (如 -3) 時，起始 ROLLBACK TRANSACTION 或替代動作。  
  
 範例如下：  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRANSACTION;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Exclusive';  
IF @result = -3  
BEGIN  
    ROLLBACK TRANSACTION;  
END  
ELSE  
BEGIN  
    EXEC @result = sp_releaseapplock @Resource = 'Form1';  
    COMMIT TRANSACTION;  
END;  
GO  
```  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 利用目前資料庫識別碼來限定資源。 因此，如果執行 sp_getapplock，即使不同資料庫使用相同參數值，結果是不同資源也會有不同的鎖定。  
  
 請利用 sys.dm_tran_locks 動態管理檢視或 sp_lock 系統預存程序來檢查鎖定資訊，或利用 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 來監視鎖定。  
  
## <a name="permissions"></a>權限  
 需要 public 角色中的成員資格。  
  
## <a name="examples"></a>範例  
 下列範例會在 `Form1` 資料庫的 `AdventureWorks2012` 資源上，設定與目前交易相關聯的共用鎖定。  
  
```  
USE AdventureWorks2012;  
GO  
BEGIN TRAN;  
DECLARE @result int;  
EXEC @result = sp_getapplock @Resource = 'Form1',   
               @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
 下列範例會將 `dbo` 指定為資料庫主體。  
  
```  
BEGIN TRAN;  
EXEC sp_getapplock @DbPrincipal = 'dbo', @Resource = 'AdventureWorks2012',   
     @LockMode = 'Shared';  
COMMIT TRAN;  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [APPLOCK_MODE &#40;Transact-sql&#41;](../../t-sql/functions/applock-mode-transact-sql.md)   
 [APPLOCK_TEST &#40;Transact-sql&#41;](../../t-sql/functions/applock-test-transact-sql.md)   
 [sp_releaseapplock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-releaseapplock-transact-sql.md)  
  
  
