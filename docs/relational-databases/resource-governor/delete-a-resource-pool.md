---
title: 刪除資源集區 | Microsoft 文件
description: 了解如何使用 SQL Server Management Studio 或 Transact-SQL 來刪除資源集區。 您必須具有 CONTROL SERVER 權限。
ms.custom: ''
ms.date: 03/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool delete
- resource pools [SQL Server], delete
ms.assetid: 3bdd348b-6582-4ffa-80ef-d49e50596ce5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2df6daca54fe31273d61226c210285fec305769d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96504818"
---
# <a name="delete-a-resource-pool"></a>刪除資源集區
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  您可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 或 Transact-SQL 刪除資源集區。  
  
-   **開始之前：** [限制事項](#LimitationsRestrictions)、[權限](#Permissions)  
  
-   **若要刪除資源集區，請使用下列方式：** [SQL Server Management Studio](#DelRPSSMS)、[Transact-SQL](#DelRPTSQL)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> 開始之前  
 如果資源集區包含工作負載群組，您就無法刪除該資源集區。  
  
###  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> 限制事項  
 您無法刪除資源管理員的預設或內部資源集區。 如果資源集區包含工作負載群組，您就無法刪除該資源集區。 如需詳細資訊，請參閱 [Delete a Workload Group](../../relational-databases/resource-governor/delete-a-workload-group.md)。  
  
###  <a name="permissions"></a><a name="Permissions"></a> 權限  
 刪除資源集區需要 CONTROL SERVER 權限。  
  
##  <a name="delete-a-resource-pool-using-object-explorer"></a><a name="DelRPSSMS"></a> 使用物件總管刪除資源集區  
 **若要使用 SQL Server Management Studio 刪除資源集區**  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，開啟 [物件總管]，然後遞迴地向下展開 **[管理]** 節點至 **[資源管理員]** 。  
  
2.  以滑鼠右鍵按一下要刪除的資源集區，然後按一下 [刪除]。  
  
3.  在 **[刪除物件]** 視窗中，資源集區列於 **[要刪除的物件]** 清單內。 若要刪除資源集區，請按一下 **[確定]** 。  

    > [!NOTE]  
    >  如果您嘗試刪除的資源集區包含工作負載群組，這項動作將會失敗。  
  
##  <a name="delete-a-resource-pool-using-transact-sql"></a><a name="DelRPTSQL"></a> 使用 Transact-SQL 刪除資源集區  
 **若要使用 Transact-SQL 刪除資源集區**  
  
1.  執行 **DROP RESOURCE POOL** 或 **DROP EXTERNAL RESOURCE POOL** 陳述式，並指定要刪除之資源集區的名稱。  
  
2.  執行 **ALTER RESOURCE GOVERNOR RECONFIGURE** 陳述式。  
  
### <a name="example-transact-sql"></a>範例 &#40;Transact-SQL&#41;  
 下列範例會卸除名稱為 `poolAdhoc`的工作負載群組。  
  
```  
DROP RESOURCE POOL poolAdhoc;  
GO  
ALTER RESOURCE GOVERNOR RECONFIGURE;  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [資源管理員](../../relational-databases/resource-governor/resource-governor.md)   
 [資源管理員資源集區](../../relational-databases/resource-governor/resource-governor-resource-pool.md)   
 [建立資源集區](../../relational-databases/resource-governor/create-a-resource-pool.md)   
 [變更資源集區設定](../../relational-databases/resource-governor/change-resource-pool-settings.md)   
 [資源管理員工作負載群組](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [資源管理員分類函數](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [DROP WORKLOAD GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/drop-workload-group-transact-sql.md)   
 [DROP RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-resource-pool-transact-sql.md)   
 [ALTER RESOURCE GOVERNOR &#40;Transact-SQL&#41;](../../t-sql/statements/alter-resource-governor-transact-sql.md)   
 [DROP EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-resource-pool-transact-sql.md)   
 [CREATE EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-resource-pool-transact-sql.md)   
 [ALTER EXTERNAL RESOURCE POOL &#40;Transact-SQL&#41;](../../t-sql/statements/alter-external-resource-pool-transact-sql.md)  
  
  
