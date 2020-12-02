---
description: 加入檔案連接管理員對話方塊 UI 參考
title: 新增檔案連線管理員對話方塊 UI 參考 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.fileconnection.f1
helpviewer_keywords:
- Add File Connection Manager
ms.assetid: 9370bfb5-5993-4ad8-a9cd-2de53f320f34
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8096ccaf1fc92710e46970744a7a4f7cbe000700
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96130604"
---
# <a name="add-file-connection-manager-dialog-box-ui-reference"></a>加入檔案連接管理員對話方塊 UI 參考

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用 **[加入檔案連接管理員]** 對話方塊來定義一組檔案或資料夾的連接。  
  
 若要深入了解多個檔案連接管理員，請參閱＜ [Multiple Files Connection Manager](../../integration-services/connection-manager/multiple-files-connection-manager.md)＞。  
  
> [!NOTE]  
>  [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 中的內建工作和資料流程元件不會使用「多個檔案」連接管理員。 但是，您可以在指令碼工作或指令碼元件中使用這個連接管理員。  
  
## <a name="options"></a>選項。  
 **使用類型**  
 指定用於多個檔案連接管理員的檔案類型。  
  
|值|描述|  
|-----------|-----------------|  
|**建立檔案**|連接管理員將建立檔案。|  
|**現有的檔案**|連接管理員將使用現有的檔案。|  
|**建立資料夾**|連接管理員將建立資料夾。|  
|**現有的資料夾**|連接管理員將使用現有的資料夾。|  
  
 **檔案 / 資料夾**  
 檢視已經使用如下所述的按鈕加入的檔案或資料夾。  
  
 **加入**  
 使用 [選取檔案] 對話方塊來加入檔案，或使用 [瀏覽資料夾] 對話方塊來加入資料夾。  
  
 **編輯**  
 選取檔案或資料夾，然後使用 [選取檔案] 或 [瀏覽資料夾] 對話方塊以不同的檔案或資料夾來取代。  
  
 **移除**  
 選取檔案或資料夾，然後使用 [移除] 按鈕將它從清單中移除。  
  
 **箭頭按鈕**  
 選取檔案或資料夾，然後使用箭頭按鈕上移或下移來指定存取順序。  
  
## <a name="see-also"></a>另請參閱  
 [Integration Services 錯誤和訊息參考](../../integration-services/integration-services-error-and-message-reference.md)  
  
  
