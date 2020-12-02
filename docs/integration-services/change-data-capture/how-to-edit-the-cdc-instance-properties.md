---
description: 如何編輯 CDC 執行個體屬性
title: 如何編輯 CDC 執行個體屬性 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 7a6c719a-3735-43b7-b3ab-dfadd325eca2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ee2861b40dbe132e7c95b52802bd10ccd46c20c1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88426090"
---
# <a name="how-to-edit-the-cdc-instance-properties"></a>如何編輯 CDC 執行個體屬性

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  此程序描述如何使用 CDC 設計工具主控台來編輯 CDC 執行個體的組態屬性。  
  
### <a name="to-edit-the-cdc-instance-configuration-properties"></a>若要編輯 CDC 執行個體組態屬性  
  
1.  從 **[開始]** 功能表選取 **[CDC 設計工具主控台]** 。  
  
2.  在左窗格中展開 **[異動資料擷取]** ，然後展開包含執行個體的服務，該執行個體包含您要編輯的屬性。  
  
3.  選取您要編輯屬性的執行個體名稱。  
  
4.  從 CDC 設計工具主控台右側的 [動作] 窗格中，按一下 **[屬性]**。  
  
     您也可以用滑鼠右鍵按一下您要編輯屬性的執行個體，然後按一下 [屬性]。  
  
5.  在屬性編輯器中，於下列索引標籤上編輯屬性：  
  
    -   **Oracle**：在屬性編輯器中使用 **[Oracle]** 索引標籤，以變更您在新增執行個體精靈中 [建立 CDC 資料庫] 頁面上提供的描述，以及變更 Oracle 記錄採礦資料庫連接資訊。  
  
         如需有關您可以在此索引標籤上編輯之內容的詳細資訊，請參閱＜ [Edit the Oracle Database Properties](../../integration-services/change-data-capture/edit-the-oracle-database-properties.md)＞。  
  
    -   **資料表**：使用 **[資料表]** 索引標籤，以變更您從 Oracle 來源資料庫選取的資料表和資料行。  
  
         如需有關您可以在此索引標籤上編輯之內容的詳細資訊，請參閱＜ [Edit Tables](../../integration-services/change-data-capture/edit-tables.md)＞。  
  
    -   **指令碼**：使用 [指令碼] 索引標籤可在即將設定補充記錄的 Oracle 來源資料庫上執行或重新執行指令碼。  
  
         如需有關您可以在此索引標籤上執行之作業的詳細資訊，請參閱＜ [Review and Generate Supplemental Logging Scripts](../../integration-services/change-data-capture/review-and-generate-supplemental-logging-scripts.md)＞。  
  
    -   **進階**：使用 **[進階]** 索引標籤，將特殊屬性加入至 CDC 執行個體。  
  
         如需有關您可以在此索引標籤上執行之作業的詳細資訊，請參閱＜ [Edit the Advanced Properties](../../integration-services/change-data-capture/edit-the-advanced-properties.md)＞。  
  
  
