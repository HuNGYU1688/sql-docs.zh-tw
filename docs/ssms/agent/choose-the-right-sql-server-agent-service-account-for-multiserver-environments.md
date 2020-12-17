---
description: 為多伺服器環境選擇適當的 SQL Server Agent 服務帳戶
title: 針對多伺服器環境選擇 Agent 服務帳戶
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, service accounts
- multiserver environments [SQL Server], SQL Server Agent service account behavior
ms.assetid: a07e2f38-281c-495b-965b-13fad03ba548
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: d5a25c6532207eb709d90ac1acb91c7ff8d031c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472359"
---
# <a name="choose-the-right-sql-server-agent-service-account-for-multiserver-environments"></a>為多伺服器環境選擇適當的 SQL Server Agent 服務帳戶

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

您為 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務所選擇的 Windows 帳戶會影響多伺服器環境的行為，如下所示：  
  
-   如果您是以不是本機 Windows Administrators 群組成員的帳戶來執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務，則將目標伺服器編列至主要伺服器會失敗。 如果這麼做，則會傳回下列錯誤訊息：  
  
    「編列作業失敗。」  
  
    重新啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務，以解決這個問題。  
  
-   如果以本機系統帳戶執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務，則只有在主要伺服器和目標伺服器都位在相同電腦上時，才支援「主要伺服器-目標伺服器」作業。 如果您使用這個組態，則將目標伺服器編列至主要伺服器時，會傳回下列訊息：  
  
    「請確定 <目標伺服器電腦名稱> 的代理程式啟動帳戶有權限以 targetServer 的身分登入」。  
  
    您可以忽略此參考訊息。 編列作業應該順利完成。  
  
如需為 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 服務選擇帳戶的詳細資訊，請參閱 [選取 SQL Server Agent 服務的帳戶](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md)。  
