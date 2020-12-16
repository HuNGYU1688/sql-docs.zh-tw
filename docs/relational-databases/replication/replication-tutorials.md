---
title: 複寫教學課程 | Microsoft Docs
description: 使用這些教學課程來協助您在 SQL Server 中準備用於複寫的伺服器，然後了解如何設定異動複寫及合併式複寫。
ms.custom: ''
ms.date: 04/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- walkthroughs [SQL Server replication]
- replication [SQL Server], tutorials
ms.assetid: 19fbd10e-5b59-4cd0-a988-52d5d9206242
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 2706b8a72aea53ef6f7cb37399580722161a4ea4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468779"
---
# <a name="replication-tutorials"></a>複寫教學課程
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
複寫是在伺服器之間移動資料或資料子集的強大解決方案。 您可以使用異動複寫，在完全連線的伺服器之間複寫資料。 您也可以使用合併式複寫，在間歇性連線的伺服器與用戶端之間複寫資料。 本文中的教學課程會協助您準備用於複寫的伺服器，再教導您設定異動複寫及合併式複寫。 
  
在複寫教學課程中，「發行者」是指包含所要複寫之來源資料的伺服器。 「訂閱者」則是指目的地伺服器。 發行者和訂閱者可以共用相同的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體，但這不是必要條件。 如需詳細資訊，請參閱[複寫發行模型概觀](../../relational-databases/replication/publish/replication-publishing-model-overview.md)。  

這些教學課程使用 NODE1\SQL2016 作為發行者和散發者。 使用 NODE2\SQL2016 作為訂閱者。 
  
> [!NOTE]  
> 這些教學課程所示範的大部分工作都可以透過程式設計的方式來執行。 如需詳細資訊，請參閱[複寫開發人員文件](../../relational-databases/replication/concepts/replication-developer-documentation.md)。  
  
## <a name="replication-tutorials"></a>複寫教學課程  
[教學課程：準備 SQL Server 進行複寫 (發行者、散發者、訂閱者)](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md) 
 
了解如何準備伺服器，以便使用最低權限執行複寫。 您必須先完成本教學課程，才能進行其他複寫教學課程。  
  
[教學課程：設定兩個已完全連線伺服器之間的複寫 (異動)](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md)

了解如何設定異動複寫，在完全連線的伺服器之間複寫資料。 本教學課程也包含一些基本的錯誤疑難排解方法。 

  
[教學課程：設定伺服器和行動用戶端之間的複寫 (合併式)](../../relational-databases/replication/tutorial-replicating-data-with-mobile-clients.md)

了解如何設定合併式複寫，在伺服器與一或多個只是偶而連線的用戶端之間交換資料。  
  
## <a name="see-also"></a>另請參閱  
[檢視及修改複寫安全性設定](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md) 

[異動複寫概觀](./transactional/transactional-replication.md) 

[合併式複寫概觀](./merge/merge-replication.md)

