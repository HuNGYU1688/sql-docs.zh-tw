---
description: 直接執行
title: 直接執行 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- ODBC applications, statements
- direct execution [ODBC]
- SQLExecDirect function
- statements [ODBC], direct execution
ms.assetid: fa36e1af-ed98-4abc-97c1-c4cc5d227b29
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0fd85df31856668e4f3cafacbe8a90157b7310b3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438381"
---
# <a name="direct-execution"></a>直接執行
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  直接執行是執行陳述式的一種最基本的方式。 應用程式會建立包含語句的字元字串 [!INCLUDE[tsql](../../../includes/tsql-md.md)] ，並使用 **SQLExecDirect** 函式提交它以供執行。 當此陳述式到達伺服器時，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 會將它編譯成執行計畫，然後立即執行此執行計畫。  
  
 直接執行通常是由執行階段建立及執行陳述式的應用程式所使用，而且對於將要單次執行的陳述式也是最有效率的方法。 但是它在許多資料庫中有一個缺點，就是每當執行此 SQL 陳述式時，都必須要剖析及編譯它，這樣會在該陳述式執行多次時增加負擔。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 會大幅改善在多使用者環境中經常執行之陳述式的直接執行效能，而且針對經常執行的 SQL 陳述式搭配參數標記使用 SQLExecDirect 可以讓已備妥的執行提高效率。  
  
 當連接到的實例時 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驅動程式會使用 [Sp_executesql](../../../relational-databases/system-stored-procedures/sp-executesql-transact-sql.md) 來傳輸 **SQLExecDirect** 上指定的 SQL 語句或批次。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 具有邏輯，可快速判斷使用 **sp_executesql** 執行的 SQL 語句或批次是否符合產生已存在於記憶體中之執行計畫的語句或批次。 如果兩者相符，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 只會重複使用現有的計畫，而不是編譯新的計畫。 這表示，在具有許多使用者的系統中，以 **SQLExecDirect** 執行的一般執行 SQL 語句，將可受益于舊版中的預存程式所能使用的許多方案重複使用優勢 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。  
  
 只有當幾位使用者正在執行相同的 SQL 陳述式或批次時，重複使用執行計畫的這項好處才有效。 請遵循編碼慣例來增加不同用戶端執行的 SQL 陳述式非常類似於能夠重複使用執行計畫的機率：  
  
-   請勿在 SQL 陳述式中包含資料常數，而是要改用繫結至程式變數的參數標記。 如需詳細資訊，請參閱 [Using 語句參數](../../../relational-databases/native-client-odbc-queries/using-statement-parameters.md)。  
  
-   使用完整的物件名稱。 如果物件名稱不是完整的，就不會重複使用執行計畫。  
  
-   盡量讓應用程式連接使用一組常用的連接和陳述式選項。 使用一組選項 (如 ANSI_NULLS) 為連接產生的執行計畫將不會重複用於具有另一組選項的連接。 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ODBC 驅動程式和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者都會針對這些選項使用相同的預設值。  
  
 如果使用 **SQLExecDirect** 執行的所有語句都使用這些慣例來撰寫程式碼， [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 則可能會在商機發生時重複使用執行計畫。  
  
## <a name="see-also"></a>另請參閱  
 [&#40;ODBC&#41;執行語句 ](../../../relational-databases/native-client-odbc-queries/executing-statements/executing-statements-odbc.md)  
  
  
