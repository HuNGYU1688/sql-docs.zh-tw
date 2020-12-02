---
description: 備份資料庫工作
title: 備份資料庫工作 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.backupdatabasetask.f1
helpviewer_keywords:
- database backups [Integration Services]
- Back Up Database task [Integration Services]
- backing up databases [Integration Services]
- transaction log backups [Integration Services]
- backing up transaction logs [Integration Services]
ms.assetid: b8839d71-13b7-41f2-a434-cb95020e79d7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 63e02be4fe2c7fdd1c0293cd5e76dcb9e0adca22
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123610"
---
# <a name="back-up-database-task"></a>備份資料庫工作

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  「備份資料庫」工作會執行不同類型的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料庫備份作業。 如需詳細資訊，請參閱 [SQL Server 資料庫的備份與還原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)。  
  
 使用「備份資料庫」工作，封裝即可備份單一資料庫或多個資料庫。 如果工作僅備份單一資料庫，您可以選擇備份元件：資料庫，或其檔案和檔案群組。  
  
## <a name="supported-recover-models-and-backup-types"></a>支援的復原模型和備份類型  
 下表列出「備份資料庫」工作支援的復原模式和備份類型。  
  
|復原模式|資料庫|資料庫差異|交易記錄|檔案或檔案差異|  
|--------------------|--------------|---------------------------|---------------------|-------------------------------|  
|簡單|必要|選用|不支援|不支援|  
|完整|必要|選用|必要|選用|  
|大量記錄|必要|選用|必要|選用|  
  
 「備份資料庫」工作會封裝 Transact-SQL BACKUP 陳述式。 如需詳細資訊，請參閱 [BACKUP &#40;Transact-SQL&#41;](../../t-sql/statements/backup-transact-sql.md)。  
  
## <a name="configuration-of-the-back-up-database-task"></a>備份資料庫工作的組態  
 您可以透過 [ [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師] 設定屬性。 這項工作位於「 **設計師」中** [工具箱] **的** [維護計畫工作] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 區段。  
  
 如需有關可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師中設定之屬性的詳細資訊，請按下列主題：  
  
-   [備份資料庫工作 &#40;維護計畫&#41;](../../relational-databases/maintenance-plans/options-in-the-back-up-database-task-for-maintenance-plan.md)  
  
 如需有關如何在「 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師」中設定這些屬性的詳細資訊，請按下列主題：  
  
-   [設定工作或容器的屬性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
