---
description: valueOf 方法 (java.sql.Timestamp, java.util.Calendar)
title: valueOf 方法 (java.sql.Timestamp, java.util.Calendar) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 7320c383-0b06-446d-963b-7005e50324a2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 6046069a5e2e93cf2d14d0ec999670054348765a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88396114"
---
# <a name="valueof-method-javasqltimestamp-javautilcalendar"></a>valueOf 方法 (java.sql.Timestamp, java.util.Calendar)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  根據所指定 java.sql.Timestamp 值以及指出位移的 java.util.Calendar 值，建立 **DateTimeOffset** 物件，這個物件表示從 GMT 特定位移的時間點。  
  
## <a name="syntax"></a>語法  
  
```  
  
public static DateTimeOffset valueOf(java.sql.Timestamp timestamp, java.util.Calendar calendar)  
```  
  
#### <a name="parameters"></a>參數  
 *timestamp*  
  
 java.sql.Timestamp 值。  
  
 *calendar*  
  
 位移值。  *行事曆*的日期和時間元件將會根據 *timestamp* 值來設定。  
  
## <a name="return-value"></a>傳回值  
 傳回 DateTimeOffset 物件，這個物件表示 java.sql.Timestamp 物件在給定 java.util.Calendar 物件時區所指定的時間點。  
  
## <a name="remarks"></a>備註  
 這個方法也會將 java.util.Calendar 物件設定為由 java.sql.Timestamp 物件所指定的時間點。  
  
## <a name="see-also"></a>另請參閱  
 [DateTimeOffset 類別](../../../connect/jdbc/reference/datetimeoffset-class.md)   
 [DateTimeOffset 成員](../../../connect/jdbc/reference/datetimeoffset-members.md)  
  
  
