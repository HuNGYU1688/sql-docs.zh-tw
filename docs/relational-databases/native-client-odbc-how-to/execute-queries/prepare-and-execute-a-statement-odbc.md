---
description: 準備和執行陳述式 (ODBC)
title: 準備和執行語句 (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- statement execution
- statement preparation
ms.assetid: 0adecc63-4da5-486c-bc48-09a004a2fae6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e26cb2ffe39eeafc510d7f17bc324e12c5a44502
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476219"
---
# <a name="prepare-and-execute-a-statement-odbc"></a>準備和執行陳述式 (ODBC)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-prepare-a-statement-once-and-then-execute-it-multiple-times"></a>準備一次陳述式，然後執行多次  
  
1.  呼叫 [SQLPrepare 函數](../../../odbc/reference/syntax/sqlprepare-function.md) 來準備語句。  
  
2.  （選擇性）呼叫 [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) 來判斷備妥語句中的參數數目。  
  
3.  (選擇性) 針對準備陳述式中的每個參數：  
  
    -   呼叫 [SQLDescribeParam](../../../relational-databases/native-client-odbc-api/sqldescribeparam.md) 以取得參數資訊。  
  
    -   使用 [SQLBindParameter](../../../relational-databases/native-client-odbc-api/sqlbindparameter.md)將每個參數系結至程式變數。 設定任何資料執行中參數。  
  
4.  針對準備陳述式的每個執行：  
  
    -   如果陳述式具有參數標記，請將資料值放在繫結參數緩衝區中。  
  
    -   呼叫 [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 來執行備妥的語句。  
  
    -   如果使用了資料執行中的輸入參數， [SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md) 會傳回 SQL_NEED_DATA。 使用 [SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md) 和 [SQLPutData](../../../relational-databases/native-client-odbc-api/sqlputdata.md)以區區塊轉送資料。  
  
### <a name="to-prepare-a-statement-with-column-wise-parameter-binding"></a>準備含有資料行取向參數繫結的陳述式  
  
1.  呼叫 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 來設定下列屬性：  
  
    -   將 SQL_ATTR_PARAMSET_SIZE 設定為參數集 (S) 的數目。  
  
    -   將 SQL_ATTR_PARAM_BIND_TYPE 設定為 SQL_PARAMETER_BIND_BY_COLUMN。  
  
    -   將 SQL_ATTR_PARAMS_PROCESSED_PTR 屬性設定為指向 SQLUINTEGER 變數，以保存已處理的參數數目。  
  
    -   將 SQL_ATTR_PARAMS_STATUS_PTR 設定為指向 SQLUSSMALLINT 變數的陣列[S]，以保存參數狀態指標。  
  
2.  呼叫 SQLPrepare 來準備語句。  
  
3.  （選擇性）呼叫 [SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md) 來判斷備妥語句中的參數數目。  
  
4.  （選擇性）針對準備語句中的每個參數，呼叫 SQLDescribeParam 以取得參數資訊。  
  
5.  針對每個參數標記：  
  
    -   配置 S 個參數緩衝區的陣列來儲存資料值。  
  
    -   配置 S 個參數緩衝區的陣列來儲存資料長度。  
  
    -   呼叫 SQLBindParameter，將參數資料值和資料長度陣列系結至語句參數。  
  
    -   如果參數是資料執行中的 text 或 image 參數，請設定此參數。  
  
    -   如果使用了任何資料執行中參數，請設定這些參數。  
  
6.  針對準備陳述式的每個執行：  
  
    -   將 S 資料值和 S 資料長度放在繫結參數陣列中。  
  
    -   呼叫 SQLExecute，以便執行準備陳述式。  
  
    -   如果使用了資料執行中輸入參數，SQLExecute 就會傳回 SQL_NEED_DATA。 使用 SQLParamData 和 SQLPutData 以區區塊轉送資料。  
  
### <a name="to-prepare-a-statement-with-row-wise-bound-parameters"></a>準備含有資料列取向繫結參數的陳述式  
  
1.  配置結構的陣列[S]，其中 S 是參數集的數目。 結構中每個參數都具有一個元素，而每個元素則具有兩部分：  
  
    -   第一個部分是適當資料類型的變數，可保存參數資料。  
  
    -   第二個部分是 SQLINTEGER 變數，可保存狀態指標。  
  
2.  呼叫 [SQLSetStmtAttr](../../../relational-databases/native-client-odbc-api/sqlsetstmtattr.md) 來設定下列屬性：  
  
    -   將 SQL_ATTR_PARAMSET_SIZE 設定為參數集 (S) 的數目。  
  
    -   將 SQL_ATTR_PARAM_BIND_TYPE 設定為步驟 1 中配置的結構大小。  
  
    -   將 SQL_ATTR_PARAMS_PROCESSED_PTR 屬性設定為指向 SQLUINTEGER 變數，以保存已處理的參數數目。  
  
    -   將 SQL_ATTR_PARAMS_STATUS_PTR 設定為指向 SQLUSSMALLINT 變數的陣列[S]，以保存參數狀態指標。  
  
3.  呼叫 SQLPrepare 來準備語句。  
  
4.  針對每個參數標記，呼叫 SQLBindParameter，以將參數資料值和資料長度指標指向在步驟1中所配置之結構陣列第一個元素中的變數。 如果參數是資料執行中參數，請設定此參數。  
  
5.  針對準備陳述式的每個執行：  
  
    -   將資料值填入繫結參數緩衝區陣列。  
  
    -   呼叫 SQLExecute，以便執行準備陳述式。 驅動程式會有效率地執行 SQL 陳述式 S 次，針對每個參數集執行一次。  
  
    -   如果使用了資料執行中輸入參數，SQLExecute 就會傳回 SQL_NEED_DATA。 使用 SQLParamData 和 SQLPutData 以區區塊轉送資料。  
  
## <a name="see-also"></a>另請參閱  
 [&#40;ODBC&#41;執行查詢的 how to 主題 ](../../../relational-databases/native-client-odbc-how-to/execute-queries/executing-queries-how-to-topics-odbc.md)  
  
