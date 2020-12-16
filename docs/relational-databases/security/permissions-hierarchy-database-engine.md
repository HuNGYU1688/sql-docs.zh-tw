---
title: 權限階層 (Database Engine) | Microsoft Docs
description: 了解可在 SQL Server 資料庫引擎中以權限保護的實體階層 (稱為安全性實體)。
ms.custom: ''
ms.date: 03/23/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
f1_keywords:
- sql13.swb.server.permissions.f1--May use common.permissions
helpviewer_keywords:
- security [SQL Server], denying access
- hierarchies [SQL Server], permissions
- securables [SQL Server]
- security [SQL Server], permissions
- permissions [SQL Server], hierarchy
- security [SQL Server], granting access
ms.assetid: f6d20a55-ef03-4e14-85f9-009902889866
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bdd56038381bfa90ee9e7aa635de1d9cca2ea4aa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479409"
---
# <a name="permissions-hierarchy-database-engine"></a>權限階層 (Database Engine)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssDE](../../includes/ssde-md.md)] 會管理階層式實體集合，這些實體可使用權限來確保安全性。 這些實體也稱為「安全性實體」。 最明顯的安全性實體為伺服器和資料庫，但是個別權限可以在更細微的層次進行設定。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 透過驗證主體是否已被授與適當的權限，進而規範主體在安全性實體上的動作。  
  
 下圖顯示 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 權限階層之間的關係。  
  
 權限系統的運作方式在所有版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]、 [!INCLUDE[ssSDS](../../includes/sssds-md.md)]、 [!INCLUDE[ssDW](../../includes/ssdw-md.md)]、 [!INCLUDE[ssAPS](../../includes/ssaps-md.md)]皆相同，然而，某些功能不適用於所有版本。 例如，伺服器層級權限無法在 Azure 產品中設定。  
  
 ![資料庫引擎權限階層的圖表](../../relational-databases/security/media/wj-security-layers.gif "資料庫引擎權限階層的圖表")  
  
## <a name="chart-of-sql-server-permissions"></a>SQL Server 權限的圖表  
 如需所有 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 權限的 PDF 格式海報大小圖表，請參閱 [https://aka.ms/sql-permissions-poster](https://aka.ms/sql-permissions-poster)。  
  
## <a name="working-with-permissions"></a>使用權限  
 您可以透過常見的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 查詢 GRANT、DENY 與 REVOKE 來操控權限。 您可以在 [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md) 和 [sys.database_permissions](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md) 目錄檢視中查看權限的相關資訊。 此外，也支援使用內建函數來查詢權限資訊。  
  
 如需設計權限系統的資訊，請參閱 [資料庫引擎權限使用者入門](../../relational-databases/security/authentication-access/getting-started-with-database-engine-permissions.md)。  
  
## <a name="see-also"></a>另請參閱  
 [保護 SQL Server 的安全](../../relational-databases/security/securing-sql-server.md)   
 [權限 &#40;資料庫引擎&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [安全性實體](../../relational-databases/security/securables.md)   
 [主體 &#40;Database Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [REVOKE &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [sys.database_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-permissions-transact-sql.md)  
  
  
