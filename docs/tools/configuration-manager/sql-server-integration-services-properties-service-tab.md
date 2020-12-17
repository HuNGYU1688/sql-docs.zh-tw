---
title: SQL Server Integration Services 屬性 (服務索引標籤)
description: 了解 [Integration Services 屬性] 對話方塊中 [服務] 索引標籤上的選項，例如 [二進位路徑]、[主機名稱] 和 [啟動模式]。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 37f0acd9-c96f-48fd-9b53-2ca0097af242
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 2b335440c5f1cf54404f74d8550e5ee687d08307
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463739"
---
# <a name="sql-server-integration-services-properties-service-tab"></a>SQL Server Integration Services 屬性 (服務索引標籤)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  您可以使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] [屬性] 對話方塊上的 [服務] 索引標籤來檢視或指定下列選項。  
  
## <a name="options"></a>選項。  
 **二進位路徑**  
 顯示這個服務使用之程式檔案的位置。  
  
 **錯誤控制**  
 1 表示 `SERVICE_ERROR_NORMAL`。 如果在電腦啟動過程中，服務無法啟動，啟動程式就會記錄錯誤並顯示快顯訊息方塊，但是仍會繼續啟動作業。 這項值不能被改變。  
  
 **結束碼**  
 定義啟動或停止服務時所遇到之問題的 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 錯誤碼。 當錯誤對於此類別所代表的服務而言是唯一時，此屬性會設定為 [ERROR_SERVICE_SPECIFIC_ERROR] \(1066)，此錯誤的相關資訊可見於 **ServiceSpecificExitCode** 屬性。 服務執行時會將此值設定為 NO_ERROR (0)，並在正常結束後再將此值設定為 NO_ERROR (0)。  
  
 **Host Name**  
 顯示執行 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 服務之電腦或叢集的名稱。  
  
 **名稱**  
 表示服務的顯示名稱。  
  
 **處理序識別碼**  
 顯示 Windows 處理序識別碼。  
  
 **SQL 服務類型**  
 顯示為呼叫處理序所提供的服務類型。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會安裝數個服務。  
  
 **啟動模式**  
 將這個服務設定為下列選擇：  
  
-   手動：當電腦啟動時，這項服務不會自動啟動。 您必須使用「 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員」或其他工具來啟動服務。  
  
-   自動：當這部電腦啟動時，這項服務會嘗試啟動。  
  
-   已停用：這項服務無法啟動。  
  
 **State**  
 表示這項服務為執行中、已停止或已停用。 " **...** " 表示狀態變更已暫止。  
  
  
