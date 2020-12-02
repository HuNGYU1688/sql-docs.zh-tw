---
description: catalog.deploy_project (SSISDB 資料庫)
title: catalog.deploy_project (SSISDB 資料庫) | Microsoft Docs
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2e3439b4-7226-4b61-a993-7a1d161eac7e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6a8bb809f349ce68b12abaee171928805d1fe0df
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129814"
---
# <a name="catalogdeploy_project-ssisdb-database"></a>catalog.deploy_project (SSISDB 資料庫)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  將專案部署至 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目錄中的資料夾，或更新先前已部署的現有專案。  
  
## <a name="syntax"></a>語法  
  
```sql  
catalog.deploy_project [@folder_name =] folder_name   
      , [ @project_name = ] project_name   
      , [ @project_stream = ] projectstream   
    [ , [ @operation_id = ] operation_id OUTPUT ]   
```  
  
## <a name="arguments"></a>引數  
 [@folder_name =] *folder_name*  
 部署專案之目標資料夾的名稱。 *folder_name* 是 **nvarchar(128)** 。  
  
 [@project_name =] *project_name*  
 資料夾中全新或已更新專案的名稱。 *project_name* 是 **nvarchar(128)** 。  
  
 [@projectstream =] *projectstream*  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 專案部署檔案 (副檔名為 .ispac) 的二進位內容。  
  
 您可以使用 SELECT 陳述式搭配 OPENROWSET 函數和 BULK 資料列集提供者，以擷取檔案的二進位內容。 如需範例，請參閱[部署 Integration Services (SSIS) 專案和套件](../../integration-services/packages/deploy-integration-services-ssis-projects-and-packages.md)。 如需 OPENROWSET 的詳細資訊，請參閱 [OPENROWSET &#40;Transact-SQL&#41;](../../t-sql/functions/openrowset-transact-sql.md)。  
  
 *projectstream* 是 **varbinary(MAX)**  
  
 [@operation_id =] *operation_id*  
 傳回部署作業的唯一識別碼。 *operation_id* 是 **bigint**。  
  
## <a name="return-code-value"></a>傳回碼值  
 0 (成功)  
  
## <a name="result-sets"></a>結果集  
 None  
  
## <a name="permissions"></a>權限  
 這個預存程序需要下列其中一個權限：  
  
-   要部署新專案之資料夾的 CREATE_OBJECTS 權限，或是要更新之專案的 MODIFY 權限  
  
-   **ssis_admin** 資料庫角色的成員資格  
  
-   **系統管理員** 伺服器角色的成員資格  
  
## <a name="errors-and-warnings"></a>錯誤和警告  
 下列清單將描述可能會造成預存程序引發錯誤的某些條件：  
  
-   參數參考到不存在的物件、參數會嘗試建立物件已經存在的物件，或參數因為其他原因而無效  
  
-   參數 *\@project_name* 的值與部署檔案中的專案名稱不符  
  
-   使用者未具備足夠的權限  
  
## <a name="remarks"></a>備註  
 在專案部署或更新期間，預存程序並不會檢查專案中的個別封裝的保護等級。  
  
  
