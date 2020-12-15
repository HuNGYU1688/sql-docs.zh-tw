---
description: " (Native Client OLE DB 提供者建立 SQL Server 索引) "
title: " (Native Client OLE DB 提供者建立 SQL Server 索引) |Microsoft Docs"
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- CreateIndex function
- constraints [OLE DB]
- SQL Server Native Client OLE DB provider, indexes
- indexes [OLE DB]
- adding indexes
ms.assetid: 6239d440-2818-4b98-bb79-732dced41952
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7b7f08c622c5a1dccb9000daf952915bc62ea78c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97419043"
---
# <a name="creating-sql-server-native-client-indexes"></a>建立 SQL Server Native Client 索引
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會公開 **IIndexDefinition：： CreateIndex** 函數，讓取用者在資料表上定義新的索引 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者會建立資料表索引做為索引或條件約束。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會將建立條件約束的權限提供給資料表擁有者、資料庫擁有者，以及特定管理角色的成員。 根據預設，只有資料表擁有者可以建立資料表的索引。 因此，**CreateIndex** 的成功或失敗，不但取決於應用程式使用者的存取權限，也取決於所建立之索引的類型。  
  
 取用者會在 *pTableID* 參數中，將資料表名稱指定為 *uName* 聯集之 *pwszName* 成員中的 Unicode 字元字串。 *pTableID* 的 *eKind* 成員必須是 DBKIND_NAME。  
  
 *PIndexID* 參數可以是 Null，如果是，則 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會為索引建立唯一的名稱。 取用者可以在 *ppIndexID* 參數中指定 DBID 的有效指標，藉以擷取索引的名稱。  
  
 取用者可以將索引名稱指定為 *pIndexID* 參數 *uName* 聯集之 *pwszName* 成員中的 Unicode 字元字串。 *pIndexID* 的 *eKind* 成員必須是 DBKIND_NAME。  
  
 取用者會指定依名稱參與索引的一或多個資料行。 對於在 **CreateIndex** 中使用的每個 DBINDEXCOLUMNDESC 結構，*pColumnID* 的 *eKind* 成員必須是 DBKIND_NAME。 在 *pColumnID* 中，資料行的名稱會指定為 *uName* 聯集之 *pwszName* 成員內的 Unicode 字元字串。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]原生用戶端 OLE DB 提供者，並 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支援索引中值的遞增順序。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果取用者在任何 DBINDEXCOLUMNDESC 結構中指定 DBINDEX_COL_ORDER_DESC，則 Native Client OLE DB 提供者會傳回 E_INVALIDARG。  
  
 **CreateIndex** 會解譯索引屬性，如下所示。  
  
