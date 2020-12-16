---
description: GRANT 資料庫範圍認證 (Transact-SQL)
title: GRANT 資料庫範圍認證 (TRANSACT-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/19/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- GRANT DATABASE SCOPED CREDENTIAL
- GRANT_DATABASE_SCOPED_CREDENTIAL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- granting permissions [SQL Server], database scoped credential
- permissions [SQL Server], database scoped credential
- database scoped credential [SQL Server], permissions
- GRANT statement, database scoped credentials
ms.assetid: 501f2c8a-6aeb-41af-bf0b-974d17af33c0
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 68d07c588109efb74554e30668ead9b6e73ef847
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465849"
---
# <a name="grant-database-scoped-credential-permissions-transact-sql"></a>GRANT 資料庫範圍認證 (Transact-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  授與資料庫範圍認證的權限。 
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
GRANT permission  [ ,...n ]    
    ON DATABASE SCOPED CREDENTIAL :: credential_name   
    TO principal [ ,...n ] [ WITH GRANT OPTION ]   
    [ AS granting_principal ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *permission*  
 指定可以授與的資料庫範圍認證權限。 如下所列。  
  
 ON DATABASE SCOPED CREDENTIAL **::** _credential_name_  
 指定要授與的資料庫範圍認證權限。 需要範圍限定詞 "::"。  
  
 *database_principal*  
 指定要對其授與權限的主體。 下列其中之一：  
  
-   資料庫使用者  
-   資料庫角色  
-   應用程式角色  
-   對應至 Windows 登入的資料庫使用者  
-   對應至 Windows 群組的資料庫使用者  
-   對應至憑證的資料庫使用者  
-   對應至非對稱金鑰的資料庫使用者  
-   未對應至伺服器主體的資料庫使用者  
  
GRANT OPTION  
 指出主體也有權授與指定權限給其他主體。  
  
AS *granting_principal*  
 指定主體，執行這項查詢的主體就是從這個主體衍生權限來授與權限。 下列其中之一：  
  
-   資料庫使用者  
-   資料庫角色  
-   應用程式角色  
-   對應至 Windows 登入的資料庫使用者  
-   對應至 Windows 群組的資料庫使用者  
-   對應至憑證的資料庫使用者  
-   對應至非對稱金鑰的資料庫使用者  
-   未對應至伺服器主體的資料庫使用者  
  
## <a name="remarks"></a>備註  
 資料庫範圍認證是包含在資料庫內的資料庫層級安全性實體，在權限階層中，該資料庫為其父系。 下方列出的資料庫範圍認證權限，是可以授與的最明確與最有限權限，以及隱含包括這些權限的一般權限。  
  
|資料庫範圍認證權限|資料庫範圍認證權限所隱含|資料庫權限所隱含|  
|----------------------------|---------------------------------------|------------------------------------|  
|CONTROL|CONTROL|CONTROL|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|ALTER|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>權限  
 同意授權者 (或是指定了 AS 選項的主體) 必須具有指定了 GRANT OPTION 的權限本身，或是具有隱含目前正在授與權限的更高權限。  
  
 如果是使用 AS 選項，就必須套用這些額外的需求。  
  
|AS *granting_principal*|其他必要的權限|  
|------------------------------|------------------------------------|  
|資料庫使用者|使用者的 IMPERSONATE 權限、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|對應至 Windows 登入的資料庫使用者|使用者的 IMPERSONATE 權限、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|對應至 Windows 群組的資料庫使用者|Windows 群組中的成員資格、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|對應至憑證的資料庫使用者|**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|對應至非對稱金鑰的資料庫使用者|**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|未對應至任何伺服器主體的資料庫使用者|使用者的 IMPERSONATE 權限、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|資料庫角色|角色的 ALTER 權限、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
|應用程式角色|角色的 ALTER 權限、**db_securityadmin** 固定資料庫角色中的成員資格、**db_owner** 固定資料庫角色中的成員資格，或 **sysadmin** 固定伺服器角色中的成員資格。|  
  
 物件擁有者可以授與他們所擁有之物件的權限。 具有安全性實體之 CONTROL 權限的主體可以授與該安全性實體的權限。  
  
 CONTROL SERVER 權限的承授者 (例如 **sysadmin** 固定伺服器角色的成員)，可以授與伺服器中任何安全性實體的任何權限。 資料庫 CONTROL 權限的承授者 (例如 **db_owner** 固定資料庫角色的成員) 可以授與資料庫中任何安全性實體的任何權限。 結構描述之 CONTROL 權限的被授與者，可以授與結構描述中任何物件的任何權限。  
  
## <a name="see-also"></a>另請參閱  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE 資料庫範圍認證 (Transact-SQL)](../../t-sql/statements/revoke-database-scoped-credential-transact-sql.md)   
 [DENY 資料庫範圍認證 (Transact-SQL)](../../t-sql/statements/deny-database-scoped-credential-transact-sql.md)   
 [權限 &#40;資料庫引擎&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [主體 &#40;Database Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [加密階層](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
  
