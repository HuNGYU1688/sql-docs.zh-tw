---
description: 跨版本相容性
title: 跨版本相容性 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters (ODBC), cross-version compatibility
ms.assetid: 5f14850b-b85c-41e2-8116-6f5b3f5e0856
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 686078a764e47c922c36f22b94adcbcba0fab90f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97436047"
---
# <a name="cross-version-compatibility"></a>跨版本相容性
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  當早於 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 用戶端或伺服器執行個體預期處理資料表值參數時，可能會發生跨版本衝突。  
  
 一般而言，資料表值參數功能僅適用於 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 用戶端 (使用 SQL Server Native Client 10.0)，或連接到 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (或更新版本) 伺服器的更新版本。 只有當連接到 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] (或更新版本) server 時，才會出現目錄函數結果集中的新資料行。  
  
 如果利用舊版 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 編譯的用戶端應用程式執行要求資料表值參數的陳述式，伺服器會透過資料轉換錯誤偵測到這個狀況，而且 ODBC 會將此狀況傳回為 SQLSTATE 07006 以及「限制的資料類型屬性違規」訊息。  
  
 如果使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 10.0 或更新版本編譯的用戶端應用程式在連接到早于的伺服器實例時嘗試使用資料表值參數，則 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 將會偵測到此情況，而且 SQLBindCol、SQLBindParameter、SQLSetDescFields 和 SQLSetDescRec 呼叫會失敗並出現 SQLSTATE 07006 和訊息「限制的資料類型屬性違規 (此連接的 SQL Server 版本不支援) 」的資料表值參數。  
  
## <a name="see-also"></a>另請參閱  
 [ODBC&#41;&#40;資料表值參數 ](../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)  
  
  
