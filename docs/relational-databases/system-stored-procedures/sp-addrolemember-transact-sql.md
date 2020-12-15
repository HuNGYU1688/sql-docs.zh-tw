---
description: sp_addrolemember (Transact-SQL)
title: sp_addrolemember (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 01/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_addrolemember_TSQL
- sp_addrolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrolemember
ms.assetid: a583c087-bdb3-46d2-b9e5-3921b3e6d10b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c4b37d6bd4acfb88aacbeb7f86186bb8a3fcf02c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484550"
---
# <a name="sp_addrolemember-transact-sql"></a>sp_addrolemember (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在目前資料庫的資料庫角色中，加入資料庫使用者、資料庫角色、Windows 登入或 Windows 群組。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] 請改用 [ALTER ROLE](../../t-sql/statements/alter-role-transact-sql.md) 。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
sp_addrolemember [ @rolename = ] 'role', [ @membername = ] 'security_account'  
```    

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>引數  
 [ @rolename =] '*role*'  
 這是目前資料庫中的資料庫角色名稱。 *role* 是 **sysname**，沒有預設值。  
  
 [ @membername =] '*security_account*'  
 這是加入角色的安全性帳戶。 *security_account* 是 **sysname**，沒有預設值。 *security_account* 可以是資料庫使用者、資料庫角色、windows 登入或 windows 群組。  
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 或 1 (失敗)  
  
## <a name="remarks"></a>備註  
 使用 sp_addrolemember 加入角色中的成員，會繼承該角色的權限。 如果新成員是沒有對應資料庫使用者的 Windows 層級主體，則會建立一個資料庫使用者，但是可能不會完全對應至登入。 請務必檢查登入是否存在以及是否擁有資料庫的存取權。  
  
 角色不能包含它自己作為成員。 這類「循環」定義是無效的，即使成員資格只是由一個或多個中繼成員資格間接隱含也一樣。  
  
 sp_addrolemember 無法將固定資料庫角色、固定伺服器角色或 dbo 加入至角色。
  
 您只能使用 sp_addrolemember 將成員加入資料庫角色中。 若要將成員加入至伺服器角色，請使用 [sp_addsrvrolemember &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)。  
  
## <a name="permissions"></a>權限  
 將成員加入彈性資料庫角色中需要下列其中一項：  
  
-   Db_securityadmin 或 db_owner 固定資料庫角色中的成員資格。  
  
-   擁有此角色之角色中的成員資格。  
  
-   **ALTER ANY role** 許可權或 Role 的 **alter** 許可權。  
  
 將成員加入固定資料庫角色需要 db_owner 固定資料庫角色中的成員資格。  
  
## <a name="examples"></a>範例  
  
### <a name="a-adding-a-windows-login"></a>A. 加入 Windows 登入  
 下列範例會將 Windows 登入 `Contoso\Mary5` `AdventureWorks2012` 做為使用者加入至資料庫 `Mary5` 。 然後會將 `Mary5` 使用者加入到 `Production` 角色。  
  
> [!NOTE]  
>  由於 `Contoso\Mary5` 也稱為 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 資料庫中的資料庫使用者 `Mary5`，因此必須指定 `Mary5` 使用者名稱。 除非 `Contoso\Mary5` 登入存在，否則此陳述式會失敗。 請從您的網域使用登入來測試。  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE USER Mary5 FOR LOGIN [Contoso\Mary5] ;  
GO  
```  
  
### <a name="b-adding-a-database-user"></a>B. 加入資料庫使用者  
 下列範例會將資料庫使用者 `Mary5` 加入目前資料庫的 `Production` 資料庫角色中。  
  
```sql  
EXEC sp_addrolemember 'Production', 'Mary5';  
```  
  
## <a name="examples-sspdw"></a>範例：[!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-adding-a-windows-login"></a>C. 加入 Windows 登入  
 下列範例會將登入加入 `LoginMary` 至 `AdventureWorks2008R2` 資料庫做為使用者 `UserMary` 。 然後會將 `UserMary` 使用者加入到 `Production` 角色。  
  
> [!NOTE]  
>  因為登 `LoginMary` 入在資料庫中稱為資料庫使用者 `UserMary` ，所以 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] `UserMary` 必須指定使用者名稱。 除非 `Mary5` 登入存在，否則此陳述式會失敗。 登入和使用者的名稱通常相同。 此範例會使用不同的名稱來區分影響登入與使用者的動作。  
  
```sql  
-- Uses AdventureWorks  
  
CREATE USER UserMary FOR LOGIN LoginMary ;  
GO  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
### <a name="d-adding-a-database-user"></a>D. 加入資料庫使用者  
 下列範例會將資料庫使用者 `UserMary` 加入目前資料庫的 `Production` 資料庫角色中。  
  
```sql  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
## <a name="see-also"></a>另請參閱  
 [安全性預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [資料庫層級角色](../../relational-databases/security/authentication-access/database-level-roles.md)  
  
  
