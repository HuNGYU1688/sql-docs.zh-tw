---
description: 時態表考量與限制
title: 時態表考量與限制 | Microsoft Docs
ms.custom: ''
ms.date: 09/12/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: c8a21481-0f0e-41e3-a1ad-49a84091b422
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3ba8729558f6e3e1736db9c380a268cd606444f1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482352"
---
# <a name="temporal-table-considerations-and-limitations"></a>時態表考量與限制


[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]


使用時態表時由於系統版本性質使然，因此需要瞭解某些考量與限制。

使用時態表時，請考慮下列事項：

- 時態表必須具有定義的主索引鍵，以將目前資料表與歷程記錄資料表的記錄相互關聯，且歷程記錄資料表不可具有定義的主索引鍵。
- 記錄 **SysStartTime** 和 **SysEndTime** 值所用的 SYSTEM_TIME 期間資料行，必須使用 datetime2 的資料類型加以定義。
- 若在建立歷程記錄資料表時指定歷程記錄資料表名稱，則您必須指定結構描述和資料表名稱。
- 根據預設，歷程記錄資料表會採 **PAGE** 壓縮處理。
- 若目前的資料表已分割，則會在預設檔案群組上建立歷程記錄資料表，這是因為資料分割設定不會自動從目前的資料表複寫至歷程記錄資料表。
- 時態表與歷程記錄資料表不可為 **FILETABLE** ，且會包含 **FILESTREAM** 以外所有受支援的資料類型資料行，這是因為 **FILETABLE** 和 **FILESTREAM** 允許在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之外操作資料，以致無法保證系統版本設定。
- 節點或邊緣資料表無法建立為或改變為時態表。
- 時態表支援 Blob 資料類型 (例如 **(n)varchar(max)** 、**varbinary(max)** 、 **(n)text** 和 **image**)，但會產生龐大的儲存成本，且其大小會影響效能。 因此您在設計系統時，應小心使用這些資料類型。
- 歷程記錄資料表必須建立於與目前資料表相同的資料庫中。 不支援針對 **Linked Server** 的時態查詢。
- 歷程記錄資料表不可具有條件約束 (主索引鍵、外部索引鍵、資料表或資料行條件約束)。
- 時態性查詢 (使用 **FOR SYSTEM_TIME** 子句的查詢) 不支援索引檢視。
- 若為系統版本設定時態表，ONLINE 選項 (**WITH (ONLINE = ON**) 對 **ALTER TABLE ALTER COLUMN** 無效。 無論針對 ONLINE 選項指定何值，都不會以線上方式執行 ALTER 資料行。
- **INSERT** 和 **UPDATE** 陳述式無法參考 SYSTEM_TIME 期間資料行。 若嘗試將值直接插入這些資料行，則會遭到系統封鎖。
- 當 **TRUNCATE TABLE** 為 **TRUNCATE TABLE** 時，不支援 **TRUNCATE TABLE**。
- 不允許直接修改歷程記錄資料表中的資料。
- **ON DELETE CASCADE** 和 **ON UPDATE CASCADE** 不得位於目前資料表。 換言之，若時態表參考外部索引鍵關係中的資料表 (對應至 sys.foreign_keys 中的 *parent_object_id* )，則不允許 CASCADE 選項。 若要因應這項限制，請使用應用程式邏輯或 after 觸發程序來維持主索引鍵資料表 (對應到 sys.foreign_keys 中的 *referenced_object_id*) 中刪除時的一致性。 若主索引鍵資料表屬於時態性，而且參考資料表屬於非時態性，則沒有這類限制。

  > [!NOTE]
  > 這項限制僅適用於 SQL Server 2016。 [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] 和 SQL Server 2017 (從 CTP 2.0 開始) 支援 CASCADE 選項。

- 目前或歷程記錄資料表不允許使用 **INSTEAD OF** 觸發程序，以避免使 DML 邏輯無效。 **AFTER** 觸發程序僅允許針對目前的資料表使用。 這些觸發程序在歷程記錄資料表上會遭到封鎖，以避免使 DML 邏輯無效。
- 能使用的複寫技術有限：

  - **Always On：** 完全支援
  - **異動資料擷取與變更資料追蹤：** 僅在目前資料表上獲得支援
  - **快照集與交易複寫**：僅支援未啟用時態的單一發行者，以及已啟用時態的單一訂閱者。 在此情況下，當訂閱者執行卸載報表時會針對 OLTP 工作負載使用發行者 (包括 'AS OF' 查詢)。 當散發代理程式啟動時，其會開啟在散發代理程式停止之前皆保持開啟的交易。 由於此行為，SysStartTime 與 SysEndTime 會填入散發代理程式開始進行第一筆交易的開始時間。 因此，如果 SysStartTime 和 SysEndTime 填入與目前系統時間接近的時間對於應用程式或組織來說很重要，則最好是依排程執行散發代理程式，而非依預設行為持續地執行散發代理程式。 不支援使用多位訂閱者，因為這可能會由於本機系統時鐘而導致不一致的時態性資料。
  - **合併式複寫：** 不支援時態表

- 一般查詢只會影響目前資料表中的資料。 若要查詢歷程記錄資料表中的資料，您必須使用時態查詢。 我們會在本文件後面的＜查詢時態資料＞一節中加以詳述。
- 最佳的索引策略，是在目前的資料表上加入叢集資料行存放區索引和/或 B 型樹狀結構資料列存放區索引，以及在歷程記錄資料表上加入叢集資料行存放區索引，藉以取得最佳儲存大小和效能。 若您建立 / 使用專屬的歷程記錄資料表，強烈建議您建立由期間資料行組成的索引類型，其會以期間資料行結尾開頭來加速 時態查詢，並可加速屬於資料一致性檢查作業的查詢。 預設的歷程記錄資料表已根據期間資料行 (結尾、開頭)，為您建立叢集資料列存放區索引。 建議您至少使用非叢集資料列存放區索引。
- 建立記錄資料表時，下列物件/屬性不會從目前的資料表複寫到記錄資料表：

  - 期間定義
  - 識別定義
  - 索引
  - 統計資料
  - 檢查條件約束
  - 觸發程序
  - 資料分割設定
  - 權限
  - 資料列層級安全性述詞

- 歷程記錄資料表不可設定為一連串歷程記錄資料表中的目前資料表。

## <a name="see-also"></a>另請參閱

- [時態表](../../relational-databases/tables/temporal-tables.md)
- [開始使用系統建立版本的時態表](../../relational-databases/tables/getting-started-with-system-versioned-temporal-tables.md)
- [時態表系統一致性檢查](../../relational-databases/tables/temporal-table-system-consistency-checks.md)
- [對時態表進行資料分割](../../relational-databases/tables/partitioning-with-temporal-tables.md)
- [時態表安全性](../../relational-databases/tables/temporal-table-security.md)
- [管理系統設定版本時態表中的歷程記錄資料保留](../../relational-databases/tables/manage-retention-of-historical-data-in-system-versioned-temporal-tables.md)
- [系統版本設定時態表與記憶體最佳化資料表](../../relational-databases/tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
- [暫存資料表中繼資料檢視和函數](../../relational-databases/tables/temporal-table-metadata-views-and-functions.md)
