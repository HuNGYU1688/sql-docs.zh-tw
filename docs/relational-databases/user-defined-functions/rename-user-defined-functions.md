---
description: 重新命名使用者定義函數
title: 重新命名使用者定義函式 | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: c2695a5c-9cc5-4b18-8771-53027ca9a9af
author: rothja
ms.author: jroth
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3af6036a3bc6cf2b751eed6a1df173fca2c9a117
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88488571"
---
# <a name="rename-user-defined-functions"></a>重新命名使用者定義函數
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  您可以透過使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 或 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ，重新命名 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的使用者定義函數。  
  
 **本主題內容**  
  
-   **開始之前：**  
  
     [限制事項](#Restrictions)  
  
     [安全性](#Security)  
  
-   **若要使用下列項目重新命名使用者定義函數：**  
  
     [Transact-SQL](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> 限制事項  
  
-   函數名稱必須符合 [識別碼](../../relational-databases/databases/database-identifiers.md)的規則。  
  
-   重新命名使用者定義函數，不會變更 **sys.sql_modules** 目錄檢視 definition 資料行中對應的物件名稱。 因此，我們建議您不要重新命名這個物件類型。 相反地，請卸除預存程序，再利用它的新名稱來重新建立預存程序。  
  
-   變更使用者定義函數的名稱或定義後，若未更新物件來反映對此函數所做的變更，則可能導致依存物件執行失敗。  
  
###  <a name="security"></a><a name="Security"></a> Security  
  
####  <a name="permissions"></a><a name="Permissions"></a> 權限  
 若要卸除函數，需要函數所屬結構描述的 ALTER 權限，或函數的 CONTROL 權限。 若要重新建立函數，需要資料庫的 CREATE FUNCTION 權限，以及此函數建立所在之結構描述的 ALTER 權限。  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-rename-user-defined-functions"></a>若要重新命名使用者定義函數  
  
1.  在 **[物件總管]** 中，按一下資料庫旁邊的加號，此資料庫包含要重新命名的函數。  
  
2.  按一下 **[可程式性]** 資料夾旁的加號。  
  
3.  按一下包含要重新命名之函數的資料夾旁邊的加號：  
  
    -   資料表值函式  
  
    -   純量值函式  
  
    -   彙總函式  
  
4.  以滑鼠右鍵按一下您要重新命名的函數，然後選取 [重新命名]。  
  
5.  輸入函式的新名稱。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **若要重新命名使用者定義函數**  
  
 您無法使用 Transact-SQL 陳述式來執行這項工作。 若要使用 Transact-SQL 來重新命名使用者定義函數，您必須先刪除現有的函數，然後使用新的名稱來重新建立函數。 確定使用函式舊名稱的所有程式碼和應用程式現在都使用新名稱。  
  
 如需詳細資訊，請參閱 [CREATE FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/create-function-transact-sql.md) 和 [DROP FUNCTION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-function-transact-sql.md)。  
  
## <a name="see-also"></a>另請參閱  
 [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md)   
 [檢視使用者定義函數](../../relational-databases/user-defined-functions/view-user-defined-functions.md)  
  
  
