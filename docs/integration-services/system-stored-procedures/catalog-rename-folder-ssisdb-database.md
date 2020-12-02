---
description: catalog.rename_folder (SSISDB 資料庫)
title: catalog.rename_folder (SSISDB 資料庫) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 336ab467-c32f-4d2e-a79c-174dc6fab75e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 65b913576b68e5c84037eac57205b23b81d53f95
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129765"
---
# <a name="catalogrename_folder-ssisdb-database"></a>catalog.rename_folder (SSISDB 資料庫)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  重新命名 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目錄中的資料夾。  
  
## <a name="syntax"></a>語法  
  
```sql  
catalog.rename_folder [ @old_name = ] old_name , [ @new_name = ] new_name  
```  
  
## <a name="arguments"></a>引數  
 [ @old_name = ] *old_name*  
 資料夾的原始名稱。 *old_name* 是 **nvarchar(128)**。  
  
 [ @new_name = ] *new_name*  
 資料夾的新名稱。 *new_name* 是 **nvarchar(128)**。  
  
## <a name="return-code-value"></a>傳回碼值  
 無  
  
## <a name="result-sets"></a>結果集  
 None  
  
## <a name="permissions"></a>權限  
 這個預存程序需要下列其中一個權限：  
  
-   **ssis_admin** 資料庫角色的成員資格  
  
-   **系統管理員** 伺服器角色的成員資格  
  
## <a name="errors-and-warnings"></a>錯誤和警告  
 下列清單將描述可能會引發錯誤或警告的某些條件：  
  
-   原始資料夾名稱無效  
  
-   新的名稱已經使用於現有資料夾  
  
  
