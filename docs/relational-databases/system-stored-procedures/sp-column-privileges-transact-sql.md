---
description: sp_column_privileges (Transact-SQL)
title: sp_column_privileges (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_column_privileges_TSQL
- sp_column_privileges
dev_langs:
- TSQL
helpviewer_keywords:
- sp_column_privileges
ms.assetid: a3784301-2517-4b1d-bbd9-47404483fad0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 279789c40dbc79dd3d7b2d421d757a936b0e6126
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482397"
---
# <a name="sp_column_privileges-transact-sql"></a>sp_column_privileges (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  傳回目前環境中單一資料表的資料行權限資訊。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_column_privileges [ @table_name = ] 'table_name'   
     [ , [ @table_owner = ] 'table_owner' ]   
     [ , [ @table_qualifier = ] 'table_qualifier' ]   
     [ , [ @column_name = ] 'column' ]  
```  
  
## <a name="arguments"></a>引數  
 [ @table_name =] '*table_name*'  
 這是用來傳回目錄資訊的資料表。 *table_name* 是 **sysname**，沒有預設值。 不支援萬用字元的模式比對。  
  
 [ @table_owner =] '*table_owner*'  
 這是用來傳回目錄資訊之資料表的擁有者。 *table_owner* 是 **sysname**，預設值是 Null。 不支援萬用字元的模式比對。 如果未指定 *table_owner* ，則會套用基礎資料庫管理系統 (DBMS) 的預設資料表可見度規則。  
  
 如果目前使用者擁有一份含指定名稱的資料表，就會傳回這份資料表的資料行。 如果未指定 *table_owner* ，且目前使用者並未擁有具有指定之 *table_name* 的資料表，sp_column 許可權會尋找具有資料庫擁有者所擁有之指定 *table_name* 的資料表。 如果資料表存在，就會傳回它的資料行。  
  
 [ @table_qualifier =] '*table_qualifier*'  
 這是資料表限定詞的名稱。 *table_qualifier* 是 *sysname*，預設值是 Null。 各種 DBMS 產品都支援資料表 (辨識 _符號_ 的三部分命名 **。**_擁有_ 者 **。**) _名稱_。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，這個資料行代表資料庫名稱。 在某些產品中，它代表資料表之資料庫環境的伺服器名稱。  
  
 [ @column_name =] '*column*'  
 這是個單一資料行，當只取得一個目錄資訊資料行時，便使用這個單一資料行。 資料 *行* 是 **Nvarchar (** 384 **)**，預設值是 Null。 如果未指定資料 *行* ，則會傳回所有資料行。 在中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，資料 *行* 表示 sys. columns 資料表中所列的資料行名稱。 資料 *行* 可以使用基礎 DBMS 的萬用字元比對模式來包含萬用字元。 若要有最大交互操作能力，閘道用戶端應該只採用 ISO 標準模式比對 (% 和 _ 萬用字元)。  
  
## <a name="result-sets"></a>結果集  
 sp_column_privileges 相當於 ODBC 中的 SQLColumnPrivileges。 傳回的結果依 TABLE_QUALIFIER、TABLE_OWNER、TABLE_NAME、COLUMN_NAME 和 PRIVILEGE 來排序。  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|TABLE_QUALIFIER|**sysname**|資料表限定詞名稱。 這個欄位可以是 NULL。|  
|TABLE_OWNER|**sysname**|資料表擁有者名稱。 這個欄位一律會傳回值。|  
|TABLE_NAME|**sysname**|資料表名稱。 這個欄位一律會傳回值。|  
|COLUMN_NAME|**sysname**|傳回的 TABLE_NAME 之各個資料行的資料行名稱。 這個欄位一律會傳回值。|  
|GRANTOR|**sysname**|已將這份 COLUMN_NAME 的權限授與列出之 GRANTEE 的資料庫使用者名稱。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，這個資料行一律與 TABLE_OWNER 相同。 這個欄位一律會傳回值。<br /><br /> 另外，GRANTOR 資料行也可能是資料庫擁有者 (TABLE_OWNER)，或資料庫擁有者利用 GRANT 陳述式中之 WITH GRANT OPTION 子句來授與權限的使用者。|  
|GRANTEE|**sysname**|列出的 GRANTOR 已將這份 COLUMN_NAME 的權限授與資料庫使用者名稱。 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，這個資料行一律包括 sysusers 資料表中的資料庫使用者。 這個欄位一律會傳回值。|  
|PRIVILEGE|**Varchar (** 32 **)**|可用的資料行權限之一。 資料行權限可以是下列值之一 (或定義實作時，資料來源所支援的其他值)：<br /><br /> SELECT = GRANTEE 可以擷取資料行的資料。<br /><br /> INSERT = GRANTEE 可以在新資料列插入資料表 (GRANTEE 執行這個動作) 時，提供這個資料行的資料。<br /><br /> UPDATE = GRANTEE 可以修改資料行中現有的資料。<br /><br /> REFERENCES = GRANTEE 可以在主索引鍵/外部索引鍵關聯性中，參考外部資料表中的資料行。 主索引鍵/外部索引鍵關聯性是利用資料表條件約束來定義的。|  
|IS_GRANTABLE|**Varchar (** 3 **)**|指出是否允許 GRANTEE 將權限授與其他使用者 (通常稱為 "grant with grant" 權限)。 它可以是 YES、NO 或 NULL。 未知 (或 NULL) 值是指不適用 "grant with grant" 的資料來源。|  
  
## <a name="remarks"></a>備註  
 當使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 時，是利用 GRANT 陳述式來授與權限，並由 REVOKE 陳述式來撤銷權限。  
  
## <a name="permissions"></a>權限  
 需要結構描述的 SELECT 權限。  
  
## <a name="examples"></a>範例  
 下列範例會傳回特定資料行的資料行權限資訊。  
  
```  
USE AdventureWorks2012;  
GO  
EXEC sp_column_privileges @table_name = 'Employee'   
    ,@table_owner = 'HumanResources'  
    ,@table_qualifier = 'AdventureWorks2012'  
    ,@column_name = 'SalariedFlag';  
```  
  
## <a name="see-also"></a>另請參閱  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
