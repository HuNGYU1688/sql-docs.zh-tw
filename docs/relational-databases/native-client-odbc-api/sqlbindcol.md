---
description: SQLBindCol
title: SQLBindCol |Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apitype: DLLExport
helpviewer_keywords:
- SQLBindCol function
ms.assetid: fbd7ba20-d917-4ca9-b018-018ac6af9f98
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f891ab146a80c99462be9c2634d687c61fe52b99
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469549"
---
# <a name="sqlbindcol"></a>SQLBindCol
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  一般來說，請考慮使用 **SQLBindCol** 造成資料轉換的含意。 例如，繫結轉換為用戶端處理序，所以擷取繫結至字元資料行的浮點值時，將會造成驅動程式在提取資料列時，於本機執行浮點對字元的轉換。 使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] CONVERT 函數可將資料轉換的成本置於伺服器上。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體可以在單一陳述式執行中傳回多個結果資料列集。 每個結果集都必須個別繫結。 如需多個結果集之系結的詳細資訊，請參閱 [SQLMoreResults](../../relational-databases/native-client-odbc-api/sqlmoreresults.md)。  
  
 開發人員可以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用 *TargetType* 值 **SQL_C_BINARY**，將資料行系結至特定的 C 資料類型。 繫結至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特有之類型的資料行無法移植。 已定義的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特有 ODBC C 資料類型符合 DB-Library 的類型定義，而移植應用程式的 DB-Library 開發人員可能會想要利用這項功能。  
  
 報告資料截斷是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 原生用戶端 ODBC 驅動程式的昂貴處理程式。 您可以確保所有繫結的資料緩衝區都夠寬而足以傳回資料，藉此來避免資料遭到截斷。 如果是字元資料，當使用字串結束的預設驅動程式行為時，寬度應該包括字串結束字元的空格。 例如，將 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **char (5)** 資料行系結至五個字元的陣列，會導致每個提取的值截斷。 將相同的資料行繫結至六個字元的陣列可藉由提供字元元素來儲存 Null 結束字元，以避免截斷的情況發生。 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md) 可以用來有效率地取出長字元和二進位資料，而不會截斷。  
  
 若為大數值資料類型，如果使用者提供的緩衝區不夠大，無法容納整個資料行的值，則會傳回 **SQL_SUCCESS_WITH_INFO** ，並傳回「字串資料;右側截斷」警告發出。 **StrLen_or_IndPtr** 引數將包含儲存在緩衝區中的字元/位元組數目。  
  
## <a name="sqlbindcol-support-for-enhanced-date-and-time-features"></a>SQLBindCol 對於增強型日期和時間功能的支援  
 日期/時間類型的結果資料行值會依照 [從 SQL 轉換成 C](../../relational-databases/native-client-odbc-date-time/datetime-data-type-conversions-from-sql-to-c.md)的說明進行轉換。請注意，若要取出 time 和 datetimeoffset 資料行做為其對應的結構 (**SQL_SS_TIME2_STRUCT** 和 **SQL_SS_TIMESTAMPOFFSET_STRUCT**) ， *TargetType* 必須指定為 **SQL_C_DEFAULT** 或 **SQL_C_BINARY**。  
  
 如需詳細資訊，請參閱 [&#40;ODBC&#41;的日期和時間改進 ](../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)。  
  
## <a name="sqlbindcol-support-for-large-clr-udts"></a>SQLBindCol 對於大型 CLR UDT 的支援  
 **SQLBindCol** 支援)  (udt 的大型 CLR 使用者自訂類型。 如需詳細資訊，請參閱 [&#40;ODBC&#41;的大型 CLR User-Defined 類型 ](../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)。  
  
## <a name="see-also"></a>另請參閱  
 [SQLBindCol 函式](../../odbc/reference/syntax/sqlbindcol-function.md)   
 [ODBC API 實作詳細資料](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
