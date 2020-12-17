---
title: 產生指令碼
description: 了解如何使用 [產生與發佈指令碼精靈] 來為多個物件建立 Transact-SQL 指令碼，以及如何使用 [物件總管] 中的 [編寫組件的指令碼為] 功能表來為個別物件或多個物件產生指令碼。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 9711c617-3c68-4e5a-aea3-befc64d51524
author: markingmyname
ms.author: maghan
ms.reviewer: mathoma
ms.custom: seo-lt-2019
ms.date: 04/07/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 76d942c643b510f956f5199920c228e4071795e8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97480649"
---
# <a name="generate-scripts-sql-server-management-studio"></a>產生指令碼 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 提供兩種產生 [!INCLUDE[tsql](../../includes/tsql-md.md)] 指令碼的機制。 您可以使用 [產生和發佈指令碼精靈] 來建立多個物件的指令碼。 您也可以使用 **物件總管** 中的 [編寫組件的指令碼為] 功能表，為個別物件或多個物件產生指令碼。

如需使用 SQL Server Management Studio (SSMS) 撰寫各種物件指令碼的詳細教學課程，請參閱[教學課程：在 SSMS 中撰寫指令碼](../tutorials/scripting-ssms.md)。

## <a name="before-you-begin"></a>開始之前

選擇最符合您需求的機制。 

###  <a name="generate-and-publish-scripts-wizard"></a><a name="GenPubScriptWiz"></a> [產生和發佈指令碼精靈]

使用 [產生和發佈指令碼精靈]，為多個物件建立 [!INCLUDE[tsql](../../includes/tsql-md.md)] 指令碼。 此精靈會產生資料庫中所有物件的指令碼，或是您所選取之物件子集的指令碼。 此精靈具有許多適用於指令碼的選項，例如是否要包含權限、定序及條件約束等。 如需有關使用此精靈的指示，請參閱 [產生和發佈指令碼精靈](./generate-and-publish-scripts-wizard.md)。
  
### <a name="object-explorer-script-as-menu"></a><a name="OEScriptAsMenu"></a> 物件總管的 [編寫組件的指令碼為] 功能表

您可以使用物件總管的 [編寫指令碼為] 功能表來編寫單一物件、多個物件或單一物件之多個陳述式的指令碼。 您可以選擇數種指令碼的其中一種，例如建立、變更或卸除物件。 您可以將指令碼儲存到 [查詢編輯器] 視窗，或是儲存到檔案或剪貼簿。 指令碼是使用 Unicode 格式所建立。

## <a name="to-generate-a-script-of-a-single-object"></a><a name="ScriptSingleObject"></a> 產生單一物件的指令碼

**編寫單一物件的指令碼**

1. 在 [物件總管] 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的執行個體，然後展開該執行個體。

2. 展開 [資料庫]，然後展開含有要編寫指令碼之物件的資料庫。

3. 展開物件的類別目錄。 例如，展開 [資料表] 或 [檢視表] 節點。

4. 以滑鼠右鍵按一下物件，然後指向 [產生 \<object type> 的指令碼為]，例如指向 [產生資料表的指令碼為]。

5. 指向指令碼類型，例如 [CREATE 至] 或 [ALTER 至]。

6. 選取儲存指令碼的位置，例如 [新增查詢編輯器視窗] 或 [剪貼簿]。

    ![指令碼資料表](media/generate-scripts-sql-server-management-studio/script-table.png)

您可以使用物件總管的 [詳細資料] 窗格，產生相同類別目錄之多個物件的指令碼。

1. 在 [物件總管] 中，連接到 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的執行個體，然後展開該執行個體。

2. 展開 [資料庫]，然後展開含有要編寫指令碼之物件的資料庫。

3. 展開想要編寫指令碼之物件類型的類別目錄節點，例如 [資料表] 節點。

4. 選取 **F7** 或是開啟 [檢視] 功能表並選取 [物件總管詳細資料]，開啟 [物件總管詳細資料] 窗格。

    ![檢視功能表](media/generate-scripts-sql-server-management-studio/object-explorer-details-view-menu.png)

5. 以滑鼠左鍵按一下您想要編寫指令碼的其中一個物件。

6. Ctrl + 以滑鼠左鍵按一下您想要編寫指令碼的第二個物件。

7. 以滑鼠右鍵按一下其中一個選取的物件，然後選取 [產生 \<object type> 的指令碼為]。

    ![詳細資料](media/generate-scripts-sql-server-management-studio/object-explorer-details.png)