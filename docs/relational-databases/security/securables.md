---
title: 安全性實體 | Microsoft Docs
description: 了解安全性實體範圍，SQL Server 資料庫引擎授權系統使用其來規範安全性實體的存取權。
ms.custom: ''
ms.date: 10/18/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.roleproperties.selectobject.f1
helpviewer_keywords:
- securables [SQL Server]
- schemas [SQL Server], securables
- database securables [SQL Server]
- hierarchies [SQL Server], securables
- server securables [SQL Server]
ms.assetid: bfa748f0-70b0-453c-870a-04b7b205b9ff
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8d780aa878779bf25a3854dbd0c7c392800f7039
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97432210"
---
# <a name="securables"></a>安全性實體
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  安全性實體是一種資源， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 授權系統會管理其存取權。 例如，資料表是安全性實體。 有些安全性實體可以包含在其他安全性實體內，因而建立稱為「範圍」的巢狀階層，以保護它們自己本身的安全。 安全性實體範圍為 **伺服器**、 **資料庫** 以及 **結構描述**。  
  
## <a name="securable-scope-server"></a>安全性實體範圍：伺服器  
 **伺服器** 安全性實體範圍包含下列安全性實體：  
  
-   可用性群組  
  
-   端點  
  
-   登入  
  
-   伺服器角色  
  
-   資料庫  
  
## <a name="securable-scope-database"></a>安全性實體範圍：資料庫  
 **資料庫** 安全性實體範圍包含下列安全性實體：  
  
-   應用程式角色  
  
-   組件  
  
-   非對稱金鑰  
  
-   憑證  
  
-   合約  
  
-   FullText 目錄  
  
-   Fulltext 停用字詞表  
  
-   訊息類型  
  
-   遠端服務繫結  
  
-   (資料庫) 角色  
  
-   路由  
  
-   結構描述  
  
-   搜尋屬性清單  
  
-   服務  
  
-   對稱金鑰  
  
-   User  
  
## <a name="securable-scope-schema"></a>安全性實體範圍：結構描述  
 **結構描述** 安全性實體範圍包含下列安全性實體：  
  
-   類型  
  
-   XML 結構描述集合  
  
-   物件 - 物件類別有下列成員：  
  
    -   Aggregate  
  
    -   函式  
  
    -   程序  
  
    -   佇列  
  
    -   同義字  
  
    -   Table  
  
    -   檢視 
    
    -   外部資料表 
  
## <a name="controlling-access-to-a-securable"></a>控制對安全性實體的存取權  
 接收安全性實體權限的實體稱為主體。 最常見的主體就是登入和資料庫使用者。 安全性實體存取權的控制是透過授與或拒絕權限，或是將登入和使用者加入至具有存取權的角色。 如需控制權限的資訊，請參閱 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)、[REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)、[DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)、[sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) 和 [sp_droprolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)。  
  
> [!CAUTION]  
>  在安裝時為系統物件授與的預設權限，經過仔細評估可能面臨的威脅，因此在強化 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝的過程中無須改變。 系統物件的任何權限變更，都可能會限制或破壞其功效，而可能會讓您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝處於不支援的狀態。  
  
## <a name="related-content"></a>相關內容  
 [資料庫引擎權限使用者入門](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)  
  
 [保護 SQL Server 的安全](../../relational-databases/security/securing-sql-server.md)  
  
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)  
  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)  
  
 [sys.server_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md)  
  
 [sys.sql_logins &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-logins-transact-sql.md)  
  
  
