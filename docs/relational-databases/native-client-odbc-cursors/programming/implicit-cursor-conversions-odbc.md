---
description: 隱含資料指標轉換 (ODBC)
title: " (ODBC) 的隱含資料指標轉換 |Microsoft Docs"
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC cursors, implicit cursor conversions
- implicit cursor conversions
- cursors [ODBC], implicit cursor conversions
ms.assetid: fe29a58d-8448-4512-9ffd-b414784ba338
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b07b3bc1dd045b42e3e7a56f40f63504b0cf38f2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438564"
---
# <a name="implicit-cursor-conversions-odbc"></a>隱含資料指標轉換 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  應用程式可以透過 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 要求資料指標類型，然後執行所要求類型之伺服器資料指標不支援的 SQL 語句。 對 **SQLExecute** 或 **SQLExecDirect** 的呼叫會傳回 SQL_SUCCESS_WITH_INFO，且 **SQLGetDiagRec** 會傳回：  
  
```  
szSqlState = "01S02", *pfNativeError = 0,  
szErrorMsg="[Microsoft][SQL Server Native Client] Cursor type changed"  
```  
  
 應用程式可以藉由呼叫 **SQLGetStmtOption** 設定為 SQL_CURSOR_TYPE，判斷目前使用哪一種類型的資料指標。 資料指標類型轉換只適用於一個陳述式。 接下來的 **SQLExecDirect** 或 **SQLExecute** 將會使用原始的語句資料指標設定來完成。  
  
## <a name="see-also"></a>另請參閱  
 [&#40;ODBC&#41;的資料指標程式設計詳細資料 ](../../../relational-databases/native-client-odbc-cursors/programming/cursor-programming-details-odbc.md)  
  
  
