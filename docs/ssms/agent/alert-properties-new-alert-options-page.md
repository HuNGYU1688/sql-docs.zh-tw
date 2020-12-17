---
description: 警示屬性 - 新增警示 (選項頁面)
title: 警示屬性 - 新增警示 (選項頁面)
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.options.f1
ms.assetid: 6e4f41aa-832d-46ba-b6b5-cf1d3b15d33f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: cebd4c7204c1f416a40e0ac61bd91212b655eb99
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97472409"
---
# <a name="alert-properties---new-alert-options-page"></a>警示屬性 - 新增警示 (選項頁面)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

> [!IMPORTANT]  
> [Azure SQL 受控執行個體](/azure/sql-database/sql-database-managed-instance)目前支援多數 (但非全部) 的 SQL Server Agent 功能。 如需詳細資料，請參閱 [Azure SQL 受控執行個體與 SQL Server 之間的 T-SQL 差異](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。

使用此頁面來檢視或修改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 的警示選項。  

## <a name="options"></a>選項  
**電子郵件**  
在電子郵件通知內包含事件中的錯誤文字 (如果有的話)。  
  
**呼叫器**  
在呼叫器通知內包含事件中的錯誤文字 (如果有的話)。  
  
**Net Send**  
在 Net Send 通知內包含事件中的錯誤文字 (如果有的話)。  
  
**要傳送的其他通知訊息**  
鍵入要包含在通知訊息中的其他文字。  
  
**回應間隔延遲**  
指定事件重複發生的延遲。 某些事件會在短期間內頻繁發生。 在此情況下，您可能想要知道事件是否已經發生，但不要讓每個事件都產生回應。 使用這個選項即可指定逾時。若有設定延遲，在警示回應事件之後， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 再次回應之前就會等候指定的延遲，不論事件是否在延遲期間有再發生。  
  
**Minutes**  
指定延遲，以分鐘為單位。 若要在每一次發生事件時產生回應，請指定 0 分和 0 秒。  
  
**秒**  
指定延遲，以秒為單位。 若要在每一次發生事件時產生回應，請指定 0 分和 0 秒。  
  
## <a name="see-also"></a>另請參閱  
[警示](../../ssms/agent/alerts.md)  
