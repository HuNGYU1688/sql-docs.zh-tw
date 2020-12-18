---
title: 經由 Proxy 伺服器連線到 SQL Server (SQL Server 組態管理員) | Microsoft Docs
description: 了解如何使用 SQL Server 組態管理員，經由 Proxy 伺服器連線到 SQL Server。 查看如何使用遠端 WinSock (RWS) 遠端接聽。
ms.custom: ''
ms.date: 12/15/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- Remote WinSock
- RWS
- LATs
- proxy servers [SQL Server]
- connections [SQL Server], proxy server
- Microsoft Proxy Server [SQL Server]
- local address tables [SQL Server]
ms.assetid: 39714de0-2a1f-4179-9091-5c3fa4612545
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 74187c8e806e6dd1cadb9ea38860b11ec0bcaba0
ms.sourcegitcommit: f87f2f0f1edc91fe400040d8e3a5810347aa8d70
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857675"
---
# <a name="connect-to-sql-server-through-a-proxy-server-sql-server-configuration-manager"></a>經由 Proxy 伺服器連接至 SQL Server (SQL Server 組態管理員)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  此主題描述如何使用 SQL Server 組態管理員，在 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中透過 Proxy 伺服器連接到 SQL Server。 若要使用遠端 WinSock (RWS) 的方法進行遠端接聽，請定義 Proxy Server 的本機位址資料表 (LAT)，使接聽節點位址在 LAT 項目範圍之外。  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> 使用 SQL Server 組態管理員  
  
#### <a name="to-enable-connections-to-sql-server-through-proxy-server"></a>透過 Proxy 伺服器連線到 SQL Server  
  
1.  請遵循[設定伺服器接聽特定 TCP 通訊埠 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md) 中的步驟，來判斷 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 所使用的 TCP/IP 通訊埠為何，或是將 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 設定為使用指定的通訊埠。  
  
2.  在您的 Proxy 伺服器中，定義 Proxy 伺服器的本機位址資料表 (LAT)，使接聽節點位址不在 LAT 項目的範圍內。 如需詳細資訊，請參閱您的 Proxy 伺服器文件集。  
  
> [!NOTE]
>  本主題適用於內部部署 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)]。 如需與 [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]相關的連線問題，請參閱 [針對 Azure SQL Database 連線問題進行疑難排解](/azure/sql-database/sql-database-troubleshoot-common-connection-issues)。
