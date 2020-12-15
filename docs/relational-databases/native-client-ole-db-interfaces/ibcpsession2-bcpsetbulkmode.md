---
description: 'IBCPSession2：： BCPSetBulkMode (Native Client OLE DB 提供者) '
title: IBCPSession2：： BCPSetBulkMode (Native Client OLE DB provider) |Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- BCPSetBulkMode function
ms.assetid: babba19f-e67b-450c-b0e6-523a0f9d23ab
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 422c80c19624201ca29b05f22e197ead2e4dbbf7
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460351"
---
# <a name="ibcpsession2bcpsetbulkmode-native-client-ole-db-provider"></a>IBCPSession2：： BCPSetBulkMode (Native Client OLE DB 提供者) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  IBCPSession2::BCPSetBulkMode 提供了 [IBCPSession::BCPColFmt &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) 的替代方式，可用於指定資料行格式。 不同於設定個別資料行格式屬性的 IBCPSession::BCPColFmt，IBCPSession2::BCPSetBulkMode 會設定所有屬性。  
  
## <a name="syntax"></a>語法  
  
```  
  
HRESULT BCPSetBulkMode (  
      int property,  
   void * pField,  
   int cbField,  
   void * pRow,  
   int cbRow  
);  
```  
  
## <a name="arguments"></a>引數  
 *property*  
 類型為 BYTE 的常數。 如需常數的清單，請參閱＜備註＞一節中的表格。  
  
 *pField*  
 欄位結束字元值的指標。  
  
 cbField  
 欄位結束字元值的長度 (以位元組為單位)。  
  
 pRow  
 資料列結束字元值的指標。  
  
 cbRow  
 資料列結束字元值的長度 (以位元組為單位)。  
  
## <a name="returns"></a>傳回值  
 IBCPSession2::BCPSetBulkMode 可能會傳回下列其中一個值：  
  
|||  
|-|-|  
|**S_OK**|此方法已成功。|  
|**E_FAIL**|發生提供者特有的錯誤，如需詳細資訊，請使用 ISQLServerErrorInfo 介面。|  
|**E_UNEXPECTED**|此方法的呼叫是非預期的。 例如，在呼叫 IBCPSession2::BCPSetBulkMode 之前，沒有呼叫 **IBCPSession2::BCPInit** 方法。|  
|**E_INVALIDARG**|此引數無效。|  
|**E_OUTOFMEMORY**|記憶體不足的錯誤。|  
  
## <a name="remarks"></a>備註  
 IBCPSession2::BCPSetBulkMode 可用來大量複製查詢或資料表。 當 IBCPSession2::BCPSetBulkMode 用來大量複製查詢陳述式時，您必須先呼叫此方法，再呼叫 `IBCPSession::BCPControl(BCP_OPTIONS_HINTS, ...)` 來指定查詢陳述式。  
  
 您應該避免在單一命令文字中結合 RPC 呼叫語法與批次查詢語法 (例如 `{rpc func};SELECT * from Tbl`)。  這樣做會導致 ICommandPrepare::Prepare 傳回錯誤並且讓您無法擷取中繼資料。 如果您需要在單一命令文字中結合預存程序執行與批次查詢，請使用 ODBC CALL 語法 (例如 `{call func}; SELECT * from Tbl`)。  
  
 下表將列出 *property* 參數的常數。  
  
|屬性|描述|  
|--------------|-----------------|  
|BCP_OUT_CHARACTER_MODE|指定字元輸出模式。<br /><br /> 對應至 BCP.EXE 中的 -c 選項，以及 *eUserDataType* 屬性設定為 **BCP_TYPE_SQLCHARACTER** 的 IBCPSession::BCPColFmt。|  
|BCP_OUT_WIDE_CHARACTER_MODE|指定 Unicode 輸出模式。<br /><br /> 對應至 BCP.EXE 中的 -w 選項，以及 *eUserDataType* 屬性設定為 **BCP_TYPE_SQLNCHAR** 的 IBCPSession::BCPColFmt。|  
|BCP_OUT_NATIVE_TEXT_MODE|指定非字元類型的原生類型和字元類型的 Unicode。<br /><br /> 對應至 BCP.EXE 中的 -N 選項，以及 *eUserDataType* 屬性設定為 **BCP_TYPE_SQLNCHAR** (如果資料行類型是字串的話) 或 **BCP_TYPE_DEFAULT** (如果不是字串的話) 的 IBCPSession::BCPColFmt。|  
|BCP_OUT_NATIVE_MODE|指定原生資料庫類型。<br /><br /> 對應至 BCP.EXE 中的 -n 選項，以及 *eUserDataType* 屬性設定為 **BCP_TYPE_DEFAULT** 的 IBCPSession::BCPColFmt。|  
  
 您可以針對不會與 IBCPSession2::BCPSetBulkMode 發生衝突的 IBCPSession::BCPControl 選項呼叫 IBCPSession::BCPControl 和 IBCPSession2::BCPSetBulkMode。 例如，您可以使用 **BCP_OPTION_FIRST** 和 IBCPSession2::BCPSetBulkMode 來呼叫 IBCPSession::BCPControl。  
  
 您不可以使用 **BCP_OPTION_TEXTFILE** 和 IBCPSession2::BCPSetBulkMode 來呼叫 IBCPSession::BCPControl。  
  
 如果您嘗試使用一連串包含 IBCPSession::BCPColFmt、IBCPSession::BCPControl 和 IBCPSession::BCPReadFmt 的函式呼叫來呼叫 IBCPSession2::BCPSetBulkMode，其中一個函式呼叫就會傳回順序錯誤失敗。 如果您選擇要更正失敗，請呼叫 IBCPSession::BCPInit 來重設設定並重新開始。  
  
 以下是會產生函式順序錯誤的一些函式呼叫範例：  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_IN);  
