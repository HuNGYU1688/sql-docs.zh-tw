---
title: 管理屬於可用性群組一部分的已複寫發行者資料庫
description: 描述如何管理和維護作為 SQL 複寫發行者並同時參與 Always On 可用性群組的資料庫。
ms.custom: seodec18
ms.date: 05/18/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 55b345fe-2eb9-4b04-a900-63d858eec360
author: cawrites
ms.author: chadam
ms.openlocfilehash: 864fca7c1d2983bec6296f1e82304cff31f658cb
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94584198"
---
# <a name="manage-a-replicated-publisher-database-as-part-of-an-always-on-availability-group"></a>管理屬於 Always On 可用性群組一部分的已複寫發行者資料庫
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  本主題將討論使用 AlwaysOn 可用性群組時維護發行集資料庫的特殊考量。  
  
##  <a name="maintaining-a-published-database-in-an-availability-group"></a><a name="MaintainPublDb"></a> 在可用性群組中維護已發行的資料庫  
 維護 AlwaysOn 發行集資料庫基本上與維護標準發行集資料庫相同，但是具有下列考量事項：  
  
-   您必須在主要複本主機上進行管理。 在 [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]中，主要複本主機以及可讀取次要複本的發行集會出現在 **[本機發行集]** 資料夾底下。 容錯移轉之後，如果升級為主要複本的次要複本無法讀取，您可能必須手動重新整理 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] ，才能反映變更。  
  
-   複寫監視器永遠都會在原始發行者底下顯示發行集資訊。 不過，只要加入原始發行者做為伺服器，您就可以在複寫監視器中檢視任何複本的這項資訊。  
  
-   使用預存程序或 Replication Management Objects (RMO) 在目前主要複本上管理複寫時，如果您指定了發行者名稱，就必須指定在其上啟用資料庫以供複寫的執行個體名稱 (原始發行者)。 若要決定適當的名稱，請使用 **PUBLISHINGSERVERNAME** 函數。 當發行資料庫聯結了可用性群組時，儲存在次要資料庫複本中的複寫中繼資料會與主要複本上的中繼資料完全相同。 因此，對於在主要複本上啟用以供複寫的發行集資料庫而言，儲存在次要複本之系統資料表中的發行者執行個體名稱是主要複本的名稱，而不是次要複本的名稱。 如果發行集資料庫容錯移轉至次要複本，這會影響複寫組態和維護。 例如，如果要在容錯移轉後於次要複本上使用預存程序來設定複寫，且想要將提取訂閱加入至不同複本上啟用的發行集資料庫，則必須將原始發行者 (而非目前發行者) 的名稱指定為 **sp_addpullsubscription** 或 **sp_addmergepullsubscription** 的 *\@publisher* 參數。 不過，如果您在容錯移轉後啟用發行集資料庫，則儲存在系統資料表中的發行者執行個體名稱就是目前主要主機的名稱。 在此情況中，您會為 *\@publisher* 參數使用目前主要複本的主機名稱。  
  
    > [!NOTE]  
    >  對於某些程序 (例如 **sp_addpublication**) 而言， *\@publisher* 參數僅支援非 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 執行個體的發行者；在這些情況中，該參數便與 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On 無關。  
  
-   若要在容錯移轉後同步處理 [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] 中的訂閱，請從訂閱者同步處理提取訂閱，並從使用中發行者同步處理發送訂閱。  
  
##  <a name="removing-a-published-database-from-an-availability-group"></a><a name="RemovePublDb"></a> 從可用性群組中移除已發行的資料庫  
 如果您已從可用性群組中移除已發行的資料庫，或者已卸除具有已發行成員資料庫的可用性群組，請考慮下列問題。  
  
-   如果您已從可用性群組主要複本中移除位於原始發行者端的發行集資料庫，就必須執行 **sp_redirect_publisher**，但不指定 *\@redirected_publisher* 參數的值，以便移除發行者/資料庫配對的重新導向。  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB';  
    ```  
  
     主要複本上的資料庫將保持復原狀態，而且必須進行還原。 一旦您這樣做之後，複寫應該會針對原始發行者維持相同的運作方式。  
  
-   如果發行集資料庫從原始發行者容錯移轉至複本，而且您已從可用性群組主要複本中移除該資料庫，請使用 **sp_redirect_publisher** 預存程序，將原始發行者明確重新導向至新的發行者。 資料庫將保持復原狀態，而且必須進行還原。 一旦您這樣做之後，複寫應該會繼續運作，如同它在可用性群組底下運作一樣。  
  
    ```  
    EXEC sys.sp_redirect_publisher   
        @original_publisher = 'MyPublisher',  
        @published_database = 'MyPublishedDB',  
        @redirected_publisher = 'MyNewPublisher';  
    ```  
  
     請勿從散發者中移除原始發行者的遠端伺服器，即使無法再存取伺服器也一樣。 散發者需要原始發行者的伺服器中繼資料，才能滿足發行集中繼資料查詢。  
  
-   如果您已移除完整可用性群組，成員複寫資料庫的相關行為就會與從可用性群組中移除已發行資料庫時的行為相同。 一旦您已經還原資料庫並且修改重新導向之後，就可以從最後一個主要複本恢復複寫。 如果資料庫是在其原始發行者端還原，您就應該移除重新導向。 如果資料庫是在不同的主機上還原，您就應該將重新導向明確導向至新主機。  
  
    > [!NOTE]  
    >  當您移除了具有已發行成員資料庫的可用性群組，或者從可用性群組中移除已發行的資料庫時，已發行資料庫的所有複本都將保持復原狀態。 如果進行還原，每個複本都會顯示成已發行的資料庫。 您應該只保留一個具有發行集中繼資料的複本。 若要針對已發行的資料庫複本停用複寫，請先從資料庫中移除所有訂閱和發行集。  
  
     執行 **sp_dropsubscription** 可移除發行集訂閱。 請務必將 *\@ignore_distributor* 參數設定為 1，以便在散發者端保留使用中發行資料庫的中繼資料。  
  
    ```  
    USE MyDBName;  
    GO  
  
    EXEC sys.sp_dropsubscription   
        @subscriber = 'MySubscriber',  
        @publication = 'MyPublication',  
        @article = 'all',  
        @ignore_distributor = 1;  
    ```  
  
     執行 **sp_droppublication** 可移除所有發行集。 同樣地，請將 *\@ignore_distributor* 參數設定為 1，以便在散發者端保留使用中發行資料庫的中繼資料。  
  
    ```  
    EXEC sys.sp_droppublication   
        @publication = 'MyPublication',  
        @ignore_distributor = 1;  
    ```  
  
     執行 **sp_replicationdboption** 可針對資料庫停用複寫。  
  
    ```  
    EXEC sys.sp_replicationdboption  
        @dbname = 'MyDBName',  
        @optname = 'publish',  
        @value = 'false';  
    ```  
  
     此時，您就可以保留或卸除已發行資料庫的複本。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相關工作  
  
-   [設定 AlwaysOn 可用性群組的複寫 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [複寫、變更追蹤、變更資料擷取和 AlwaysOn 可用性群組 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replicate-track-change-data-capture-always-on-availability.md)  
  
-   [複寫管理常見問題集](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
-   [複寫訂閱者及 AlwaysOn 可用性群組 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)  
  
## <a name="see-also"></a>另請參閱  
 [AlwaysOn 可用性群組的必要條件、限制和建議 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [AlwaysOn 可用性群組概觀 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Always On 可用性群組：互通性 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [SQL Server 複寫](../../../relational-databases/replication/sql-server-replication.md)  
  
  
