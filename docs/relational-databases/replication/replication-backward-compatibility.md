---
title: 複寫回溯相容性 | Microsoft Docs
description: 在您升級之前，或是在複寫拓撲中具有數個版本的 SQL Server 時，請針對複寫中的回溯相容性檢閱這些資源。
ms.custom: ''
ms.date: 03/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- transactional replication, backward compatibility
- backward compatibility [SQL Server replication]
- merge replication backward compatibility [SQL Server replication]
- replication [SQL Server], backward compatibility
- backward compatibility [SQL Server], replication
- snapshot replication [SQL Server], backward compatibility
- compatibility [SQL Server replication]
ms.assetid: 091c51dc-8b32-4b4f-847e-b317456c8394
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a48ca05acd680cfd89f4438a64cac83c5d00dd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468819"
---
# <a name="replication-backward-compatibility"></a>複寫回溯相容性
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

如果您要升級，或是複寫拓撲中有不只一個 SQL Server 版本，則一定要了解回溯相容性。 

一般規則如下： 

-   散發者可以是任何版本，只要其高於或等於發行者版本 (在許多情況下，散發者與發行者為同一執行個體)。    
-   發行者可以是任何版本，只要它小於或等於散發者版本即可。    
-   訂閱者版本視發行集的類型而定：    
    - 交易式發行集的訂閱者可以是兩個發行者版本內的任何版本。 例如：SQL Server 2012 (11.x) 發行者可以有 SQL Server 2014 (12.x) 和 SQL Server 2016 (13.x) 訂閱者，而 SQL Server 2016 (13.x) 發行者可以有 SQL Server 2014 (12.x) 和 SQL Server 2012 (11.x) 訂閱者。     
    - 合併式發行集的訂閱者可以是等於或小於版本生命週期支援週期所支援發行者版本的所有版本。  


## <a name="replication-matrix"></a>複寫矩陣
[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]


## <a name="additional-resources"></a>其他資源
 [SQL Server 複寫中已被取代的功能](../../relational-databases/replication/deprecated-features-in-sql-server-replication.md)  
 在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中為了與舊版相容而保留的複寫功能，將在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的未來版本中移除。  
  
 [SQL Server 複寫中的重大變更](../../relational-databases/replication/breaking-changes-in-sql-server-replication.md)  
 需要對應用程式做一些改變的複寫功能變更。 

 [升級複寫的資料庫](../../database-engine/install-windows/upgrade-replicated-databases.md)  
 升級參與複寫拓撲的 SQL Server 時的步驟和考量。 
  
  
