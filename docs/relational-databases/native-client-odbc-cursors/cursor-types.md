---
description: 資料指標類型
title: 資料指標類型 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, cursors
- ODBC applications, cursors
- cursors [ODBC], types
- ODBC cursors, types
ms.assetid: 3a916cc7-f352-42cb-8b83-f78e06cef991
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9bffc8d5197117533913dca63c171fb4fffab588
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438651"
---
# <a name="cursor-types"></a>資料指標類型
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  ODBC 定義 Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] NATIVE Client ODBC 驅動程式所支援的四種資料指標類型。 這些資料指標在偵測結果集變更的能力，以及它們所使用的資源（例如記憶體和 **tempdb** 中的空間）之間有不同的能力。 資料指標只有在嘗試重新提取變更過的資料列時，才能偵測到這些資料列的變更；沒有方法可讓資料來源通知資料指標目前所提取的資料列已變更。 交易隔離等級也會影響資料指標偵測到並非透過該資料指標所進行之變更的能力。  
  
 以下是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 所支援的四種 ODBC 資料指標類型：  
  
-   順向資料指標：此種資料指標不支援捲動，而僅支援從資料指標開頭到結尾循序提取資料列。  
  
-   當資料指標開啟時，靜態資料指標會建立在 **tempdb** 中。 這種資料指標一定會顯示結果集在資料指標開啟時的原貌， 而不會反映出資料的變更。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 靜態資料指標永遠是唯讀的。 由於靜態伺服器資料指標是以 **tempdb** 中的工作資料表形式建立的，因此資料指標結果集的大小不能超過所允許的資料列大小上限 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   索引鍵集驅動的資料指標：這種資料指標開啟時，會將結果集中資料列的成員資格和順序設為固定。 您可透過此資料指標看到非索引鍵資料行的變更。  
  
-   動態資料指標是靜態資料指標的相反。 動態資料指標會反映其結果集之中變更的資料列。 每回擷取時結果集之中資料列的資料值、順序和成員均會變動。  
  
## <a name="see-also"></a>另請參閱  
 [使用 ODBC&#41;&#40;資料指標 ](../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
  
  
