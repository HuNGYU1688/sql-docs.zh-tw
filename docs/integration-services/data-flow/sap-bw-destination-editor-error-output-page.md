---
description: SAP BW 目的地編輯器 (錯誤輸出頁面)
title: SAP BW 目的地編輯器 (錯誤輸出頁面) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.sapbwdestination.erroroutput.f1
ms.assetid: a543d811-0bd2-4890-a0d3-f5fdcd4524b8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6c3c080e03e34b8b7cf953a5d3544cb4d3bb39ab
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88484518"
---
# <a name="sap-bw-destination-editor-error-output-page"></a>SAP BW 目的地編輯器 (錯誤輸出頁面)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 [SAP BW 目的地編輯器] 的 [錯誤輸出] 頁面可以指定錯誤處理選項。  
  
 若要深入了解 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Connector 1.1 for SAP BW 的 SAP BW 目的地，請參閱 [SAP BW 目的地](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  Microsoft Connector 1.1 for SAP BW 的文件集是假設使用者已熟悉 SAP Netweaver BW 環境。 如需有關 SAP Netweaver BW 的詳細資訊，或有關如何設定 SAP Netweaver BW 物件與處理序的詳細資訊，請參閱 SAP 文件集。  
  
 **開啟錯誤輸出頁面**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，開啟包含 SAP BW 目的地的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 套件。  
  
2.  在 [資料流程]  索引標籤上，按兩下 SAP BW 目的地。  
  
3.  在 [SAP BW 目的地編輯器] 中，按一下 [錯誤輸出] 開啟編輯器的 [錯誤輸出] 頁面。  
  
## <a name="options"></a>選項。  
  
> [!NOTE]  
>  如果您不知道設定目的地的所有必要值，可能必須詢問 SAP 系統管理員。  
  
 **輸入或輸出**  
 檢視輸入的名稱。  
  
 **資料行**  
 這個選項不適用。  
  
 **錯誤**  
 指定目的地應該在發生錯誤時採取的動作：忽略失敗、重新導向資料列，或使元件失效。  
  
 **截斷**  
 這個選項不適用。  
  
 **說明**  
 檢視作業的描述。  
  
 **將這個值設定到選取的資料格**  
 指定目的地應該在發生錯誤或截斷時對所有選取之資料格採取的動作：忽略失敗、重新導向資料列，或使元件失效。  
  
 **套用**  
 將錯誤處理選項套用至選取的資料格。  
  
## <a name="see-also"></a>另請參閱  
 [SAP BW 目的地編輯器 &#40;連線管理員頁面&#41;](../../integration-services/data-flow/sap-bw-destination-editor-connection-manager-page.md)   
 [SAP BW 目的地編輯器 &#40;對應頁面&#41;](../../integration-services/data-flow/sap-bw-destination-editor-mappings-page.md)   
 [SAP BW 目的地編輯器 &#40;進階頁面&#41;](../../integration-services/data-flow/sap-bw-destination-editor-advanced-page.md)   
 [Microsoft Connector for SAP BW F1 說明](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
