---
description: '資料列集和參數中的資料類型對應 (Native Client OLE DB 提供者) '
title: 資料列集和參數中的資料類型對應 (Native Client OLE DB 提供者) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- mapping data types [OLE DB]
- DBTYPE_SQLVARIANT data type
- SQL Server Native Client OLE DB provider, data types
- rowsets [OLE DB], data type mapping
- data types [OLE DB]
- GetColumnInfo function
- parameters [OLE DB]
- SSPROP_ALLOWNATIVEVARIANT property
- GetParameterInfo function
- OLE DB, data types
ms.assetid: 3d831ff8-3b79-4698-b2c1-2b5dd2f8235c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: f99e4c0106817b0defdcc09fc045ce985634571e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97477949"
---
# <a name="sql-server-native-client-data-type-mapping-in-rowsets-and-parameters"></a>SQL Server Native Client 資料列集和參數中的資料類型對應
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  在資料列集和 as 參數值中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 使用下列 OLE DB 的定義資料類型來表示資料 **： IColumnsInfo：： GetColumnInfo** 和 **ICommandWithParameters：： GetParameterInfo** 中所報告的資料類型。  
  
|SQL Server 資料類型|OLE DB 資料類型|  
|--------------------------|----------------------|  
|**bigint**|DBTYPE_I8|  
|**binary**|DBTYPE_BYTES|  
|**bit**|DBTYPE_BOOL|  
|**char**|DBTYPE_STR|  
|**datetime**|DBTYPE_DBTIMESTAMP|  
|**datetime2**|DBTYPE_DBTIME2|  
|**decimal**|DBTYPE_NUMERIC|  
|**float**|DBTYPE_R8|  
|**image**|DBTYPE_BYTES|  
|**int**|DBTYPE_I4|  
|**money**|DBTYPE_CY|  
|**nchar**|DBTYPE_WSTR|  
|**ntext**|DBTYPE_WSTR|  
|**numeric**|DBTYPE_NUMERIC|  
|**nvarchar**|DBTYPE_WSTR|  
|**real**|DBTYPE_R4|  
|**smalldatetime**|DBTYPE_DBTIMESTAMP|  
|**smallint**|DBTYPE_I2|  
|**smallmoney**|DBTYPE_CY|  
|**sql_variant**|DBTYPE_VARIANT、DBTYPE_SQLVARIANT|  
|**sysname**|DBTYPE_WSTR|  
|**text**|DBTYPE_STR|  
|**timestamp**|DBTYPE_BYTES|  
|**tinyint**|DBTYPE_UI1|  
|**UDT**|DBTYPE_UDT|  
|**uniqueidentifier**|DBTYPE_GUID|  
|**varbinary**|DBTYPE_BYTES|  
|**varchar**|DBTYPE_STR|  
|**XML**|DBTYPE_XML|  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者支援取用者要求的資料轉換，如圖所示。  
  
 **sql_variant** 物件可保存任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料類型的資料，下列類型除外：text、ntext、image、varchar(max)、nvarchar(max)、varbinary(max)、xml、timestamp 和 Microsoft .NET Framework Common Language Runtime (CLR) 使用者定義型別。 sql_variant 資料的執行個體不能用 sql_variant 做為它的基礎基底資料類型。 例如，資料行可以在某些資料列中包含 **smallint** 值，在其他資料列中包含 **float** 值，而在剩餘的資料列中包含 **char**/**nchar** 值。  
  
> [!NOTE]  
>  **sql_variant** 資料類型類似於 Microsoft Visual Basic® 中的 Variant 資料類型，以及 OLEDB 中的 DBTYPE_VARIANT、DBTYPE_SQLVARIANT。  
  
 以 DBTYPE_VARIANT 擷取 **sql_variant** 資料時，會將該資料置於緩衝區的 VARIANT 結構中。 但是 VARIANT 結構中的子類型可能不會對應到定義於 **sql_variant** 資料類型中的子類型。 接下來必須以 DBTYPE_SQLVARIANT 擷取 **sql_variant** 資料，才能讓所有的子類型相互對應。  
  
## <a name="dbtype_sqlvariant-data-type"></a>DBTYPE_SQLVARIANT 資料類型  
 為了支援 **SQL_variant** 資料類型， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會公開稱為 DBTYPE_SQLVARIANT 的提供者特定資料類型。 以 DBTYPE_SQLVARIANT 擷取 **sql_variant** 資料時，該資料會儲存在提供者特定的 SSVARIANT 結構中。 SSVARIANT 結構包含的所有子類型都符合 **sql_variant** 資料類型的子類型。  
  
 工作階段屬性 SSPROP_ALLOWNATIVEVARIANT 也必須設定為 TRUE。  
  
## <a name="provider-specific-property-ssprop_allownativevariant"></a>提供者特定的屬性 SSPROP_ALLOWNATIVEVARIANT  
 您在提取資料時，可以明確地指定要針對資料行或參數傳回何種資料類型。 **IColumnsInfo** 也可以用來取得資料行資訊，並用該資訊來進行繫結。 使用 **IColumnsInfo** 來取得用於繫結用途的資料行資訊時，如果 SSPROP_ALLOWNATIVEVARIANT 工作階段屬性為 FALSE (預設值)，則會針對 **sql_variant** 資料行傳回 DBTYPE_VARIANT。 如果 SSPROP_ALLOWNATIVEVARIANT 屬性為 FALSE，則 DBTYPE_SQLVARIANT 不受支援。 如果 SSPROP_ALLOWNATIVEVARIANT 屬性設定為 TRUE，則資料行類型會傳回為 DBTYPE_SQLVARIANT，在此種情況下，緩衝區會保存 SSVARIANT 結構。 以 DBTYPE_SQLVARIANT 擷取 **sql_variant** 資料時，工作階段屬性 SSPROP_ALLOWNATIVEVARIANT 必須設定為 TRUE。  
  
 SSPROP_ALLOWNATIVEVARIANT 屬性是提供者特定之 DBPROPSET_SQLSERVERSESSION 屬性集的一部分，而且是工作階段屬性。  
  
 DBTYPE_VARIANT 適用於所有其他的 OLE DB 提供者。  
  
## <a name="ssprop_allownativevariant"></a>SSPROP_ALLOWNATIVEVARIANT  
 SSPROP_ALLOWNATIVEVARIANT 是工作階段屬性，而且是 DBPROPSET_SQLSERVERSESSION 屬性集的一部分。  
  
|屬性識別碼|說明|  
|-|-|  
|SSPROP_ALLOWNATIVEVARIANT|輸入：VT_BOOL<br /><br /> R/W︰讀取/寫入<br /><br /> 預設值：VARIANT_FALSE<br /><br /> 描述：決定擷取的資料是否為 DBTYPE_VARIANT 或 DBTYPE_SQLVARIANT。<br /><br /> VARIANT_TRUE：資料行類型會以 DBTYPE_SQLVARIANT 傳回，在此情況下，緩衝區將會保留 SSVARIANT 結構。<br /><br /> VARIANT_FALSE：資料行類型會以 DBTYPE_VARIANT 傳回，而且緩衝區將具有 VARIANT 結構。|  
  
## <a name="see-also"></a>另請參閱  
 [資料類型 &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-data-types/data-types-ole-db.md)  
  
  
