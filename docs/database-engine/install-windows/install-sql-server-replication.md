---
title: 安裝 SQL Server 複寫 | Microsoft Docs
description: 您可使用 [SQL Server 安裝精靈] 或在命令提示視窗中安裝複寫元件。
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- components [SQL Server replication]
- command line installations [SQL Server replication]
- installing replication
- replication [SQL Server], installing
- command prompt [SQL Server replication]
ms.assetid: c50ad078-060b-4a8d-ad45-9e31a8d85729
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016||=sqlallproducts-allversions'
ms.openlocfilehash: 55a8a744c384df0536cef1073e05b2fc61a7a1ac
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125931"
---
# <a name="install-sql-server-replication"></a>安裝 SQL Server 複寫

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝精靈或命令提示字元來安裝複寫元件。 請在安裝 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]或修改現有的執行個體時安裝複寫。  
  
安裝複寫元件之後，您必須先設定伺服器，才能使用複寫。 如需詳細資訊，請參閱《 [線上叢書》的](../../relational-databases/replication/configure-distribution.md) 設定散發 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
>[!IMPORTANT]  
>如果在修改現有的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體時安裝複寫元件，則必須在安裝完成之後停止並重新啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent。 這個動作有助於確保 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 能夠辨識複寫代理程式子系統，而且可以從作業步驟呼叫複寫代理程式。  
  
## <a name="installing-replication-by-using-setup"></a>使用安裝程式安裝複寫  
**在安裝新執行個體時安裝複寫 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- 若要安裝複寫元件 (包括 Replication Management Objects (RMO))，請在安裝精靈的 [特徵選取] 頁面上選取 [SQL Server 複寫]。  
  
## <a name="installing-replication-from-the-command-prompt"></a>從命令提示字元安裝複寫  
 **在安裝新執行個體時安裝複寫 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**  
  
- 請參閱[從命令提示字元安裝 SQL Server](./install-sql-server-from-the-command-prompt.md)。  
  
## <a name="see-also"></a>另請參閱  
 [安裝 SQL Server](../../database-engine/install-windows/install-sql-server.md)   
 [從命令提示字元安裝 SQL Server](./install-sql-server-from-the-command-prompt.md)   
 [SQL Server 版本支援的功能](../../sql-server/editions-and-components-of-sql-server-2017.md)  
  
