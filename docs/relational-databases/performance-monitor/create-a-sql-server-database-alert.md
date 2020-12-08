---
title: 建立 SQL Server 資料庫警示 | Microsoft Docs
description: 您可使用系統監視器來建立 SQL Server 資料庫警示，以在系統監視器計數器達到閾值時引發。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- database performance [SQL Server], alerts
- alerts [SQL Server], creating
- thresholds [SQL Server]
- database alerts [SQL Server]
- tuning databases [SQL Server], alerts
- monitoring performance [SQL Server], alerts
- monitoring server performance [SQL Server], alerts
- database monitoring [SQL Server], alerts
- server performance [SQL Server], alerts
ms.assetid: 0511136a-1b6b-4095-aa45-39e77b67aba2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 512c902e0b35e15b65866464500b4928e105d4ad
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506032"
---
# <a name="create-a-sql-server-database-alert"></a>建立 SQL Server 資料庫警示
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  您可以使用「系統監視器」來建立警示，讓它在達到「系統監視器」計數器的臨界值時引發。 為了回應此警示，「系統監視器」可啟動某個應用程式，諸如撰寫來處理此警示條件的自訂應用程式。 例如，您可以建立一個警示，讓它在死結 (Deadlock) 個數超過特定數值時引發。  
  
 您也可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 與 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 來定義警示。 如需詳細資訊，請參閱 [警示](../../ssms/agent/alerts.md)。  
  
 如需使用「系統監視器」來建立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料庫警示的詳細資訊，請參閱[設定 SQL Server 資料庫警示 &#40;Windows&#41;](../../relational-databases/performance/set-up-a-sql-server-database-alert-windows.md)。  
  
## <a name="see-also"></a>另請參閱  
 [sp_add_alert &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md)   
 [sys.sysperfinfo &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysperfinfo-transact-sql.md)  
  
  
