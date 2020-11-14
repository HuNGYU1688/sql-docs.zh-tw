---
description: sp_setapprole (Transact-SQL)
title: sp_setapprole (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 10/12/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_setapprole
- sp_setapprole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_setapprole
ms.assetid: cf0901c0-5f90-42d4-9d5b-8772c904062d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 0513878f65513e57a6e26bb52d8101ba6c5d672c
ms.sourcegitcommit: 2144a22ad4380182133e87664a907fe6f06b5f95
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2020
ms.locfileid: "94570955"
---
# <a name="sp_setapprole-transact-sql"></a>sp_setapprole (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  啟動目前資料庫中，與應用程式角色相關聯的權限。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  

```sql
sp_setapprole [ @rolename = ] 'role',  
    [ @password = ] { encrypt N'password' }
      |  
        'password' [ , [ @encrypt = ] { 'none' | 'odbc' } ]  
        [ , [ @fCreateCookie = ] true | false ]  
    [ , [ @cookie = ] @cookie OUTPUT ]  
```

## <a name="arguments"></a>引數

`[ @rolename = ] 'role'` 這是目前資料庫中定義的應用程式角色名稱。 *role* 是 **sysname** ，沒有預設值。 *角色* 必須存在於目前的資料庫中。  
  
`[ @password = ] { encrypt N'password' }` 這是啟用應用程式角色所需的密碼。 *password* 是 **sysname** ，沒有預設值。 您可以使用 ODBC **encrypt** 函數來模糊 *密碼* 。 當您使用 **encrypt** 函式時，必須在第一個引號之前放置 **N** ，將密碼轉換成 Unicode 字串。  
  
 使用 **SqlClient** 的連接不支援加密選項。  
  
> [!IMPORTANT]  
> ODBC **encrypt** 函數不提供加密。 您不應該依賴這個函數來保護透過網路傳輸的密碼。 如果此資訊會透過網路傳輸，請使用 TLS 或 IPSec。
  
 **@encrypt = ' none '**  
 指定不使用模糊化。 密碼會以純文字格式傳遞至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 此為預設值。  
  
 **@encrypt= ' odbc '**  
 指定 ODBC 會使用 ODBC **encrypt** 函數來模糊化密碼，然後再將密碼傳送給 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 。 只有在使用 ODBC 用戶端或 SQL Server 的 OLE DB Provider 時才可以作這項指定。  
  
`[ @fCreateCookie = ] true | false` 指定是否要建立 cookie。 **true** 會隱含地轉換成1。 **false** 會隱含地轉換成0。  
  
`[ @cookie = ] @cookie OUTPUT` 指定要包含 cookie 的輸出參數。 只有當 **\@ fCreateCookie** 的值為 **true** 時，才會產生 cookie。 **varbinary(8000)**  
  
> [!NOTE]  
> **sp_setapprole** 的 Cookie **OUTPUT** 參數目前記載成 **varbinary(8000)** ，這是正確的長度上限。 但目前的實作會傳回 **varbinary(50)** 。 應用程式應繼續保留 **Varbinary (8000)** ，以便在未來版本中的 cookie 傳回大小增加時，讓應用程式繼續正常運作。
  
## <a name="return-code-values"></a>傳回碼值

 0 (成功) 和 1 (失敗)  
  
## <a name="remarks"></a>備註

 使用 **sp_setapprole** 啟動應用程式角色之後，角色會保持作用中，直到使用者中斷與伺服器的連線或執行 **sp_unsetapprole** 為止。 **sp_setapprole** 只能由直接 [!INCLUDE[tsql](../../includes/tsql-md.md)] 語句在臨機操作層級執行，而不是在另一個預存程式、觸發程式或使用者定義的交易內執行。  
  
 如需應用程式角色的總覽，請參閱 [應用程式角色](../../relational-databases/security/authentication-access/application-roles.md)。  
  
> [!IMPORTANT]  
> 若要在透過網路傳輸應用程式角色密碼時加以保護，請在啟用應用程式角色時一律使用加密的連接。
> [!INCLUDE[msCoName](../../includes/msconame-md.md)] **SqlClient** 不支援 ODBC **encrypt** 選項。 如果必須保存認證，請利用 crypto API 函數來加密認證。 參數 *密碼* 會儲存為單向雜湊。 為了維持與舊版的相容性 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ， **sp_addapprole** 不會強制執行密碼複雜性原則。 若要強制執行密碼複雜性原則，請使用 [建立應用程式角色](../../t-sql/statements/create-application-role-transact-sql.md)。  
  
## <a name="permissions"></a>權限

需要 **公開** 的成員資格以及角色的密碼知識。  
  
## <a name="examples"></a>範例  
  
### <a name="a-activating-an-application-role-without-the-encrypt-option"></a>A. 不使用加密選項而啟動應用程式角色

 下列範例會啟動一個名為 `SalesAppRole` 的應用程式角色，其純文字密碼為 `AsDeF00MbXX`，而且是利用專門針對目前的使用者使用之應用程式設計的權限來建立的。

```sql
EXEC sys.sp_setapprole 'SalesApprole', 'AsDeF00MbXX';  
GO
```

### <a name="b-activating-an-application-role-with-a-cookie-and-then-reverting-to-the-original-context"></a>B. 啟動內含 Cookie 的應用程式角色，再還原成原始內容

 下列範例會啟動含有密碼 `Sales11` 的 `fdsd896#gfdbfdkjgh700mM` 應用程式角色，然後建立 Cookie。 這個範例會傳回目前使用者的名稱，然後執行 `sp_unsetapprole` 來還原為原始內容。  

```sql
DECLARE @cookie varbinary(8000);  
EXEC sys.sp_setapprole 'Sales11', 'fdsd896#gfdbfdkjgh700mM'  
    , @fCreateCookie = true, @cookie = @cookie OUTPUT;  
-- The application role is now active.  
SELECT USER_NAME();  
-- This will return the name of the application role, Sales11.  
EXEC sys.sp_unsetapprole @cookie;  
-- The application role is no longer active.  
-- The original context has now been restored.  
GO  
SELECT USER_NAME();  
-- This will return the name of the original user.
GO
```

## <a name="see-also"></a>另請參閱

 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [安全性預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-application-role-transact-sql.md)   
 [sp_unsetapprole &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-unsetapprole-transact-sql.md)
