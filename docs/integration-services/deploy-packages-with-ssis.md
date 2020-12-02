---
description: 使用 SSIS 部署封裝
title: 使用 SSIS 部署套件 | Microsoft Docs
ms.custom: ''
ms.date: 08/20/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: quickstart
helpviewer_keywords:
- deployment tutorial [Integration Services]
- deploying packages [Integration Services]
- SSIS, tutorials
- Integration Services, tutorials
- deploying packages [Integration Services], installing
- SQL Server Integration Services, tutorials
- walkthroughs [Integration Services]
- deployment utility [Integration Services]
- deploying packages [Integration Services], configurations
ms.assetid: de18468c-cff3-48f4-99ec-6863610e5886
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d82aae4ee0195adca300d16bf9f2a2217c40a38c
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123324"
---
# <a name="deploy-packages-with-ssis"></a>使用 SSIS 部署封裝

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


[!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 提供數個工具，可讓您輕鬆地將套件部署到另一部電腦。 部署工具也可以用來管理任何相依性，例如封裝所需的組態和檔案。 在這個教學課程中，您會學到如何使用這些工具，將封裝及其相依性安裝到目標電腦上。    
    
首先，您會執行一些部署的準備工作。 您會在 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 中建立一個新的 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)] 專案，並且將現有的封裝和資料檔加入至該專案中。 您不需要從頭開始建立新的封裝，而是使用針對這個教學課程所建立的已完成的封裝。 您在這個教學課程中並不會修改封裝的功能，不過，在您將封裝加入至專案之後，若能在 [ [!INCLUDE[ssIS](../includes/ssis-md.md)] 設計師] 中開啟封裝並檢閱各個封裝的內容，可能會很有幫助。 因為您可以藉由檢查封裝，而了解封裝的相依性 (例如記錄檔) 以及封裝的其他有趣功能。    
    
在為部署做準備時，您還要更新封裝以使用組態。 組態會使封裝和封裝物件的屬性，在執行階段變成可更新的狀態。 在這個教學課程中，您會使用組態來更新記錄檔和文字檔的連接字串，以及封裝所使用之 XML 和 XSD 檔案的位置。 如需詳細資訊，請參閱 [封裝組態](./packages/legacy-package-deployment-ssis.md) 和 [建立封裝組態](./packages/legacy-package-deployment-ssis.md)。    
    
當您確認封裝可以在 [!INCLUDE[ssBIDevStudioFull](../includes/ssbidevstudiofull-md.md)]中順利執行之後，就要建立用來安裝封裝的部署配套。 這個部署配套將會包含您已加入至 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 專案中的封裝檔案和其他項目、 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 自動納入的封裝相依性，以及您所建立的部署公用程式。 如需詳細資訊，請參閱 [建立部署公用程式](./packages/legacy-package-deployment-ssis.md)。    
    
接下來，您會將部署配套複製到目標電腦上，然後執行「封裝安裝精靈」來安裝封裝和封裝相依性。 封裝將會安裝在 msdb SQL Server 資料庫中，而支援檔案和輔助檔案則會安裝在檔案系統中。 由於部署的封裝會使用組態，因此您要更新組態使用新值，才能讓封裝在新的環境中順利執行。    
    
最後，您會使用「執行封裝公用程式」在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中執行封裝。    
    
這個教學課程的目標是，模擬在實際部署時可能遇到的各種問題的複雜性。 但是，如果您無法將封裝部署到其他電腦上，仍然可以進行這個教學課程，只要將封裝安裝在 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]本機執行個體上的 msdb 資料庫中，然後從本機執行個體上的 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 執行封裝就可以了。    

**完成這個教學課程的估計時間：** 2 小時

