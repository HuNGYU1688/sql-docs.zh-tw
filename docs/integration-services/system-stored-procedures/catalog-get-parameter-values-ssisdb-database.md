---
description: catalog.get_parameter_values (SSISDB 資料庫)
title: catalog.get_parameter_values (SSISDB 資料庫) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 5b1aeaf7-c938-4aef-bafc-e4d7a82eb578
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 349a68fa94cfb479c6c3823b2b295a3a65498692
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129779"
---
# <a name="catalogget_parameter_values-ssisdb-database"></a>catalog.get_parameter_values (SSISDB 資料庫)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  從 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目錄中的專案和對應封裝解析與擷取預設參數值。  
  
## <a name="syntax"></a>語法  
  
```sql  
catalog.get_parameter_values [ @folder_name = ] folder_name  
     , [ @project_name = ] project_name  
     , [ @package_name = ] package_name  
  [  , [ @reference_id = ] reference_id  ]  
  
```  
  
## <a name="arguments"></a>引數  
 [ @folder_name = ] *folder_name*  
 包含專案之資料夾的名稱。 *folder_name* 是 **nvarchar(128)** 。  
  
 [ @project_name = ] *project_name*  
 參數所在的專案名稱。 *project_name* 是 **nvarchar(128)** 。  
  
 [ @package_name = ] *package_name*  
 封裝名稱。 指定封裝名稱，以擷取所有專案參數和來自特定封裝的參數。 *package_name* 是 **nvarchar(260)**。  
  
 [ @reference_id = ] *reference_id*  
 環境參考的唯一識別碼。 這是選擇性參數。 *reference_id* 是 **bigint**。  
  
## <a name="return-code-value"></a>傳回碼值  
 0 (成功)  
  
## <a name="result-sets"></a>結果集  
 傳回具有下列格式的資料表：  
  
|資料行名稱|資料類型|描述|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|參數類型。 專案參數的值會是 `20`，而封裝參數的值則會是 `30`。|  
|parameter_data_type|**nvarchar(128)**|參數的資料類型。|  
|parameter_name|**sysname**|參數名稱。|  
|parameter_value|**sql_variant**|參數的值。|  
|sensitive|**bit**|當值為 `1` 時，參數值為敏感值。 當值為 `0` 時，參數值則不是敏感值。|  
|必要|**bit**|當值為 `1` 時，必須有參數值才能開始執行。 當值為 `0` 時，不需要參數值即可開始執行。|  
|value_set|**bit**|當值為 `1` 時，表示參數值已指派。 當值為 `0` 時，表示參數值未指派。|  
  
> [!NOTE]  
>  常值會以純文字顯示。 敏感值的位置會顯示 **NULL**。  
  
## <a name="permissions"></a>權限  
 這個預存程序需要下列其中一個權限：  
  
-   專案的 READ 權限，以及 (如果適用的話) 參考環境的 READ 權限  
  
-   **ssis_admin** 資料庫角色的成員資格  
  
-   **系統管理員** 伺服器角色的成員資格  
  
## <a name="errors-and-warnings"></a>錯誤和警告  
 下列清單將描述可能會引發錯誤或警告的某些條件：  
  
-   在指定的資料夾或專案中找不到封裝  
  
-   使用者未具備適當的權限  
  
-   指定的環境參考不存在  
  
  
