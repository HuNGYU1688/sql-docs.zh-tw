---
description: sys.bandwidth_usage (Azure SQL Database)
title: sys.bandwidth_usage (Azure SQL Database) Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.reviewer: ''
ms.topic: language-reference
f1_keywords:
- bandwidth_usage
- sys.bandwidth_usage
- bandwidth_usage_TSQL
- sys.bandwidth_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.bandwidth_usage
- bandwidth_usage
ms.assetid: 43ed8435-f059-4907-b5c0-193a258b394a
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: c71fdc21c634e8f473d628373ae5adfa9c1c072f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473029"
---
# <a name="sysbandwidth_usage-azure-sql-database"></a>sys.bandwidth_usage (Azure SQL Database)

[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

> [!NOTE]
> 這僅適用于 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V11。 * *  
  
 傳回 **[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] V11 資料庫伺服器** 中每個資料庫所使用網路頻寬的相關資訊。 針對所指資料庫傳回的每一個資料列都會摘要說明在一小時內的單一使用方向和類別。  
  
 **這在中已被取代 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。**  
  
 **Sys.bandwidth_usage** view 包含下列資料行。  
  
|資料行名稱|描述|  
|-----------------|-----------------|  
|**time**|頻寬消耗的小時。 這個檢視中的資料列是以每小時為基礎。 例如，2009-09-19 02:00:00.000 表示頻寬是在 2009 年 9 月 19 日的上午 2:00  和 3:00 之間耗用。|  
|**database_name**|使用頻寬的資料庫名稱。|  
|**direction**|使用的頻寬類型，下列其中一個值：<br /><br /> 輸入：移入的資料 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。<br /><br /> 輸出：即將移出的資料 [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 。|  
|**class**|使用的頻寬類別，下列其中一個值：<br />內部：在 Azure 平臺內移動的資料。<br />外部：從 Azure 平臺移出的資料。<br /><br /> 這個類別只會在資料庫參與區域 ([!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]) 之間的連續複製關聯性時傳回。 如果指定的資料庫未參與任何連續複製關聯性，則不會傳回 "Interlink" 資料列。 如需詳細資訊，請參閱本主題後面的＜備註＞一節。|  
|**time_period**|發生使用時的時間週期為 Peak 或 OffPeak。 The Peak time is based on the region in which the server was created. 例如，如果伺服器是在 "US_Northwest" 區域中建立，則尖峰時間會定義為介於太平洋標準時間上午 10:00 到 和 06:00:00 執行報表， 之間。|  
|**quantity**|使用的頻寬數量，以 KB 為單位。|  
  
## <a name="permissions"></a>權限

 只有 **master** 資料庫中的伺服器層級主體登入才能使用此視圖。  
  
## <a name="remarks"></a>備註  
  
### <a name="external-and-internal-classes"></a>外部和內部類別

 針對在指定時間使用的每個資料庫， **sys.bandwidth_usage** view 會傳回顯示類別和頻寬使用方向的資料列。 下列範例說明所指資料庫可能會公開的資料。 在這個範例中，時間是 2012-04-21 17:00:00，發生在尖峰時段。 資料庫名稱為 Db1。 在此範例中， **sys.bandwidth_usage** 已針對輸入和輸出方向和外部和內部類別的四個組合傳回一列，如下所示：  
  
|time|database_name|direction|Class - 類別|time_period|quantity|  
|----------|--------------------|---------------|-----------|------------------|--------------|  
|2012-04-21 17:00:00|Db1|輸入|外部|Peak|66|  
|2012-04-21 17:00:00|Db1|輸出|外部|Peak|741|  
|2012-04-21 17:00:00|Db1|輸入|內部|Peak|1052|  
|2012-04-21 17:00:00|Db1|輸出|內部|Peak|3525|  
  
### <a name="interpreting-data-direction-for-ssgeodr"></a>解譯 [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)] 的資料方向

 如果是 [!INCLUDE[ssGeoDR](../../includes/ssgeodr-md.md)]，頻寬使用量資料會同時在連續複製關聯性兩端的邏輯 master 資料庫中顯示。 因此，您必須從您要查詢的資料庫觀點來解讀輸入和輸出方向指標。 例如，請考慮從來源伺服器傳送 1MB 資料至目標伺服器的複寫資料流。 在這種情況下，1MB 在來源伺服器上會計入傳送的總資料量中，而在目標伺服器上，1MB 會記錄為接收的資料。  
  
> [!NOTE]  
> 傳送的大量資料是依照使用者資料流程的方向，從來源伺服器傳送到目標伺服器。 不過，有些資料需朝其他方向傳送。  
