---
description: 在指令碼元件中記錄
title: 在指令碼元件中記錄 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Script component [Integration Services], logging
ms.assetid: 17c19787-379e-43fe-9107-e36e17ecda53
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9d761fd4bd949f066a7dc0c59a69efa4a46edfff
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88430290"
---
# <a name="logging-in-the-script-component"></a>在指令碼元件中記錄

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 封裝中記錄可以讓您藉由記錄預先定義之事件或使用者定義訊息，記錄關於執行進度、結果和問題的詳細資訊以供稍後分析。 指令碼元件可以使用 **ScriptMain** 類別的 <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> 方法記錄使用者定義的資料。 如果已啟用記錄，而且已在 [設定 SSIS 記錄] 對話方塊的 [詳細資料] 索引標籤上選取 **ScriptComponentLogEntry** 事件以進行記錄，<xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.Log%2A> 方法的單一呼叫會儲存為資料流程工作設定之所有記錄提供者中的事件資訊。  
  
 以下是記錄的一個簡單範例：  
  
 `Dim bt(0) As Byte`  
  
 `Me.Log("Test Log Event", _`  
  
 `0, _`  
  
 `bt)`  
  
> [!NOTE]  
>  雖然您可以直接從指令碼元件執行記錄，不過您可能會想要考慮實作事件，而不是記錄。 使用事件時，不僅可以啟用事件訊息的記錄，還可用預設或使用者定義的事件處理常式回應事件。  
  
 如需記錄的詳細資訊，請參閱 [Integration Services &#40;SSIS&#41; 記錄](../../../integration-services/performance/integration-services-ssis-logging.md)。  
  
## <a name="see-also"></a>另請參閱  
 [Integration Services &#40;SSIS&#41; 記錄](../../../integration-services/performance/integration-services-ssis-logging.md)  
  
  
