---
title: 為記憶體最佳化物件定義持久性 | Microsoft 文件
description: 了解 SQL Server 中適用於經記憶體最佳化的資料表的持久性選項：預設的 SCHEMA_AND_DATA 和 SCHEMA_ONLY。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 0fe85fbf-8e8d-4983-96fd-d04b3c7d6d65
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 299695e0602b41623550a90e84aca9d6e6f633a2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481229"
---
# <a name="defining-durability-for-memory-optimized-objects"></a>為記憶體最佳化的物件定義持久性
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  記憶體最佳化的資料表有兩個持久性選項：  
  
 SCHEMA_AND_DATA (預設值)  
 此選項提供結構描述和資料的持久性。 資料持久性層級取決於您是否將交易認可為完全持久或具有延遲持久性。 完全持久交易提供與資料和結構描述相同的持久性保證，類似以磁碟為基礎的資料表。 延遲持久性可增進效能，但是在伺服器當機或容錯移轉的情況下可能會導致資料損失。 (如需延遲持久性的詳細資訊，請參閱 [控制交易持久性](../../relational-databases/logs/control-transaction-durability.md)。)  
  
 SCHEMA_ONLY  
 此選項可確保資料表結構描述的持久性。 當 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在 Azure SQL Database 中重新啟動或重新設定時，會保存資料表結構描述，但資料表中的資料會遺失。 (這不同於 tempdb 中的資料表，後者的資料表及資料表資料都會在重新啟動之後遺失)。建立非持久性資料表的典型案例為儲存暫時性資料，例如 ETL 處理序的暫存資料表。 SCHEMA_ONLY 持久性會避免交易記錄和檢查點，這樣可大幅減少 I/O 作業。  
  
 使用預設 SCHEMA_AND_DATA 資料表時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會提供與磁碟資料表相同的持久性保證：  
  
 交易持久性  
 當您認可一項對記憶體最佳化資料表進行 (DDL 或 DML) 變更的完全持久交易時，對持久的記憶體最佳化資料表所做的變更就會變成永久變更。  
  
 當您將延遲的持久交易認可到記憶體最佳化資料表時，只有將記憶體中的交易記錄儲存至磁碟之後，交易才會變成持久。 (如需延遲持久性的詳細資訊，請參閱 [控制交易持久性](../../relational-databases/logs/control-transaction-durability.md)。)  
  
 重新啟動持久性  
 當 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 在當機或計劃中的關機之後重新啟動時，記憶體最佳化的持久性資料表會重新具現化，以還原到關機或當機之前的狀態。  
  
 媒體故障持久性  
 如果故障或損毀的磁碟包含持久性記憶體最佳化物件的一個或多個保存複本， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 備份和還原功能會在新的媒體上還原記憶體最佳化的資料表。  
  
## <a name="see-also"></a>另請參閱  
 [建立及管理記憶體最佳化物件的儲存體](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
