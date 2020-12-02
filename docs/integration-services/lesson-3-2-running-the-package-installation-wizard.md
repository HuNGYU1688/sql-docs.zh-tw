---
description: 課程 3-2 - 執行套件安裝精靈
title: 步驟 2：執行套件安裝精靈 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: tutorial
ms.assetid: f91fbb89-4626-4c47-b96d-56052dc45861
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f30bc07edc2d6d513eb9078e1758caf99a2822cf
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88472045"
---
# <a name="lesson-3-2---running-the-package-installation-wizard"></a>課程 3-2 - 執行套件安裝精靈

[!INCLUDE[sqlserver-ssis](../includes/applies-to-version/sqlserver-ssis.md)]


在這項工作中，您會執行「封裝安裝精靈」，將「部署教學課程」專案中的封裝部署到 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]的執行個體上。 只有封裝可以安裝在 msdb [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 資料庫的 sysssispackages 資料表中，部署配套所包含的支援檔案則會部署到檔案系統中。  
  
「封裝安裝精靈」會引導您完成安裝和設定封裝的步驟。 您會將封裝安裝到目的地電腦 (即複製部署配套所使用的電腦) 上的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 執行個體中。 此外，您還會建立一個 C:\DeploymentTutorialInstall 資料夾，讓精靈用來安裝非封裝檔案。  
  
在先前的課程中，您已將教學課程中的封裝修改為使用組態。 因此，您將會使用「封裝安裝精靈」編輯這些組態，讓封裝能夠在安裝的目標環境中順利執行。  
  
### <a name="to-install-the-packages"></a>若要安裝封裝  
  
1.  在目的地電腦上，找到部署配套。  
  
    如果您使用預設值 (bin\Deployment) 作為部署公用程式的位置，則部署配套就是「部署教學課程」專案中的 [Deployment] 資料夾。  
  
2.  在 [Deployment] 資料夾中，按兩下資訊清單檔 Deployment Tutorial.SSISDeploymentManifest。  
  
3.  在 [封裝安裝精靈] 的 [歡迎使用] 頁面上，按一下 [下一步]。  
  
4.  在 [部署 SSIS 封裝] 頁面上，選取 [SQL Server 部署] 選項，再選取 [安裝之後驗證封裝] 核取方塊，然後按一下 [下一步]。  
  
5.  在 [指定目標 SQL Server] 頁面上的 [伺服器名稱] 方塊中，指定 **(local)**。  
  
6.  如果 SQL Server 的執行個體支援 Windows 驗證，請選取 [使用 Windows 驗證]，否則請選取 [使用 SQL Server 驗證] 並提供使用者名稱和密碼。  
  
7.  確認已清除 [依賴伺服器儲存體進行加密] 核取方塊。  
  
8.  按 **[下一步]**。  
  
9. 在 [選取安裝資料夾] 頁面上，按一下 [瀏覽]。  
  
10. 在 [瀏覽資料夾] 對話方塊中，展開 [我的電腦]，然後按一下 [本機磁碟 (C:)]。  
  
11. 按一下 [建立新資料夾]，並且以 **DeploymentTutorialInstall** 取代新資料夾的 **新增資料夾** 預設名稱。  
  
    > [!IMPORTANT]  
    > 在組態使用的環境變數值中會參考這個名稱， 因此資料夾和參考的名稱必須相符，否則封裝無法執行。  
  
12. 按一下 [確定]。  
  
13. 在 [選取安裝資料夾] 頁面上，確認 [資料夾] 方塊中包含的是 **C:\DeploymentTutorialInstall**，然後按一下 [下一步]。  
  
14. 在 [確認安裝] 頁面上，按一下 [下一步]。  
  
    精靈便會安裝封裝。 當安裝完成之後，[設定封裝] 頁面隨即開啟。  
  
15. 在 [設定封裝] 頁面上，確認 [設定檔] 方塊中有列出 datatransferconfig.dtsconfig 和 loadxmldataconfig.dtsconfig。  
  
16. 在 [設定檔] 清單中，按一下 **datatransferconfig.dtsconfig**、展開 [設定] 方塊中 [路徑] 資料行的 [屬性]，並以下列值更新 [值] 資料行：  
  
    |屬性|值|更新的值|  
    |------------|---------|-----------------|  
    |\Package.Connections[Deployment Tutorial Log].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Completed Packages\Deployment Tutorial Log|C:\DeploymentTutorialInstall\Deployment Tutorial Log|  
    |\Package.Connections[NewCustomers].Properties[ConnectionString]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\NewCustomers.txt|C:\DeploymentTutorialInstall\NewCustomers.txt|  
  
17. 在 [設定檔] 清單中，按一下 loadxmldataconfig.dtsconfig、展開 [設定] 方塊中 [路徑] 資料行的 [屬性]，並以下列值更新 [值] 資料行：  
  
    |屬性|值|更新的值|  
    |------------|---------|-----------------|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLData]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xml|C:\DeploymentTutorialInstall\orders.xml|  
    |\Package.LoadXMLData.Properties[[XML Source].[XMLSchemaDefinition]]|C:\Program Files\Microsoft SQL Server\100\Samples\Integration Services\Tutorial\Deploying Packages\Sample Data\orders.xsd|C:\DeploymentTutorialInstall\orders.xsd|  
  
18. 在 [封裝驗證] 頁面上，檢視所安裝之各個封裝的驗證結果，然後按一下 [下一步]。  
  
    由於目的地電腦上的環境變數值與開發電腦上的環境變數值不同，因此 [封裝驗證] 頁面上會出現一些警告。 您應該會看到下列這四個警告：  
  
    -   組態檔名稱 "C:\DeploymentTutorial\DataTransferConfig.dtsConfig" 無效。 請檢查組態檔名稱。  
  
    -   無法載入封裝至少其中一個組態項目。 請檢查組態項目和之前的警告，查看哪個組態失敗的描述。  
  
    -   組態檔名稱 "C:\DeploymentTutorial\LoadXMLDataConfig.dtsConfig" 無效。 請檢查組態檔名稱。  
  
    -   無法載入封裝至少其中一個組態項目。 請檢查組態項目和之前的警告，查看哪個組態失敗的描述。  
  
    這些警告並不會影響封裝安裝。  
  
    如果未選取 [部署 SSIS 封裝] 頁面上的 [安裝之後驗證封裝] 選項，則 [封裝驗證] 頁面就不會開啟，而且精靈不會顯示有關驗證的後續安裝資訊。  
  
19. 閱讀 [完成封裝安裝精靈] 頁面上的安裝摘要，然後按一下 [完成]。  
  
    > [!NOTE]  
    > 暫存記錄檔是為了在封裝驗證中使用而建立的， 執行封裝時，並不會使用這個檔案。  
  
## <a name="next-task-in-lesson"></a>本課程的下一項工作  
[步驟 3：測試部署的封裝](../integration-services/lesson-3-3-testing-the-deployed-packages.md)  
  
## <a name="see-also"></a>另請參閱  
[Integration Services 服務 &#40;SSIS 服務&#41;](../integration-services/service/integration-services-service-ssis-service.md)  
