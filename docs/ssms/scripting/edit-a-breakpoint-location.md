---
title: 編輯中斷點位置
description: 了解如何將 Transact-SQL 指令檔中其中斷點移至到指令碼的另一個位置，或移至不同指令碼。
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint location
ms.assetid: 5c28e411-0377-4210-a7ce-2a5c13dcdf74
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b8e99fef7e7ffff995f5f23afc69bcfe0c9800c2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480639"
---
# <a name="edit-a-breakpoint-location"></a>編輯中斷點位置

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

中斷點位置會指定中斷點位於 [!INCLUDE[tsql](../../includes/tsql-md.md)] 指令碼檔案中的行和字元。 您可以編輯中斷點位址，將中斷點移至指令碼中的另一個位置，或移至不同的指令碼。

## <a name="editing-a-location"></a>編輯位置

當您編輯中斷點位置時，中斷點就會移至新的位置，並且帶有所有現有的屬性，例如叫用計數或條件。  

#### <a name="to-edit-a-breakpoint-location"></a>若要編輯中斷點位置

1. 在編輯器視窗中，以滑鼠右鍵按一下中斷點圖像，然後按一下快速鍵功能表上的 [位置]。  
  
     -或-  
  
     在 [中斷點] 視窗中，以滑鼠右鍵按一下中斷點圖像，然後按一下快速鍵功能表上的 [位置]。  
  
2. 在 [檔案中斷點] 對話方塊中，編輯 [檔案] 以指定新的檔案、編輯 [行] 以指定新的行，或編輯 [字元] 以指定該行中的新位置。 如果您所指定的新檔案已經開啟在 [查詢編輯器] 視窗中，中斷點就會移至該編輯器視窗。 如果該檔案尚未開啟，系統就會開啟新的編輯器視窗、載入該檔案，而且中斷點會移至新的位置。  
  
     偵錯 [!INCLUDE[tsql](../../includes/tsql-md.md)] 時，[允許原始程式碼與原始版本不同] 選項沒有任何作用。  
  
## <a name="see-also"></a>另請參閱

- [指定叫用計數](./specify-a-hit-count.md)
- [指定中斷點動作](./specify-a-breakpoint-action.md)
- [指定中斷點條件](./specify-a-breakpoint-condition.md)
- [指定中斷點篩選條件](./specify-a-breakpoint-filter.md)