---
title: OLE DB
description: SQL Server Native Client OLE DB 提供者是用來存取資料的 COM API，用於需要高效能的工具、公用程式或低層級元件。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, about SQL Server Native Client OLE DB provider
- OLE DB, SQL Server Native Client OLE DB provider
- data access [SQL Server Native Client], OLE DB
- SQLNCLI, OLE DB
- OLE DB
- SQL Server Native Client OLE DB provider
- SQL Server Native Client, OLE DB
ms.assetid: da846da4-ec19-4a4f-81fb-7d5a2b2bf80a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 133f9a53d6de89b6d8720781606301b939a32bf3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467549"
---
# <a name="sql-server-native-client-ole-db"></a>SQL Server Native Client (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者 (SQLNCLI) 是用來存取資料的低層級 COM API。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 建議將 Native Client OLE DB 提供者用於開發工具、公用程式或需要高效能的低階元件。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者是原生、高效能的提供者，會直接存取 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 表格式資料流 (TDS) 通訊協定。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client 會對連接到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 的應用程式提供 OLE DB 支援。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者是 OLE DB 版本2.0 相容的提供者。  
 
> [!IMPORTANT]
> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]原生用戶端 OLE DB (SQLNCLI) 仍會被取代，因此不建議用於新的開發工作。 請改為使用新的 [Microsoft OLE DB Driver for SQL Server](../../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其會進行更新且具備最新的伺服器功能。
  
## <a name="in-this-section"></a>本節內容  
  
-   [建立 SQL Server Native Client OLE DB 提供者應用程式](../../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
-   [資料來源物件 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
-   [命令](../../../relational-databases/native-client-ole-db-commands/commands.md)  
  
-   [資料列集](../../../relational-databases/native-client-ole-db-rowsets/rowsets.md)  
  
-   [預存程序](../../../relational-databases/native-client/ole-db/stored-procedures.md)  
  
-   [BLOB 與 OLE 物件](../../../relational-databases/native-client-ole-db-blobs/blobs-and-ole-objects.md)  
  
-   [資料表和索引](../../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
-   [資料類型 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
-   [結構描述資料列集支援 &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/schema-rowset-support-ole-db.md)  
  
-   [資料表值參數 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)  
  
-   [日期和時間改善 &#40;OLE DB&#41;](../../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
-   [大型 CLR 使用者定義型別 &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/large-clr-user-defined-types-ole-db.md)  
  
-   [FILESTREAM 支援 &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/filestream-support-ole-db.md)  
  
-   [交易](../../../relational-databases/native-client-ole-db-transactions/transactions.md)  
  
-   [錯誤](../../../relational-databases/native-client-ole-db-errors/errors.md)  
  
-   [用戶端連接 &#40;OLE DB&#41; 中的服務主體名稱 &#40;SPN&#41;](../../../relational-databases/native-client/ole-db/service-principal-names-spns-in-client-connections-ole-db.md)  
  
-   [疏鬆資料行支援 &#40;OLE DB&#41;](../../../relational-databases/native-client/ole-db/sparse-columns-support-ole-db.md)  
  
-   [SQL Server Native Client &#40;OLE DB&#41; 參考](../../../relational-databases/native-client-ole-db-interfaces/sql-server-native-client-ole-db-interfaces.md)  
  
-   [OLE DB 的使用說明主題](../../../relational-databases/native-client-ole-db-how-to/ole-db-how-to-topics.md)  
  
## <a name="see-also"></a>另請參閱  
 [SQL Server Native Client 程式設計](../../../relational-databases/native-client/sql-server-native-client-programming.md)  
  
  
