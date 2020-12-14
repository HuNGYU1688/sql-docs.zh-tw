---
description: bcp_exec
title: bcp_exec |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_exec
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_exec function
ms.assetid: b23ea2cc-8545-4873-b0c1-57e76b0a3a7b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 06e9cdecd37e5fc1f78de6726384715ad7b8c9bf
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97406738"
---
# <a name="bcp_exec"></a>bcp_exec
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在資料庫資料表和使用者檔案間執行資料的完整大量複製。  
  
## <a name="syntax"></a>語法  
  
```  
  
RETCODE bcp_exec (  
        HDBC hdbc,  
        LPDBINT pnRowsProcessed);  
```  
  
## <a name="arguments"></a>引數  
 *hdbc*  
 這是已啟用大量複製的 ODBC 連接控制代碼。  
  
 *pnRowsProcessed*  
 這是 DBINT 的指標。 **Bcp_exec** 函式會以成功複製的資料列數目來填滿此 DBINT。 如果 *pnRowsProcessed* 為 Null， **bcp_exec** 會忽略它。  
  
## <a name="returns"></a>傳回  
 SUCCEED、SUCCEED_ASYNC 或 FAIL。 如果複製所有資料列， **bcp_exec** 函數會傳回成功。 如果非同步大量複製作業仍未處理， **bcp_exec** 會傳回 SUCCEED_ASYNC。 如果發生完成失敗，或產生錯誤的資料列數目達到使用 [bcp_control](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-control.md)為 BCPMAXERRS 指定的值， **BCP_EXEC** 會傳回失敗。 BCPMAXERRS 預設為 10。 BCPMAXERRS 選項僅會影響提供者在從資料檔讀取資料列 (而非傳送到伺服器的資料列) 時所偵測到的語法錯誤。 伺服器會在偵測到資料列的錯誤時中止批次。 檢查 *pnRowsProcessed* 參數是否有成功複製的資料列數目。  
  
## <a name="remarks"></a>備註  
 此函數會根據 [bcp_init](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-init.md)中 *eDirection* 參數的值，將資料從使用者檔案複製到資料庫資料表，反之亦然。  
  
 在呼叫 **bcp_exec** 之前，請使用有效的使用者檔案名來呼叫 **bcp_init** 。 無法執行這項操作時，會發生錯誤。  
  
 **bcp_exec** 是唯一的大量複製函數，可能會在任何時間長度內都未完成。 因此，它是唯一支援非同步模式的大量複製函數。 若要設定非同步模式，請使用 [SQLSetConnectAttr](../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) 將 SQL_ATTR_ASYNC_ENABLE 設定為 SQL_ASYNC_ENABLE_ON，然後再呼叫 **bcp_exec**。 若要測試完成，請使用相同的參數來呼叫 **bcp_exec** 。 如果大量複製尚未完成， **bcp_exec** 會傳回 SUCCEED_ASYNC。 它也會在 *pnRowsProcessed* 中傳回已傳送到伺服器的資料列數目的狀態計數。 到達批次的結尾之前，不會認可傳送至伺服器的資料列。  
  
 如需從開始大量複製之重大變更的相關資訊 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ，請參閱 [&#40;ODBC&#41;執行大量複製作業 ](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md)。  
  
## <a name="example"></a>範例  
 下列範例顯示如何使用 **bcp_exec**：  
  
```  
// Variables like henv not specified.  
HDBC      hdbc;  
DBINT      nRowsProcessed;  
  
// Application initiation, get an ODBC environment handle, allocate the  
// hdbc, and so on.  
...   
  
// Enable bulk copy prior to connecting on allocated hdbc.  
SQLSetConnectAttr(hdbc, SQL_COPT_SS_BCP, (SQLPOINTER) SQL_BCP_ON,  
   SQL_IS_INTEGER);  
  
// Connect to the data source, return on error.  
if (!SQL_SUCCEEDED(SQLConnect(hdbc, _T("myDSN"), SQL_NTS,  
   _T("myUser"), SQL_NTS, _T("myPwd"), SQL_NTS)))  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Initialize bulk copy.   
if (bcp_init(hdbc, _T("pubs..authors"), _T("authors.sav"), NULL, DB_OUT)  
   == FAIL)  
   {  
   // Raise error and return.  
   return;  
   }  
  
// Now, execute the bulk copy.   
if (bcp_exec(hdbc, &nRowsProcessed) == FAIL)  
   {  
   if (nRowsProcessed == -1)  
      {  
      printf_s("No rows processed on bulk copy execution.\n");  
      }  
   else  
      {  
      printf_s("Incomplete bulk copy.   Only %ld row%s copied.\n",  
         nRowsProcessed, (nRowsProcessed == 1) ? "": "s");  
      }  
   return;  
   }  
  
printf_s("%ld rows processed.\n", nRowsProcessed);  
  
// Carry on.  
```  
  
## <a name="see-also"></a>另請參閱  
 [大量複製函數](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
