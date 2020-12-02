---
title: 升級 SQL Server 管理工具 | Microsoft Docs
description: 此文章說明升級 SQL Server 管理工具與管理元件 (例如 SQL Server Agent) 的支援。
ms.custom: ''
ms.date: 07/24/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- management tools, upgrading
ms.assetid: 1dab50b9-d16c-49a1-9ecc-af72adb6c378
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: c26897d0783788d458745904f412ce81b20244b2
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125738"
---
# <a name="upgrade-sql-server-management-tools"></a>升級 SQL Server 管理工具

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 支援從 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更新版本升級。 本文記載升級 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理工具和管理元件 (如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent、Database Mail、維護計畫、XPStar 和 XPWeb) 的支援與行為。  
  
> [!IMPORTANT]  
>  若是本機安裝，您必須以管理員身分執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝程式。 如果您是從遠端共用位置執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝程式，則必須使用對遠端共用位置具有讀取和執行權限的網域帳戶。  
  
## <a name="known-upgrade-issues"></a>已知的升級問題  
在您升級到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]之前，請先考慮以下問題：  
  
### <a name="for-all-upgrade-scenarios"></a>如果是全部升級狀況：  
  
- 在升級 MSX 伺服器之前，應該要升級所有的 TSX 伺服器。 如需 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中 MSX/TSX 的詳細資訊，請參閱 [將整個企業的管理自動化](../../ssms/agent/automated-administration-across-an-enterprise.md)。  
  
-   您必須同時升級 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體的所有元件。 在 [!INCLUDE[ssDE](../../includes/ssde-md.md)]執行個體內， [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]、 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]元件的版本號碼都必須相同。  
  
-   當您升級到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 時，可以將元件加入到現有的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]安裝。 如需詳細資訊，請參閱[使用安裝精靈升級 SQL Server 2016 &#40;安裝程式&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)。  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用戶端工具 (例如 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]、 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]、 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Tuning Advisor、sqlcmd 和 osql) 不會升級到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]。 用戶端工具改為使用舊版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的工具進行並存執行。 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 支援從舊版的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用戶端工具匯入設定。  
  
-   在升級期間，從 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的驗證將會更新為 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 驗證到 Windows 驗證。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中不支援 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]驗證。  
  
-   在升級到 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]的期間，將會保留作業與警示的資料。  
  
-   如果要升級的執行個體上有使用 SQLMail，關聯的 XP 將會在升級之後受到支援並啟用。 否則會將其關閉。  
  
-   Database Mail (也稱為 SQLiMail) 將會與 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]元件一起升級。 根據預設，Database Mail 將會在升級之後關閉。 任何結構描述更新都應該在升級之後與更新指令碼一致。  
  
## <a name="see-also"></a>另請參閱  
 [支援的版本與版本升級](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [回溯相容性_已刪除](/previous-versions/sql/sql-server-2016/cc280407(v=sql.130))   
 [使用安裝精靈升級 SQL Server &#40;安裝程式&#41;](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)  
  