|屬性識別碼|描述|  
|-----------------|-----------------|  
|DBPROP_INDEX_AUTOUPDATE|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_CLUSTERED|R/W︰讀取/寫入<br /><br /> 預設值：VARIANT_FALSE<br /><br /> 描述：控制索引叢集。<br /><br /> VARIANT_TRUE： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會嘗試在資料表上建立叢集索引 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在任何資料表上，最多支援一個叢集索引。<br /><br /> VARIANT_FALSE： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會嘗試在資料表上建立非叢集索引 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。|  
|DBPROP_INDEX_FILLFACTOR|R/W︰讀取/寫入<br /><br /> 預設值：0<br /><br /> 描述：指定用於儲存之索引頁面的百分比。 如需詳細資訊，請參閱 [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)。<br /><br /> 變數的類型為 VT_I4。 其值必須大於或等於 1 且小於或等於 100。|  
|DBPROP_INDEX_INITIALIZE|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_NULLCOLLATION|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_NULLS|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_PRIMARYKEY|R/W︰讀取/寫入<br /><br /> 預設值：VARIANT_FALSE 描述：建立索引作為參考完整性，也就是 PRIMARY KEY 條件約束。<br /><br /> VARIANT_TRUE：索引建立之後，即可支援資料表的 PRIMARY KEY 條件約束。 資料行必須是不允許為 Null。<br /><br /> VARIANT_FALSE：索引不會作為資料表中之資料列值的 PRIMARY KEY 條件約束使用。|  
|DBPROP_INDEX_SORTBOOKMARKS|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_TEMPINDEX|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_TYPE|R/W︰讀取/寫入<br /><br /> 預設值：None<br /><br /> 描述： [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者不支援此屬性。 嘗試在 **CreateIndex** 中設定屬性會使 DB_S_ERRORSOCCURRED 傳回值。 屬性結構的 *dwStatus* 成員表示 DBPROPSTATUS_BADVALUE。|  
|DBPROP_INDEX_UNIQUE|R/W︰讀取/寫入<br /><br /> 預設值：VARIANT_FALSE<br /><br /> 描述：建立索引作為參與之一或多個資料行的 UNIQUE 條件約束。<br /><br /> VARIANT_TRUE：此索引用於唯一限制資料表中的資料列值。<br /><br /> VARIANT_FALSE：此索引不會唯一限制資料列值。|  
  
 在提供者特定的屬性集 DBPROPSET_SQLSERVERINDEX 中， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供者會定義下列資料來源資訊屬性。  
  
|屬性識別碼|描述|  
|-----------------|-----------------|  
|SSPROP_INDEX_XML|輸入：VT_BOOL (R/W)<br /><br /> 預設值：VARIANT_FALSE<br /><br /> 描述：利用包含 IIndexDefinition::CreateIndex 的 VARIANT_TRUE 值指定此屬性時，會建立對應到要建立索引之資料行的主要 xml 索引。 如果此屬性為 VARIANT_TRUE，cIndexColumnDescs 應該為 1，否則就是錯誤。|  
  
 此範例會建立一個主索引鍵索引：  
  
```  
// This CREATE TABLE statement shows the referential integrity and   
// PRIMARY KEY constraint on the OrderDetails table that will be created   
// by the following example code.  
//  
// CREATE TABLE OrderDetails  
// (  
//    OrderID      int      NOT NULL  
//    ProductID   int      NOT NULL  
//        CONSTRAINT PK_OrderDetails  
//        PRIMARY KEY CLUSTERED (OrderID, ProductID),  
//    UnitPrice   money      NOT NULL,  
//    Quantity   int      NOT NULL,  
//    Discount   decimal(2,2)   NOT NULL  
//        DEFAULT 0  
// )  
//  
HRESULT CreatePrimaryKey  
    (  
    IIndexDefinition* pIIndexDefinition  
    )  
    {  
    HRESULT             hr = S_OK;  
  
    DBID                dbidTable;  
    DBID                dbidIndex;  
    const ULONG         nCols = 2;  
    ULONG               nCol;  
    const ULONG         nProps = 2;  
    ULONG               nProp;  
  
    DBINDEXCOLUMNDESC   dbidxcoldesc[nCols];  
    DBPROP              dbpropIndex[nProps];  
    DBPROPSET           dbpropset;  
  
    DBID*               pdbidIndexOut = NULL;  
  
    // Set up identifiers for the table and index.  
    dbidTable.eKind = DBKIND_NAME;  
    dbidTable.uName.pwszName = L"OrderDetails";  
  
    dbidIndex.eKind = DBKIND_NAME;  
    dbidIndex.uName.pwszName = L"PK_OrderDetails";  
  
    // Set up column identifiers.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        dbidxcoldesc[nCol].pColumnID = new DBID;  
        dbidxcoldesc[nCol].pColumnID->eKind = DBKIND_NAME;  
  
        dbidxcoldesc[nCol].eIndexColOrder = DBINDEX_COL_ORDER_ASC;  
        }  
    dbidxcoldesc[0].pColumnID->uName.pwszName = L"OrderID";  
    dbidxcoldesc[1].pColumnID->uName.pwszName = L"ProductID";  
  
    // Set properties for the index. The index is clustered,  
    // PRIMARY KEY.  
    for (nProp = 0; nProp < nProps; nProp++)  
        {  
        dbpropIndex[nProp].dwOptions = DBPROPOPTIONS_REQUIRED;  
        dbpropIndex[nProp].colid = DB_NULLID;  
  
        VariantInit(&(dbpropIndex[nProp].vValue));  
  
        dbpropIndex[nProp].vValue.vt = VT_BOOL;  
        }  
    dbpropIndex[0].dwPropertyID = DBPROP_INDEX_CLUSTERED;  
    dbpropIndex[0].vValue.boolVal = VARIANT_TRUE;  
  
    dbpropIndex[1].dwPropertyID = DBPROP_INDEX_PRIMARYKEY;  
    dbpropIndex[1].vValue.boolVal = VARIANT_TRUE;  
  
    dbpropset.rgProperties = dbpropIndex;  
    dbpropset.cProperties = nProps;  
    dbpropset.guidPropertySet = DBPROPSET_INDEX;  
  
    hr = pIIndexDefinition->CreateIndex(&dbidTable, &dbidIndex, nCols,  
        dbidxcoldesc, 1, &dbpropset, &pdbidIndexOut);  
  
    // Clean up dynamically allocated DBIDs.  
    for (nCol = 0; nCol < nCols; nCol++)  
        {  
        delete dbidxcoldesc[nCol].pColumnID;  
        }  
  
    return (hr);  
    }  
```  
  
## <a name="see-also"></a>另請參閱  
 [資料表和索引](../../relational-databases/native-client-ole-db-tables-indexes/tables-and-indexes.md)  
  
  
