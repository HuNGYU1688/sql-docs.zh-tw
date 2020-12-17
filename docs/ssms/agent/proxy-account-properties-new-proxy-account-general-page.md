---
description: Proxy 帳戶屬性 - 新增 Proxy 帳戶 (一般頁面)
title: Proxy 帳戶屬性 - 新增 Proxy 帳戶 (一般頁面)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.proxy.general.f1
ms.assetid: 5cd81265-bf59-413b-8397-150ddc70d0c7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: efff382c4b65711f71e619ff33cee26c13210c87
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474399"
---
# <a name="proxy-account-properties---new-proxy-account-general-page"></a>Proxy 帳戶屬性 - 新增 Proxy 帳戶 (一般頁面)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此頁面來檢視或變更 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 的 Proxy 帳戶屬性。  
  
## <a name="options"></a>選項  
**Proxy 名稱**  
鍵入 Proxy 的名稱。  
  
**認證名稱**  
鍵入 Proxy 的認證名稱。  
  
> [!NOTE]  
> 提供的認證名稱必須是現有的認證名稱。 如需建立認證的資訊，請參閱[如何：建立 Proxy](../../relational-databases/security/authentication-access/create-a-credential.md)  
  
**...**  
啟動 [選取認證] 對話方塊。  
  
**說明**  
輸入 Proxy 的描述。  
  
**對下列子系統有效**  
選取 Proxy 帳戶可以存取的子系統。  
  
**重新指派作業步驟給**  
選取要被重新指派作業步驟的 Proxy。 當撤銷存取 Proxy 先前已存取過的子系統時，會啟用此清單。  
  
## <a name="see-also"></a>另請參閱  
[建立 SQL Server Agent Proxy](../../ssms/agent/create-a-sql-server-agent-proxy.md)  
