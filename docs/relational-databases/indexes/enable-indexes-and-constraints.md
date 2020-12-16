---
description: 啟用索引與條件約束
title: 啟用索引和條件約束 | Microsoft Docs
ms.custom: ''
ms.date: 02/17/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- indexes [SQL Server], enabling
- nonclustered indexes [SQL Server], enabling a disabled index
- index enabling [SQL Server]
- disabled indexes [SQL Server], how to enable
- constraints [SQL Server], enabling
- clustered indexes, enabling disabled indexes
ms.assetid: c55c8865-322e-4ab0-ba04-ea1f56735353
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b9df5eb968e329f67fd9a98f7ddb1d4eff027a35
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460361"
---
# <a name="enable-indexes-and-constraints"></a>啟用索引與條件約束
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  此主題描述如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)]中啟用已停用的索引。 在停用索引後，在重建或卸除索引之前仍然是停用狀態。  
  
 **本主題內容**  
  
-   **開始之前：**  
  
     [限制事項](#Restrictions)  
  
     [安全性](#Security)  
  
-   **使用下列方法啟用已停用的索引：**  
  
     [Transact-SQL](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制事項  
  
-   在重建索引後，任何因為停用索引而停用的條件約束，都必須手動啟用。 重建關聯索引將可啟用 PRIMARY KEY 與 UNIQUE 條件約束。 您必須先重建 (啟用) 索引，才可以啟用參考 PRIMARY KEY 或 UNIQUE 條件約束的 FOREIGN KEY 條件約束。 FOREIGN KEY 條件約束是使用 ALTER TABLE CHECK CONSTRAINT 陳述式來啟用。  
  
-   當 ONLINE 選項設定為 ON 時，將無法重建停用的叢集索引。  
  
-   當停用或啟用叢集索引，而非叢集索引是停用狀態時，叢集索引動作在停用的非叢集索引上具有下列結果。  
  
    |叢集索引動作|停用的非叢集索引...|  
    |----------------------------|-----------------------------------|  
    |ALTER INDEX REBUILD|仍然停用。|  
    |ALTER INDEX ALL REBUILD|已重建和啟用。|  
    |DROP INDEX|仍然停用。|  
    |CREATE INDEX WITH DROP_EXISTING|仍然停用。|  
  
     建立新叢集索引的行為會與 ALTER INDEX ALL REBUILD 相同。  
  
-   在與叢集索引關聯的非叢集索引上所允許的動作，將視此兩種索引類型的停用或啟用狀態而定。 下表摘要非叢集索引可允許的動作。  
  
    |非叢集索引動作|當叢集和非叢集索引都已停用時。|當叢集索引已啟用，而非叢集索引處於停用或啟用狀態時。|  
    |-------------------------------|--------------------------------------------------------------------|----------------------------------------------------------------------------------------|  
    |ALTER INDEX REBUILD|此動作會失敗。|此動作會成功。|  
    |DROP INDEX|此動作會成功。|此動作會成功。|  
    |CREATE INDEX WITH DROP_EXISTING|此動作會失敗。|此動作會成功。|  

-   重建已停用的壓縮非叢集索引時，data_compression 會預設為 'none'，表示將解壓縮索引。 原因是停用非叢集索引時，壓縮設定中繼資料遺失。 若要解決這個問題，您必須在重建陳述式中指定明確的資料壓縮。

###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
 需要資料表或檢視表的 ALTER 權限。 如果使用 DBCC DBREINDEX，使用者必須擁有該資料表，或者是 **系統管理員** 固定伺服器角色或 **db_ddladmin** 和 **db_owner** 固定資料庫角色的成員。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-enable-a-disabled-index"></a>啟用已停用的索引  
  
1.  在 [物件總管] 中，按一下加號展開資料庫，此資料庫包含您要啟用索引的資料表。  
  
2.  按一下加號展開 **[資料表]** 資料夾。  
  
3.  按一下加號展開要啟用索引的資料表。  
  
4.  按一下加號展開 **[索引]** 資料夾。  
  
5.  以滑鼠右鍵按一下您要啟用的索引，然後選取 [重建]。  
  
6.  在 **[重建索引]** 對話方塊中，確認 **[要重建的索引]** 方格中有正確索引，然後按一下 **[確定]** 。  
  
#### <a name="to-enable-all-indexes-on-a-table"></a>啟用資料表上的所有索引  
  
1.  在 [物件總管] 中，按一下加號展開資料庫，此資料庫包含您要啟用索引的資料表。  
  
2.  按一下加號展開 **[資料表]** 資料夾。  
  
3.  按一下加號展開要啟用索引的資料表。  
  
4.  以滑鼠右鍵按一下 [索引] 資料夾，並選取 [全部重建]。  
  
5.  在 **[重建索引]** 對話方塊中，確認 **[要重建的索引]** 方格中有正確索引，然後按一下 **[確定]**。 若要從 **[要重建的索引]** 方格中移除索引，請選取索引，然後按下 DELETE 鍵。  
  
 **[重建索引]** 對話方塊中提供下列資訊：  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-enable-a-disabled-index-using-alter-index"></a>使用 ALTER INDEX 啟用已停用的索引  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- Enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table.  
  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
    REBUILD;   
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-create-index"></a>使用 CREATE INDEX 啟用已停用的索引  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- re-creates the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    -- using the OrganizationLevel and OrganizationNode columns  
    -- and then deletes the existing IX_Employee_OrganizationLevel_OrganizationNode index  
    CREATE INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee  
       (OrganizationLevel, OrganizationNode)  
    WITH (DROP_EXISTING = ON);  
    GO  
    ```  
  
#### <a name="to-enable-a-disabled-index-using-dbcc-dbreindex"></a>使用 DBCC DBREINDEX 啟用已停用的索引  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables the IX_Employee_OrganizationLevel_OrganizationNode index  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", IX_Employee_OrganizationLevel_OrganizationNode);  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-alter-index"></a>使用 ALTER INDEX 啟用資料表上的所有索引  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    ALTER INDEX ALL ON HumanResources.Employee  
    REBUILD;  
    GO  
    ```  
  
#### <a name="to-enable-all-indexes-on-a-table-using-dbcc-dbreindex"></a>使用 DBCC DBREINDEX 啟用資料表上的所有索引  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    -- enables all indexes  
    -- on the HumanResources.Employee table  
    DBCC DBREINDEX ("HumanResources.Employee", " ");  
    GO  
    ```  
  
 如需詳細資訊，請參閱 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)、[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md) 和 [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)。  
  
  
