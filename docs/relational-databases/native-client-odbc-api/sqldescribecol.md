---
description: SQLDescribeCol
title: SQLDescribeCol |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLDescribeCol function
ms.assetid: ffbf34c6-8268-434f-829a-82009a6cda59
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f85320fff9c44a1904ea3500b32b0d9305237b99
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465324"
---
# <a name="sqldescribecol"></a>SQLDescribeCol
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  對於執行的語句而言， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驅動程式不需要查詢伺服器來描述結果集中的資料行。 在此情況下， **SQLDescribeCol** 不會造成伺服器往返。 如同 [SQLColAttribute](../../relational-databases/native-client-odbc-api/sqlcolattribute.md)和 [SQLNumResultCols](../../relational-databases/native-client-odbc-api/sqlnumresultcols.md)，在備妥但未執行的語句上呼叫 **SQLDescribeCol** 會產生伺服器往返。  
  
 當 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式或陳述式批次傳回多個結果資料列集時，按序數參考的資料行有可能源自於個別的資料表，或是參考結果集中完全不同的資料行。 應針對每個集合呼叫 **SQLDescribeCol** 。 當結果集變更時，應用程式應該在提取資料列結果以前先重新繫結資料值。 如需處理多個結果集傳回的詳細資訊，請參閱 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)。  
  
 當已備妥的 SQL 陳述式批次產生多個結果集時，只有第一個結果集會報告資料行屬性。  
  
 若為大數值資料類型， *DataTypePtr* 中傳回的值為 SQL_VARCHAR、SQL_VARBINARY 或 SQL_NVARCHAR。 *ColumnSizePtr* 中的值 SQL_SS_LENGTH_UNLIMITED 表示大小為「無限制」。  
  
 資料庫引擎的增強功能從 [ [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 允許 SQLDescribeCol] 取得預期結果更精確的描述。 這些更精確的結果可能與舊版的 SQLDescribeCol 所傳回的值不同 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如需詳細資訊，請參閱[中繼資料探索](../../relational-databases/native-client/features/metadata-discovery.md)。  
  
## <a name="sqldescribecol-support-for-enhanced-date-and-time-features"></a>SQLDescribeCol 對增強型日期和時間功能的支援  
 針對日期/時間類型所傳回的值如下：  
  
| 屬性 | *DataTypePtr* | *ColumnSizePtr* | *DecimalDigitsPtr* |  
| --------- | ------------- |---------------- | ------------------ |  
|Datetime|SQL_TYPE_TIMESTAMP|23|3|  
|smalldatetime|SQL_TYPE_TIMESTAMP|16|0|  
|date|SQL_TYPE_DATE|10|0|  
|time|SQL_SS_TIME2|8, 10..16|0..7|  
|datetime2|SQL_TYPE_TIMESTAMP|19, 21..27|0..7|  
|datetimeoffset|SQL_SS_TIMESTAMPOFFSET|26, 28..34|0..7|  
  
 如需詳細資訊，請參閱 [&#40;ODBC&#41;的日期和時間改進 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqldescribecol-support-for-large-clr-udts"></a>SQLDescribeCol 對於大型 CLR UDT 的支援  
 **SQLDescribeCol** 支援)  (udt 的大型 CLR 使用者自訂類型。 如需詳細資訊，請參閱 [&#40;ODBC&#41;的大型 CLR User-Defined 類型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另請參閱  
 [SQLDescribeCol 函式](../../odbc/reference/syntax/sqldescribecol-function.md)   
 [ODBC API 實作詳細資料](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
