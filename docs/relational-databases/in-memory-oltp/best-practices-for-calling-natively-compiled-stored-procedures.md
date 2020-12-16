---
title: 最佳做法 - 原生編譯預存程序
description: 深入了解原生方式編譯預存程序 (通常用於應用程式的效能嚴重不足部分) 的最佳做法。
ms.custom: seo-dt-2019
ms.date: 03/24/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: f39fc1c7-cfec-4a95-97f6-6b95954694b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 04035aa7d1a94050e8b4f1d248fb1313a2ac17f0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465399"
---
# <a name="best-practices-for-calling-natively-compiled-stored-procedures"></a>呼叫原生編譯預存程序的最佳作法
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  原生編譯預存程序：  
  
-   通常用於應用程式的關鍵性效能組件中。  
  
-   經常執行。  
  
-   預期非常快速。  
  
 使用原生編譯預存程序的效能優勢，會隨程序所處理的資料列數目和邏輯數量增多而提升。 例如，如果原生編譯預存程序使用下列一或多個元件，則會展現更佳的效能：  
  
-   彙總：  
  
-   巢狀迴圈聯結。  
  
-   多重陳述式選取、插入、更新和刪除作業。  
  
-   複雜運算式。  
  
-   程序邏輯，例如條件陳述式和迴圈。  
  
 如果您只需要處理一個資料列，使用原生編譯預存程序可能不會提供效能優勢。  
  
 若要避免伺服器必須對應參數名稱和轉換類型，請確定：  
  
-   讓傳遞至程序的參數類型與程序定義中的類型相符。  
  
-   當呼叫原生編譯預存程序時，請使用序數 (無名) 參數。 若要以最有效率方式執行，請勿使用具名參數。  
  
 原生編譯預存程序參數中的無效率情況，可以透過 XEvent **natively_compiled_proc_slow_parameter_passing** 加以偵測：
 - 類型不符︰**reason=parameter_conversion**
 - 具名參數：**reason=named_parameters**
 - 預設值：**reason=default** 
  
## <a name="see-also"></a>另請參閱  
 [原生編譯的預存程序](./a-guide-to-query-processing-for-memory-optimized-tables.md)