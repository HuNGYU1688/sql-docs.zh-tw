---
description: 'ISSAsynchStatus：： WaitForAsynchCompletion in SQL Server Native Client (OLE DB) '
title: ISSAsynchStatus：： WaitForAsynchCompletion (Native Client OLE DB provider) |Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- ISSAsynchStatus::WaitForAsynchCompletion (OLE DB)
apitype: COM
helpviewer_keywords:
- WaitForAsynchCompletion method
ms.assetid: 9f65e9e7-eb93-47a1-bc42-acd4649fbd0e
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8496a19a7ce6d532fbcf740368bbe35571789d0e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97469309"
---
# <a name="issasynchstatuswaitforasynchcompletion-in-sql-server-native-client-ole-db"></a>ISSAsynchStatus：： WaitForAsynchCompletion in SQL Server Native Client (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  等到非同步執行的作業完成或發生逾時為止。  
  
## <a name="syntax"></a>語法  
  
```  
  
HRESULT WaitForAsynchCompletion(   
        DWORD dwMillisecTimeOut);  
```  
  
## <a name="arguments"></a>引數  
 *dwMillisecTimeOut*[in]  
 逾時 (以毫秒為單位)。  
  
## <a name="return-code-values"></a>傳回碼值  
 S_OK  
 此方法已成功。  
  
 E_UNEXPECTED  
 因為已呼叫 **ITransaction：： Commit** 或 **ITransaction：： Abort** ，或已在其初始化階段取消資料列集，所以資料列集處於未使用狀態。  
  
 DB_E_CANCELED  
 非同步處理已在資料列集擴展或資料來源物件初始化期間取消。  
  
 DB_S_ASYNCHRONOUS  
 即使已經達到指定的逾時，此作業還是尚未完成。  
  
> [!NOTE]  
>  除了以上列出的傳回碼值，**ISSAsynchStatus::WaitForAsynchCompletion** 方法也支援透過核心 OLEDB **ICommand::Execute** 和 **IDBInitialize::Initialize** 方法所傳回的傳回碼值。  
  
## <a name="remarks"></a>備註  
 在逾時值 (以毫秒為單位) 已過，或暫止的作業完成前，**ISSAsynchStatus::WaitForAsynchCompletion** 方法將不會傳回。 **Command** 物件的 **CommandTimeout** 屬性會控制查詢在逾時前執行的秒數。如果搭配 **ISSAsynchStatus::WaitForAsynchCompletion** 方法使用，將會忽略 **CommandTimeout** 屬性。  
  
 非同步作業會忽略逾時屬性。 **ISSAsynchStatus::WaitForAsynchCompletion** 的逾時參數會指定將控制項傳回給呼叫端前經過的時間上限。 如果這個逾時過期，會傳回 DB_S_ASYNCHRONOUS。 逾時絕不會取消非同步作業。 如果應用程式需要取消沒有在逾時期間內完成的非同步作業，它必須等到逾時，然後明確地取消此作業 (如果有傳回 DB_S_ASYNCHRONOUS)。  
  
> [!NOTE]  
>  使用 OLE DB 服務元件時，如果為 DB_S_ASYNCHRONOUS，可能會傳回 S_OK；因此，應用程式應該呼叫 [ISSAsynchStatus::GetStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-getstatus-ole-db.md) 來檢查傳回 S_OK 或 DB_S_ASYNCHRONOUS 時，作業是否完成。  
  
 如果 *dwMillisecTimeOut* 值設定為 INFINITE，**ISSAsynchStatus::WaitForAsynchCompletion** 方法會封鎖，直到作業完成為止。 如果 *dwMillisecTimeOut* 值設定為 0，則方法將會立即傳回暫止之作業的狀態。 如果逾時在作業完成前過期，將會傳回 DB_S_ASYNCHRONOUS。  
  
 如果作業在逾時過期前完成，傳回的 HRESULT 將會是此作業所傳回的 HRESULT (已經傳回的 HRESULT 已經讓此作業以同步方式執行)。  
  
 此外，SSPROP_ISSAsynchStatus 屬性已加入到 DBPROPSET_SQLSERVERROWSET 屬性集。 支援 [ISSAsynchStatus](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md) 介面的提供者必須使用 VARIANT_TRUE 的值實作此屬性。  
  
## <a name="see-also"></a>另請參閱  
 [執行非同步作業](../../relational-databases/native-client/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
