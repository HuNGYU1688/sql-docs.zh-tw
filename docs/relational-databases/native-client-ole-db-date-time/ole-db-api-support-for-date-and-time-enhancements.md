---
description: 'OLE DB API 支援 (Native Client OLE DB 提供者的日期和時間增強功能) '
title: '適用于日期和時間增強功能的 API 支援 (Native Client OLE DB 提供者) '
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
ms.assetid: e65c9253-bd99-4dc3-9cb8-7613f754c966
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 753bc7c2ad83d85977729ab7be241fcf4601d4a6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97433804"
---
# <a name="ole-db-api-support-for-date-and-time-enhancements-native-client-ole-db-provider"></a>OLE DB API 支援 (Native Client OLE DB 提供者的日期和時間增強功能) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  下列 OLE DB API 支援日期/時間增強功能。  
  
|函式|描述|  
|--------------|-----------------|  
|IAccessor::CreateAccessor|在 DBBINDING 結構中新增一個旗標，讓應用程式可以區分 **datetime**、**datetime2** 及 **smalldatetime** 值。 如需詳細資訊，請參閱[參數和資料列集中繼資料](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md)。|  
|IBCPSession::BCPColFmt|如需詳細資訊，請參閱 [&#40;OLE DB 和 ODBC&#41;的增強型日期和時間類型的大量複製變更 ](../../relational-databases/native-client-odbc-date-time/bulk-copy-changes-for-enhanced-date-and-time-types-ole-db-and-odbc.md)。|  
|ICommandWithParameters::GetParameterInfo|如需詳細資訊，請參閱[參數和資料列集中繼資料](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md)。|  
|ICommandWithParameters::SetParameterinfo|如需詳細資訊，請參閱[參數和資料列集中繼資料](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md)。|  
|IColumnsRowset::GetColumnsRowset|如需詳細資訊，請參閱[參數和資料列集中繼資料](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md)。|  
|IColumnsInfo::GetColumnInfo|如需詳細資訊，請參閱[參數和資料列集中繼資料](../../relational-databases/native-client-ole-db-date-time/metadata-parameter-and-rowset.md)。|  
|IDBSchemaRowset::GetRowset|如需受影響結構描述資料列集的詳細資訊，請參閱[日期和時間以及結構描述資料列集](../../relational-databases/native-client-ole-db-date-time/metadata-date-and-time-and-schema-rowsets.md)。|  
|IRowsetFastLoad|此介面支援新的日期/時間類型，但是它的介面沒有任何變更。|  
|ITableDefinition::CreateTable|如需詳細資訊，請參閱[對 OLE DB 日期和時間改善的資料類型支援](../../relational-databases/native-client-ole-db-date-time/data-type-support-for-ole-db-date-and-time-improvements.md)。|  
  
## <a name="see-also"></a>另請參閱  
 [日期和時間改善 &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-date-time/date-and-time-improvements-ole-db.md)  
  
  