## <a name="what-you-learn"></a>學習內容    
若要熟悉 [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 所提供的新工具、控制項和功能，最好的方法就是使用它們。 這個教學課程會逐步解說各個步驟，教您建立 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 專案，然後將封裝和其他必要檔案加入至專案中。 當專案完成之後，您還要建立部署配套、將部署配套複製到目的地電腦，然後將封裝安裝到目的地電腦上。    
    
## <a name="prerequisites"></a>必要條件    
本教學課程的主要對象是已經熟悉基本檔案系統作業，但對於 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 所提供新功能較為陌生的使用者。 為了進一步了解在這個教學課程中所要用到的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 基本概念，若能先完成下列 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 教學課程，將會很有幫助： [SSIS 如何建立 ETL 封裝](../integration-services/ssis-how-to-create-an-etl-package.md)。    
    
### <a name="on-the-source-computer"></a>在來源電腦上

要用來建立部署配套的電腦 **必須安裝下列元件：**

- SQL Server。 (從 [SQL Server 下載](https://www.microsoft.com/sql-server/sql-server-downloads)來下載免費評估或開發人員版本的 SQL Server。)

- 範例資料、完成的套件、組態和讀我檔案。 若要將範例資料與課程套件下載為 ZIP 檔案，請參閱 [SQL Server Integration Services Tutorial Files](https://www.microsoft.com/download/details.aspx?id=56827) (SQL Server Integration Services 教學課程檔案)。 ZIP 檔案中的檔案大部分都是唯讀，以避免不小心變更。 若要將輸出寫入至檔案或變更它，您可能必須關閉檔案屬性中的唯讀屬性。

-   **AdventureWorks2014** 範例資料庫。 若要下載 **AdventureWorks2014** 資料庫，請從 [AdventureWorks 範例資料庫](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)下載 `AdventureWorks2014.bak`，並還原備份。  

-   您必須具有在 AdventureWorks 資料庫中建立和卸除資料表的權限。
    
-   [SQL Server Data Tools (SSDT)](../ssdt/download-sql-server-data-tools-ssdt.md) 。    
    
### <a name="on-the-destination-computer"></a>在目的電腦上

要在其中部署套件的電腦 **必須安裝下列元件：**    
    
- SQL Server。 (從 [SQL Server 下載](https://www.microsoft.com/sql-server/sql-server-downloads)來下載免費評估或開發人員版本的 SQL Server。)

- 範例資料、完成的套件、組態和讀我檔案。 若要將範例資料與課程套件下載為 ZIP 檔案，請參閱 [SQL Server Integration Services Tutorial Files](https://www.microsoft.com/download/details.aspx?id=56827) (SQL Server Integration Services 教學課程檔案)。 ZIP 檔案中的檔案大部分都是唯讀，以避免不小心變更。 若要將輸出寫入至檔案或變更它，您可能必須關閉檔案屬性中的唯讀屬性。

-   **AdventureWorks2014** 範例資料庫。 若要下載 **AdventureWorks2014** 資料庫，請從 [AdventureWorks 範例資料庫](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks)下載 `AdventureWorks2014.bak`，並還原備份。  
    
- [SQL Server Management Studio](../ssms/download-sql-server-management-studio-ssms.md)。    
    
-   [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)]. 若要安裝 SSIS，請參閱[安裝 Integration Services](install-windows/install-integration-services.md)。
    
-   您必須具有在 AdventureWorks 資料庫中建立和卸除資料表的權限，以及在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中執行 SSIS 套件的權限。    
    
-   您必須具有 `msdb` [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 系統資料庫中 `sysssispackages` 資料表的讀取和寫入權限。    
    
如果您計畫將封裝部署到建立部署配套時所使用的同一部電腦，則該部電腦必須同時符合來源電腦和目的地電腦的需求。    
        
## <a name="lessons-in-this-tutorial"></a>本教學課程中的課程    
[第 1 課：準備建立部署配套](../integration-services/lesson-1-preparing-to-create-the-deployment-bundle.md)    
在這一課中，您會建立一個新的 [!INCLUDE[ssISnoversion](../includes/ssisnoversion-md.md)] 專案，並且將封裝和其他必要檔案加入至專案中，以開始部署 ETL 方案。    
    
[第 2 課：在 SSIS 中建立部署配套](../integration-services/lesson-2-create-the-deployment-bundle-in-ssis.md)    
在這一課中，您會建立部署公用程式，並且確認部署配套包含所有必要的檔案。    
    
[第 3 課：安裝 SSIS 套件](../integration-services/lesson-3-install-ssis-packages.md)    
在這一課中，您會將部署配套複製到目標電腦上、安裝封裝，然後執行封裝。    
