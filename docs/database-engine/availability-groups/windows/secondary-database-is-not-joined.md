---
title: 次要資料庫未聯結 | Microsoft Docs
description: 可用性資料庫聯結狀態會檢查次要資料庫的聯結狀態，作為 Always On 可用性群組其原則式管理的一部分。
ms.custom: ''
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: high-availability
ms.topic: conceptual
f1_keywords:
- sql13.swb.agdashboard.drp2joined.issues.f1
helpviewer_keywords:
- Availability Groups [SQL Server], policies
ms.assetid: 10817e5e-75fa-42dd-baa2-359bea3ad051
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1c66fa7c407469ec8f52b11c8393bf7e03ddc734
ms.sourcegitcommit: 54cd97a33f417432aa26b948b3fc4b71a5e9162b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94583945"
---
# <a name="secondary-database-is-not-joined"></a>次要資料庫未加入
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
    
## <a name="introduction"></a>簡介  
  
|||  
|-|-|  
|**原則名稱**|可用性資料庫聯結狀態|  
|**問題**|次要資料庫並未聯結。|  
|**類別目錄**|**警告**|  
|**Facet**|可用性資料庫|  
  
## <a name="description"></a>描述  
 此原則會檢查次要資料庫 (也稱為「次要資料庫複本」) 的聯結狀態。 當資料庫複本未聯結時，原則為狀況不良。 否則原則為狀況良好。  
  
> [!NOTE]  
>  在此 [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)]版本中，可能原因和解決方案的資訊位於 TechNet Wiki 上的 [Secondary database is not joined](https://go.microsoft.com/fwlink/p/?LinkId=220862) (次要資料庫並未聯結)。  
  
## <a name="possible-causes"></a>可能的原因  
 此次要資料庫並未聯結至可用性群組。 此次要資料庫的組態不完整。  
  
## <a name="possible-solution"></a>可能的解決方案  
 使用 Transact-SQL、PowerShell 或 SQL Server Management Studio，將次要複本聯結至可用性群組。 如需將次要複本聯結至可用性群組的詳細資訊，請參閱 [將次要複本聯結至可用性群組 (SQL Server)](https://msdn.microsoft.com/library/ff878473\(SQL.110\).aspx)。  
  
## <a name="see-also"></a>另請參閱  
 [AlwaysOn 可用性群組概觀 &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [使用 AlwaysOn 儀表板 &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-always-on-dashboard-sql-server-management-studio.md)  
  
  
