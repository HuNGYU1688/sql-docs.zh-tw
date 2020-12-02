---
description: 課程 1-5：新增及設定一般檔案來源
title: 步驟 5：新增及設定一般檔案來源 | Microsoft Docs
ms.custom: ''
ms.date: 01/03/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: 5c95ce51-e0fe-4fc5-95eb-2945929f2b13
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfc6ffa21bf3f6c1205b1c9c667cc6e75868cece
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88390864"
---
# <a name="lesson-1-5-add-and-configure-the-flat-file-source"></a>課程 1-5：新增及設定一般檔案來源

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在此工作中，您會在套件中新增及設定「一般檔案」來源。 「一般檔案」來源是一個資料流程元件，此元件會使用「一般檔案」連線管理員所定義的中繼資料。 此中繼資料會指定轉換程序要從一般檔案中擷取之資料的格式和結構。 「一般檔案」來源會使用「一般檔案」連線管理員中的格式定義，從單一一般檔案擷取資料。  
  
針對此工作，您需設定「一般檔案」來源，以使用您先前建立的 [範例一般檔案來源資料] 連線管理員。  
  
## <a name="add-a-flat-file-source-component"></a>新增一般檔案來源元件  
  
1.  若要開啟 [資料流程] 設計工具，請按兩下 [擷取範例貨幣資料] 資料流程工作，或選取 [資料流程] 索引標籤。  
  
2.  在 [SSIS 工具箱] 中，展開 [其他來源]，然後將 [一般檔案來源] 拖曳至 [資料流程] 索引標籤的設計介面中。  
  
3.  在 [資料流程] 設計介面上，於剛新增的 [一般檔案來源] 上按一下滑鼠右鍵，選取 [重新命名]，然後將名稱變更為 **擷取範例貨幣資料**。  
  
4.  按兩下「一般檔案」來源以開啟 [一般檔案來源編輯器] 對話方塊。  
  
5.  在 **[一般檔案連線管理員]** 欄位中，選取 [範例一般檔案來源資料]。  
  
6.  選取 [資料行] 並確認資料行的名稱正確無誤。  
  
7.  選取 [確定]。  
  
8.  在「一般檔案」來源上按一下滑鼠右鍵，然後選取 [屬性]。  
  
9. 在 [屬性] 視窗中，確認 [LocaleID] 屬性是設為 [英文 (美國)]。  
  
## <a name="go-to-next-task"></a>移至下一個工作
[步驟 6：新增及設定查閱轉換](../integration-services/lesson-1-6-adding-and-configuring-the-lookup-transformations.md)  
  
## <a name="see-also"></a>請參閱  
[一般檔案來源](../integration-services/data-flow/flat-file-source.md)  
[一般檔案連線管理員](../integration-services/connection-manager/flat-file-connection-manager.md)  
  
  
  
