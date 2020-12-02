---
title: 安裝 SQL Server 服務更新 | Microsoft Docs
description: 本文提供在新安裝期間或安裝 SQL Server 後，安裝 SQL Server 更新的資訊。
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 7d6c962b-c8d0-49f7-a2ac-00ad8dca930a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 0bb5751c8e0bce55ecf83d8f1226a102b9fc691f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125888"
---
# <a name="install-sql-server-servicing-updates"></a>安裝 SQL Server 服務更新

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

本文提供安裝 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 更新的資訊。 本節提供下列作業的相關資訊：
  
- 在進行新安裝期間安裝 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 的更新。  
  
- 在 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 安裝完成之後安裝更新。  
  
即時安裝最新的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新，確保系統保持在最新狀態而且具有最新的安全性更新。  
  
## <a name="installing-updates-for-noversion-during-a-new-installation"></a>在進行新安裝期間安裝 [!INCLUDE[noVersion](../../includes/ssNoVersion-md.md)] 的更新。  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝程式會整合最新產品更新與主要產品安裝，因此主要產品及其適用的更新可同時安裝。 產品更新可以從下列位置搜尋適用的更新：  
  
- [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update  
  
- Windows Server Update Services (WSUS)  
  
- 本機資料夾  
  
- 網路共用  
  
安裝程式找到最新版本的可用更新後，會使用目前的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝程序進行下載與整合。 產品更新可能包括累計更新、Service Pack，或 Service Pack 加上累計更新。  
  
## <a name="installing-updates-for-ssnoversion-after-it-has-already-been-installed"></a>在 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] 安裝完成之後安裝更新  
在已安裝的 [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]執行個體上，建議您套用最新的安全性更新和重大更新，包括一般發行版本 (GDR)、Service Pack (SP) 和累積更新 (CU)。 如需其他資訊，請參閱 [March, 2016 announcement on the SQL Server Incremental Servicing Model (ISM)](/archive/blogs/sqlreleaseservices/announcing-updates-to-the-sql-server-incremental-servicing-model-ism) (2016 年 3 月的 SQL Server 增量服務模型 (ISM) 公告)。

> [!NOTE]
> 從 [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] 開始，我們將採用經過簡化的可預測主流服務生命週期，不再提供 Service Pack (SP)。 只會視需要提供累積更新 (CU) 和一般發行版本 (GDR)。
> 如需其他資訊，請參閱 [September, 2017 announcement on the Modern Servicing Model for SQL Server (MSM)](/archive/blogs/sqlreleaseservices/announcing-the-modern-servicing-model-for-sql-server) (2017 年 9 月的 SQL Server 新式服務模型 (MSM) 公告)。
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 更新可透過 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新 (MU)、Windows Server Update Services (WSUS) 和 Microsoft 下載中心取得。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的安全性和重大更新是透過 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 提供，若要查看這些更新，您必須透過 [控制台] 中的 Windows Update 小程式選擇加入 MU。  
  
當您透過 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 更新收到更新時，它會以自動模式將所有 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能更新至最新版本。 如果您需要更多彈性，或者是無法存取網際網路或 WSUS，就必須從 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 下載中心取得更新。  
  
## <a name="see-also"></a>另請參閱  
[從 [安裝精靈] &#40;安裝程式&#41; 安裝 SQL Server](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)        
[將功能新增至 SQL Server 的執行個體 &#40;安裝程式&#41;](./add-features-to-an-instance-of-sql-server-setup.md)         
[修復失敗的 SQL Server 安裝](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)