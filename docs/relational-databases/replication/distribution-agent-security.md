---
description: 散發代理程式安全性
title: 散發代理程式安全性 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.security.DA.f1
helpviewer_keywords:
- Distribution Agent Security dialog box
ms.assetid: de40cc21-2e58-4464-9be7-b5b90c925e9b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5cfa887d79c8ecc2e4c0fa5b0fbea498bae176c3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484890"
---
# <a name="distribution-agent-security"></a>散發代理程式安全性
::: moniker range=">=sql-server-2016"
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
**[散發代理程式安全性]** 對話方塊，可以讓您指定散發代理程式執行用的 Windows 帳戶。 若為發送訂閱，散發代理程式會在散發者端執行；若為提取訂閱，則散發代理程式會在訂閱者端執行。 此 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帳戶亦稱為 *處理帳戶*，因為代理程式處理序會以此帳戶執行。 對話方塊中其他可用的選項會視您存取的方式而定：  
  
-   如果是從新增訂閱精靈中存取對話方塊，它還可以讓您指定散發代理程式連接到訂閱者 (適用於發送訂閱) 或散發者 (適用於提取訂閱) 所用的內容。 此連線可以藉由模擬 Windows 帳戶，或用您指定的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶內容來進行。  
  
-   如果是從 **[訂閱屬性]** 對話方塊中存取對話方塊，請按一下 **[訂閱者連接]** 中的屬性按鈕 ( **...** )，或該對話方塊的 **[散發者連接]** 資料列，即可指定散發代理程式進行連接所用的內容。 如需存取 [訂閱屬性]  對話方塊的詳細資訊，請參閱[檢視及修改發送訂閱屬性](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)和[如何：檢視及修改提取訂閱屬性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)。  
  
 所有帳戶都必須有效，並且每個帳戶皆有指定正確的密碼。 等到代理程式執行時，才會驗證帳戶與密碼。  
  
## <a name="options"></a>選項。  
 **Process Account**  
 輸入散發代理程式執行用的 Windows 帳戶：  
  
-   針對發送訂閱，帳戶必須：  
  
    -   至少是散發資料庫中 **db_owner** 固定資料庫角色的成員。  
  
    -   是發行存取清單 (PAL) 的成員。  
  
    -   擁有快照集共用上的讀取權限。  
  
    -   如果訂閱是針對非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者，訂閱者之 OLE DB 提供者的安裝目錄上，就要擁有讀取權限。  
  
-   針對提取訂閱，帳戶至少必須是訂閱資料庫中之 **db_owner** 固定資料庫角色的成員。  
  
 如果是在進行連接時模擬處理帳戶，則需要其他的權限。 請參閱以下的＜ **連接到散發者** ＞和＜ **連接到訂閱者** ＞章節。  
  
 無法為   的提取訂閱指定 [處理帳戶][!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]，因為散發代理程式無法在 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] 的執行個體上執行。  
  
 **[密碼]** 與 **[確認密碼]**  
 輸入 Windows 帳戶的密碼。  
  
 **連接到散發者**  
 對於發送訂閱，要連接到散發者時，一律都會以模擬 **[處理帳戶]** 文字方塊中所指定的帳戶來進行。  
  
 對於提取訂閱，選取散發代理程式是否應模擬 **[處理帳戶]** 文字方塊中所指定的帳戶，還是要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶，來連接到散發者。 如果您選取使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶，請輸入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入和密碼。  
  
> [!NOTE]  
>  建議您選取模擬 Windows 帳戶，而不要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶。  
  
 用於連接的 Windows 帳戶或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶必須：  
  
-   為 PAL 的成員。  
  
-   擁有快照集共用上的讀取權限。  
  
 **連接到訂閱者**  
 針對提取訂閱，一律會藉由模擬 **[處理帳戶]** 文字方塊中指定的帳戶連接到訂閱者。  
  
 對於發送訂閱， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者與非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者的選項並不相同：  
  
-   對於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者：選取散發代理程式是否應模擬 **[處理帳戶]** 文字方塊中所指定的帳戶，還是要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶，來連接到訂閱者。 如果您選取使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶，請輸入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入和密碼。  
  
    > [!NOTE]  
    >  建議您選取模擬 Windows 帳戶，而不要使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶。  
  
     用於連接到訂閱者的 Windows 帳戶或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 帳戶至少必須是訂閱資料庫中之 **db_owner** 固定資料庫角色的成員。  
  
-   對於非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者，請指定當散發代理程式要連接到訂閱者時，應使用的訂閱者端之資料庫登入。 此登入應擁有在訂閱資料庫中建立物件的權限。 如需設定非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者的詳細資訊，請參閱[為非 SQL Server 訂閱者建立訂閱](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  
  
 **其他連接選項**  
 僅限非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者。 以連接字串的格式，為訂閱者指定任何連接選項 (Oracle 並不需要其他選項)。 每個選項應以分號分隔。 以下為 IBM DB2 連接字串的範例 (分行符號僅為便於閱讀)：  
  
```  
Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
Persist Security Info=False;Connection Pooling=True;  
```  
  
 字串中的大多數選項是您設定之 DB2 伺服器的專用選項，但 **將二進位當作字元處理** 選項，應一律設定為 [False]  。 需要為 **初始目錄** 選項指定值，以便識別訂閱資料庫。 如需相關資訊，請參閱 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)。  
  
