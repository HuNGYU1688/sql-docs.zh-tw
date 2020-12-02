---
description: HAS_PERMS_BY_NAME (Transact-SQL)
title: HAS_PERMS_BY_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- HAS_PERMS_BY_NAME
- HAS_PERMS_BY_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- current permission status
- checking permission status
- verifying permission status
- testing permissions
- HAS_PERMS_BY_NAME function
ms.assetid: eaf8cc82-1047-4144-9e77-0e1095df6143
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c2240ad1cbc9bb8c9fd252eefd6633e81e4ab2f8
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "91115231"
---
# <a name="has_perms_by_name-transact-sql"></a>HAS_PERMS_BY_NAME (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  評估安全性實體上目前使用者的有效權限。 相關的函式為 [fn_my_permissions](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
HAS_PERMS_BY_NAME ( securable , securable_class , permission    
    [ , sub-securable ] [ , sub-securable_class ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *securable*  
 這是安全性實體的名稱。 如果安全性實體是伺服器本身，這個值應該設為 NULL。 *securable* 為 **sysname** 類型的純量運算式。 沒有預設值。  
  
 *securable_class*  
 這是用來測試權限之安全性實體的類別名稱。 *securable_class* 為 **nvarchar(60)** 類型的純量運算式。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，securable_class 引數必須設為下列其中一項：**DATABASE**、**OBJECT**、**ROLE**、**SCHEMA** 或 **USER**。  
  
 *permission*  
 類型為 **sysname**，不可為 Null 的純量運算式，表示要檢查的權限名稱。 沒有預設值。 權限名稱 ANY 是萬用字元。  
  
 *sub-securable*  
 **sysname** 類型的選擇性純量運算式，表示用來測試權限之安全性實體的子實體名稱。 預設值是 NULL。  
  
> [!NOTE]  
>  在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 及更新版本中，子安全性實體不能使用格式為 **'[**_sub name_**]'** 的括弧。 請改為使用 **'** _sub name_ **'** 。  
  
 *sub-securable_class*  
 **nvarchar(60)** 類型的選擇性純量運算式，表示測試權限的安全性實體之子實體的類別。 預設值是 NULL。  
  
 在 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 中，只有當 securable_class 引數設定為 **OBJECT** 時，sub-securable_class 才有效。 如果 securable_class 引數設定為 **OBJECT**，sub-securable_class 引數必須設定為 **COLUMN**。  
  
## <a name="return-types"></a>傳回型別  
 **int**  
  
 如果查詢失敗，則傳回 NULL。  
  
## <a name="remarks"></a>備註  
 這個內建函數會測試目前主體對於指定的安全性實體是否有特定有效權限。 當使用者對於安全性實體具有有效權限時，HAS_PERMS_BY_NAME 會傳回 1；當使用者對於安全性實體沒有任何有效權限，則傳回 0；當安全性實體類別或權限無效時，則傳回 NULL。 有效權限為下列任何一項：  
  
-   直接授與主體的權限，且不被拒絕。  
  
-   由主體保留的較高層級權限所隱含的權限，且不被拒絕。  
  
-   授與角色或群組的權限，主體為其成員之一，且不被拒絕。  
  
-   角色或群組所保留的權限，主體為其成員之一，且不被拒絕。  
  
 權限評估一律在呼叫端的安全性內容中執行。 若要決定另一位使用者是否具有有效權限，呼叫端對該使用者必須具有 IMPERSONATE 權限。  
  
 如果是結構描述層級實體，則接受一、二或三部分非 Null 名稱。 如果是資料庫層級實體，則接受一部分名稱，Null 值則表示目前資料庫。 如果是伺服器本身，則 Null 值 (表示「目前伺服器」) 是必要的。 這個函數無法檢查連結伺服器的權限或尚未建立伺服器層級主體之 Windows 使用者的權限。  
  
 下列查詢將傳回內建安全性實體類別的清單：  
  
```  
SELECT class_desc FROM sys.fn_builtin_permissions(default);  
```  
  
 使用的定序如下：  
  
-   目前資料庫定序：包含結構描述未包含之安全性實體的資料庫層級安全性實體；一或兩部分結構描述範圍的安全性實體；使用三部分名稱時的目標資料庫。  
  
-   master 資料庫定序：伺服器層級安全性實體。  
  
-   資料行層級檢查不支援 'ANY'。 您必須指定適當的權限。  
  
## <a name="examples"></a>範例  
  
### <a name="a-do-i-have-the-server-level-view-server-state-permission"></a>A. 我有伺服器層級 VIEW SERVER STATE 權限嗎？  
  
**適用於**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更新版本
  
```sql  
SELECT HAS_PERMS_BY_NAME(null, null, 'VIEW SERVER STATE');  
```  
  
### <a name="b-am-i-able-to-impersonate-server-principal-ps"></a>B. 我可以模擬 (IMPERSONATE) 伺服器主體 Ps 嗎？  
  
**適用於**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更新版本
  
```sql  
SELECT HAS_PERMS_BY_NAME('Ps', 'LOGIN', 'IMPERSONATE');  
```  
  
### <a name="c-do-i-have-any-permissions-in-the-current-database"></a>C. 我在目前資料庫中有任何權限嗎？  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
```  
  
### <a name="d-does-database-principal-pd-have-any-permission-in-the-current-database"></a>D. 資料庫主體 Pd 在目前資料庫中有任何權限嗎？  
 假設呼叫端有主體 `Pd` 的 IMPERSONATE 權限。  
  
```sql  
EXECUTE AS user = 'Pd'  
GO  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'ANY');  
GO  
REVERT;  
GO  
```  
  
### <a name="e-can-i-create-procedures-and-tables-in-schema-s"></a>E. 我可以在結構描述 S 中建立程序和資料表嗎？  
 下列範例需要 `ALTER` 的 `S` 權限，以及資料庫的 `CREATE PROCEDURE` 權限，對資料表也是一樣。  
  
```sql  
SELECT HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE PROCEDURE')  
    & HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_procs,  
    HAS_PERMS_BY_NAME(db_name(), 'DATABASE', 'CREATE TABLE') &  
    HAS_PERMS_BY_NAME('S', 'SCHEMA', 'ALTER') AS _can_create_tables;  
```  
  
### <a name="f-which-tables-do-i-have-select-permission-on"></a>F. 我有 SELECT 權限的資料表有哪些？  
  
```sql  
SELECT HAS_PERMS_BY_NAME  
(QUOTENAME(SCHEMA_NAME(schema_id)) + '.' + QUOTENAME(name),   
    'OBJECT', 'SELECT') AS have_select, * FROM sys.tables  
```  
  
### <a name="g-do-i-have-insert-permission-on-the-salesperson-table-in-adventureworks2012"></a>G. 我對 AdventureWorks2012 的 SalesPerson 資料表有 INSERT 權限嗎？  
 下列範例會假設 `AdventureWorks2012` 是我的目前資料庫內容，並且使用兩部分名稱。  
  
```sql  
SELECT HAS_PERMS_BY_NAME('Sales.SalesPerson', 'OBJECT', 'INSERT');  
```  
  
 下列範例不假設我的目前資料庫內容，並且使用三部分名稱。  
  
```sql  
SELECT HAS_PERMS_BY_NAME('AdventureWorks2012.Sales.SalesPerson',   
    'OBJECT', 'INSERT');  
```  
  
### <a name="h-which-columns-of-table-t-do-i-have-select-permission-on"></a>H. 我對資料表 T 的哪些資料行有 SELECT 權限？  
  
```sql  
SELECT name AS column_name,   
    HAS_PERMS_BY_NAME('T', 'OBJECT', 'SELECT', name, 'COLUMN')   
    AS can_select   
    FROM sys.columns AS c   
    WHERE c.object_id=object_id('T');  
```  
  
## <a name="see-also"></a>另請參閱  
 [權限 &#40;資料庫引擎&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [安全性實體](../../relational-databases/security/securables.md)   
 [權限階層 &#40;Database Engine&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [安全性目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/security-catalog-views-transact-sql.md)  
  
  
