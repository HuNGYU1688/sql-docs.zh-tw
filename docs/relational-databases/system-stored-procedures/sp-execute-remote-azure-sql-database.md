---
description: sp_execute_remote (Azure SQL Database)
title: sp_execute_remote (Azure SQL Database) Microsoft Docs
ms.custom: ''
ms.date: 02/01/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- sp_execute_remote
- sp_execute_remote_TSQL
helpviewer_keywords:
- remote execution
- queries, remote execution
ms.assetid: ca89aa4c-c4c1-4c46-8515-a6754667b3e5
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current
ms.openlocfilehash: 2ab1c51c53282b5f245cf7da0d33cf4f797bf53a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97439460"
---
# <a name="sp_execute_remote-azure-sql-database"></a>sp_execute_remote (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

  在以 [!INCLUDE[tsql](../../includes/tsql-md.md)] 水準資料分割配置形式分區的單一遠端 Azure SQL Database 或一組資料庫上執行語句。  
  
 預存程式是彈性查詢功能的一部分。  請參閱 [Azure SQL Database 彈性資料庫查詢總覽](/azure/azure-sql/database/elastic-query-overview) 和 [彈性資料庫查詢，以分區化 (水準資料分割) ](/azure/azure-sql/database/elastic-query-horizontal-partitioning)。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
sp_execute_remote [ @data_source_name = ] datasourcename  
[ , @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>引數  
 [ \@ data_source_name =]   
 識別執行語句的外部資料源。 請參閱 [CREATE EXTERNAL DATA SOURCE &#40;transact-sql&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)。 外部資料源可以是 "RDBMS" 或 "SHARD_MAP_MANAGER" 類型。  
  
 [ \@ stmt =] *語句*  
 這是包含 [!INCLUDE[tsql](../../includes/tsql-md.md)] 語句或批次的 Unicode 字串。 \@stmt 必須是 Unicode 常數或 Unicode 變數。 不允許使用比較複雜的 Unicode 運算式，如用 + 運算子來串連兩個字串。 不允許使用字元常數。 如果指定了 Unicode 常數，則前面必須加上 **N**。例如，Unicode 常數 **N ' sp_who '** 有效，但字元常數 **' Sp_who '** 則否。 字串大小只受到可用資料庫伺服器記憶體的限制。 在64位伺服器上，字串的大小限制為 2 GB， **Nvarchar (max)** 的大小上限。  
  
> [!NOTE]  
>  \@stmt 可以包含具有和變數名稱相同形式的參數，例如： `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 在 stmt 中包含的每個參數都 \@ 必須在 \@ params 參數定義清單和參數值清單中都有對應的專案。  
  
 [ \@ params =] N ' \@*parameter_name * * data_type* [,.。。 *n* ] '  
 這是一個字串，其中包含已內嵌在 stmt 中之所有參數的定義 \@ 。字串必須是 Unicode 常數或 Unicode 變數。 每個參數定義都由參數名稱和資料類型組成。 *n* 是指出其他參數定義的預留位置。 Stmtmust 中指定的每個參數 \@ 都是在 params 中定義 \@ 。 如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] stmt 中的語句或批次不 \@ 包含參數， \@ 則不需要 params。 這個參數的預設值是 NULL。  
  
 [ \@ param1 =] '*value1*'  
 這是參數字串所定義的第一個參數的值。 這個值可以是 Unicode 常數或 Unicode 變數。 在 stmt 中包含的每個參數都必須提供參數值 \@ 。如果 [!INCLUDE[tsql](../../includes/tsql-md.md)] stmt 中的語句或批次沒有參數，則不需要這些值 \@ 。  
  
 *n*  
 這是其他參數值的預留位置。 這些值只能是常數或變數。 這些值不能是比較複雜的運算式，如函數或利用運算子來建立的運算式。  
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 或非零 (失敗)  
  
## <a name="result-sets"></a>結果集  
 從第一個 SQL 語句傳回結果集。  
  
## <a name="permissions"></a>權限  
 需要 `ALTER ANY EXTERNAL DATA SOURCE` 權限。  
  
## <a name="remarks"></a>備註  
 `sp_execute_remote` 參數必須依照上述語法一節中所述的特定順序輸入。 如果未按順序輸入參數，就會出現錯誤訊息。  
  
 `sp_execute_remote` 的行為與 [EXECUTE &#40;transact-sql&#41;](../../t-sql/language-elements/execute-transact-sql.md) 的行為與批次和名稱範圍有關。 在執行 sp_execute_remote 語句之前，不會編譯 sp_execute_remote *\@ stmt* 參數中的 transact-sql 語句或批次。  
  
 `sp_execute_remote` 將額外的資料行加入至名為 ' $ShardName ' 的結果集，其中包含產生資料列的遠端資料庫名稱。  
  
 `sp_execute_remote` 可以使用類似于 [sp_executesql &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md)。  
  
## <a name="examples"></a>範例  
### <a name="simple-example"></a>簡單範例  
 下列範例會在遠端資料庫上建立並執行簡單的 SELECT 語句。  
  
```sql  
EXEC sp_execute_remote  
    N'MyExtSrc',  
    N'SELECT COUNT(w_id) AS Count_id FROM warehouse'   
```  
  
### <a name="example-with-multiple-parameters"></a>具有多個參數的範例  
在使用者資料庫中建立資料庫範圍認證，並指定 master 資料庫的系統管理員認證。 建立指向 master 資料庫的外部資料源，並指定資料庫範圍認證。 然後，在使用者資料庫中執行下列範例，在 `sp_set_firewall_rule` master 資料庫中執行程式。 此程式 `sp_set_firewall_rule` 需要3個參數，而且需要 `@name` 參數必須是 Unicode。

```
EXEC sp_execute_remote @data_source_name  = N'PointToMaster', 
@stmt = N'sp_set_firewall_rule @name, @start_ip_address, @end_ip_address', 
@params = N'@name nvarchar(128), @start_ip_address varchar(50), @end_ip_address varchar(50)',
@name = N'TempFWRule', @start_ip_address = '0.0.0.2', @end_ip_address = '0.0.0.2';
```

## <a name="see-also"></a>另請參閱：

[CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md)  
[CREATE EXTERNAL DATA SOURCE (Transact-SQL)](../../t-sql/statements/create-external-data-source-transact-sql.md)  
