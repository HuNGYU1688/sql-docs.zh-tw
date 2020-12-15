---
description: SQLCancel
title: SQLCancel |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLCancel function
ms.assetid: d4c965ae-c1ac-4e9d-b4b9-32b561401106
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8c6215abca068aa221d13303f53de3aa70ba11b5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473889"
---
# <a name="sqlcancel"></a>SQLCancel
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md)主題指出在 ODBC 2.x 中，如果應用程式在語句上未進行任何處理時呼叫 **SQLCancel** ，則 **SQLCancel** 的效果與使用 **SQL_CLOSE** 選項的 **SQLFreeStmt** 相同：此行為僅針對完整性而定義，應用程式應該呼叫 **SQLFreeStmt** 或 **SQLCloseCursor** 來關閉資料指標。 但是，即使您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 原生用戶端應用程式將 ODBC API 版本設定為 3.5. x 或更新版本， **SQLCancel** 函式仍會使用 ODBC 2.x 行為。  
  
## <a name="see-also"></a>另請參閱  
 [SQLCancel](../../odbc/reference/syntax/sqlcancel-function.md)   
 [ODBC API 實作詳細資料](../../relational-databases/native-client-odbc-api/odbc-api-implementation-details.md)  
  
