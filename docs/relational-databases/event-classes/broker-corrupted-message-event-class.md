---
description: Broker:Corrupted Message 事件類別
title: Broker:Corrupted Message 事件類別 | Microsoft Docs
ms.custom: ''
ms.date: 05/24/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- Broker:Corrupted Message event class
ms.assetid: 084bf198-2138-438e-bdc7-4ff1e04300f7
author: stevestein
ms.author: sstein
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 053d1f562f65360272fa33312915b3c45f75bd6f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469929"
---
# <a name="brokercorrupted-message-event-class"></a>Broker:Corrupted Message 事件類別

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 當 Service Broker 收到損毀的訊息時， 會建立 **Broker:Corrupted Message** 事件。  
  
## <a name="brokercorrupted-message-event-class-data-columns"></a>Broker:Corrupted Message 事件類別資料行  
  
|資料行|類型|描述|資料行編號|可篩選|  
|-----------------|----------|-----------------|-------------------|----------------|  
|**ApplicationName**|**nvarchar**|建立 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體之連接的用戶端應用程式名稱。 這個資料行會填入應用程式所傳送的值，而非程式的顯示名稱。|10|是|  
|**BigintData1**|**bigint**|此訊息的序號。|52|否|  
|**BinaryData**|**image**|此訊息的訊息主體。|2|是|  
|**ClientProcessID**|**int**|主機電腦指派給用戶端應用程式執行中處理序的識別碼。 如果用戶端提供處理序識別碼，這個資料行就會擴展。|9|是|  
|**DatabaseID**|**int**|由 USE *database* 陳述式所指定的資料庫識別碼，或者如果沒有針對指定執行個體發出 USE *database* 陳述式，則是預設資料庫的識別碼。 [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] 資料行，則 **ServerName** 會顯示資料庫的名稱。 請使用 DB_ID 函數判斷資料庫的值。|3|是|  
|**錯誤**|**int**|針對事件內文字的 **sys.messages** 中之訊息識別碼。|31|否|  
|**EventClass**|**int**|擷取的事件類別類型。 對於 **Broker:Corrupted Message** ，一律是 **161**。|27|否|  
|**EventSequence**|**int**|此事件的序號。|51|否|  
|**FileName**|**nvarchar**|遠端結束點的網路位址。|36|否|  
|**GUID**|**uniqueidentifier**|損毀訊息所屬之交談的交談識別碼。 此識別碼是以訊息的一部份傳送，並在交談的兩端之間共用。|54|否|  
|**Host Name**|**nvarchar**|執行用戶端的電腦名稱。 這個資料行會在用戶端提供主機名稱時填入。 若要判斷主機名稱，請使用 HOST_NAME 函數。|8|是|  
|**IntegerData**|**int**|此訊息的片段編號。|25|是|  
|**IsSystem**|**int**|指出事件是發生在系統處理序或使用者處理序。 1 = 系統，0 = 使用者。|60|否|  
|**LoginSid**|**image**|已登入之使用者的安全性識別碼 (SID)。 伺服器上的每一個登入之 SID 是唯一的。|41|是|  
|**NTDomainName**|**nvarchar**|使用者所隸屬的 Windows 網域。|7|是|  
|**NTUserName**|**nvarchar**|擁有產生此事件之連接的使用者名稱。|6|是|  
|**ObjectName**|**nvarchar**|交談另一端的服務名稱，以及遠端資料庫用來連接此資料庫的連接字串。|34|否|  
|**RoleName**|**nvarchar**|接收此訊息的結束點角色。 下列其中一個值。<br /><br /> **起始端**：接收的結束點是交談的起始端。<br /><br /> **目標**：接收的結束點是交談的目標。|38|否|  
|**ServerName**|**nvarchar**|正在追蹤之 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體的名稱。|26|否|  
|**嚴重性**|**int**|若錯誤造成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 將訊息卸除，此錯誤的嚴重性。|29|否|  
|**SPID**|**int**|由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 指派給用戶端關聯之處理序的伺服器處理序識別碼。|12|是|  
|**StartTime**|**datetime**|事件啟動的時間 (如果有的話)。|14|是|  
|**State**|**int**|指出產生事件的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 原始程式碼內的位置。 每個可能產生此事件的位置都有不同的狀態碼。 Microsoft 支援工程師可以使用此狀態碼來尋找產生事件的位置。|30|否|  
|**TextData**|**ntext**|偵測到的損毀之描述。|1|是|  
|**Transaction ID**|**bigint**|系統指派的交易識別碼。|4|否|  
  
 此事件的 **TextData** 資料行包含描述此訊息問題的訊息。  
  
  
