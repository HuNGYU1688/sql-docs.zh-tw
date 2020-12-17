---
title: SQL 全文檢索篩選背景程式啟動器 (服務索引標籤)
description: 了解 SQL Server 全文檢索搜尋所使用的 SQL 全文檢索篩選精靈啟動器。 了解其 [屬性] 對話方塊的 [服務] 索引標籤。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 6aad7ebe-c4be-4d37-8536-61502f51faa2
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c682e1e3598750cbbcc2e0e508b427ed88dc1e1f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463769"
---
# <a name="sql-full-text-filter-daemon-launcher-service-tab"></a>SQL 全文檢索篩選背景程式啟動器 (服務索引標籤)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  從 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]開始， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 全文檢索就會使用 SQL 全文檢索篩選背景程式啟動器 (FDHOST 啟動器) 服務。 如果您使用全文檢索搜尋，這個服務就必須執行。 如需有關篩選背景程式主機處理序的詳細資訊，請參閱《 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 線上叢書》中的＜全文檢索搜尋架構＞。  
  
 您可以使用 [SQL 全文檢索篩選背景程式啟動器屬性] 對話方塊的 [服務] 索引標籤來檢視或指定下列選項。  
  
## <a name="options"></a>選項。  
 **二進位路徑**  
 列出這個服務使用之程式檔案的位置。  
  
 **錯誤控制**  
 1 表示 `SERVICE_ERROR_NORMAL`。 如果在電腦啟動過程中，服務無法啟動，啟動程式就會記錄錯誤並顯示快顯訊息方塊，但是仍會繼續啟動作業。 這項值不能被改變。  
  
 **結束碼**  
 發生錯誤時，錯誤號碼會在這個方塊中顯示。 請使用這個號碼進行疑難排解，方法是在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 知識庫中搜尋該號碼，或者將號碼提供給技術支援人員。  
  
 **Host Name**  
 顯示執行 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服務的電腦或叢集名稱。  
  
 **名稱**  
 表示服務的顯示名稱。  
  
 **處理序識別碼**  
 顯示 Windows 處理序識別碼。  
  
 **SQL 服務類型**  
 顯示為呼叫處理序所提供的服務類型。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會安裝數個服務。  
  
 **啟動模式**  
 將這個服務設定為下列選擇：  
  
-   手動：當電腦啟動時，這項服務不會自動啟動。 您必須使用「 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員」或其他工具來啟動服務。  
  
-   自動：當這部電腦啟動時，這項服務會嘗試啟動。  
  
-   已停用：這項服務無法啟動。  
  
 **State**  
 表示這項服務為執行中、已停止或已停用。 " **...** " 表示狀態變更已暫止。  
  
  
