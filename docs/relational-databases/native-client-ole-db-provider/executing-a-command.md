---
description: " (Native Client OLE DB 提供者執行命令) "
title: " (Native Client OLE DB 提供者執行命令) |Microsoft Docs"
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- commands [SQL Server Native Client]
- OLE DB, executing commands
- sessions [SQL Server Native Client]
- OLE DB extensions for XML
- SQL Server Native Client OLE DB provider, command execution
ms.assetid: bb0b3cbf-fe45-46ba-b2ec-c5a39e3c7081
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f8c68daf6bf558df408d320567ae7c14d7498ec3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463499"
---
# <a name="executing-a-sql-server-native-client-command"></a>執行 SQL Server Native Client 命令
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  建立資料來源的連接後，取用者會呼叫 **IDBCreateSession::CreateSession** 方法來建立工作階段。 此工作階段會當做命令、資料列集或交易 Factory 運作。  
  
 若要直接使用個別的資料表或索引，取用者會要求 **IOpenRowset** 介面。 **IOpenRowset::OpenRowset** 方法會從單一基底資料表或索引開啟並傳回包含所有資料列的資料列集。  
  
 若要執行命令 (例如 SELECT \* FROM Authors)，取用者會要求 **IDBCreateCommand** 介面。 取用者可以執行 **IDBCreateCommand::CreateCommand** 方法來建立命令物件，並要求 **ICommandText** 介面。 **ICommandText::SetCommandText** 方法可用於指定要執行的命令。  
  
 **Execute** 命令用於執行命令。 此命令可以是任何 SQL 陳述式或程序名稱。 並非所有命令都會產生結果集 (資料列集) 物件。 SELECT * FROM Authors 之類的命令會產生結果集。  
  
## <a name="see-also"></a>另請參閱  
 [建立 SQL Server Native Client OLE DB 提供者應用程式](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)  
  
  
