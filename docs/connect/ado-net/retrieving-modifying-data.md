---
title: 擷取及修改資料
description: 在 .NET Framework 中，Microsoft SqlClient Data Provider for SQL Server 可作為應用程式與資料來源之間的橋樑來讀取及更新資料。
ms.date: 11/13/2020
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d6e4d6c298c632c446e1671b5d9adabaa19e0776
ms.sourcegitcommit: c127c0752e84cccd38a7e23ac74c0362a40f952e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/07/2020
ms.locfileid: "96761486"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>在 ADO.NET 中擷取及修改資料

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

任何資料庫應用程式都有一個主要功能，那就是連接到資料來源並擷取其內含的資料。 SqlClient 資料提供者可作為應用程式與資料來源之間的橋樑，讓您能夠執行命令，以及使用 **DataReader** 或 **DataAdapter** 來擷取資料。 任何資料庫應用程式都有一個主要功能，那就是更新資料庫中儲存的資料。 在 Microsoft SqlClient Data Provider for SQL Server 中，更新資料牽涉到使用 **DataAdapter** 與 <xref:System.Data.DataSet>，以及 **Command** 物件；而且也可能涉及使用異動。

## <a name="in-this-section"></a>本節內容

[連線到資料來源](connecting-to-data-source.md)  
說明如何建立資料來源的連接，以及如何使用連接事件。

[連接字串](connection-strings.md)  
包含一些主題，其中說明連接字串 (包含連接字串關鍵字、安全性資訊) 的使用、儲存和擷取的各種層面。

[連接共用](connection-pooling.md)  
描述適用於 Microsoft SqlClient Data Provider for SQL Server 的連接共用。

[命令和參數](commands-parameters.md)  
包含一些主題，其中說明如何建立命令和命令產生器、設定參數，以及執行命令來擷取和修改資料。

[DataAdapter 和 DataReader](dataadapters-datareaders.md)  
包含一些主題，其中說明 DataReader、DataAdapter、參數、處理 DataAdapter 事件，以及執行批次作業。

## <a name="see-also"></a>請參閱

- [ADO.NET 中的資料類型對應](data-type-mappings-ado-net.md)
- [SQL Server and ADO.NET](./sql/index.md) (SQL Server 和 ADO.NET)
- [Microsoft ADO.NET for SQL Server](microsoft-ado-net-sql-server.md)
