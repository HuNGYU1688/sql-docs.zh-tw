---
description: sp_settriggerorder (Transact-SQL)
title: sp_settriggerorder (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sp_settriggerorder
- sp_settriggerorder_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_settriggerorder
ms.assetid: 8b75c906-7315-486c-bc59-293ef12078e8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6d8ef1360e35d084703a4f86590ade57be6a4c24
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468299"
---
# <a name="sp_settriggerorder-transact-sql"></a>sp_settriggerorder (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  指定第一個或最後一個引發的 AFTER 觸發程序。 在第一個及最後一個觸發程序之間啟動的 AFTER 觸發程序，會以未定義的順序執行。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
  
sp_settriggerorder [ @triggername = ] '[ triggerschema. ] triggername'   
    , [ @order = ] 'value'   
    , [ @stmttype = ] 'statement_type'   
    [ , [ @namespace = ] { 'DATABASE' | 'SERVER' | NULL } ]  
```  
  
## <a name="arguments"></a>引數  
`[ @triggername = ] '[ _triggerschema.] _triggername'` 這是觸發程式的名稱及其所屬的架構（如果適用的話），其順序將設定或變更。 [_triggerschema_]**。***triggername* 為 **sysname**。 如果名稱未對應於觸發程序，或名稱對應於 INSTEAD OF 觸發程序，程序會傳回錯誤。 無法為 DDL 或登入觸發程式指定 *triggerschema* 。  
  
`[ @order = ] 'value'` 這是觸發程式新順序的設定。 *值* 是 **Varchar (10)** ，它可以是下列任何一個值。  
  
> [!IMPORTANT]  
>  **第一個** 和 **最後一個** 觸發程式必須是兩個不同的觸發程式。  
  
|值|描述|  
|-----------|-----------------|  
|**第一個**|最先引發觸發程序。|  
|**最後一個**|最後引發觸發程序。|  
|**無**|觸發程序的引發，沒有任何既定順序。|  
  
`[ @stmttype = ] 'statement_type'` 指定引發觸發程式的 SQL 語句。 *statement_type* 是 **Varchar (50)** 而且可以是 INSERT、UPDATE、DELETE、LOGON 或 [!INCLUDE[tsql](../../includes/tsql-md.md)] [DDL 事件](../../relational-databases/triggers/ddl-events.md)中所列的任何語句事件。 您不能指定事件群組。  
  
 只有在該觸發程式已定義為該語句類型的觸發程式之後，才可以將觸發程式指定為語句類型的 **第一個** 或 **最後** 一個觸發程式。 例如，如果 **TR1** 定義為 insert 觸發程式，則可以 **先** 針對資料表 **T1** 上的 insert 指定觸發 **TR1** 。 [!INCLUDE[ssDE](../../includes/ssde-md.md)]如果 **TR1**（已定義為 INSERT 觸發程式）已設定為 UPDATE 語句的 **第一個** 或 **最後** 一個觸發程式，則會傳回錯誤。 如需詳細資訊，請參閱＜備註＞一節。  
  
 **\@ namespace =** { **' 資料庫 '**  |  **' 伺服器 '** |;  
 當 *triggername* 是 DDL 觸發程式時， **\@ namespace** 會指定是否以資料庫範圍或伺服器範圍建立 *triggername* 。 如果 *triggername* 是登入觸發程式，則必須指定伺服器。 如需 DDL 觸發程式範圍的詳細資訊，請參閱 [Ddl 觸發](../../relational-databases/triggers/ddl-triggers.md)程式。 如果未指定，或指定了 Null，則 *triggername* 會是 DML 觸發程式。  
  
* 伺服器適用于： [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 和更新版本。
  
## <a name="return-code-values"></a>傳回碼值  
 0 (成功) 和 1 (失敗)  
  
## <a name="remarks"></a>備註  
  
## <a name="dml-triggers"></a>DML 觸發程序  
 單一資料表上的每個語句只能有一個 **第** 一個和 **最後** 一個觸發程式。  
  
 如果已在資料表、資料庫或伺服器上定義 **第一個** 觸發程式，則您不能針對相同的 *statement_type*，將相同資料表、資料庫或伺服器的新觸發程式指定為 **第一個** 觸發程式。 這種限制也適用于 **最後一個** 觸發程式。  
  
 複寫會為包含在即時更新或佇列式更新訂閱的資料表，自動產生第一個觸發程序。 複寫需要觸發程序是第一個觸發程序。 當您嘗試將含有第一個觸發程序的資料表包括在立即更新或佇列更新訂閱中，複寫會引發錯誤。 如果您嘗試在資料表已加入訂閱後，將觸發程序變成第一個觸發程序，則 **sp_settriggerorder** 會傳回錯誤。 如果您對複寫觸發程式使用 ALTER TRIGGER，或使用 **sp_settriggerorder** 將複寫觸發程式變更為 **最後一個** 或 **無** 觸發程式，則訂閱無法正常運作。  
  
## <a name="ddl-triggers"></a>DDL 觸發程序  
 如果具有資料庫範圍的 DDL 觸發程式和具有伺服器範圍的 DDL 觸發程式存在於相同事件上，您可以指定兩個觸發程式都是 **第一個** 觸發程式或 **最後** 一個觸發程式。 不過，伺服器範圍的觸發程序一定會先觸發。 一般來說，存在於相同事件上之 DDL 觸發程序的執行順序如下：  
  
1.  **第一個** 標記的伺服器層級觸發程式。  
  
2.  其他伺服器層級觸發程序。  
  
3.  標示為 **Last** 的伺服器層級觸發程式。  
  
4.  **第一個** 標記的資料庫層級觸發程式。  
  
5.  其他資料庫層級觸發程序。  
  
6.  標示為 **Last** 的資料庫層級觸發程式。  
  
## <a name="general-trigger-considerations"></a>一般觸發程序考量  
 如果 ALTER TRIGGER 語句變更第一個或最後一個觸發程式，則會卸載觸發程式最初設定的 **第一個** 或 **最後** 一個屬性，而該值會取代為 **None**。 您必須使用 **sp_settriggerorder** 來重設順序值。  
  
 如果必須將相同的觸發程式指定為一個以上的語句類型的第一個或最後一個順序，則必須針對每個語句類型執行 **sp_settriggerorder** 。 此外，您必須先針對語句類型定義觸發程式，才能將它指定為要針對該語句類型引發的 **第一個** 或 **最後一個** 觸發程式。  
  
## <a name="permissions"></a>權限  
 若要設定具有伺服器範圍 (在 ON ALL SERVER 上建立) 的 DDL 觸發程序或登入觸發程序的順序，便需要 CONTROL SERVER 權限。  
  
 若要設定具有資料庫範圍 (建立時設定 ON DATABASE) 的 DDL 觸發程序順序，便需要 ALTER ANY DATABASE DDL TRIGGER 權限。  
  
 設定 DML 觸發程序的順序時，需要觸發程序定義所在之資料表或檢視的 ALTER 權限。  
  
## <a name="examples"></a>範例  
  
### <a name="a-setting-the-firing-order-for-a-dml-trigger"></a>A. 設定 DML 觸發程序的引發順序  
 下列範例將 `uSalesOrderHeader` 觸發程序指定為在 `UPDATE` 資料表中發生 `Sales.SalesOrderHeader` 作業之後，所引發的第一個觸發程序。  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'Sales.uSalesOrderHeader', @order='First', @stmttype = 'UPDATE';  
```  
  
### <a name="b-setting-the-firing-order-for-a-ddl-trigger"></a>B. 設定 DDL 觸發程序的引發順序  
 下列範例將 `ddlDatabaseTriggerLog` 觸發程序指定為 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫發生 `ALTER_TABLE` 事件之後，所引發的第一個觸發程序。  
  
```  
USE AdventureWorks2012;  
GO  
sp_settriggerorder @triggername= 'ddlDatabaseTriggerLog', @order='First', @stmttype = 'ALTER_TABLE', @namespace = 'DATABASE';  
```  
  
## <a name="see-also"></a>另請參閱  
 [系統預存程序 &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [&#40;Transact-sql&#41;的資料庫引擎預存程式 ](../../relational-databases/system-stored-procedures/database-engine-stored-procedures-transact-sql.md)   
 [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
  
