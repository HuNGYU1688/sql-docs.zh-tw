---
description: ORIGINAL_LOGIN (Transact-SQL)
title: ORIGINAL_LOGIN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ORIGINAL_LOGIN_TSQL
- ORIGINAL_LOGIN
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], context switches
- context switching [SQL Server], login names
- original login names [SQL Server]
- ORIGINAL_LOGIN function
- names [SQL Server], logins
ms.assetid: ddfb0991-cde3-4b97-a5b7-ee450133f160
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: eea845c7ebe9b40db0d5dfc2b09a7f69d94321f2
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "91115862"
---
# <a name="original_login-transact-sql"></a>ORIGINAL_LOGIN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回連接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體的登入名稱。 您可以利用這個函數來傳回有許多明確或隱含內容切換的工作階段中原始登入的識別。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
ORIGINAL_LOGIN( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>傳回型別
 **sysname**  
  
## <a name="remarks"></a>備註  
 這個函數在稽核原始連接內容的識別時很有用。 [SESSION_USER](../../t-sql/functions/session-user-transact-sql.md) 和 [CURRENT_USER](../../t-sql/functions/current-user-transact-sql.md) 這類函式會傳回目前的執行內容，ORIGINAL_LOGIN 則會傳回在該工作階段中，最先連線到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體的登入識別。  
 
  
## <a name="examples"></a>範例  
 下列範例會將目前工作階段的執行內容從陳述式的呼叫端切換到 `login1`。  和  函數用來傳回目前工作階段使用者 (內容切換後的使用者)，以及原始登入帳戶。 
 
  >[!NOTE]
  > 雖然 Azure SQL Database 支援 ORIGINAL_LOGIN 函式，但由於 Azure SQL Database 不支援「以登入身分執行」，因此下列指令碼會失敗。 
  
```sql  
USE AdventureWorks2012;  
GO  
--Create a temporary login and user.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE USER user1 FOR LOGIN login1;  
GO  
--Execute a context switch to the temporary login account.  
DECLARE @original_login sysname;  
DECLARE @current_context sysname;  
EXECUTE AS LOGIN = 'login1';  
SET @original_login = ORIGINAL_LOGIN();  
SET @current_context = SUSER_SNAME();  
SELECT 'The current executing context is: '+ @current_context;  
SELECT 'The original login in this session was: '+ @original_login  
GO  
-- Return to the original execution context  
-- and remove the temporary principal.  
REVERT;  
GO  
DROP LOGIN login1;  
DROP USER user1;  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)  
  
  
