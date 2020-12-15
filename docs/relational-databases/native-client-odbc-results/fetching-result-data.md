---
description: 提取結果資料
title: 正在提取結果資料 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function
- SQL Server Native Client ODBC driver, result sets
- ODBC applications, result sets
- data types [ODBC], fetching
- SQLBindCol function
- result sets [ODBC], fetching
- fetching [ODBC]
- ODBC data types, fetching
- SQLFetch function
- SQL Server Native Client ODBC driver, data types
- SQLGetData function
ms.assetid: b289c7fb-5017-4d7e-a2d3-19401e9fc4cd
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 97781af1d4bc7dbde4c26f6a64b02ea3024c42aa
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438262"
---
# <a name="fetching-result-data"></a>提取結果資料
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 應用程式具備三個提取結果資料的選項。  
  
 第一個選項是以 [SQLBindCol](../../relational-databases/native-client-odbc-api/sqlbindcol.md)為基礎。 在提取結果集之前，應用程式會使用 **SQLBindCol** 將結果集中的每個資料行系結至程式變數。 系結資料行之後，當應用程式每次呼叫 **SQLFetch** 或 [SQLFetchScroll](../../relational-databases/native-client-odbc-api/sqlfetchscroll.md)時，驅動程式會將目前資料列的資料傳輸至系結至結果集資料行的變數。 如果結果集資料行和程式變數的資料類型不同，驅動程式會處理資料轉換。 如果應用程式 SQL_ATTR_ROW_ARRAY_SIZE 設定為大於1，它可以將結果資料行系結至變數陣列，這些變數將會填入 **SQLFetchScroll** 的每個呼叫。  
  
 第二個選項是以 [SQLGetData](../../relational-databases/native-client-odbc-api/sqlgetdata.md)為基礎。 應用程式不會使用 **SQLBindCol** 將結果集資料行系結至程式變數。 每次呼叫 **SQLFetch** 之後，應用程式會針對結果集中的每個資料行呼叫 **SQLGetData** 一次。 **SQLGetData** 指示驅動程式將資料從特定的結果集資料行傳送至特定的程式變數，並指定資料行和變數的資料類型。 如果結果資料行和程式變數的資料類型不同，這會讓驅動程式轉換資料。 **Text**、 **Ntext** 和 **image** 資料行通常太大而無法放入程式變數中，但仍可使用 **SQLGetData** 進行取出。 如果結果資料行中的 **text**、 **Ntext** 或 **image** 資料大於程式變數， **SQLGetData** 會傳回 SQL_SUCCESS_WITH_INFO 和 SQLSTATE 01004 (字串資料，右邊的截斷) 。 後續的 **SQLGetData** 呼叫會傳回 **文字** 或 **影像** 資料的連續區塊。 到達資料的結尾時， **SQLGetData** 會傳回 SQL_SUCCESS。 如果 SQL_ATTR_ROW_ARRAY_SIZE 大於 1，每次提取都會傳回一組資料列或資料列集。 使用 **SQLGetData** 之前，您必須先使用 **SQLSetPos** 將資料列集內的特定資料列指定為目前的資料列。  
  
 第三個選項是混合使用 **SQLBindCol** 和 **SQLGetData**。 例如，應用程式可以系結結果集的前10個數據行，然後在每次提取時呼叫 **SQLGetData** 三次，以從三個未系結的資料行中取出資料。 當結果集包含一或多個 **text** 或 **image** 資料行時，通常會使用此方法。  
  
 根據針對結果集設定的資料指標選項，應用程式也可以使用 **SQLFetchScroll** 的滾動選項，在結果集周圍滾動。  
  
 過度使用 **SQLBindCol** 將結果集資料行系結至程式變數的成本很高，因為 **SQLBINDCOL** 會導致 ODBC 驅動程式配置記憶體。 當您將結果資料行系結至變數時，該系結會維持有效，直到您呼叫 [SQLFreeHandle](../../relational-databases/native-client-odbc-api/sqlfreehandle.md) 釋放語句控制碼或呼叫 [SQLFreeStmt](../../relational-databases/native-client-odbc-api/sqlfreestmt.md) ，並將 *fOption* 設定為 SQL_UNBIND。 當陳述式完成時，不會自動復原繫結。  
  
 此邏輯可讓您利用不同的參數，有效地處理執行相同的 SELECT 陳述式數次。 由於結果集會保留相同的結構，因此您可以系結結果集一次、處理所有 SELECT 語句，然後在最後一次執行之後，將 *fOption* 設定為 SQL_UNBIND 來呼叫 **SQLFreeStmt** 。 您不應該呼叫 **SQLBindCol** 來系結結果集中的資料行，而不需要先呼叫 **SQLFreeStmt** ，並將 *fOption* 設定為 SQL_UNBIND 來釋放任何先前的系結。  
  
 使用 **SQLBindCol** 時，您可以進行資料列取向或資料行取向的系結。 資料列取向的繫結比資料行取向的繫結稍快。  
  
 您可以使用 **SQLGetData** 依資料行逐一取出資料，而不是使用 **SQLBindCol** 系結結果集資料行。 如果結果集只包含幾個資料列，則使用 **SQLGetData** 而非 **SQLBindCol** 的速度較快;否則， **SQLBindCol** 可提供最佳效能。 如果您不一定要將資料放在同一組變數中，則應該使用 **SQLGetData** ，而不是不斷重新系結。 當所有資料行都與 **SQLBindCol** 系結之後，您只能在選取清單中的資料行上使用 **SQLGetData** 。 資料行也必須出現在您已使用 **SQLGetData** 的任何資料行之後。  
  
 處理將資料移入或移出程式變數（例如 **SQLGetData**、 **SQLBindCol** 和 [SQLBindParameter](../../relational-databases/native-client-odbc-api/sqlbindparameter.md)）的 ODBC 函數支援隱含的資料類型轉換。 例如，如果應用程式將整數資料行繫結至字元字串程式變數，驅動程式會先自動將資料從整數轉換為字元，然後再將其放入程式變數中。  
  
 在應用程式中進行的資料轉換應該降至最低。 出非需要進行資料轉換才能讓應用程式完成處理，否則，應用程式應該將資料行和參數繫結至相同資料類型的程式變數。 不過，如果資料必須從一種類型轉換為另一種類型，讓驅動程式執行轉換比在應用程式中進行轉換還要有效率。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ODBC 驅動程式一般只會直接將資料從網路緩衝區轉換到應用程式的變數。 要求驅動程式執行資料轉換會強制驅動程式緩衝處理資料，並使用 CPU 循環轉換資料。  
  
 除了 **text**、 **Ntext** 和 **image** 資料以外，程式變數應該夠大以容納從資料行傳入的資料。 如果應用程式嘗試擷取結果集資料，並將其放入太小而無法容納它的變數中，驅動程式會產生警告。 這會強迫驅動程式為訊息配置記憶體，而且驅動程式和應用程式都必須花費 CPU 循環來處理訊息並進行錯誤處理。 應用程式應該配置夠大的變數來容納要擷取的資料，或使用選取清單中的 SUBSTRING 函數來縮減資料行在結果集中的大小。  
  
 使用 SQL_C_DEFAULT 來指定 C 變數的類型時請務必小心。 SQL_C_DEFAULT 指定 C 變數的類型必須符合資料行或參數的 SQL 資料類型。 如果為 **Ntext**、 **Nchar** 或 **Nvarchar** 資料行指定 SQL_C_DEFAULT，則會將 Unicode 資料傳回給應用程式。 如果尚未撰寫應用程式的程式碼來處理 Unicode 資料，這可能會導致各種問題。 **Uniqueidentifier** (SQL_GUID) 資料類型可能會發生相同類型的問題。  
  
 **text**、 **Ntext** 和 **image** 資料通常太大而無法放入單一程式變數中，而且通常會使用 **SQLGetData** 而不是 **SQLBindCol** 來處理。 使用伺服器資料指標時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native CLIENT ODBC 驅動程式會經過優化，不會在提取資料列時傳輸未系結之 **text**、 **Ntext** 或 **image** 資料行的資料。 在應用程式發出資料行的 **SQLGetData** 之前，不會實際從伺服器取出 **text**、 **Ntext** 或 **image** 資料。  
  
 這項優化可以套用至應用程式，如此一來，當使用者在游標上向上和向下滾動時，不會顯示 **text**、 **Ntext** 或 **image** 資料。 當使用者選取資料列之後，應用程式可以呼叫 **SQLGetData** 來取得 **text**、 **Ntext** 或 **image** 資料。 這會將使用者未選取的任何資料列的 **text**、 **Ntext** 或 **image** 資料傳輸，而且可以節省大量資料的傳輸。  
  
## <a name="see-also"></a>另請參閱  
 [處理 &#40;ODBC&#41;的結果 ](../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
  
  
