---
description: catalog.get_project (SSISDB 資料庫)
title: catalog.get_project (SSISDB 資料庫) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: f263c9e4-a7db-4888-a458-70ae99b1f729
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d26de0736fc41d3b39f0f6c3e149b044c538ba41
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129771"
---
# <a name="catalogget_project-ssisdb-database"></a>catalog.get_project (SSISDB 資料庫)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  擷取已部署至 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 伺服器之專案的二進位資料流。  
  
## <a name="syntax"></a>語法  
  
```sql  
catalog.get_project [ @folder_name = ] folder_name , [ @project_name = ] project_name   
```  
  
## <a name="arguments"></a>引數  
 [ @folder_name = ] *folder_name*  
 包含專案之資料夾的名稱。 *folder_name* 是 **nvarchar(128)**。  
  
 [ @project_name = ] *project_name*  
 專案的名稱。 *project_name* 是 **nvarchar(128)**。  
  
## <a name="return-code-value"></a>傳回碼值  
 0 (成功)  
  
## <a name="result-sets"></a>結果集  
 專案的二進位資料流會當作 **varbinary(MAX)** 傳回。 如果找不到資料夾或專案，則會不傳回任何結果。  
  
## <a name="permissions"></a>權限  
 這個預存程序需要下列其中一個權限：  
  
-   專案的 READ 權限  
  
-   **ssis_admin** 資料庫角色的成員資格  
  
-   **系統管理員** 伺服器角色的成員資格  
  
## <a name="errors-and-warnings"></a>錯誤和警告  
 下列清單將描述可能會造成 get_project 預存程序引發錯誤的某些條件：  
  
-   專案不存在  
  
-   資料夾不存在  
  
-   使用者未具備適當的權限  
  
  
