---
description: 建立 InfoPackage
title: 建立 InfoPackage | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 9cd4a848-409f-4681-a390-1c49a2aadbd7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 38c7183853dcac7043672d5e4a622362af08c10b
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96127287"
---
# <a name="create-infopackage"></a>建立 InfoPackage

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 [建立 InfoPackage] 對話方塊可以在 SAP Netweaver BW 系統中建立新的 InfoPackage。  
  
 您可以從 [SAP BW 目的地編輯器] 的 [連線管理員] 頁面開啟 [建立 InfoPackage] 對話方塊。 若要深入了解 SAP BW 目的地，請參閱 [SAP BW Destination](../../integration-services/data-flow/sap-bw-destination.md)。  
  
> [!IMPORTANT]  
>  Microsoft Connector 1.1 for SAP BW 的文件集是假設使用者已熟悉 SAP Netweaver BW 環境。 如需有關 SAP Netweaver BW 的詳細資訊，或有關如何設定 SAP Netweaver BW 物件與處理序的詳細資訊，請參閱 SAP 文件集。  
  
 **若要開啟建立 InfoPackage 對話方塊**  
  
1.  在 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]中，開啟包含 SAP BW 目的地的 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 套件。  
  
2.  在 [資料流程]  索引標籤上，按兩下 SAP BW 目的地。  
  
3.  在 **[SAP BW 目的地編輯器]** 中，按一下 **[連接管理員]** 開啟編輯器的 **[連接管理員]** 頁面。  
  
4.  在 [連線管理員] 頁面的 [建立 SAP BW 物件] 群組方塊中，選取 [InfoPackage]，然後按一下 [建立]。  
  
## <a name="options"></a>選項。  
 **InfoSource**  
 輸入新 InfoPackage 應該依據的 InfoSource 名稱。  
  
 **簡短描述**  
 輸入新 InfoPackage 的描述。  
  
 **來源系統**  
 選取應該與新 InfoPackage 相關聯的來源系統。  
  
 **屬性**  
 表示 InfoPackage 將會載入屬性資料。  
  
 **文字**  
 表示 InfoPackage 將會載入文字資料。  
  
 **交易**  
 表示 InfoPackage 將會載入交易資料。  
  
 **儲存並啟用**  
 儲存並啟用新的 InfoPackage。  
  
## <a name="see-also"></a>另請參閱  
 [Microsoft Connector for SAP BW F1 說明](../../integration-services/microsoft-connector-for-sap-bw-f1-help.md)  
  
  
