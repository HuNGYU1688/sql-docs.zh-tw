---
description: 開始使用系統建立版本的時態表
title: 開始使用系統建立版本的時態表 | Microsoft Docs
ms.custom: ''
ms.date: 03/28/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: d431f216-82cf-4d97-825e-bb35d3d53a45
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1d62bbb6d38da6b60179f723e4bb363dec6ad225
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440535"
---
# <a name="getting-started-with-system-versioned-temporal-tables"></a>開始使用系統建立版本的時態表


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


根據您的案例，您可以建立新的系統建立版本的時態表，或修改現有的時態表，方法是將 temporal 屬性加入現有的資料表結構描述。 時態表中的資料修改時，系統就會建置應用程式和使用者看不到的版本歷程記錄。 如此一來，使用系統建立版本的時態表不需要對資料表的修改方法，或查詢資料的最新 (實際) 狀態的方式進行任何變更。

除了一般資料操作語言 (DML) 和查詢，Temporal 也提供方便且簡單的方式，透過擴充的 Transact-SQL 語法，從資料歷程記錄中取得見解。 每個系統建立版本的資料表中有一個指派的歷程記錄資料表，但是使用者完全看不到，除非他們想要藉由建立額外的索引，或選擇不同的儲存體選項，最佳化工作負載效能或儲存體使用量。

下列圖表描述使用系統建立版本之時態表的一般工作流程︰![開始使用時態表](../../relational-databases/tables/media/getting-started-with-temporal.png "開始使用時態表")

本主題分成以下 5 個子主題：

- [建立系統設定版本時態表](../../relational-databases/tables/creating-a-system-versioned-temporal-table.md)
- [修改系統建立版本時態表中的資料](../../relational-databases/tables/modifying-data-in-a-system-versioned-temporal-table.md)
- [查詢系統建立版本時態表中的資料](../../relational-databases/tables/querying-data-in-a-system-versioned-temporal-table.md)
- [變更系統建立版本時態表的結構描述](../../relational-databases/tables/changing-the-schema-of-a-system-versioned-temporal-table.md)
- [停止系統設定版本時態表上的系統版本設定功能](../../relational-databases/tables/stopping-system-versioning-on-a-system-versioned-temporal-table.md)

## <a name="next-steps"></a>後續步驟

- [時態表](../../relational-databases/tables/temporal-tables.md)
- [時態表系統一致性檢查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [對時態表進行資料分割](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [時態表考量與限制](../../relational-databases/tables/temporal-table-considerations-and-limitations.md)
- [時態表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理系統設定版本時態表中的歷程記錄資料保留](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系統版本設定時態表與記憶體最佳化資料表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [暫存資料表中繼資料檢視和函數](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
