---
description: updateRef 方法 (int, java.sql.Ref)
title: updateRef 方法 (int, java.sql.Ref) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLServerResultSet.updateRef (int, java.sql.Ref)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: eab3ebae-3f68-4303-869a-fee06e3a9c71
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 9358fdc1c788e2e2be19b4999c3d10ca825d2799
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88353164"
---
# <a name="updateref-method-int-javasqlref"></a>updateRef 方法 (int, java.sql.Ref)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  透過給定的資料行索引，使用 java.sql.Ref 值來更新指定的資料行。  
  
## <a name="syntax"></a>語法  
  
```  
  
public void updateRef(int columnIndex,  
                      java.sql.Ref x)  
```  
  
#### <a name="parameters"></a>參數  
 *columnIndex*  
  
 指出資料行索引的 **int**。  
  
 *x*  
  
 Ref 物件。  
  
## <a name="exceptions"></a>例外狀況  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>備註  
 這個 updateRef 方法是由 java.sql.ResultSet 介面中的 updateRef 方法指定。  
  
## <a name="see-also"></a>另請參閱  
 [updateRef 方法 &#40;SQLServerResultSet&#41;](../../../connect/jdbc/reference/updateref-method-sqlserverresultset.md)   
 [SQLServerResultSet 成員](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 類別](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
