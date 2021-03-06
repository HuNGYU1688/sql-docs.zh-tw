---
title: SQL Server 可用性群組的強制容錯移轉
description: 叢集類型為 NONE 之可用性群組的強制容錯移轉
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: eeef45e1678a1770f2dd0fc89c38943fa76cef72
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/05/2020
ms.locfileid: "91726416"
---
每個可用性群組只有一個主要複本。 主要複本允許讀取和寫入。 若要變更作為主要的複本，您可以進行容錯移轉。 在高可用性的可用性群組中，叢集管理員會自動化容錯移轉程序。 在叢集類型為 NONE 的可用性群組中，容錯移轉程序是手動的。 

叢集類型為 NONE 的可用性群組中，有兩種方式可進行主要複本容錯移轉：

- 強制手動容錯移轉 (可能遺失資料)
- 手動容錯移轉 (不會遺失資料)

### <a name="forced-manual-failover-with-data-loss"></a>強制手動容錯移轉 (可能遺失資料)

當主要複本無法使用且無法復原時，請使用這個方法。 

若要強制容錯移轉 (可能遺失資料)，請連線到裝載目標次要複本的 SQL Server 執行個體，然後執行下列命令：

```SQL
ALTER AVAILABILITY GROUP [ag1] FORCE_FAILOVER_ALLOW_DATA_LOSS;
```

當先前的主要複本復原時，它也會假設主要角色。 若要確定先前的主要複本會轉換成次要角色，請在先前的主要複本上執行下列命令。

```SQL
ALTER AVAILABILITY GROUP [ag1]  SET (ROLE = SECONDARY);
```

### <a name="manual-failover-without-data-loss"></a>手動容錯移轉 (不會遺失資料)

當主要複本可以使用，但您需要暫時或永久變更設定並變更裝載主要複本的 SQL Server 執行個體時，請使用這個方法。 若要避免遺失資料的可能性，發出手動容錯移轉之前，請確定目標次要複本是最新狀態。 

若要手動容錯移轉 (不會遺失資料)：

1. 請製作目前的主要複本與目標次要複本 `SYNCHRONOUS_COMMIT`。

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. 若要識別使用中交易已認可至主要複本及至少一個同步次要複本，請執行下列查詢： 

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   當 `synchronization_state_desc` 為 `SYNCHRONIZED` 時，即會同步處理次要複本。

1. 將 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 更新為 1。

   下列指令碼會將名為 `ag1` 的可用性群組上的 `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` 設為 1。 執行下列指令碼之前，以您的可用性群組名稱取代 `ag1`：

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   這項設定可確保所有使用中交易都已認可至主要複本及至少一個同步次要複本。 
   >[!NOTE]
   >這項設定非容錯移轉所特定，且應該根據環境的需求進行設定。
   
1. 將主要複本離線，以準備進行角色變更。
   ```SQL
   ALTER AVAILABILITY GROUP [ag1] OFFLINE
   ```

1. 將目標次要複本升階為主要。 

   ```SQL
   ALTER AVAILABILITY GROUP ag1 FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ``` 

1. 將舊的主要角色更新為 `SECONDARY`，並在裝載舊主要複本的 SQL Server 執行個體上執行下列命令：

   ```SQL
   ALTER AVAILABILITY GROUP [ag1] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE] 
   > 若要刪除可用性群組，請使用 [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md)。 針對以叢集類型 NONE 或 EXTERNAL 建立的可用性群組，請在屬於可用性群組的所有複本上執行此命令。

1. 繼續進行資料移動，在裝載主要複本的 SQL Server 執行個體上，針對可用性群組中的每個資料庫執行下列命令： 

   ```sql
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. 重新建立為讀取縮放目的而建立的任何接聽程式，且不由叢集管理員所管理。 如果原始接聽程式指向舊的主要複本，請將其捨棄，然後重新建立接聽程式以指向新的主要複本。