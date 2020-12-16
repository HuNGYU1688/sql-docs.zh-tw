---
title: 在散發者端啟用遠端發行者 (SSMS)
description: 了解如何使用 SQL Server Management Studio (SSMS) 在散發者端啟用遠端發行者。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- remote Distributors [SQL Server replication]
- Publishers [SQL Server replication]
ms.assetid: 6f8e2831-5c45-4e39-8e51-d37e2813cf3d
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 3633ff588a8d7252b78f5a2510eaad96f7c1eb64
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460251"
---
# <a name="enable-a-remote-publisher-at-a-distributor-sql-server-management-studio"></a>在散發者端啟用遠端發行者 (SQL Server Management Studio)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  在 **[發行者]** 頁面中讓「發行者」使用遠端「散發者」。 此頁面會在 [設定散發] 精靈與 [散發者屬性 - \<Distributor>] 對話方塊中提供。 如需使用精靈以及存取對話方塊的詳細資訊，請參閱[設定發行和散發](../../relational-databases/replication/configure-publishing-and-distribution.md)和[檢視和修改散發者和發行者屬性](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)。  
  
### <a name="to-enable-a-publisher-in-the-configure-distribution-wizard"></a>若要在設定散發精靈中啟用發行者  
  
1.  在「設定散發精靈」的 **[發行者]** 頁面中，按一下 **[加入]** 。  
  
2.  按一下 **[加入 SQL Server 發行者]** 。 如需有關啟用 Oracle 發行者以使用散發者的資訊，請參閱＜ [Create a Publication from an Oracle Database](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)＞。  
  
3.  在 **[連接到伺服器]** 對話方塊中，指定將使用遠端散發者的發行者連接資訊，然後按一下 **[連接]** 。  
  
4.  在 **[散發者密碼]** 頁面的 **[密碼]** 和 **[確認密碼]** 文字方塊中，為 **distributor_admin** 帳戶 (複寫將用其從「發行者」連接到「散發者」以執行管理工作) 指定增強式密碼。  
  
5.  若要檢視和修改發行者的設定，請按一下屬性按鈕 ([...])。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

### <a name="to-enable-a-publisher-in-the-distributor-properties-dialog-box"></a>若要在散發者屬性對話方塊中啟用發行者  
  
1.  在 [散發者屬性 - \<Distributor>] 對話方塊的 [發行者] 頁面上，按一下 [新增]。  
  
2.  按一下 **[加入 SQL Server 發行者]** 。 如需有關啟用 Oracle 發行者以使用散發者的資訊，請參閱＜ [Create a Publication from an Oracle Database](../../relational-databases/replication/publish/create-a-publication-from-an-oracle-database.md)＞。  
  
3.  在 **[連接到伺服器]** 對話方塊中，指定將使用遠端散發者的發行者連接資訊，然後按一下 **[連接]** 。  
  
4.  在 **[發行者]** 頁面的 **[密碼]** 和 **[確認密碼]** 文字方塊中，為 **distributor_admin** 帳戶 (複寫將用其從「發行者」連接到「散發者」以執行管理工作) 指定增強式密碼。  
  
5.  若要檢視和修改發行者的設定，請按一下屬性按鈕 ([...])。  
  
6.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>另請參閱  
 [設定發行和散發](../../relational-databases/replication/configure-publishing-and-distribution.md)   
 [[設定散發]](../../relational-databases/replication/configure-distribution.md)   
 [保護散發者](../../relational-databases/replication/security/secure-the-distributor.md)  
  
  
