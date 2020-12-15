---
description: 捲動和擷取資料列 - ODBC 中的書籤資料列
title: 將 ODBC 中的資料列加上書簽 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- cursors [ODBC], fetching rows
- ODBC cursors, fetching rows
- cursors [ODBC], scrolling rows
- ODBC cursors, scrolling rows
- bookmarks [ODBC]
ms.assetid: 9cfcd243-c9d4-4c2a-abc4-399dbabe5f6b
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ee71f95033bfef05d38578a437481c20da74d3db
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473689"
---
# <a name="scrolling-and-fetching-rows---bookmarking-rows-in-odbc"></a>捲動和擷取資料列 - ODBC 中的書籤資料列
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  書籤是用來識別資料列的值。 書籤值的意義僅適用於驅動程式或資料來源。 例如，書籤可能跟資料列號碼一樣簡單，也可能跟磁碟位址一樣複雜。 在 ODBC 中，應用程式會要求特定資料列的書籤、將其儲存起來，然後將其傳回資料指標，即可傳回到資料列。  
  
 使用 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)提取資料列時，應用程式可以使用書簽作為選取起始列的基礎。 這是一個絕對位址的形式，因為它不相依於目前的資料指標位置。 若要滾動至有書簽的資料列，應用程式會以 SQL_FETCH_BOOKMARK 的 *FetchOrientation* 呼叫 **SQLFetchScroll** 。 此作業使用 SQL_ATTR_FETCH_BOOKMARK_PTR 選項屬性所指向的書籤。 它會傳回資料列集，從該書籤識別的資料列開始。 應用程式可以在 **SQLFetchScroll** 呼叫的 *FetchOffset* 引數中，指定此作業的位移。 指定位移時，所傳回之資料列集的第一個資料列會透過將 FetchOffset 引數中的數字加入到書籤所識別之資料列數目決定。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驅動程式僅針對靜態和索引鍵集資料指標支援書籤。 如果設定書籤開啟時要求動態資料指標，就會改為開啟索引鍵集。  
  
 書簽也可以搭配 **SQLBulkOperations** 函式使用，以在從書簽開始的一組資料列上執行作業。  
  
## <a name="see-also"></a>另請參閱  
 [捲動與提取資料列](../../relational-databases/native-client-odbc-cursors/scrolling-and-fetching-rows.md)  
  
  
