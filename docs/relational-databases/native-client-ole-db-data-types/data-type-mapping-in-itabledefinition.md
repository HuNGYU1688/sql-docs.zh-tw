---
description: 'ITableDefinition (Native Client OLE DB 提供者中的資料類型對應) '
title: ITableDefinition (Native Client OLE DB provider) 中的資料類型對應 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- SQL Server Native Client OLE DB provider, data types
- ITableDefinition interface
- DBCOLUMNDESC structure
- data types [OLE DB]
- CreateTable function
- OLE DB, data types
ms.assetid: 13292d1f-c17e-4d11-bf98-3460a10cbb18
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 601dced15dd86a4caf639f5bc48aeadf81166925
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477959"
---
# <a name="sql-server-native-client-data-type-mapping-in-itabledefinition"></a>ITableDefinition 中的 SQL Server Native Client 資料類型對應
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  使用 **ITableDefinition：： CreateTable** 函數建立資料表時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者取用者可以 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在傳遞之 DBCOLUMNDESC 陣列的 *>pwsztypename* 成員中指定資料類型。 如果取用者依照名稱來指定資料行的資料類型，系統就會忽略 OLE DB 資料對應 (由 DBCOLUMNDESC 結構的 *wType* 成員代表)。  
  
 使用 DBCOLUMNDESC 結構 *wType* 成員指定具有 OLE DB 資料類型的新資料行資料類型時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會對應 OLE DB 資料類型，如下所示。  
  
|OLE DB 資料類型|SQL Server<br /><br /> 資料類型|其他資訊|  
|----------------------|------------------------------|----------------------------|  
|DBTYPE_BOOL|**bit**||  
|DBTYPE_BYTES|**binary**、**varbinary**、**image** 或 **varbinary(max)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會檢查 DBCOLUMNDESC 結構的 *>ulcolumnsize* 成員。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會根據實例的值和版本，將 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 此類型對應至 **影像**。<br /><br /> 如果 *>ulcolumnsize* 的值小於 **二進位** 資料類型資料行的最大長度，則 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會檢查 DBCOLUMNDESC *rgPropertySets* 成員。 如果 DBPROP_COL_FIXEDLENGTH VARIANT_TRUE， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **二進位** 檔。 如果 VARIANT_FALSE 屬性的值， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **Varbinary**。 不論是哪一種情況，DBCOLUMNDESC *ulColumnSize* 成員都會決定所建立之 SQL Server 資料行的寬度。|  
|DBTYPE_CY|**money**||  
|DBTYPE_DBTIMESTAMP|**datetime**||  
|DBTYPE_GUID|**uniqueidentifier**||  
|DBTYPE_I2|**smallint**||  
|DBTYPE_I4|**int**||  
|DBTYPE_NUMERIC|**numeric**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會檢查 DBCOLUMDESC *>bprecision* 和 *bScale* 成員，以判斷數值資料行的有效位數和小 **數位** 數。|  
|DBTYPE_R4|**real**||  
|DBTYPE_R8|**float**||  
|DBTYPE_STR|**char**、**varchar**、**text** 或 **varchar(max)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會檢查 DBCOLUMNDESC 結構的 *>ulcolumnsize* 成員。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會根據實例的值和版本，將 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 此類型對應至 **文字**。<br /><br /> 如果 *>ulcolumnsize* 的值小於多位元組字元資料類型資料行的最大長度，則 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會檢查 DBCOLUMNDESC *rgPropertySets* 成員。 如果 DBPROP_COL_FIXEDLENGTH VARIANT_TRUE， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **char**。 如果 VARIANT_FALSE 屬性的值， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **Varchar**。 不論是哪一種情況，DBCOLUMNDESC *ulColumnSize* 成員都會決定所建立之 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料行的寬度。|  
|DBTYPE_UDT|**UDT**|當需要 UDT 資料行時，下列資訊會用於 **ITableDefinition::CreateTable** 所使用的 **DBCOLUMNDESC** 結構中：<br /><br /> 已忽略 *pwSzTypeName*。<br /><br /> *rgPropertySets* 必須包含 **DBPROPSET_SQLSERVERCOLUMN** 屬性集，如 [使用使用者定義型別](../../relational-databases/native-client/features/using-user-defined-types.md)中有關 **DBPROPSET_SQLSERVERCOLUMN** 的小節中所述。|  
|DBTYPE_UI1|**tinyint**||  
|DBTYPE_WSTR|**nchar**、**nvarchar**、**ntext** 或 **nvarchar(max)**|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會檢查 DBCOLUMNDESC 結構的 *>ulcolumnsize* 成員。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會根據此值，將此類型對應至 **Ntext**。<br /><br /> 如果 *>ulcolumnsize* 的值小於 Unicode 字元資料類型資料行的最大長度，則 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會檢查 DBCOLUMNDESC *rgPropertySets* 成員。 如果 DBPROP_COL_FIXEDLENGTH VARIANT_TRUE， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **Nchar**。 如果 VARIANT_FALSE 屬性的值， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會將此類型對應至 **Nvarchar**。 不論是哪一種情況，DBCOLUMNDESC *ulColumnSize* 成員都會決定所建立之 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料行的寬度。|  
|DBTYPE_XML|**XML**||  
  
> [!NOTE]  
>  建立新的資料表時，[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者只會對應上表中指定的 OLE DB 資料類型列舉值。 嘗試建立含有任何其他 OLE DB 資料類型之資料行的資料表都會產生錯誤。  
  
## <a name="see-also"></a>另請參閱  
 [資料類型 &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
