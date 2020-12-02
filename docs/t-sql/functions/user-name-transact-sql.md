---
description: USER_NAME (Transact-SQL)
title: USER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- USER_NAME
- USER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- usernames [SQL Server]
- IDs [SQL Server], databases
- USER_NAME function
- users [SQL Server], database username
- names [SQL Server], database users
- identification numbers [SQL Server], databases
- database usernames [SQL Server]
ms.assetid: ab32d644-4228-449a-9ef0-5a975c305775
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 13c27b4f23e6361592a72082c94fb033a96ce0d7
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "90570676"
---
# <a name="user_name-transact-sql"></a>USER_NAME (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  傳回指定識別碼的資料庫使用者名稱。  
  
 ![文章連結圖示](../../database-engine/configure-windows/media/topic-link.gif "文章連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql  
USER_NAME ( [ id ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *id*  
 關聯至資料庫使用者的識別碼。 *id* 為 **int**。它必須用括號括住。  
  
## <a name="return-types"></a>傳回型別  
 **nvarchar(128)**  
  
## <a name="remarks"></a>備註  
 當省略 *id* 時，會假設為目前內容中的目前使用者。 如果參數包含 NULL 一詞，就會傳回 NULL。 在 EXECUTE AS 陳述式後不指定 *id* 來呼叫 USER_NAME 時，USER_NAME 會傳回模擬使用者的名稱。 如果 Windows 主體利用群組中的成員資格來存取資料庫，則 USER_NAME 會傳回 Windows 主體名稱而非群組。  
 
> [!NOTE]
> 雖然 Azure SQL Database 支援 USER_NAME 函式，但 Azure SQL Database 不支援使用 USER_NAME 的「執行身分」。 
  
## <a name="examples"></a>範例  
  
### <a name="a-using-user_name"></a>A. 使用 USER_NAME  
 下列範例會傳回使用者識別碼 `13` 的使用者名稱。  
  
```sql  
SELECT USER_NAME(13);  
GO  
```  
  
### <a name="b-using-user_name-without-an-id"></a>B. 使用 USER_NAME 而不指定識別碼  
 下列範例不指定識別碼來尋找目前使用者的名稱。  
  
```sql  
SELECT USER_NAME();  
GO  
```  
  
 使用者是系統管理員 (sysadmin) 固定伺服器角色成員的結果集如下：  
  
 ```
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="c-using-user_name-in-the-where-clause"></a>C. 在 WHERE 子句中使用 USER_NAME  
 下列範例會在 `sysusers` 中尋找資料列，這個資料列名稱等於套用系統函數 `USER_NAME` 至使用者識別碼 `1` 所得的結果。  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
name  
------------------------------  
dbo  
  
(1 row(s) affected)
```  
  
### <a name="d-calling-user_name-during-impersonation-with-execute-as"></a>D. 於使用 EXECUTE AS 進行模擬期間呼叫 USER_NAME  
 下列範例會顯示 `USER_NAME` 在模擬期間的行為方式。  
  
```sql  
SELECT USER_NAME();  
GO  
EXECUTE AS USER = 'Zelig';  
GO  
SELECT USER_NAME();  
GO  
REVERT;  
GO  
SELECT USER_NAME();  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
DBO  
Zelig  
DBO
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>範例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-using-user_name-without-an-id"></a>E. 使用 USER_NAME 而不指定識別碼  
 下列範例不指定識別碼來尋找目前使用者的名稱。  
  
```sql  
SELECT USER_NAME();  
```  
  
 以下為目前登入使用者的結果集。  
  
```  
------------------------------   
User7                              
```  
  
### <a name="f-using-user_name-in-the-where-clause"></a>F. 在 WHERE 子句中使用 USER_NAME  
 下列範例會在 `sysusers` 中尋找資料列，這個資料列名稱等於套用系統函數 `USER_NAME` 至使用者識別碼 `1` 所得的結果。  
  
```sql  
SELECT name FROM sysusers WHERE name = USER_NAME(1);  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
name                             
------------------------------   
User7                              
```  
  
## <a name="see-also"></a>另請參閱  
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)   
 [CURRENT_TIMESTAMP &#40;Transact-SQL&#41;](../../t-sql/functions/current-timestamp-transact-sql.md)   
 [CURRENT_USER &#40;Transact-SQL&#41;](../../t-sql/functions/current-user-transact-sql.md)   
 [SESSION_USER &#40;Transact-SQL&#41;](../../t-sql/functions/session-user-transact-sql.md)   
 [系統函數 &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [SYSTEM_USER &#40;Transact-SQL&#41;](../../t-sql/functions/system-user-transact-sql.md)  
  
