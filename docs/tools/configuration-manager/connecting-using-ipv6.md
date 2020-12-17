---
title: 使用 IPv6 連接
description: 了解 SQL Server 和 SQL Server Native Client 中 IPv4 與 IPv6 支援，並了解如何為所要使用的位址設定資料庫引擎。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Internet Protocol
- IPv4
- IPv6
ms.assetid: 2669098c-f5f1-43da-aec6-e91003ac89f6
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: a429bb74fce5226ddd65d853da8c1d67d94e6636
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481669"
---
# <a name="connecting-using-ipv6"></a>使用 IPv6 連接
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 完整支援網際網路通訊協定第 4 版 (IPv4) 與網際網路通訊協定第 6 版 (IPv6)。 當 Windows 是設定為 IPv6 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]時，元件會自動辨識 IPv6 的存在性。 不需要特殊的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態。  
  
 支援包括但不限於下列各項：  
  
-   [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 和其他伺服器元件可同時接聽 IPv4 和 IPv6 位址。 當 IPv4 和 IPv6 都出現時，您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員，來設定 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 只接聽 IPv4 位址或 IPv6 位址。  
  
-   當支援 IPv4 和 IPv6 的電腦上執行的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務是在 IPv4 位址上接受查詢時，它的回應是將 IPv4 位址及第一個 IPv4 TCP 通訊埠列在其清單中。 在 IPv6 位址上接受查詢時，它的回應是將 IPv6 位址及第一個 IPv6 TCP 通訊埠列在其清單中。 若要避免不一致，我們建議 IPv4 和 IPv6 接聽程式設定為接聽相同通訊埠。  
  
-   像 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員之類的工具接受 IPv4 和 IPv6 格式的 IP 位址。 在大部分情況下，如果使用伺服器主機名稱或完整網域名稱 (FQDN) 來指定 \<*computer_name*>\\<執行個體名稱>，則不需要修改連接字串。 如果伺服器電腦有 IPv4 和 IPv6，其主機名稱或 FQDN 將解析成多個 IP 位址，包括至少一個 IPv4 位址及多個 IPv6 位址。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 嘗試依照從 TCP/IP 接收的順序來使用這些 IP 位址建立連線，以及使用第一個成功的連線。 由於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 無法預測順序，因此，這應該被視為隨機順序。 如果 IPv4 和 IPv6 位址都出現，則先嘗試使用 IPv4 位址。 此邏輯對 ODBC、OLE DB 或 ADO.NET 的使用者是透明化的。  
  
    > [!NOTE]  
    >  如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 未接聽 IPv4，在嘗試使用 IPv6 位址之前，嘗試建立的 IPv4 連接必須等到逾時。 若要避免發生此情形，請直接連接到 IPv6 IP 位址，或在具有 IPv6 位址的用戶端上設定別名。  
  
## <a name="see-also"></a>另請參閱  
 [SQL Server 組態管理員](../../relational-databases/sql-server-configuration-manager.md)  
  
  
