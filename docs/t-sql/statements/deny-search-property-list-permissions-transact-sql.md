---
title: DENY 搜尋屬性清單權限
description: 拒絕搜尋屬性清單的權限。
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], permissions
- DENY statement, search property list permissions
- denying permissions [SQL Server], search property lists
- search property lists [SQL Server], permissions
ms.assetid: 96513cb4-a9c0-4834-97a4-ddc0777b8415
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca76b99653cfb66f53358fc6556981db1aaf73b1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466009"
---
# <a name="deny-search-property-list-permissions-transact-sql"></a>DENY 搜尋屬性清單權限 (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  拒絕搜尋屬性清單的權限。  
 
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
DENY permission [ ,...n ] ON  
        SEARCH PROPERTY LIST :: search_property_list_name  
    TO database_principal [ ,...n ] [ CASCADE ]  
    [ AS denying_principal ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *permission*  
 這是權限的名稱。 安全性實體權限的有效對應描述於本主題後面的「備註」一節中。  
  
ON SEARCH PROPERTY LIST **::** _search_property_list_name_  
 指定要拒絕其權限的搜尋屬性清單。 必須具備範圍限定詞 ::。  
  
*database_principal*  
 指定要拒絕其權限的主體。 主體可以是下列其中一項：  
  
-   資料庫使用者  
-   資料庫角色  
-   應用程式角色  
-   對應至 Windows 登入的資料庫使用者  
-   對應至 Windows 群組的資料庫使用者  
-   對應至憑證的資料庫使用者  
-   對應至非對稱金鑰的資料庫使用者  
-   未對應至伺服器主體的資料庫使用者  
  
CASCADE  
 指出目前受到拒絕的權限，也為這個主體曾授與此權限的其他主體所拒絕。  
  
*denying_principal*  
 指定主體，執行這項查詢的主體會從這個主體衍生權限來拒絕權限。 主體可以是下列其中一項：  
  
-   資料庫使用者  
-   資料庫角色  
-   應用程式角色  
-   對應至 Windows 登入的資料庫使用者  
-   對應至 Windows 群組的資料庫使用者  
-   對應至憑證的資料庫使用者  
-   對應至非對稱金鑰的資料庫使用者  
-   未對應至伺服器主體的資料庫使用者  
  
## <a name="remarks"></a>備註  
  
## <a name="search-property-list-permissions"></a>SEARCH PROPERTY LIST 權限  
 搜尋屬性清單是一個由資料庫所自主資料庫層級安全性實體，在權限階層中，此資料庫為該安全性實體的父系。 下表所列的是可以拒絕之最特定且最有限的搜尋屬性清單權限，並列出利用隱含方式來併入這些權限的較通用權限。  
  
|搜尋屬性清單權限|搜尋屬性清單權限所隱含|資料庫權限所隱含|  
|-------------------------------------|------------------------------------------------|------------------------------------|  
|ALTER|CONTROL|ALTER ANY FULLTEXT CATALOG|  
|CONTROL|CONTROL|CONTROL|  
|REFERENCES|CONTROL|REFERENCES|  
|TAKE OWNERSHIP|CONTROL|CONTROL|  
|VIEW DEFINITION|CONTROL|VIEW DEFINITION|  
  
## <a name="permissions"></a>權限  
 需要全文檢索目錄的 CONTROL 權限。 如果使用 AS 選項，指定的主體必須擁有全文檢索目錄。  
  
## <a name="see-also"></a>另請參閱  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE SEARCH PROPERTY LIST &#40;Transact-SQL&#41;](../../t-sql/statements/create-search-property-list-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [加密階層](../../relational-databases/security/encryption/encryption-hierarchy.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [GRANT 搜尋屬性清單權限 &#40;Transact-SQL&#41;](../../t-sql/statements/grant-search-property-list-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)   
 [主體 &#40;Database Engine&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [REVOKE 搜尋屬性清單權限 &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-search-property-list-permissions-transact-sql.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [使用搜索屬性清單搜索文件屬性](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
