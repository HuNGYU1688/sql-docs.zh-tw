---
description: '將資料行加入至 SQL Server 資料表 (Native Client OLE DB 提供者) '
title: 將資料行加入至 SQL Server 資料表 (Native Client OLE DB 提供者) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- columns [OLE DB]
- AddColumn function
- SQL Server Native Client OLE DB provider, columns
- adding columns
ms.assetid: 22bae18a-bc9d-4617-8660-ed8b17a468d4
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: aac8aaed64dda5de6ebadbe26feeaa27998caae8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475979"
---
# <a name="adding-a-column-to-a-table-in-sql-server-native-client"></a>在 SQL Server Native Client 中將資料行加入至資料表
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會公開 **ITableDefinition：： AddColumn** 函數。 如此可讓取用者將資料行加入至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料表。  
  
 當您將資料行加入至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料表時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者取用者會受到限制，如下所示：  
  
-   如果 DBPROP_COL_AUTOINCREMENT 是 VARIANT_TRUE，DBPROP_COL_NULLABLE 就必須是 VARIANT_FALSE。  
  
-   如果資料行是使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **timestamp** 資料類型所定義，DBPROP_COL_NULLABLE 就必須是 VARIANT_FALSE。  
  
-   如果是其他任何資料行定義，DBPROP_COL_NULLABLE 必須為 VARIANT_TRUE。  
  
 取用者會在 *pTableID* 參數中，將資料表名稱指定為 *uName* 聯集之 *pwszName* 成員中的 Unicode 字元字串。 *pTableID* 的 *eKind* 成員必須是 DBKIND_NAME。  
  
 在 *pColumnDesc* 之 DBCOLUMNDESC 參數的 *dbcid* 成員中，新的資料行名稱會指定為 *uName* 聯集之 *pwszName* 成員內的 Unicode 字元字串。 *eKind* 成員必須是 DBKIND_NAME。  
  
## <a name="see-also"></a>另請參閱  
 [資料表和索引](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
  
  