BCPSetBulkMode();  
```  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPReadFmt();  
```  
  
```  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPSetBulkMode();  
```  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPColFmt();  
```  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPReadFmt();  
BCPColFmt();  
```  
  
```  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetBulkMode();  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPReadFmt();  
```  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPColumns();  
```  
  
```  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetColFmt();  
```  
  
## <a name="example"></a>範例  
 下列範例會使用不同的 IBCPSession2::BCPSetBulkMode 設定來建立四個檔案。  
  
```  
  
// compile with: sqlncli11.lib oleaut32.lib ole32.lib  
  
#include <stdio.h>  
#include "sqlncli.h"  
  
IDBInitialize*  g_pIDBInitialize = NULL;  
IBCPSession2 * g_pIBcpSession = NULL;  
class COLEDBPropSet : public DBPROPSET {  
public:  
   COLEDBPropSet() {  
      rgProperties = NULL;  
      cProperties = 0;  
   };  
   COLEDBPropSet(const GUID& guid) {  
      rgProperties = NULL;  
      cProperties = 0;  
      guidPropertySet = guid;  
   };  
   ~COLEDBPropSet() {  
      for ( ULONG i = 0 ; i < cProperties ; i++ )  
         VariantClear(&rgProperties[i].vValue);  
      CoTaskMemFree(rgProperties);  
   }  
   void SetGUID(const GUID& guid) {  
      guidPropertySet = guid;  
   };  
   bool AddProperty(DWORD dwPropertyID, bool bValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BOOL;  
      rgProperties[cProperties].vValue.boolVal = (bValue) ? VARIANT_TRUE : VARIANT_FALSE;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID, long nValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID  = dwPropertyID;  
      rgProperties[cProperties].vValue.vt     = VT_I4;  
      rgProperties[cProperties].vValue.lVal   = nValue;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID,LPCWSTR szValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BSTR;  
      rgProperties[cProperties].vValue.bstrVal = SysAllocString(szValue);  
      cProperties++;  
      return true;  
   };  
   bool Add() {  
      DBPROP* p = (DBPROP*)CoTaskMemRealloc(rgProperties, (cProperties + 1) * sizeof(DBPROP));  
      if (p != NULL) {  
         rgProperties = p;  
         rgProperties[cProperties].dwOptions = DBPROPOPTIONS_REQUIRED;  
         rgProperties[cProperties].colid = DB_NULLID;  
         rgProperties[cProperties].vValue.vt = VT_EMPTY;  
         return true;  
      }  
      else  
         return false;  
   };  
};  
  
void OLEDBCleanUp() {  
   if (g_pIDBInitialize) {  
      g_pIDBInitialize->Release();  
      g_pIDBInitialize = NULL;  
   }  
   if (g_pIBcpSession) {  
      g_pIBcpSession->Release();  
      g_pIBcpSession = NULL;  
   }  
}  
  
