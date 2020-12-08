---
title: 開啟活動監視器 (SSMS)
description: 了解如何在 SQL Server Management Studio 中開啟活動監視器。 活動監視器會查詢受監視的執行個體，以取得並顯示資訊。
ms.custom: seo-dt-2019
ms.date: 08/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Activity Monitor [SQL Server], setting the refresh interval
- refresh interval for Activity Monitor
- Activity Monitor [SQL Server], opening
- opening Activity Monitor
ms.assetid: 0a6eeb16-f02b-479d-9a60-543e40ebf46b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5569286a31b9897b8fcca0eafea6830593d05183
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505992"
---
# <a name="open-activity-monitor-in-sql-server-management-studio-ssms"></a>在 SQL Server Management Studio (SSMS) 中開啟活動監視器
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   
 [活動監視器] 會在監視的執行個體上執行查詢，以便取得 [活動監視器] 顯示窗格的資訊。 當重新整理間隔的設定小於 10 秒時，用來執行這些查詢的時間就可能會影響伺服器效能。  
  
  
##  <a name="check-your-permissions"></a><a name="Permissions"></a> 檢查您的權限！  
 若要檢視實際活動，您必須具有 VIEW SERVER STATE 權限。 若要檢視活動監視器的 [資料檔案 I/O] 區段，除了 VIEW SERVER STATE 之外，您也必須具有 CREATE DATABASE、ALTER ANY DATABASE 或 VIEW ANY DEFINITION 權限。  
  
 若要針對處理序執行 KILL 命令，使用者必須是系統管理員 (sysadmin) 或處理序管理員 (processadmin) 固定伺服器角色的成員。  
  
  
## <a name="open-activity-monitor"></a>開啟活動監視器  

### <a name="keyboard-shortcut"></a>鍵盤快速鍵  
 - 輸入 **CTRL+ALT+A** 隨時開啟活動監視器。

 >**提示！** 將滑鼠停留在 SSMS 中的任何圖示上方來了解其功能及用以啟用的鍵盤快速鍵！

### <a name="toolbar"></a>工具列

從標準工具列，按一下 [活動監視器]。 它位於正中間，就在 [復原/取消復原] 按鈕的右邊。
![Activity_Monitor_icon](../../relational-databases/performance-monitor/media/activity-monitor-icon.png)  
  
如果您尚未連接到所要監視的 SQL Server 執行個體，請完成 [連接到伺服器] 對話方塊。
  
## <a name="launch-activity-monitor-and-object-explorer-on-startup"></a>在啟動時啟動活動監視器和物件總管
  
1.  在 **[工具]** 功能表中，按一下 **[選項]** 。  
  
2.  在 [選項] 對話方塊中，展開 [環境]，然後選取 [啟動]。  
  
3.  從 [啟動時] 下拉式清單，選取 [開啟物件總管和活動監視器]。  

4.  按一下 [確定]。

![open_object_explorer](../../relational-databases/performance-monitor/media/open-object-explorer.png)
  
  
## <a name="set-the-activity-monitor-refresh-interval"></a>設定活動監視器的重新整理間隔  
  
1.   開啟 [活動監視器]。  
  
2.   以滑鼠右鍵按一下 [概觀]，選取 [重新整理間隔]，然後選取活動監視器應該用來取得新執行個體資訊的間隔。  
  
  
