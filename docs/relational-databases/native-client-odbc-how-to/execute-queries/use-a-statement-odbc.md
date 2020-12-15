---
description: 使用陳述式 (ODBC)
title: 使用 (ODBC) 的語句 |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statements [ODBC]
ms.assetid: f7573f8f-6f21-4e03-8dd5-a5f2ea4878cc
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 85b14a32b4a680da3f2a70fc18d18aca2b1ed5f3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473559"
---
# <a name="use-a-statement-odbc"></a>使用陳述式 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-use-a-statement"></a>使用陳述式  
  
1.  利用 SQL_HANDLE_STMT 的 *HandleType* 來呼叫 [SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)，以配置陳述式控制代碼。  
  
2.  您可以選擇呼叫 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 來設定陳述式選項，或是呼叫 [SQLGetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlgetstmtattr.md) 來取得陳述式屬性。  
  
     若要使用伺服器資料指標，您必須將資料指標屬性設定為預設值以外的值。  
  
3.  如果此陳述式將會執行數次，您可以選擇預備此陳述式搭配 [SQLPrepare 函數](../../../odbc/reference/syntax/sqlprepare-function.md)一起執行。  
  
4.  如果此陳述式已經繫結參數標記，您可以選擇使用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md) 將這些參數標記繫結至程式變數。 如果此陳述式已備妥，您可以呼叫 [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) 和 [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) 來尋找參數的數目和參數的特性。  
  
5.  使用 SQLExecDirect 直接執行陳述式  
  
     \- 或 -  
  
     如果此陳述式已備妥，請使用 [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 將它執行多次。  
  
     \- 或 -  
  
     呼叫目錄函數，這樣會傳回結果。  
  
6.  若要處理結果，請將結果集資料行繫結至程式變數、使用 [SQLGetData](../../../relational-databases/native-client-odbc-api/sqlgetdata.md) 將資料從結果集資料行中移到程式變數，或是結合這兩個方法。  
  
     透過陳述式的結果集一次提取一個資料列。  
  
     \- 或 -  
  
     透過結果集，利用區塊資料指標一次提取數個資料列。  
  
     \- 或 -  
  
     呼叫 [SQLRowCount](../../../relational-databases/native-client-odbc-api/sqlrowcount.md) 來判斷受到 INSERT、UPDATE 或 DELETE 陳述式影響的資料列數。  
  
     如果 SQL 陳述式可以有多個結果集，請在每一個結果集的結尾呼叫 [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md)，以查看是否有其他結果集要處理。  
  
7.  處理完結果之後，可能需要採取以下動作，以便提供陳述式控制代碼來執行新的陳述式：  
  
    -   如果您要等到它傳回 SQL_NO_DATA 之後才會呼叫 [SQLMoreResults](../../../relational-databases/native-client-odbc-api/sqlmoreresults.md)，請呼叫 [SQLCloseCursor](../../../relational-databases/native-client-odbc-api/sqlclosecursor.md) 來關閉此資料指標。  
  
    -   如果您將參數標記繫結至程式變數，請呼叫 [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 並將 *Option* 設定為 SQL_RESET_PARAMS，以釋放繫結參數。  
  
    -   如果您將結果集資料行繫結至程式變數，請呼叫 [SQLFreeStmt](../../../relational-databases/native-client-odbc-api/sqlfreestmt.md) 並將 *Option* 設定為 SQL_UNBIND，以釋放繫結的資料行。  
  
    -   若要重複使用陳述式控制代碼，請移至步驟 2。  
  
8.  使用 SQL_HANDLE_STMT 的 *HandleType* 呼叫 [SQLFreeHandle](../../../relational-databases/native-client-odbc-api/sqlfreehandle.md)，以釋放陳述式控制代碼。  
  
## <a name="see-also"></a>另請參閱  
 [&#40;ODBC&#41;執行查詢的 how to 主題 ](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
