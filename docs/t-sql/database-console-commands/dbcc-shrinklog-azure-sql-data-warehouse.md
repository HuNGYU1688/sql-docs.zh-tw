---
description: DBCC SHRINKLOG (平行資料倉儲)
title: DBCC SHRINKLOG (平行資料倉儲)
ms.custom: ''
ms.date: 03/16/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
dev_langs:
- TSQL
author: pmasl
ms.author: umajay
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 58bda1e035c25ae373fef14bfb574a6f2c139835
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480419"
---
# <a name="dbcc-shrinklog-parallel-data-warehouse"></a>DBCC SHRINKLOG (平行資料倉儲)

[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

「跨設備」  降低目前 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] 資料庫的交易記錄大小。 資料重組是為了壓縮交易記錄。 資料庫交易記錄可能會隨著時間變得分散和沒有效率。 使用 DBCC SHRINKLOG 可減少資料分散程度，並縮減記錄大小。
  
![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例 &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>語法  
  
```syntaxsql
DBCC SHRINKLOG   
    [ ( SIZE = { target_size [ MB | GB | TB ]  } | DEFAULT ) ]   
    [ WITH NO_INFOMSGS ]   
[;]  
```  

## <a name="arguments"></a>引數

SIZE = { *target_size* [ MB \| **GB** \| TB ]  } \| **DEFAULT**。  
*target_size* 是 DBCC SHRINKLOG 完成後期望的交易記錄大小 (跨所有計算節點)。 其為大於 0 的整數。  
記錄大小的測量單位是 MB、GB 或 TB。 它是所有計算節點上的交易記錄合併的大小。  
根據預設，DBCC SHRINKLOG 會將交易記錄縮減為儲存在資料庫中繼資料中的記錄大小。 中繼資料的記錄大小由 [CREATE DATABASE &#40;Azure Synapse Analytics&#41;](../statements/create-database-transact-sql.md) 或 [ALTER DATABASE &#40;Azure Synapse Analytics&#41;](../statements/alter-database-transact-sql.md) 中的 LOG_SIZE 參數所決定。 指定 `SIZE=DEFAULT` 或省略 `SIZE` 子句時，DBCC SHRINKLOG 會將交易記錄大小縮減至預設大小。
  
WITH NO_INFOMSGS  
DBCC SHRINKLOG 結果中不會顯示資訊訊息。  
  
## <a name="permissions"></a>權限

需要 ALTER SERVER STATE 權限。

## <a name="general-remarks"></a>一般備註

DBCC SHRINKLOG 不會變更儲存在資料庫中繼資料的記錄大小。 中繼資料會繼續包含在 CREATE DATABASE 或 ALTER DATABASE 陳述式中指定的 LOG_SIZE 參數。
  
## <a name="examples"></a>範例

### <a name="a-shrink-the-transaction-log-to-the-original-size-specified-by-create-database"></a>A. 將交易記錄縮減至 CREATE DATABASE 指定的原始大小。  
假設位址資料庫建立時設定的交易記錄大小是 100 MB。 也就是說，位址資料庫的 CREATE DATABASE 陳述式中含有 LOG_SIZE = 100 MB。 現在，假設記錄已成長至 150 MB，而您想將記錄壓縮回 100 MB。
  
下方的各個陳述式會嘗試將位址資料庫的交易記錄壓縮至預設大小 100 MB。 如果將記錄壓縮至 100 MB 會導致資料遺失，DBCC SHRINKLOG 會盡可能將記錄壓縮到沒有資料遺失的最小大小 (大於 100 MB)。

```sql
USE Addresses;  
DBCC SHRINKLOG ( SIZE = 100 MB );  
DBCC SHRINKLOG ( SIZE = DEFAULT );  
DBCC SHRINKLOG;  
```