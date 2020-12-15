---
description: 追蹤及重新執行事件
title: 追蹤及重新執行事件 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- replaying events
- traces [SMO]
- events [SMO], replaying
- events [SMO], tracing
ms.assetid: f41b3f85-2f6c-4c3e-9776-8c73d2cc7a53
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8280f8891d4ccd57b4496125a62bd96ba03eb49f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475429"
---
# <a name="tracing-and-replaying-events"></a>追蹤及重新執行事件
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 SMO 中，命名空間中的 **Trace** 和 **Replay** 物件 <xref:Microsoft.SqlServer.Management.Trace> 提供以程式設計方式存取 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] 功能，此功能可用來監視或的實例 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 。 您可以擷取每一個事件的相關資料，並將資料儲存至檔案或資料表，以供稍後分析。 例如，您可以監視實際環境，查看哪些程序由於執行速度過慢而妨礙效能。  
  
 **Trace** 和 **Replay** 物件提供一組物件，可用來在的實例上建立追蹤 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 您可以從自己的應用程式中使用這些物件，以手動方式為 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 或 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 建立追蹤。 此外，SMO **追蹤** 物件也可以用來讀取由監視 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 、 [!INCLUDE[ssASnoversion](../../../includes/ssasnoversion-md.md)] 或 DTS 記錄所建立的 SQL 追蹤檔案和資料表。  
  
 SMO **追蹤** 物件可讓您執行下列功能：  
  
-   建立追蹤。  
  
-   設定追蹤的篩選。  
  
-   設定正在追蹤的事件。  
  
-   停止或啟動追蹤。  
  
-   讀取追蹤檔案和追蹤資料表。  
  
-   取得有關追蹤事件的資訊。  
  
-   取得有關追蹤篩選的資訊。  
  
-   以程式設計方式操作追蹤資料。  
  
-   撰寫追蹤資料表和追蹤檔案。  
  
-   重新執行追蹤檔案或追蹤資料表。  
  
 來自 **追蹤****和重新** 執行物件的追蹤資料可由 SMO 應用程式使用，或者可以使用 [SQL Server Profiler](../../../tools/sql-server-profiler/sql-server-profiler.md)手動檢查。 追蹤資料也會與也提供追蹤功能的 [SQL 追蹤](../../../relational-databases/sql-trace/sql-trace.md) 預存程式相容。  
  
 SMO 追蹤物件位於 <xref:Microsoft.SqlServer.Management.Trace> 命名空間內，該命名空間需要參考 Microsoft.SQLServer.ConnectionInfo.dll 檔。  
  
 **Trace** 和 **Replay** 物件需要 [ServerConnection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120)) <xref:Microsoft.SqlServer.Management.Smo.Server.%23ctor%2A> 物件，才能建立與實例的連接 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 [ServerConnection](/previous-versions/sql/sql-server-2014/ms218641(v=sql.120))物件位於[Microsoft SqlServer. Common](/previous-versions/sql/sql-server-2014/ms212673(v=sql.120))命名空間中，需要 Microsoft.SQLServer.ConnectionInfo.dll 檔的參考。  
  
> [!NOTE]  
>  64位平臺上不支援 **Trace** 和 **Replay** 物件。  
  
