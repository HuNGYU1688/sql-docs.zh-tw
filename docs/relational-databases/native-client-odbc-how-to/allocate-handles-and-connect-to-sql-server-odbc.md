---
description: 配置控制代碼並連接到 SQL Server (ODBC)
title: 配置控制碼並連接至 SQL Server (ODBC) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- handles [ODBC]
- handles [ODBC], connection
- handles [ODBC], about handles
ms.assetid: 6172cd52-9c9a-467d-992f-def07f3f3bb1
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 040089efc3cba453381381768d1445c8d85109c1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97483269"
---
# <a name="allocate-handles-and-connect-to-sql-server-odbc"></a>配置控制代碼並連接到 SQL Server (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

    
### <a name="to-allocate-handles-and-connect-to-sql-server"></a>配置控制代碼並連接到 SQL Server  
  
1.  加入 ODBC 標頭檔 Sql.h、Sqlext.h、Sqltypes.h。  
  
2.  加入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 驅動程式專屬的標頭檔 Odbcss.h。  
  
3.  使用 SQL_HANDLE_ENV 的 **HandleType** 呼叫 [SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) ，以初始化 ODBC 並配置環境控制碼。  
  
4.  將 **屬性** 設為 SQL_ATTR_ODBC_VERSION，並將 **ValuePtr** 設定為 SQL_OV_ODBC3，以指出應用程式將會使用 ODBC 3.x 格式函式呼叫，以呼叫 [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) 。  
  
5.  （選擇性）呼叫 [SQLSetEnvAttr](../../relational-databases/native-client-odbc-api/sqlsetenvattr.md) 來設定其他環境選項，或呼叫 [SQLGetEnvAttr](../../odbc/reference/syntax/sqlgetenvattr-function.md) 來取得環境選項。  
  
6.  以 SQL_HANDLE_DBC 的 **HandleType** 呼叫 [SQLAllocHandle](../../odbc/reference/syntax/sqlallochandle-function.md) ，以配置連接控制碼。  
  
7.  （選擇性）呼叫 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 來設定連接選項，或呼叫 [SQLGetConnectAttr](../../relational-databases/native-client-odbc-api/sqlgetconnectattr.md) 來取得連接選項。  
  
8.  呼叫 SQLConnect，以使用現有的資料來源來連接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
     Or  
  
     呼叫 [SQLDriverConnect](../../relational-databases/native-client-odbc-api/sqldriverconnect.md) ，以使用連接字串連接至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
     完整的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 連接字串至少擁有下列其中一種形式：  
  
    ```  
    DSN=dsn_name;Trusted_connection=yes;  
    DRIVER={SQL Server Native Client 10.0};SERVER=server;Trusted_connection=yes;  
    ```  
  
     如果連接字串未完成， **SQLDriverConnect** 可能會提示您輸入必要的資訊。 這是由針對 *DriverCompletion* 參數指定的值所控制。  
  
     \- 或 -  
  
     以反復的方式多次呼叫 [SQLBrowseConnect](../../relational-databases/native-client-odbc-api/sqlbrowseconnect.md) ，以建立連接字串並連接至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
9. （選擇性）呼叫 [SQLGetInfo](../../relational-databases/native-client-odbc-api/sqlgetinfo.md) 以取得資料來源的驅動程式屬性和行為 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
10. 配置與使用陳述式。  
  
11. 呼叫 SQLDisconnect 以中斷連接 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，並讓連接控制碼可用於新的連接。  
  
12. 使用 SQL_HANDLE_DBC **HandleType** 呼叫 [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) ，以釋放連接控制碼。  
  
13. 使用 SQL_HANDLE_ENV **HandleType** 呼叫 **SQLFreeHandle** ，以釋放環境控制碼。  
  
> [!IMPORTANT]  
>  盡可能使用 Windows 驗證。 如果無法使用 Windows 驗證，請提示使用者在執行階段輸入認證。 請避免將認證儲存在檔案中。 如果您必須保存認證，則應該用 [Win32 crypto API](/windows/win32/seccrypto/cryptography-reference) 加密這些認證。  
  
## <a name="example"></a>範例  
 這個範例會示範如何呼叫 **SQLDriverConnect** ，以連接到的實例， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 而不需要現有的 ODBC 資料來源。 藉由將不完整的連接字串傳遞至 **SQLDriverConnect**，它會導致 ODBC 驅動程式提示使用者輸入遺漏的資訊。  
  
```  
#define MAXBUFLEN   255  
  
SQLHENV      henv = SQL_NULL_HENV;  
SQLHDBC      hdbc1 = SQL_NULL_HDBC;  
SQLHSTMT      hstmt1 = SQL_NULL_HSTMT;  
  
SQLCHAR      ConnStrIn[MAXBUFLEN] =  
         "DRIVER={SQL Server Native Client 10.0};SERVER=MyServer";  
  
SQLCHAR      ConnStrOut[MAXBUFLEN];  
SQLSMALLINT   cbConnStrOut = 0;  
  
// Make connection without data source. Ask that driver   
// prompt if insufficient information. Driver returns  
// SQL_ERROR and application prompts user  
// for missing information. Window handle not needed for  
// SQL_DRIVER_NOPROMPT.  
retcode = SQLDriverConnect(hdbc1,      // Connection handle  
                  NULL,         // Window handle  
                  ConnStrIn,      // Input connect string  
                  SQL_NTS,         // Null-terminated string  
                  ConnStrOut,      // Address of output buffer  
                  MAXBUFLEN,      // Size of output buffer  
                  &cbConnStrOut,   // Address of output length  
                  SQL_DRIVER_PROMPT);  
```  
  
