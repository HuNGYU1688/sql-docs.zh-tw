---
description: 資料截斷 (SSIS)
title: 資料截斷 (SSIS) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- truncating data
- data truncation [Integration Services]
- expressions [Integration Services], data truncation
- truncate options [Integration Services]
ms.assetid: 02461e15-49ca-445b-abb3-5c369c283ec2
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 12431bc9717b28552cc79d6f7d8e3026ed90c7cd
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123263"
---
# <a name="data-truncation-ssis"></a>資料截斷 (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  將某個資料類型的值轉換成其他類型，可能會導致值被截斷。  
  
 發生截斷的狀況︰  
  
-   如果原始字串包含雙位元組字元，將字串資料從 *DT_WSTR* 轉換成相同長度的 *DT_STR* 時。  
  
-   當整數從 *DT_I4* 轉型成 *DT_I2* 時，可能會遺失大量位數。  
  
-   不帶正負號的整數轉換成帶正負號的整數時，可能會遺失大量位數。  
  
-   當實數從 *DT_R8* 轉型成 *DT_R4* 時，可能會遺失少量位數。  
  
-   當整數從 *DT_I4* 轉型成 *DT_R4* 時，可能會遺失少量位數。  
  
 運算式評估工具會識別可能導致截斷的明確轉換，並在剖析運算式時發出警告。 例如，如果將 30 個字元的字串轉換成 20 個字元的字串，則運算式評估工具會向您發出警告。  
  
 不過，執行階段不檢查截斷。 資料在執行階段截斷時，不會發出警告。 多數資料配接器和轉換支援錯誤輸出，可處理錯誤資料列的配置。  
  
 如需處理資料截斷的詳細資訊，請參閱 [Error Handling in Data](../../integration-services/data-flow/error-handling-in-data.md)(處理資料時發生錯誤)。  
  
  
