---
description: 將資料表加入至 CDC 執行個體
title: 將資料表新增至 CDC 執行個體 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- addTabs
ms.assetid: ad260e19-c021-4035-9311-c02fc96ceaea
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e025963ad981e05696af4676155efe642ee49893
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123753"
---
# <a name="add-tables-to-a-cdc-instance"></a>將資料表加入至 CDC 執行個體

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 [資料表選取範圍] 對話方塊，將 Oracle 來源中的其他資料表加入至 CDC 執行個體。 選取的資料表會加入至屬性編輯器中 **[資料表]** 索引標籤的清單。  
  
 根據預設，此對話方塊的資料表清單中不包含任何資料表。 您可以選取 [(全選)] 核取方塊或搜尋特定的資料表。  
  
 **若要搜尋特定資料表**  
 依照以下方式輸入搜尋準則，然後按一下 [搜尋]：  
  
-   **結構描述**：從清單中選取資料庫結構描述。 清單中只會包含具有該結構描述的資料表。  
  
-   **資料表名稱模式**：輸入任何字元字串。 只會顯示包含所輸入之字元字串的資料表。  
  
> [!NOTE]  
>  您可以在一個或兩個欄位中輸入準則。  
  
-   **顯示前 1000 個相符的資料表**：預設會選取這個核取方塊。 它會將顯示畫面限制為前 1000 個相符的資料表。 如果您清除此核取方塊，符合準則的所有資料表都會顯示。 如果有大量的資料表，則顯示清單可能需要很長的時間。  
  
 **若要選取包含在 CDC 執行個體中的資料表**  
 按一下您想要包含之任何資料表旁邊的核取方塊，然後按一下 [加入]。 資料表隨即加入至新增執行個體精靈中 **[選取資料表和資料行]** 頁面的清單中。  
  
 按一下 **[關閉]** ，關閉此對話方塊，而不加入任何資料表。  
  
> [!NOTE]  
>  如果您選取的資料表包含不支援的資料類型，您會看到錯誤訊息而且不會包含該資料表。  
  
> [!NOTE]  
>  您可以在檢視器中檢視資料表清單。 當使用檢視器時，此資訊是唯讀的。 檢視器也包括資料表中擷取的資料行清單。 如需有關如何存取此檢視器的詳細資訊，請參閱＜ [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md)＞。  
  
## <a name="see-also"></a>另請參閱  
 [如何編輯 CDC 執行個體屬性](../../integration-services/change-data-capture/how-to-edit-the-cdc-instance-properties.md)   
 [How to Manage a CDC Instance](../../integration-services/change-data-capture/how-to-manage-a-cdc-instance.md)   
 [選取 Oracle 資料表以擷取變更](../../integration-services/change-data-capture/select-oracle-tables-for-capturing-changes.md)  
  
  
