---
description: 資料庫引擎擴充預存程序 - 程式設計
title: 擴充預存程式程式設計 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- gateway applications [SQL Server]
- extended stored procedures [SQL Server], about extended stored procedures
- Open Data Services [SQL Server]
- ODS [SQL Server]
ms.assetid: 561305cd-c803-48af-9eec-2c19f4d311ce
author: rothja
ms.author: jroth
ms.openlocfilehash: c9b17ecfe69eef7f30015cdcef84aaef4790afc3
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88460837"
---
# <a name="database-engine-extended-stored-procedures---programming"></a>資料庫引擎擴充預存程序 - 程式設計
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] 請改用 CLR 整合。  
  
 過去 Open Data Services 是用來撰寫伺服器應用程式，例如到非 SQL Server 資料庫環境的閘道。 [!INCLUDE[msCoName](../../includes/msconame-md.md)]不 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支援開放式資料服務 API 的過時部分。 原始的 Open Data Services API 中唯一仍受 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支援的部分是擴充預存程序函數，所以 API 已重新命名為「擴充預存程序 API」。  
  
 隨著分散式查詢和 CLR 整合之類的更新和更強大技術的出現，擴充預存程序 API 應用程式的需求也大致被取代。  
  
> [!NOTE]  
>  如果您具備現有的閘道應用程式，就無法使用隨附於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的 opends60.dll 來執行應用程式。 閘道應用程式已不再受到支援。  
  
## <a name="extended-stored-procedures-vs-clr-integration"></a>擴充預存程序與 CLR 整合的比較  
 在舊版的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，擴充預存程序 (XP) 提供資料庫應用程式開發人員用來撰寫伺服器端邏輯的唯一機制，這些邏輯在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 中不是難以表示，就是無法撰寫。 CLR 整合會提供更健全的替代方法來撰寫此類預存程序。 此外，使用 CLR 整合時，過去以預存程序形式所撰寫的邏輯常可更精準地表示為資料表值函式，如此就可以用 SELECT 陳述式 (將其內嵌於 FROM 子句) 來查詢函數建立的結果。  
  
## <a name="see-also"></a>另請參閱  
 [Common Language Runtime &#40;CLR&#41; 整合總覽](../../relational-databases/clr-integration/common-language-runtime-integration-overview.md)   
 [CLR 資料表值函式](../../relational-databases/clr-integration-database-objects-user-defined-functions/clr-table-valued-functions.md)  
  
  
