---
description: 稽核全文檢索事件類別
title: Audit Fulltext 事件類別 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
ms.assetid: 95e4c5fd-e16f-446e-b42b-105495a8f39a
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 51cc9e52574085e6d751098bac6f545e88cac467
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96126816"
---
# <a name="audit-fulltext-event-class"></a>稽核全文檢索事件類別
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  當 **連接至全文檢索篩選背景程式處理序並與之通訊時，會發生** Audit Fulltext [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件類別。  
  
## <a name="audit-fulltext-event-class-data-columns"></a>稽核全文檢索事件類別資料行  
  
|資料行名稱|資料類型|描述|資料行識別碼|可篩選|  
|----------------------|---------------|-----------------|---------------|----------------|  
|**錯誤**|**int**|如果此事件報告錯誤，即為 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 錯誤號碼。|31|是|  
|**EventSequence**|**int**|要求中之給定事件的順序。|51|否|  
|**EventSubClass**|**int**|登入使用的連接類型。 1 = 非共用，2 = 共用。|21|是|  
|**IsSystem**|**int**|指出事件是發生在系統處理序或使用者處理序。 1 = 系統，0 = 使用者。|60|是|  
|**SessionLoginName**|**nvarchar**|引發工作階段之使用者的登入名稱。 例如，如果您使用 Login1 連接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，並以 Login2 執行陳述式，則 **SessionLoginName** 將顯示 Login1 而 **LoginName** 則顯示 Login2。 此資料行將同時顯示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 登入。|64|是|  
|**SPID**|**int**|事件發生所在之工作階段的識別碼。|12|是|  
|**StartTime**|**datetime**|事件啟動的時間 (如果有的話)。|14|是|  
|「成功」|**int**|1 = 成功。 0 = 失敗。 例如，值為 1 指出權限檢查成功，而值為 0 指出該檢查失敗。|23|是|  
|**TargetLoginName**|**int**|對於目標為登入的動作 (例如，加入新登入)，這是目標登入的名稱。|42|Yes|  
|**TargetLoginSid**|**int**|對於目標為登入的動作 (例如，加入新登入)，這是目標登入的安全性識別碼 (SID)。|43|是|  
|**TextData**|**ntext**|全文檢索事件的文字資訊。 通常，此欄位提供有關 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 處理序和全文檢索篩選背景程式處理序之間連接的資訊。|1|是|  
  
## <a name="see-also"></a>另請參閱  
 [擴充事件](../../relational-databases/extended-events/extended-events.md)   
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