## <a name="see-also"></a>另請參閱  
 [用於複寫的身分識別和存取控制](../../relational-databases/replication/security/identity-and-access-control-replication.md)   
 [複寫代理程式安全性模型](../../relational-databases/replication/security/replication-agent-security-model.md)   
 [複寫代理程式概觀](../../relational-databases/replication/agents/replication-agents-overview.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [訂閱發行集](../../relational-databases/replication/subscribe-to-publications.md)  
::: moniker-end
  
::: moniker range="azuresqldb-mi-current"
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
[散發代理程式安全性]  對話方塊可以讓您指定用來執行散發代理程式的 SQL 驗證帳戶。 若為發送訂閱，散發代理程式會在散發者端執行；若為提取訂閱，則散發代理程式會在訂閱者端執行。  對話方塊中其他可用的選項會視您存取的方式而定：  
  
-   如果是從新增訂閱精靈中存取對話方塊，它還可以讓您指定散發代理程式連接到訂閱者 (適用於發送訂閱) 或散發者 (適用於提取訂閱) 所用的內容。 應使用 SQL Server 驗證帳戶來進行連線。 
  
-   如果是從 **[訂閱屬性]** 對話方塊中存取對話方塊，請按一下 **[訂閱者連接]** 中的屬性按鈕 ( **...** )，或該對話方塊的 **[散發者連接]** 資料列，即可指定散發代理程式進行連接所用的內容。 如需存取 [訂閱屬性]  對話方塊的詳細資訊，請參閱[檢視及修改發送訂閱屬性](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)和[如何：檢視及修改提取訂閱屬性](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)。  
  
 所有帳戶都必須有效，並且每個帳戶皆有指定正確的密碼。 等到代理程式執行時，才會驗證帳戶與密碼。  
  
## <a name="options"></a>選項。  
 **Process Account**  
 輸入用來執行散發代理程式的 SQL Server 驗證帳戶：  
  
-   針對發送訂閱，帳戶必須：  
  
    -   至少是散發資料庫中 **db_owner** 固定資料庫角色的成員。  
  
    -   是發行存取清單 (PAL) 的成員。  
  
    -   擁有快照集共用上的讀取權限。  
  
    -   如果訂閱是針對非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者，訂閱者之 OLE DB 提供者的安裝目錄上，就要擁有讀取權限。  
  
-   針對提取訂閱，帳戶至少必須是訂閱資料庫中之 **db_owner** 固定資料庫角色的成員。  
  
    
**[密碼]** 與 **[確認密碼]**  
輸入 Windows 帳戶的密碼。  
  
**連接到散發者**  
對於發送訂閱，要連接到散發者時，一律都會以模擬 **[處理帳戶]** 文字方塊中所指定的帳戶來進行。  
  
針對提取訂閱，選取散發代理程式是否應模擬 [處理帳戶]  文字方塊中所指定的帳戶，還是要使用 SQL Server 驗證帳戶，來連線至散發者。 
  
  
 **連接到訂閱者**  
 針對提取訂閱，一律會藉由模擬 **[處理帳戶]** 文字方塊中指定的帳戶連接到訂閱者。  
  
 對於發送訂閱， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者與非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者的選項並不相同：

  
-   對於非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者，請指定當散發代理程式要連接到訂閱者時，應使用的訂閱者端之資料庫登入。 此登入應擁有在訂閱資料庫中建立物件的權限。 如需設定非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者的詳細資訊，請參閱[為非 SQL Server 訂閱者建立訂閱](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  
  
 **其他連接選項**  
 僅限非[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者。 以連接字串的格式，為訂閱者指定任何連接選項 (Oracle 並不需要其他選項)。 每個選項應以分號分隔。 以下為 IBM DB2 連接字串的範例 (分行符號僅為便於閱讀)：  
  
```  
Provider=DB2OLEDB;Initial Catalog=MY_SUBSCRIBER_DB;Network Transport Library=TCP;Host CCSID=1252;  
PC Code Page=1252;Network Address=MY_SUBSCRIBER;Network Port=50000;Package Collection=MY_PKGCOL;  
Default Schema=MY_SCHEMA;Process Binary as Character=False;Units of Work=RUW;DBMS Platform=DB2/NT;  
Persist Security Info=False;Connection Pooling=True;  
```  
  
 字串中的大多數選項是您設定之 DB2 伺服器的專用選項，但 **將二進位當作字元處理** 選項，應一律設定為 [False]  。 需要為 **初始目錄** 選項指定值，以便識別訂閱資料庫。 如需相關資訊，請參閱 [IBM DB2 Subscribers](../../relational-databases/replication/non-sql/ibm-db2-subscribers.md)。  
  
## <a name="see-also"></a>另請參閱  
 [使用 Azure SQL Database 的異動複寫](/azure/sql-database/sql-database-managed-instance-transactional-replication) [針對 Azure SQL 受控執行個體設定複寫](/azure/sql-database/replication-with-sql-database-managed-instance)
::: moniker-end


