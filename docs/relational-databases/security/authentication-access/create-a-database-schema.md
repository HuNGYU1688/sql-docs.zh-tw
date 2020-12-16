---
title: 建立資料庫結構描述 | Microsoft 文件
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 在 SQL Server 中建立結構描述，包括限制。
ms.custom: ''
ms.date: 07/05/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.schemas.general.f1
helpviewer_keywords:
- creating schemas with Management Studio
- CREATE SCHEMA [Management Studio]
- database schemas
- schemas [SQL Server], creating
ms.assetid: ed2a5522-f4d2-4111-95a4-d3e1e5081739
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bfb3ee2b9dee35b5154c019f47a25570b6f7eeae
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468579"
---
# <a name="create-a-database-schema"></a>建立資料庫結構描述
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  此主題描述如何使用 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ，在 [!INCLUDE[tsql](../../../includes/tsql-md.md)]中建立結構描述。  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制事項  
  
-   新結構描述的擁有者是下列資料庫層級主體之一：資料庫使用者、資料庫角色或應用程式角色。 在結構描述中建立的物件由結構描述擁有者擁有，其在 **sys.objects** 中具有 NULL **principal_id**。 結構描述包含物件的擁有權可以轉移到任何資料庫層級主體，但結構描述擁有者恆保有結構描述中物件的 CONTROL 權限。  
  
-   建立資料庫物件時，如果您將有效的網域主體 (使用者或群組) 指定為物件擁有者，即會將該網域主體新增至資料庫作為結構描述。 新的結構描述將由該網域主體所擁有。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
  
-   需要資料庫的 CREATE SCHEMA 權限。  
  
-   若要指定其他使用者做為建立之結構描述的擁有者，呼叫者必須具有該使用者的 IMPERSONATE 權限。 如果將資料庫角色指定為擁有者，則呼叫端必須符合下列準則之一：角色的成員資格或角色的 ALTER 權限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
##### <a name="to-create-a-schema"></a>若要建立結構描述  
  
1.  在 [物件總管] 中，展開 **[資料庫]** 資料夾。  
  
2.  展開要建立新資料庫結構描述的資料庫。  
  
3.  以滑鼠右鍵按一下 [安全性] 資料夾，指向 [新增]，然後選取 [結構描述]。  
  
4.  在 [結構描述 - 新增] 對話方塊的 [一般] 頁面上，將新結構描述的名稱輸入 [結構描述名稱] 方塊中。  
  
5.  在 **[結構描述擁有者]** 方塊中，輸入擁有結構描述之資料庫使用者或角色的名稱。 或者，按一下 **[搜尋]** 開啟 **[搜尋角色和使用者]** 對話方塊。  
  
6.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  

> [!NOTE]
> 如果對 **Azure SQL Database** 或 **Azure Synapse Analytics** 使用 SSMS 建立結構描述，則不會顯示對話方塊。 您將需要執行所產生的「建立結構描述範本 T-SQL 陳述式」。
  
### <a name="additional-options"></a>其他選項  
 [結構描述 - 新增] 對話方塊也在其他兩個頁面上提供選項：[權限] 和 [延伸屬性]。  
  
-   **[權限]** 頁面列出所有可能的安全性實體以及可授與登入的安全性實體權限。  
  
-   **[擴充屬性]** 頁面讓您能夠將自訂屬性加入至資料庫使用者。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
  
#### <a name="to-create-a-schema"></a>若要建立結構描述  
  
1.  在 **[物件總管]** 中，連接到 [!INCLUDE[ssDE](../../../includes/ssde-md.md)]的執行個體。  
  
2.  在標準列上，按一下 **[新增查詢]** 。  
  
3.  下例會建立名為 `Chains` 的結構描述，然後建立名為 `Sizes` 的資料表。  
    ```sql  
    CREATE SCHEMA Chains;
    GO
    CREATE TABLE Chains.Sizes (ChainID int, width dec(10,2));
    ```

4.  其他選項可在單一陳述式中執行。 下例會建立 Annik 擁有的 `Sprockets` 結構描述，並包含 `NineProngs` 資料表。 陳述式將 `SELECT` 授與 Mandar，拒絕將 `SELECT` 授與 Prasanna。  

    ```sql  
    CREATE SCHEMA Sprockets AUTHORIZATION Annik  
        CREATE TABLE NineProngs (source int, cost int, partnumber int)  
        GRANT SELECT ON SCHEMA::Sprockets TO Mandar  
        DENY SELECT ON SCHEMA::Sprockets TO Prasanna;  
    GO  
    ```  
5. 執行下列陳述式，以檢視此資料庫的結構描述：

   ```sql
   SELECT * FROM sys.schemas;
   ```

 如需詳細資訊，請參閱 [CREATE SCHEMA &#40;Transact-SQL&#41;](../../../t-sql/statements/create-schema-transact-sql.md)。  
  
  
