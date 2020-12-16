---
title: 應用程式角色 | Microsoft Docs
description: 使用應用程式角色，只針對透過 SQL Server 中的特定應用程式連線的使用者，啟用資料的存取權。
ms.custom: ''
ms.date: 08/06/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- application roles [SQL Server], about application roles
- principals [SQL Server], application roles
- credentials [SQL Server], roles
- application roles [SQL Server]
- roles [SQL Server], application
- permissions [SQL Server], roles
- users [SQL Server], application roles
- authentication [SQL Server], roles
- groups [SQL Server], roles
ms.assetid: dca18b8a-ca03-4b7f-9a46-8474d5b66f76
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 340ea589ac9032b1aa85de02b0056f82805a147e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468569"
---
# <a name="application-roles"></a>應用程式角色
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  應用程式角色是資料庫主體，可以讓應用程式以其自有、類似使用者的權限來執行。 利用應用程式角色，您可以只允許透過特定應用程式來連接的使用者存取特定的資料。 不像資料庫角色，應用程式角色不包含任何成員，且依預設是非使用中狀態。 應用程式角色是使用 **sp_setapprole**(需要有密碼) 予以啟用。 因為應用程式角色是資料庫層級主體，所以它們只可以透過那些資料庫中授與 **guest** 的權限來存取其他資料庫。 因此，其他資料庫中的應用程式角色將無法存取任何已停用 **guest** 的資料庫。  
  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，因為應用程式角色未與伺服器層級主體產生關聯，所以應用程式角色無法存取伺服器層級中繼資料。 若要停用這項限制，並藉此允許應用程式角色存取伺服器層級中繼資料，請設定全域旗標 4616。 如需詳細資訊，請參閱[追蹤旗標 &#40;Transact-SQL&#41;](../../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 和 [DBCC TRACEON &#40;Transact-SQL&#41;](../../../t-sql/database-console-commands/dbcc-traceon-transact-sql.md)。  
  
## <a name="connecting-with-an-application-role"></a>與應用程式角色連接  
 下列步驟是應用程式角色切換安全性內容的程序：  
  
1.  使用者執行用戶端應用程式。  
  
2.  用戶端應用程式以使用者身分連接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的執行個體。  
  
3.  接著，應用程式利用只有該應用程式知道的密碼執行 **sp_setapprole** 預存程序。  
  
4.  若應用程式角色的名稱與密碼是有效的，則會啟用該應用程式角色。  
  
5.  此時連接會遺失使用者的權限，並假設應用程式角色的權限。  

 透過應用程式角色取得的權限在連接階段中仍為有效。  
  
 在舊版 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]中，使用者在啟動應用程式角色後要重新取得其原始安全性內容的唯一方式，就是中斷連接並重新連接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]。 從 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]開始， **sp_setapprole** 就有一個選項可建立 Cookie。 此 Cookie 包含在啟用應用程式角色之前的內容資訊。 **sp_unsetapprole** 可使用此 Cookie 將工作階段還原為其原始內容。 如需有關這個新選項與範例的詳細資訊，請參閱 [sp_setapprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)的權限來存取其他資料庫。  
  
> [!IMPORTANT]  
>  **SqlClient** 不支援 ODBC [加密] 選項。 當您透過網路傳輸機密資訊時，請使用傳輸層安全性 (TLS) (先前稱為安全通訊端層 (SSL)) 或 IPsec 來加密該通道。 如果您必須將認證保存在用戶端應用程式中，請使用 Cypto API 函數來加密認證。 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 及更新的版本中， *password* 參數儲存為單向雜湊。  
  
## <a name="related-tasks"></a>相關工作  
  
| Task | 類型 |
| ---- | ---- |
|建立應用程式角色。|[建立應用程式角色](../../../relational-databases/security/authentication-access/create-an-application-role.md)和 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/create-application-role-transact-sql.md)|  
|改變應用程式角色。|[ALTER APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-application-role-transact-sql.md)|  
|刪除應用程式角色。|[DROP APPLICATION ROLE &#40;Transact-SQL&#41;](../../../t-sql/statements/drop-application-role-transact-sql.md)|  
|使用應用程式角色。|[sp_setapprole &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-setapprole-transact-sql.md)|  
  
## <a name="see-also"></a>另請參閱  
 [保護 SQL Server 的安全](../../../relational-databases/security/securing-sql-server.md)  
  
  
