---
description: catalog.set_folder_description (SSISDB 資料庫)
title: catalog.set_folder_description (SSISDB 資料庫) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 802416f6-5177-4db5-bca5-976dec5faf53
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 18e7cb22f94e33328ed08968052d03b0261c73df
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96129645"
---
# <a name="catalogset_folder_description-ssisdb-database"></a>catalog.set_folder_description (SSISDB 資料庫)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  設定 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目錄中資料夾的描述。  
  
## <a name="syntax"></a>語法  
  
```sql  
catalog.set_folder_description [ @folder_name = ] folder_name  
    , [ @folder_description = ] folder_description  
```  
  
## <a name="arguments"></a>引數  
 [ @folder_name = ] *folder_name*  
 資料夾的名稱。 *folder_name* 是 **nvarchar(128)** 。  
  
 [ @folder_description = ] *folder_description*  
 資料夾的描述。 *folder_description* 是 **nvarchar(MAX)**。  
  
## <a name="return-code-value"></a>傳回碼值  
 無  
  
## <a name="result-sets"></a>結果集  
 None  
  
## <a name="permissions"></a>權限  
 這個預存程序需要下列其中一個權限：  
  
-   **ssis_admin** 資料庫角色的成員資格  
  
-   **系統管理員** 伺服器角色的成員資格  
  
## <a name="errors-and-warnings"></a>錯誤和警告  
 預存程序會傳回訊息，以確認新資料夾描述的設定。  
  
  