BOOL MakeOLEDBConnect(LPWSTR  pServer) {  
   BOOL ret = true;  
   IDBProperties * pIDBProperties = NULL;  
   IDBCreateSession * pIDBCreateSession = NULL;  
   COLEDBPropSet PropSet(DBPROPSET_DBINIT);  
   COLEDBPropSet BcpProperty(DBPROPSET_SQLSERVERDATASOURCE);  
   try {  
      HRESULT hr = CoInitializeEx(NULL,COINIT_MULTITHREADED);   
      hr = CoCreateInstance(SQLNCLI_CLSID, NULL, CLSCTX_INPROC_SERVER, IID_IDBInitialize, (LPVOID *)&g_pIDBInitialize);  
      if (FAILED(hr)) {  
         printf("CoCreateInstance failed\n");  
         return false;  
      }  
      PropSet.AddProperty(DBPROP_INIT_DATASOURCE, (LPWSTR)pServer);  
      PropSet.AddProperty(DBPROP_AUTH_INTEGRATED, L"SSPI");  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBProperties, (void**) &pIDBProperties);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->QueryInterface(IID_IDBProperties...) failed\n");  
         throw false;  
      }  
      hr = pIDBProperties->SetProperties(1, &PropSet);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties(...) failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->Initialize();  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->Initialize() failed\n");  
         throw false;  
      }  
      BcpProperty.AddProperty(SSPROP_ENABLEFASTLOAD, true);  
      BcpProperty.AddProperty(SSPROP_ENABLEBULKCOPY, true);  
      hr = pIDBProperties->SetProperties(1, &BcpProperty);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties() for bcp failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void**) &pIDBCreateSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->QueryInterface(IID_IDBCreateSession..) failed\n");  
         throw false;  
      }  
  
      hr = pIDBCreateSession->CreateSession(NULL, IID_IBCPSession2, (IUnknown**) &g_pIBcpSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBCreateSession->CreateSession() failed\n");  
         throw false;  
      }  
   }  
   catch(...) {  
      ret = false;  
   }  
   if (pIDBProperties)  
      pIDBProperties->Release();  
   if (pIDBCreateSession)  
      pIDBCreateSession->Release();  
   return ret;  
}  
  
BOOL BCPSetBulkMode(LPWSTR pszServer, LPTSTR pszQureryOut, char BCPType, LPWSTR pszDataFile) {  
   HRESULThr;  
   if (!MakeOLEDBConnect(pszServer))  
      return false;  
   hr = g_pIBcpSession->BCPInit(NULL, pszDataFile, NULL, BCP_DIRECTION_OUT );   // bcp init for queryout  
   if (FAILED(hr)) {  
      printf("BCP init failed\n");  
      OLEDBCleanUp();  
      return false;  
   }  
   // setbulkmode  
   char ColTerm[] = "\t";  
   char RowTerm[] = "\r\n";  
   wchar_t wColTerm[] = L"\t";  
   wchar_t wRowTerm[] = L"\r\n";  
   BYTE * pColTerm = NULL;  
   int cbColTerm = NULL;  
   BYTE * pRowTerm = 0;  
   int cbRowTerm = 0;  
   int bulkmode = -1;  
  
   if(BCPType == 'c') {   // bcp -c  
      pColTerm = (BYTE*)ColTerm;  
      pRowTerm = (BYTE*)RowTerm;  
      cbColTerm = 1;  
      cbRowTerm = 2;  
      bulkmode = BCP_OUT_CHARACTER_MODE;  
   }  
   else  
      if(BCPType == 'w') {   // bcp -w  
         pColTerm = (BYTE*)wColTerm;  
         pRowTerm = (BYTE*)wRowTerm;  
         cbColTerm = 2;  
         cbRowTerm = 4;  
         bulkmode = BCP_OUT_WIDE_CHARACTER_MODE;  
      }  
      else  
         if (BCPType == 'n')   // bcp -n  
            bulkmode = BCP_OUT_NATIVE_MODE;  
         else  
            if (BCPType == 'N')   // bcp -n  
               bulkmode = BCP_OUT_NATIVE_TEXT_MODE;  
            else {  
               printf("unknown bcp mode\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            hr = g_pIBcpSession->BCPSetBulkMode(bulkmode, pColTerm, cbColTerm, pRowTerm, cbRowTerm);  
            if (FAILED(hr)) {  
               printf("BCPSetBulkMode failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
  
            // set queryout TSQL statement  
            hr = g_pIBcpSession->BCPControl(BCP_OPTION_HINTS, pszQureryOut);  
            if (FAILED(hr)) {  
               printf("BCPControl failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            // bcp copy  
            DBROWCOUNT nRowsInserted = 0;  
            hr = g_pIBcpSession->BCPExec(&nRowsInserted);  
            if (FAILED(hr)) {  
               printf("BCPExec failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            printf("bcp done\n");  
            OLEDBCleanUp();  
            return true;  
}  
  
int main() {  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'c', L"bcpc.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'w', L"bcpw.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'n', L"bcpn.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'N', L"bcp_N.dat");  
}  
```  
  
## <a name="see-also"></a>另請參閱  
 [IBCPSession2 &#40;OLE DB&#41;](../../relational-databases/native-client-ole-db-interfaces/ibcpsession2-ole-db.md)  
  
  
