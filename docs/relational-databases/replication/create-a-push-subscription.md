---
title: 建立發送訂閱 | Microsoft Docs
description: 了解如何使用 SQL Server Management Studio、Transact-SQL 或 Replication Management Objects，在 SQL Server 中建立發送訂閱。
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- push subscriptions [SQL Server replication], creating
- merge replication subscribing [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- snapshot replication [SQL Server], subscribing
- transactional replication, subscribing
ms.assetid: adfbbc61-58d1-4330-9ad6-b14ab1142e2b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: bb4684cf3a50259338122fbadfbe6d3f5ff796be
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480019"
---
# <a name="create-a-push-subscription"></a>建立發送訂閱
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  本主題描述如何使用 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 、 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]或 Replication Management Objects (RMO) 來建立 [!INCLUDE[tsql](../../includes/tsql-md.md)]中的發送訂閱。 如需為非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 訂閱者建立發送訂閱的資訊，請參閱[為非 SQL Server 訂閱者建立訂閱](../../relational-databases/replication/create-a-subscription-for-a-non-sql-server-subscriber.md)。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
 
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
使用 [新增訂閱精靈] 在「發行者」或「訂閱者」端建立發送訂閱。 依照精靈中各頁面進行：  
  
- 指定發行者和發行集。  
  
- 選取要執行複寫代理程式的位置。 對於發送訂閱，根據發行集的類型在 **[散發代理程式位置]** 頁面或 **[合併代理程式位置]** 頁面上選取 **[在散發者端執行所有代理程式 (發送訂閱)]** 。  
  
- 指定訂閱者與訂閱資料庫。  
  
