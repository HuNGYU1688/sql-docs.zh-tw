---
description: PolyBase 功能和限制
title: PolyBase 功能和限制 | Microsoft Docs
descriptions: This article summarizes PolyBase features available for SQL Server products and services. It lists T-SQL operators supported for pushdown and known limitations.
ms.date: 11/13/2020
ms.prod: sql
ms.technology: polybase
ms.topic: conceptual
ms.assetid: 6591994d-6109-4285-9c5b-ecb355f8a111
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ce4c3624600741d900d51dc832ab4a685c28107a
ms.sourcegitcommit: 0f484f32709a414f05562bbaafeca9a9fc57c9ed
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/14/2020
ms.locfileid: "94631744"
---
# <a name="polybase-features-and-limitations"></a>PolyBase 功能和限制

[!INCLUDE[appliesto-ss2016-asdb-asdw-pdw-md](../../includes/tsql-appliesto-ss2016-all-md.md)]

本文摘要說明 SQL Server 產品和服務可用的 PolyBase 功能。  
  
## <a name="feature-summary-for-product-releases"></a>產品版本的功能摘要

下表列出 PolyBase 的重要功能以及提供這些功能的產品。  

|**功能** |**SQL Server 2016** |**Azure SQL Database** |**Azure Synapse Analytics** |**平行處理資料倉儲** |
|---------|---------|---------|---------|---------|
|使用下列項目查詢 Hadoop 資料： [!INCLUDE[tsql](../../includes/tsql-md.md)]|是|否|否|是|
|從 Hadoop 匯入資料|是|否|否|是|
|匯出資料至 Hadoop  |是|否|否| 是|
|在 Azure HDInsight 中查詢、匯入、匯出 |否|否|否|否
|將查詢計算下推到 Hadoop|是|否|否|是|  
|從 Azure Blob 儲存體匯入資料|是|否|是|是|
|匯出資料至 Azure Blob 儲存體|是|否|是|是|  
|從 Azure Data Lake Store 匯入資料|否|否|是|否|
|將資料匯出至 Azure Data Lake Store|否|否|是|否|
|從 Microsoft BI 工具執行 PolyBase 查詢|是|否|是|是|

## <a name="pushdown-computation-supported-by-t-sql-operators"></a>T-SQL 運算子支援下推計算

在 SQL Server 和 APS 中，並非所有 T-SQL 運算子都可以下推到 Hadoop 叢集。 下表列出所有支援運算子和部分不受支援運算子。

|**運算子類型** |**可推送到 Hadoop** |**可推送到 Blob 儲存體** |
|---------|---------|---------|
|資料行投影|是|否|
|述詞|是|否|
|彙總|部分|否|
|外部資料表之間的聯結|否|否|
|外部資料表和本機資料表之間的聯結|否|否|
|排序|否|否|

部分彙總表示最後的彙總必須在資料到達 SQL Server 之後進行。 但是一部分的彙總會在 Hadoop 中進行。 這是大量平行處理系統計算彙總的常見方法。  

## <a name="known-limitations"></a>已知限制

PolyBase 具有下列限制：

- 若要使用 PolyBase，您必須在資料庫上具有系統管理員或 CONTROL SERVER 層級權限。

- 在 SQL Server 中最大資料列大小 (包括可變長度資料行的完整長度) 不能超過 32 KB，在 Azure Synapse Analytics 中則不能超過 1 MB。

- 將資料從 SQL Server 或 Azure Synapse Analytics 匯出為 ORC 檔案格式時，可能會限制具有大量文字的資料行。 由於 Java 記憶體不足錯誤訊息，因此只能限制為最少 50 個資料行。 若要解決這個問題，只需要匯出資料行的子集。

- 如果已啟用 Knox，PolyBase 就無法連線到 Hortonworks 執行個體。

- 若您使用 Hive 資料表，且 transactional = true，PolyBase 就無法存取 Hive 資料表目錄中的資料。

<!--SQL Server 2016-->
::: moniker range="= sql-server-2016 || =sqlallproducts-allversions"

- [將節點新增至 SQL Server 2016 容錯移轉叢集時，不會安裝 PolyBase](https://support.microsoft.com/help/3173087/fix-polybase-feature-doesn-t-install-when-you-add-a-node-to-a-sql-server-2016-failover-cluster)。

::: moniker-end

## <a name="next-steps"></a>後續步驟

如需 PolyBase 的詳細資訊，請參閱[什麼是 PolyBase？](polybase-guide.md)。
