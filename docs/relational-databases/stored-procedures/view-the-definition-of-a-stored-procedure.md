---
title: 檢視預存程序的定義 | Microsoft 文件
description: 了解如何在物件總管中檢視預存程序的定義，以及在查詢編輯器中使用系統預存程序、系統函數和物件目錄檢視。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], viewing
- definition of stored procedure
- viewing stored procedures
- displaying stored procedures
ms.assetid: 93318587-a0c5-4788-946f-3b5dc8372ea9
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8951647f20a2bf48b8cee26ed946795fbf79eda7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467029"
---
# <a name="view-the-definition-of-a-stored-procedure"></a>檢視預存程序的定義
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
    
##  <a name="view-the-definition-of-a-stored-procedure"></a><a name="Top"></a> 檢視預存程序的定義 

本主題描述如何在 [物件總管] 中，以及透過系統預存程序、系統函數和查詢編輯器中的物件目錄檢視，來檢視程序的定義。  
  
-   **開始之前：** [安全性](#Security)  
  
-   **若要檢視程序中的定義，使用：** [SQL Server Management Studio](#SSMSProcedure)、[Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
 系統預存程序： **sp_helptext**  
 需要 **public** 角色的成員資格。 系統物件定義是公開顯示的。 凡具有下列任一權限的物件擁有者或承授者，都看得到使用者物件的定義：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。  
  
 系統函數：**OBJECT_DEFINITION**  
 系統物件定義是公開顯示的。 凡具有下列任一權限的物件擁有者或承授者，都看得到使用者物件的定義：ALTER、CONTROL、TAKE OWNERSHIP 或 VIEW DEFINITION。 **db_owner**、 **db_ddladmin** 和 **db_securityadmin** 固定資料庫角色的成員隱含地擁有這些權限。  
  
 物件目錄檢視： **sys.sql_modules**  
 目錄檢視內中繼資料的可見性會限制在使用者所擁有的安全性實體，或已授與使用者某些權限的安全性實體。 如需相關資訊，請參閱 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
##  <a name="how-to-view-the-definition-of-a-stored-procedure"></a><a name="Procedures"></a> 如何檢視預存程序的定義  
 您可以使用下列其中一項：  
  
-   [SQL Server Management Studio](#SSMSProcedure)  
  
-   [Transact-SQL](#TsqlProcedure)  
  
###  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
 **在物件總管中檢視程序的定義**  
  
1.  在物件總管中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的執行個體，然後展開該執行個體。  
  
2.  依序展開 **[資料庫]** 、程序所屬的資料庫，以及 **[可程式性]** 。  
  
3.  展開 [預存程序]，以滑鼠右鍵按一下程序，然後按一下 [產生預存程序的指令碼為]，然後按一下下列其中一個項目：[建立為]、[變更為] 或 [DROP 並 CREATE 至]。  
  
4.  選取 [新增查詢編輯器視窗]。 這會顯示程序定義。  

###  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **在查詢編輯器中檢視程序的定義**  
  
 系統預存程序： **sp_helptext**  
 1.  在 [物件總管] 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在工具列上，按一下 **[新增查詢]** 。  
  
3.  在查詢視窗中，輸入下列使用 **sp_helptext** 系統預存程序的陳述式。 請變更資料庫名稱和預存程序名稱，使其參考您需要的資料庫和預存程序。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    EXEC sp_helptext N'AdventureWorks2012.dbo.uspLogError';  
    ```  
  
 系統函數：**OBJECT_DEFINITION**  
 1.  在 [物件總管] 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在工具列上，按一下 **[新增查詢]** 。  
  
3.  在查詢視窗中，輸入下列使用 **OBJECT_DEFINITION** 系統函數的陳述式。 請變更資料庫名稱和預存程序名稱，使其參考您需要的資料庫和預存程序。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID(N'AdventureWorks2012.dbo.uspLogError'));  
    ```  
  
 物件目錄檢視： **sys.sql_modules**  
 1.  在 [物件總管] 中，連接到 [!INCLUDE[ssDE](../../includes/ssde-md.md)]的執行個體。  
  
2.  在工具列上，按一下 **[新增查詢]** 。  
  
3.  在查詢視窗中，輸入下列使用 **sys.sql_modules** 目錄檢視的陳述式。 請變更資料庫名稱和預存程序名稱，使其參考您需要的資料庫和預存程序。  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition  
    FROM sys.sql_modules  
    WHERE object_id = (OBJECT_ID(N'AdventureWorks2012.dbo.uspLogError'));  
    ```  
  
## <a name="see-also"></a>另請參閱  
 [建立預存程序](../../relational-databases/stored-procedures/create-a-stored-procedure.md)   
 [修改預存程序](../../relational-databases/stored-procedures/modify-a-stored-procedure.md)   
 [刪除預存程序](../../relational-databases/stored-procedures/delete-a-stored-procedure.md)   
 [重新命名預存程序](../../relational-databases/stored-procedures/rename-a-stored-procedure.md)   
 [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md)   
 [OBJECT_ID &#40;Transact-SQL&#41;](../../t-sql/functions/object-id-transact-sql.md)  
  
  
