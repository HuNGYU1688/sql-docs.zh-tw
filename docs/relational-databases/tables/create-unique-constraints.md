---
description: 建立唯一的條件約束
title: 建立唯一的條件約束 | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- UNIQUE_TSQL
helpviewer_keywords:
- UNIQUE constraints [SQL Server], creating
- constraints [SQL Server], creating
- constraints [SQL Server], unique
ms.assetid: a86f9d6f-f242-43be-b65d-b3435b71b62a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 06f465d3c4ff6086d74c155c4f810730eca50742
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484480"
---
# <a name="create-unique-constraints"></a>建立唯一的條件約束

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  您可以使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中建立唯一條件約束，確保在沒有參與主索引鍵之特定資料行中輸入的值不會重複。 建立唯一條件約束會自動建立對應的唯一索引。  
  
> [!NOTE]    
> 如需 Azure Synapse Analytics 唯一條件約束的資訊，請參閱 [Azure Synapse Analytics 的主要金鑰、外部索引鍵和唯一索引鍵](/azure/sql-data-warehouse/sql-data-warehouse-table-constraints) (機器翻譯)。
  
 **本主題內容**  
  
-   **開始之前：**  
  
     [安全性](#Security)  
  
-   **若要使用下列項目來建立唯一條件約束：**  
  
     [Transact-SQL](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
 需要資料表的 ALTER 權限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-create-a-unique-constraint"></a>若要建立唯一條件約束  
  
1.  在 **物件總管** 中，以滑鼠右鍵按一下要加入唯一條件約束的資料表，然後按一下 [設計]。  
  
2.  在 [資料表設計工具] 功能表上，按一下 [索引/索引鍵]。  
  
3.  在 [索引/索引鍵] 對話方塊中，按一下 [加入]。  
  
4.  在 [一般] 底下的方格中，按一下 [類型]、從屬性右邊的下拉式清單方塊中選擇 [唯一索引鍵]，然後按一下 [關閉]。  
  
5.  在 [檔案]  功能表上，按一下 [儲存「資料表名稱」]。  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-unique-constraint"></a>若要建立唯一條件約束  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 此範例會建立 `TransactionHistoryArchive4` 資料表並且在 `TransactionID`資料行上建立唯一條件約束。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE Production.TransactionHistoryArchive4  
     (  
       TransactionID int NOT NULL,   
       CONSTRAINT AK_TransactionID UNIQUE(TransactionID)   
    );   
    GO  
  
    ```  
  
#### <a name="to-create-a-unique-constraint-on-an-existing-table"></a>若要在現有的資料表上建立唯一條件約束  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 此範例會在 `PasswordHash` 資料表中的 `PasswordSalt` 和 `Person.Password`資料行上建立唯一條件約束。  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    ALTER TABLE Person.Password   
    ADD CONSTRAINT AK_Password UNIQUE (PasswordHash, PasswordSalt);   
    GO  
  
    ```  
  
#### <a name="to-create-a-unique-constraint-on-a-new-table"></a>在新的資料表中建立唯一條件約束  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  複製下列範例並將其貼到查詢視窗中，然後按一下 **[執行]** 。 此範例會建立資料表並且在 `TransactionID`資料行上定義唯一條件約束。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    CREATE TABLE Production.TransactionHistoryArchive2  
    (  
       TransactionID int NOT NULL,  
       CONSTRAINT AK_TransactionID UNIQUE(TransactionID)  
    );  
    GO  
  
    ```  
  
     如需詳細資訊，請參閱 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)、[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md) 和 [table_constraint &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-table-constraint-transact-sql.md)。  
  
###  <a name="TsqlExample"></a>