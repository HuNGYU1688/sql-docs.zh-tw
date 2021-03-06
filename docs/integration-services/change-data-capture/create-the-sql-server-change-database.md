---
description: 建立 SQL Server 變更資料庫
title: 建立 SQL Server 變更資料庫 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- oraIns
ms.assetid: 4f79c24a-e99a-4a06-8637-51eeec406259
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b49e099d675aec6158048d8a2a0fb4fe1c0ba64a
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88457729"
---
# <a name="create-the-sql-server-change-database"></a>建立 SQL Server 變更資料庫

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  當您啟動新增執行個體精靈時，隨即開啟 [建立 CDC 資料庫] 頁面。 使用 [建立 CDC 資料庫] 頁面可提供有關新的 CDC 執行個體及建立新的變更資料庫的資訊。  
  
 當您建立新的 CDC 資料庫時，它會啟用 SQL Server CDC，而這樣的啟用需要屬於 `sysadmin` 固定伺服器角色成員的登入。  
  
 如果啟動「建立 Oracle CDC 執行個體」精靈的使用者不是 `sysadmin` 固定伺服器角色的成員，便會開啟 [連接到 SQL Server] 對話方塊，並要求系統管理員 (sysadmin) 角色成員的認證，才能執行「為 SQL Server CDC 啟用資料庫」工作。 當建立 CDC 資料庫時，將會捨棄 `sysadmin` 登入，而且會使用之前進入 Oracle 設計工具主控台時所使用的原始 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入繼續工作。  
  
> [!IMPORTANT]  
>  如果是「為 SQL Server CDC 啟用資料庫」以外的工作，用於執行 [新增執行個體精靈] 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登入必須擁有 `dbcreator` 固定伺服器角色及 MSXDBCDC 資料庫的 UPDATE 權限。  
  
 如需有關在 [連接到 SQL Server] 對話方塊中輸入資料的詳細資訊，請參閱＜ [SQL Server Connection for Instance Creation](../../integration-services/change-data-capture/sql-server-connection-for-instance-creation.md)＞。  
  
## <a name="options"></a>選項  
 **Oracle CDC 執行個體**  
 輸入以下有關您建立之 CDC 執行個體的資訊。  
  
-   **名稱**：輸入新服務的名稱。 這也會是新的變更資料庫的名稱。  
  
-   **描述**：輸入新執行個體的描述，幫助您識別該執行個體。 這是選擇性的。  
  
 **SQL Server 變更資料庫**  
 此區段是用來建立資料庫。  
  
1.  **變更資料庫**：新的變更資料庫的名稱。 此資料庫的名稱與您提供給執行個體的名稱相同。 這個唯讀欄位會顯示資料庫的完整路徑。  
  
2.  **建立資料庫**：按一下 **[建立資料庫]** ，即可建立資料庫。  
  
     若要建立資料庫，登入必須擁有 `sysasmin` 伺服器角色。 如需詳細資訊，請參閱上述的安全性注意事項。  
  
     在您建立資料庫之後，可以按 **[下一步]**[Connect to an Oracle Source Database](../../integration-services/change-data-capture/connect-to-an-oracle-source-database.md)。  
  
## <a name="see-also"></a>另請參閱  
 [如何建立 SQL Server 變更資料庫執行個體](../../integration-services/change-data-capture/how-to-create-the-sql-server-change-database-instance.md)   
 [Oracle CDC 服務](../../integration-services/change-data-capture/the-oracle-cdc-service.md)  
  
  