- 指定複寫代理程式要連接用的登入和密碼：  
  
  - 針對快照集和交易式發行集的訂閱，請於 **[散發代理程式安全性]** 頁面指定認證。  
  
  - 針對合併式發行集的訂閱，請於 **[合併代理程式安全性]** 頁面指定認證。  
  
    如需各代理程式所需權限的資訊，請參閱[複寫代理程式安全性模型](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
- 指定同步處理排程，以及訂閱者要初始化的時間。  
  
- 指定合併式發行集的其他選項：訂閱類型以及參數化篩選的值。  
  
- 指定允許更新訂閱的交易式發行集其他選項。 其中一個選項是決定訂閱者是否應立即在發行者端認可變更，或將其寫入佇列。 另一個選項是設定用來從訂閱者連線至發行者的認證。  
  
- 選擇性地撰寫訂閱指令碼。  
  
#### <a name="to-create-a-push-subscription-from-the-publisher"></a>若要從發行者端建立發送訂閱  
  
1. 連線到 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 的發行者，然後展開伺服器節點。  
  
2. 展開 **[複寫]** 資料夾，然後展開 **[本機發行集]** 資料夾。  
  
3. 以滑鼠右鍵按一下您要建立一或多個訂閱的發行集，然後選取 [新增訂閱]。  
  
4. 在新增訂閱精靈中完成頁面。  
  
#### <a name="to-create-a-push-subscription-from-the-subscriber"></a>若要從訂閱者建立發送訂閱  
  
1. 連接到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的訂閱者，然後展開伺服器節點。  
  
2. 展開 **[複寫]** 資料夾。  
  
3. 以滑鼠右鍵按一下 [區域訂閱] 資料夾，然後選取 [新增訂閱]。  
  
4. 在 [新增訂閱] 精靈的 [發行集] 頁面上，從 [發行者] 下拉式清單中選取 [\<Find SQL Server Publisher>] 或 [\<Find Oracle Publisher>]。  
  
5. 連接到 **[連接到伺服器]** 對話方塊中的發行者。  
  
6. 選取 **[發行集]** 頁面上的發行集。  
  
7. 在新增訂閱精靈中完成頁面。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
您可以使用複寫預存程序，以程式設計的方式建立發送訂閱。 使用哪些預存程序要依訂閱所屬的發行集類型而定。  
  
> [!IMPORTANT]
> 可能的話，會在執行階段提示使用者輸入安全性認證。 如果您必須將認證儲存在指令碼檔案中，則必須維護這個檔案的安全性，使他人無法在未獲授權的情況下擅自存取。  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>若要建立快照式或交易式發行集的發送訂閱  
  
1. 在發行集資料庫的發行者上，確認發行集確實是藉由執行 [sp_helppublication](../../relational-databases/system-stored-procedures/sp-helppublication-transact-sql.md) 來支援發送訂閱。  
  
   - 如果 **allow_push** 的值為 **1**，則發送訂閱受到支援。  
  
   - 如果 **allow_push** 的值為 **0**，請執行 [sp_changepublication](../../relational-databases/system-stored-procedures/sp-changepublication-transact-sql.md)。 針對 **\@property** 指定 **allow_push**，並針對 **\@value** 指定 **true**。  
  
2. 在發行集資料庫的發行者上，執行 [sp_addsubscription](../system-stored-procedures/sp-addsubscription-transact-sql.md)。 指定 **\@publication**、 **\@subscriber** 和 **\@destination_db**。 將 **\@subscription_type** 的值指定為 **push**。 如需如何更新訂閱的資訊，請參閱[建立交易式發行集的可更新訂閱](publish/create-an-updatable-subscription-to-a-transactional-publication.md)。  
  
3. 在發行集資料庫的發行者上，執行 [sp_addpushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md)。 指定下列項目：  
  
   - **\@subscriber**、 **\@subscriber_db** 和 **\@publication** 參數。  
  
   - [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 認證，執行「散發者」上的「散發代理程式」時會針對 **\@job_login** 和 **\@job_password** 使用該認證。  
  
     > [!NOTE]
     > 透過 Windows 整合式驗證進行的連線，一律使用由 **\@job_login** 和 **\@job_password** 指定的 Windows 認證。 散發代理程式一律使用 Windows 整合式驗證建立與散發者的本機連線。 根據預設，代理程式會使用 Windows 整合式驗證連線至訂閱者。  
  
   - (選擇性) **\@subscriber_security_mode** 的值 **0**，以及 **\@subscriber_login** 和 **\@subscriber_password** 的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入資訊。 如果您在連接到「訂閱者」時需要使用「SQL Server 驗證」，請指定這些參數。  
  
   - 此訂閱之散發代理程式作業的排程。 如需詳細資訊，請參閱[指定同步處理排程](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
> [!IMPORTANT]
> 當您使用遠端散發者來建立發行者上的發送訂閱時，提供給所有參數的值 (包括 *job_login* 和 *job_password*) 都會以純文字格式傳送給散發者。 您應該先加密發行者及其遠端散發者之間的連線，再執行這個預存程序。 如需詳細資訊，請參閱[啟用資料庫引擎的加密連線 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>若要建立合併式發行集的發送訂閱  
  
1. 在發行集資料庫的發行者上，確認發行集確實是藉由執行 [sp_helpmergepublication](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) 來支援發送訂閱。  
  
   - 如果 **allow_push** 的值為 **1**，則發行集支援發送訂閱。  
  
   - 如果 **allow_push** 的值不為 **1**，請執行 [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)。 針對 **\@property** 指定 **allow_push**，並針對 **\@value** 指定 **true**。  
  
2. 在發行集資料庫的發行者上，執行 [sp_addmergesubscription](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)。 指定下列參數：  
  
   - **\@Publication**。 這是發行集的名稱。  
  
   - **\@subscriber_type**。 針對客訂閱，請指定 **local**。 針對主訂閱，請指定 **global**。  
  
   - **\@subscription_priority**。 指定主訂閱的訂閱優先權 (從 **0.00** 到 **99.99**)。  
  
   如需詳細資訊，請參閱[進階合併式複寫衝突偵測與解決](../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)。  
  
3. 在發行集資料庫的發行者上，執行 [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md)。 指定下列項目：  
  
   - **\@subscriber**、 **\@subscriber_db** 和 **\@publication** 參數。  
  
   - Windows 認證，執行「散發者」上的「合併代理程式」時會針對 **\@job_login** 和 **\@job_password** 使用該認證。  
  
     > [!NOTE]
     > 透過 Windows 整合式驗證進行的連線，一律使用由 **\@job_login** 和 **\@job_password** 指定的 Windows 認證。 合併代理程式一律使用 Windows 整合式驗證來建立到散發者的本機連線。 根據預設，代理程式會使用 Windows 整合式驗證連線至訂閱者。  
  
   - (選擇性) **\@subscriber_security_mode** 的值 **0**，以及 **\@subscriber_login** 和 **\@subscriber_password** 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入資訊。 如果您在連接到「訂閱者」時需要使用「SQL Server 驗證」，請指定這些參數。  
  
   - (選擇性) **\@publisher_security_mode** 的值 **0**，以及 **\@publisher_login** 和 **\@publisher_password** 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入資訊。 如果您在連接到「發行者」時需要使用「SQL Server 驗證」，請指定這些值。  
  
   - 此訂閱之「合併代理程式」作業的排程。 如需詳細資訊，請參閱[指定同步處理排程](../../relational-databases/replication/specify-synchronization-schedules.md)。  
  
> [!IMPORTANT]
> 當您使用遠端散發者來建立發行者上的發送訂閱時，提供給所有參數的值 (包括 *job_login* 和 *job_password*) 都會以純文字格式傳送給散發者。 您應該先加密發行者及其遠端散發者之間的連線，再執行這個預存程序。 如需詳細資訊，請參閱[啟用資料庫引擎的加密連線 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> 範例 (Transact-SQL)  
 下列範例會建立交易式發行集的發送訂閱。 登入和密碼值是在執行階段透過 **sqlcmd** 指令碼變數所提供。  
  
 [!code-sql[HowTo#sp_addtranpushsubscription_agent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_1.sql)]  
  
 下列範例會建立合併式發行集的發送訂閱。 登入和密碼值是在執行階段透過 **sqlcmd** 指令碼變數所提供。  
  
 [!code-sql[HowTo#sp_addmergepushsubscriptionagent](../../relational-databases/replication/codesnippet/tsql/create-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects"></a><a name="RMOProcedure"></a> 使用 Replication Management Objects  
 您可以使用 Replication Management Objects (RMO) 以程式設計的方式建立發送訂閱。 用於建立發送訂閱的 RMO 類別，依該訂閱所屬的發行集類型而定。  
  
> [!IMPORTANT]
> 可能的話，會在執行階段提示使用者輸入安全性認證。 如果您必須儲存認證，請使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows .NET Framework 提供的[密碼編譯服務](/previous-versions/aa719848(v=vs.71))。  
  
#### <a name="to-create-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>若要建立快照式或交易式發行集的發送訂閱  
  
1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 類別建立與發行者的連接。  
  
2. 使用步驟 1 中的發行者連接建立 <xref:Microsoft.SqlServer.Replication.TransPublication> 類別的執行個體。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3. 呼叫 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果這個方法傳回 **false**，則表示步驟 2 中指定的屬性不正確，或伺服器上沒有該發行集存在。  
  
4. 在 **&** 屬性和 **And** 之間執行位元運算邏輯 AND (Visual C# 中的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 和 Visual Basic 中的 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>＞。 如果結果為 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，則將 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 設為 **|** 屬性和 **Or** 之間位元運算邏輯 OR (Visual C# 中的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，並將 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>＞。 然後呼叫 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以啟用發送訂閱。  
  
5. 如果訂閱資料庫不存在，可使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 類別建立它。 如需詳細資訊，請參閱[建立、改變和移除資料庫](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6. 建立 <xref:Microsoft.SqlServer.Replication.TransSubscription> 類別的執行個體。  
  
7. 設定下列訂閱屬性：  
  
   - 將 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 設為到步驟 1 中建立之「發行者」的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>設為訂閱資料庫的名稱。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>設為「訂閱者」的名稱。 
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>設為發行集資料庫的名稱。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>設為發行集的名稱。
  
   - 將 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，並將 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> (或 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> ) 欄位，為「散發代理程式」在「散發者」上執行時所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帳戶提供認證。 此帳戶用於建立到散發者的本機連線，並使用 Windows 驗證建立遠端連線。  
  
     > [!NOTE]
     > 當訂用帳戶是由 **sysadmin** 固定伺服器角色的成員所建立時，則不需要設定 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>，但我們還是建議您對其進行設定。 在這種情況下，代理程式會模擬「SQL Server Agent」帳戶。 如需詳細資訊，請參閱[複寫代理程式安全性模型](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
   - (選擇性) **@value** 的 <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> 值 (預設值)，以建立用於同步處理訂閱的代理程式作業。 如果指定 **false**，則只能以程式設計的方式同步處理訂閱。  
  
   - (選擇性) 在使用「SQL Server 驗證」連接到「訂閱者」時，設定 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> (或 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> ) 欄位。  
  
8. 呼叫 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。  
  
> [!IMPORTANT]
> 當您使用遠端散發者來建立發行者的發送訂閱時，提供給所有屬性的值 (包括 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>) 都會以純文字方式傳送給散發者。 您應該先加密「發行者」及其遠端「散發者」之間的連線，然後再呼叫 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。 如需詳細資訊，請參閱[啟用資料庫引擎的加密連線 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
#### <a name="to-create-a-push-subscription-to-a-merge-publication"></a>若要建立合併式發行集的發送訂閱  
  
1. 使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 類別建立與發行者的連接。  
  
2. 使用步驟 1 中的發行者連接建立 <xref:Microsoft.SqlServer.Replication.MergePublication> 類別的執行個體。 指定 <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>、 <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A>和 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
3. 呼叫 <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> 方法。 如果這個方法傳回 **false**，則表示步驟 2 中指定的屬性不正確，或伺服器上沒有該發行集存在。  
  
4. 在 **&** 屬性和 **And** 之間執行位元運算邏輯 AND (Visual C# 中的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 和 Visual Basic 中的 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>＞。 如果結果為 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.None>，則將 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> 設為 **|** 屬性和 **Or** 之間位元運算邏輯 OR (Visual C# 中的 <xref:Microsoft.SqlServer.Replication.Publication.Attributes%2A> ，並將 <xref:Microsoft.SqlServer.Replication.PublicationAttributes.AllowPush>＞。 然後呼叫 <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> 以啟用發送訂閱。  
  
5. 如果訂閱資料庫不存在，可使用 <xref:Microsoft.SqlServer.Management.Smo.Database> 類別建立它。 如需詳細資訊，請參閱[建立、改變和移除資料庫](../../relational-databases/server-management-objects-smo/tasks/creating-altering-and-removing-databases.md)。  
  
6. 建立 <xref:Microsoft.SqlServer.Replication.MergeSubscription> 類別的執行個體。  
  
7. 設定下列訂閱屬性：  
  
   - 將 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 設為到步驟 1 中建立之「發行者」的 <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>設為訂閱資料庫的名稱。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>設為「訂閱者」的名稱。    
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>設為發行集資料庫的名稱。  
  
   - 將 <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>設為發行集的名稱。    
   - 將 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Login%2A> ，並將 <xref:Microsoft.SqlServer.Replication.IProcessSecurityContext.Password%2A> (或 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A> ) 欄位，為「散發代理程式」在「散發者」上執行時所使用的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 帳戶提供認證。 此帳戶用於建立對散發者的本機連線，並使用 Windows 驗證建立遠端連線。  
  
     > [!NOTE]
     > 當訂用帳戶是由 **sysadmin** 固定伺服器角色的成員所建立時，則不需要設定 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>，但我們還是建議您對其進行設定。 在這種情況下，代理程式會模擬「SQL Server Agent」帳戶。 如需詳細資訊，請參閱[複寫代理程式安全性模型](../../relational-databases/replication/security/replication-agent-security-model.md)。  
  
   - (選擇性) **@value** 的 <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A> 值 (預設值)，以建立用於同步處理訂閱的代理程式作業。 如果指定 **false**，則只能以程式設計的方式同步處理訂閱。  
  
   - (選擇性) 在使用「SQL Server 驗證」連接到「訂閱者」時，設定 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> (或 <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberSecurity%2A> ) 欄位。  
  
   - (選擇性) 在使用「SQL Server 驗證」連接到「發行者」時，設定 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardLogin%2A> 的 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SqlStandardPassword%2A> 和 <xref:Microsoft.SqlServer.Replication.ConnectionSecurityContext.SecureSqlStandardPassword%2A> (或 <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherSecurity%2A> ) 欄位。  
  
8. 呼叫 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。  
  
> [!IMPORTANT]  
> 當您使用遠端散發者來建立發行者的發送訂閱時，提供給所有屬性的值 (包括 <xref:Microsoft.SqlServer.Replication.Subscription.SynchronizationAgentProcessSecurity%2A>) 都會以純文字方式傳送給散發者。 您應該先加密「發行者」及其遠端「散發者」之間的連線，然後再呼叫 <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> 方法。 如需詳細資訊，請參閱[啟用資料庫引擎的加密連線 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine.md)。  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> 範例 (RMO)  
 此範例會建立交易式發行集的新發送訂閱。 用於執行散發代理程式作業的 Windows 帳戶認證，是在執行階段傳遞的。  
  
 [!code-cs[HowTo#rmo_CreateTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createtranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createtranpushsub)]  
  
 此範例會建立合併式發行集的新發送訂閱。 用於執行合併代理程式作業的 Windows 帳戶認證，是在執行階段傳遞的。  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>另請參閱  
 [檢視及修改發送訂閱屬性](../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [複寫安全性最佳做法](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [建立發行集](../../relational-databases/replication/publish/create-a-publication.md)   
 [複寫管理物件概念](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [同步處理發送訂閱](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [訂閱發行集](../../relational-databases/replication/subscribe-to-publications.md)   
 [以指令碼變數使用 sqlcmd](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md)  
  
