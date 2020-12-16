---
title: 檢視或變更資料庫的屬性 | Microsoft 文件
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中檢視或變更資料庫的屬性。
ms.custom: ''
ms.date: 01/05/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- displaying databases
- database viewing [SQL Server]
- databases [SQL Server], viewing
- viewing databases
ms.assetid: 9e8ac097-84b7-46c7-85e3-c1e79f94d747
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6195e52fba9bf21c4ef7c154dcd5f89d50cca6cd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478339"
---
# <a name="view-or-change-the-properties-of-a-database"></a>檢視或變更資料庫的屬性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  此主題描述如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中檢視或變更資料庫的屬性。 變更資料庫屬性之後，修改會立即生效。  
  
 **本主題內容**  
  
-   **開始之前：**  
  
     [建議](#Recommendations)  
  
     [安全性](#Security)  
  
-   **使用下列方法檢視或變更資料庫的屬性：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> 建議  
  
-   當 AUTO_CLOSE 是 ON 時， [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目錄檢視中的某些資料行及 DATABASEPROPERTYEX 函數會傳回 NULL，因為資料庫無法擷取資料。 若要解決此問題，請開啟資料庫。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
 需要具有資料庫的 ALTER 權限，才能變更資料庫的屬性。 至少需要具有公用資料庫角色的成員資格，才能檢視資料庫的屬性。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-view-or-change-the-properties-of-a-database"></a>檢視或變更資料庫的屬性  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]的執行個體，然後展開該執行個體。  
  
2.  展開 [資料庫]，並以滑鼠右鍵按一下要檢視的資料庫，然後按一下 [屬性]。  
  
3.  在 **[資料庫屬性]** 對話方塊中，選取一個頁面以檢視對應的資訊。 例如，選取 **[檔案]** 頁面以檢視資料和記錄檔資訊。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 Transact-SQL 提供許多不同的方法來檢視資料庫的屬性，以及變更資料庫屬性。 若要檢視資料庫的屬性，您可以使用 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md) 函數和 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目錄檢視。 若要變更資料庫的屬性，您可以使用您環境適用的 ALTER DATABASE 陳述式版本：[ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md) 或 [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md)。 若要檢視資料庫範圍的屬性，請使用 [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 目錄檢視，若要改變資料庫範圍的屬性，請使用 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md) 陳述式。  
  
#### <a name="to-view-a-property-of-a-database-by-using-the-databasepropertyex-function"></a>使用 DATABASEPROPERTYEX 函數檢視資料庫的屬性  
  
1.  連接至 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然後連接至您要檢視其屬性的資料庫。  
  
2.  在標準列中，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 這個範例使用 [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) 系統函數傳回 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫之 AUTO_SHRINK 資料庫選項的狀態。 傳回值為 1 表示選項設定為 ON，傳回值為 0 表示選項設定為 OFF。  
  
    ```sql  
    SELECT DATABASEPROPERTYEX('AdventureWorks2012', 'IsAutoShrink');  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-by-querying-sysdatabases"></a>透過查詢 sys.databases 檢視資料庫的屬性  
  
1.  連接至 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然後連接至您要檢視其屬性的資料庫。  
  
2.  在標準列中，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 這個範例會查詢 [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) 目錄檢視，以查看 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫的數個屬性。 這個範例會傳回資料庫識別碼 (`database_id`)、資料庫是唯讀還是讀寫 (`is_read_only`)、資料庫定序 (`collation_name`)，以及資料庫相容性層級 (`compatibility_level`)。  
  
    ```sql  
    SELECT database_id, is_read_only, collation_name, compatibility_level  
    FROM sys.databases WHERE name = 'AdventureWorks2012';  
    ```  
  
#### <a name="to-view-the-properties-of-a-database-scoped-configuration-by-querying-sysdatabases_scoped_configuration"></a>查詢 sys.databases_scoped_configuration 檢視資料庫範圍組態的屬性  
  
1.  連接至 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，然後連接至您要檢視其屬性的資料庫。  
  
2.  在標準列中，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 這個範例會查詢 [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) 目錄檢視，以檢視目前資料庫的數個屬性。  
  
    ```sql  
    SELECT configuration_id, name, value, value_for_secondary  
    FROM sys.database_scoped_configurations;  
    ```  
  
     如需其他範例，請參閱 [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  
  
#### <a name="to-change-the-properties-of-a-sql-server-2016-database-using-alter-database"></a>使用 ALTER DATABASE 變更 SQL Server 2016 資料庫的屬性  
  
1.  連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]。  
  
2.  在標準列中，按一下 **[新增查詢]** 。  
  
3.  將下列範例複製並貼入查詢視窗中。 此範例判斷 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 資料庫上快照集隔離的狀態、變更屬性狀態，然後驗證變更。  
  
     若要判斷快照集隔離的狀態，請選取第一個 `SELECT` 陳述式並按一下 **[執行]** 。  
  
     若要變更快照集隔離的狀態，請選取 `ALTER DATABASE` 陳述式並按一下 **[執行]** 。  
  
     若要驗證變更，請選取第二個 `SELECT` 陳述式並按一下 **[執行]** 。  
  
     [!code-sql[DatabaseDDL#AlterDatabase9](../../relational-databases/databases/codesnippet/tsql/view-or-change-the-prope_1.sql)]  
  
#### <a name="to-change-the-database-scoped-properties-using-alter-database-scoped-configuration"></a>使用 ALTER DATABASE SCOPED CONFIGURATION 變更資料庫範圍的屬性  
  
1.  連接至 SQL Server 執行個體中的資料庫。  
  
2.  在標準列中，按一下 **[新增查詢]** 。  
  
3.  將下列範例複製並貼入查詢視窗中。 下列範例會將次要資料庫的 MAXDOP 設定為主要資料庫的值。  
  
    ```sql  
    ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY   
    ```  
  
## <a name="see-also"></a>另請參閱  
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)   
 [DATABASEPROPERTYEX &#40;Transact-SQL&#41;](../../t-sql/functions/databasepropertyex-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE (Azure SQL Database)](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER DATABASE SCOPED CONFIGURATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)   
 [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)  

  
