---
description: Migrate to a Partially Contained Database
title: 移轉至部分自主資料庫 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- contained database, migrating to
ms.assetid: 90faac38-f79e-496d-b589-e8b2fe01c562
author: stevestein
ms.author: sstein
ms.openlocfilehash: 2a942af36a551498e81b3dbaeaa02686dec9c28b
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88410994"
---
# <a name="migrate-to-a-partially-contained-database"></a>Migrate to a Partially Contained Database
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  此主題討論如何準備變更為部分自主資料庫模型，然後提供移轉步驟。  
  
 **本主題內容：**  
  
-   [準備移轉資料庫](#prepare)  
  
-   [啟用自主資料庫](#enable)  
  
-   [將資料庫轉換成部分自主資料庫](#convert)  
  
-   [將使用者移轉至自主資料庫使用者](#users)  
  
##  <a name="preparing-to-migrate-a-database"></a><a name="prepare"></a> 準備移轉資料庫  
 當您考慮將資料庫移轉至部分自主資料庫模型時，請檢閱下列項目。  
  
-   您應該了解部分自主資料庫模型。 如需相關資訊，請參閱 [自主資料庫](../../relational-databases/databases/contained-databases.md)。  
  
-   您應該了解部分自主資料庫獨有的風險。 如需詳細資訊，請參閱 [Security Best Practices with Contained Databases](../../relational-databases/databases/security-best-practices-with-contained-databases.md)。  
  
-   自主資料庫不支援複寫、異動資料擷取，或變更追蹤。 請確認資料庫不會使用這些功能。  
  
-   請檢閱針對部分自主資料庫修改的資料庫功能清單。 如需詳細資訊，請參閱[修改的功能 &#40;自主資料庫&#41;](../../relational-databases/databases/modified-features-contained-database.md)。  
  
-   請查詢 [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) 來尋找資料庫中的非內含性物件或功能。 如需詳細資訊，請參閱。  
  
-   請監視 **database_uncontained_usage** XEvent 來查看使用非內含性功能的時間。  
  
##  <a name="enable-contained-databases"></a><a name="enable"></a> 啟用自主資料庫  
 您必須先在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]執行個體上啟用自主資料庫，然後才能建立自主資料庫。  
  
### <a name="enabling-contained-databases-using-transact-sql"></a>使用 Transact-SQL 來啟用自主資料庫  
 下列範例會在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]執行個體上啟用自主資料庫。  
  
```sql  
sp_configure 'contained database authentication', 1;  
GO  
RECONFIGURE ;  
GO  
```  
  
#### <a name="enabling-contained-databases-using-management-studio"></a>使用 Management Studio 來啟用自主資料庫  
 下列範例會在 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]執行個體上啟用自主資料庫。  
  
1.  在物件總管中，以滑鼠右鍵按一下伺服器名稱，然後按一下 [屬性]。  
  
2.  在 [進階] 頁面的 [內含項目] 區段中，將 [啟用自主資料庫] 選項設定為 [True]。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

##  <a name="converting-a-database-to-partially-contained"></a><a name="convert"></a> 將資料庫轉換成部分自主資料庫  
 您可以透過變更 [內含項目] 選項，將資料庫轉換成自主資料庫。  
  
### <a name="converting-a-database-to-partially-contained-using-transact-sql"></a>使用 Transact-SQL，將資料庫轉換成部分自主資料庫  
 下列範例會將名為 `Accounting` 的資料庫轉換成部分自主資料庫。  
  
```sql  
USE [master]  
GO  
ALTER DATABASE [Accounting] SET CONTAINMENT = PARTIAL  
GO  
```  
  
### <a name="converting-a-database-to-partially-contained-using-management-studio"></a>使用 Management Studio，將資料庫轉換成部分自主資料庫  
 下列範例會將資料庫轉換成部分自主資料庫。  
  
1.  在物件總管中，展開 [資料庫]，以滑鼠右鍵按一下要轉換的資料庫，然後按一下 [屬性]。  
  
2.  在 [選項] 頁面上，將 [內含項目類型] 選項變更為 [部分]。  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="migrating-users-to-contained-database-users"></a><a name="users"></a> 將使用者移轉至自主資料庫使用者  
 下列範例會將以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入為基礎的所有使用者移轉至具有密碼之自主資料庫使用者。 此範例會排除未啟用的登入。 您必須在自主資料庫中執行此範例。  
  
```sql  
DECLARE @username sysname ;  
DECLARE user_cursor CURSOR  
    FOR   
        SELECT dp.name   
        FROM sys.database_principals AS dp  
        JOIN sys.server_principals AS sp   
        ON dp.sid = sp.sid  
        WHERE dp.authentication_type = 1 AND sp.is_disabled = 0;  
OPEN user_cursor  
FETCH NEXT FROM user_cursor INTO @username  
    WHILE @@FETCH_STATUS = 0  
    BEGIN  
        EXECUTE sp_migrate_user_to_contained   
        @username = @username,  
        @rename = N'keep_name',  
        @disablelogin = N'disable_login';  
    FETCH NEXT FROM user_cursor INTO @username  
    END  
CLOSE user_cursor ;  
DEALLOCATE user_cursor ;  
```  
  
## <a name="see-also"></a>另請參閱  
 [自主資料庫](../../relational-databases/databases/contained-databases.md)   
 [sp_migrate_user_to_contained &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-migrate-user-to-contained-transact-sql.md)   
 [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md)  
  
  
