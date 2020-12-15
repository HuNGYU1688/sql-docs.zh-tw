---
title: 為您的內部部署資料庫 (Data Migration Assistant) ，找出正確的 Azure SQL Database SKU |Microsoft Docs
description: 瞭解如何使用 Data Migration Assistant 來識別您的內部部署資料庫的正確 Azure SQL Database SKU
ms.custom: ''
ms.date: 05/06/2019
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.technology: dma
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
author: rajeshsetlem
ms.author: rajpo
ms.openlocfilehash: 8d4de2aea0a9020a7f971f36dd011c435a3cec3d
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489494"
---
# <a name="identify-the-right-azure-sql-databasemanaged-instance-sku-for-your-on-premises-database"></a>為您的內部部署資料庫識別正確的 Azure SQL Database/受控執行個體 SKU

將資料庫移轉至雲端可能會很複雜，尤其是當您嘗試為您的資料庫選取最佳的 Azure 資料庫目標和 SKU 時。 使用資料庫 Migration Assistant (DMA) 的目標是要協助解決這些問題，並藉由在使用者易記的輸出中提供這些 SKU 建議，讓您的資料庫移轉體驗更輕鬆。

本文著重于 DMA 的 Azure SQL Database SKU 建議功能。 Azure SQL Database 和 Azure SQL 受控執行個體有數種部署選項，包括：

- 單一資料庫
- 彈性集區
- 受控執行個體

「SKU 建議」功能可讓您根據收集自電腦 () 裝載資料庫的效能計數器，識別 Azure SQL Database 單一資料庫或 Azure SQL 受控執行個體 SKU 的最低建議。 此功能提供定價層、計算層級和最大資料大小的相關建議，以及每月預估成本。 它也可以為所有建議的資料庫大量布建單一資料庫和受控實例。

> [!NOTE]
> 這項功能目前僅可透過命令列介面 (CLI) 取得。

下列指示可協助您判斷 SKU 建議，並在 Azure 中使用 DMA 來布建對應的單一資料庫 () 或受控實例 (s) 。

## <a name="prerequisites"></a>Prerequisites

- 下載並安裝最新版本的 [DMA](https://aka.ms/get-dma)。 如果您已經有舊版的工具，請開啟它，系統就會提示您升級 DMA。
- 確定您的電腦具有 [PowerShell 5.1 版](https://www.microsoft.com/download/details.aspx?id=54616) 或更新版本，才能執行所有腳本。 如需如何找出電腦上所安裝之 PowerShell 版本的相關資訊，請參閱 [下載並安裝 Windows PowerShell 5.1](/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-5-1)。
  > [!NOTE]
  > 為了收集電腦資訊，資料收集腳本會使用已在 PowerShell 6 中淘汰的 Get-WmiObject Cmdlet。 若要在 PowerShell 6 或7中執行此腳本，您必須使用較新的 CIM Cmdlet 來取代 WMI Cmdlet。
- 確定您的電腦已安裝 Azure Powershell 模組。 如需詳細資訊，請參閱 [安裝 Azure PowerShell 課程模組](/powershell/azure/install-az-ps?view=azps-1.8.0&preserve-view=true)一文。
- 確認收集效能計數器所需的 PowerShell 檔案 **SkuRecommendationDataCollectionScript.ps1** 已安裝在 [DMA] 資料夾中。
- 確定您要執行此程式的電腦具有裝載資料庫之電腦的系統管理員許可權。

## <a name="collect-performance-counters"></a>收集效能計數器

此程式的第一個步驟是收集資料庫的效能計數器。 您可以在裝載資料庫的電腦上執行 PowerShell 命令，以收集效能計數器。 DMA 會為您提供此 PowerShell 檔案的複本，但您也可以使用自己的方法，從您的電腦抓取效能計數器。

您不需要針對每個資料庫個別執行此工作。 從電腦收集的效能計數器，可以用來為電腦上裝載的所有資料庫建議 SKU。

1. 在 [DMA] 資料夾中，找出 PowerShell 檔案 SkuRecommendationDataCollectionScript.ps1。 需要此檔案才能收集效能計數器。

    ![[DMA] 資料夾中顯示的 PowerShell 檔案](../dma/media/dma-sku-recommend-data-collection-file.png)

2. 使用下列引數執行 PowerShell 腳本：
    - **ComputerName**：裝載資料庫的電腦名稱稱。
    - **OutputFilePath**：用來儲存所收集計數器的輸出檔案路徑。
    - **CollectionTimeInSeconds**：您要收集效能計數器資料的時間量。 請至少取得40分鐘的效能計數器，以取得有意義的建議。 捕捉的持續時間越長，建議也愈精確。 此外，請確定工作負載正在針對所需的資料庫執行，以提供更精確的建議。
    - **DbConnectionString**：連接字串，指向您要從中收集效能計數器資料的電腦上所裝載的 master 資料庫。

    以下是範例調用：

    ```
    .\SkuRecommendationDataCollectionScript.ps1
     -ComputerName Foobar1
     -OutputFilePath D:\counters2.csv
     -CollectionTimeInSeconds 2400
     -DbConnectionString Server=localhost;Initial Catalog=master;Integrated Security=SSPI;
    ```

    執行命令之後，程式會將檔案（包括效能計數器）輸出到您指定的位置。 您可以使用這個檔案做為下一個程式部分的輸入，這會針對單一資料庫和受控實例選項提供 SKU 建議。

## <a name="use-the-dma-cli-to-get-sku-recommendations"></a>使用 DMA CLI 取得 SKU 建議

使用您為此進程所建立的效能計數器輸出檔案作為輸入。

針對單一資料庫選項，DMA 將提供針對您電腦上每個資料庫的 Azure SQL Database 單一資料庫定價層、計算層級和最大資料大小的建議。 如果您的電腦上有多個資料庫，您也可以指定想要建議的資料庫。 DMA 也會為您提供每個資料庫的每月預估成本。

針對受控實例，建議支援隨即轉移案例。 如此一來，DMA 將為您提供 Azure SQL 受控執行個體定價層、計算層級，以及電腦上一組資料庫的最大資料大小的建議。 同樣地，如果您的電腦上有多個資料庫，您也可以指定想要建議的資料庫。 DMA 也會為您提供受控實例的每月預估成本。

若要使用 DMA CLI 來取得 SKU 建議，請在命令提示字元下，使用下列引數執行 dmacmd.exe：

- **/Action = SkuRecommendation**：輸入此引數以執行 SKU 評定。
- **/SkuRecommendationInputDataFilePath**：上一節中所收集之計數器檔案的路徑。
- **/SkuRecommendationTsvOutputResultsFilePath**：以 TSV 格式寫入輸出結果的路徑。
- **/SkuRecommendationJsonOutputResultsFilePath**：以 JSON 格式寫入輸出結果的路徑。
- **/SkuRecommendationHtmlResultsFilePath**：以 HTML 格式寫入輸出結果的路徑。

此外，請選取下列其中一個引數：

- 避免重新整理價格
  - **/SkuRecommendationPreventPriceRefresh**：如果設為 True，則會防止價格重新整理髮生，並假設為預設價格。 如果以離線模式執行，請使用。 如果您未使用此參數，您必須指定下列參數，以根據指定的區域取得最新價格。
- 取得最新價格
  - **/SkuRecommendationCurrencyCode**：用來顯示價格的貨幣 (例如「USD」 ) 。
  - **/SkuRecommendationOfferName**：供應專案名稱 (例如 "Ms-azr-0003p-ms-azr-0003p" ) 。 如需詳細資訊，請參閱 [Microsoft Azure 供應專案詳細資料](https://azure.microsoft.com/support/legal/offer-details/) 頁面。
    - **/SkuRecommendationRegionName**：區功能變數名稱稱 (例如 "WestUS" ) 。
    - **/SkuRecommendationSubscriptionId**：訂用帳戶識別碼。
    - **/AzureAuthenticationTenantId**：驗證租使用者。
    - **/AzureAuthenticationClientId**：用於驗證的 AAD 應用程式的用戶端識別碼。
    - 下列其中一個驗證選項：
      - 互動式
        - **AzureAuthenticationInteractiveAuthentication**：驗證快顯視窗的設定為 true。
      - 以憑證為基礎
        - **AzureAuthenticationCertificateStoreLocation**：設定為憑證存放區位置 (例如 "CurrentUser" ) 。
        - **AzureAuthenticationCertificateThumbprint**：設定為憑證指紋。
      - 以權杖為基礎
        - **AzureAuthenticationToken**：設定為憑證權杖。

> [!NOTE]
> 若要取得 ClientId 和 TenantId 以進行互動式驗證，您需要設定新的 AAD 應用程式。 如需驗證和取得這些認證的詳細資訊，請 [參閱 Microsoft Azure 計費 api 程式碼範例： RATECARD API](https://github.com/Azure-Samples/billing-dotnet-ratecard-api)中的 **步驟1：在您的 AAD 租使用者中設定原生用戶端應用程式** 中的指示。

最後，您可以使用選擇性的引數來指定想要建議的資料庫： 

- **/SkuRecommendationDatabasesToRecommend**：要提出建議的資料庫清單。 資料庫名稱會區分大小寫，而且必須 (1) 在輸入 .csv 中， (2) 每個都以雙引號括住，而 (3) 每個都是以名稱之間的單一空格分隔 (例如/SkuRecommendationDatabasesToRecommend = "Database1" "Database2" "Database3" ) 。 省略此參數可確保針對輸入 .csv 檔案中識別的所有使用者資料庫提供建議。  

以下是一些範例調用：

**範例1：取得預設價格的建議。以離線模式執行時，或當您沒有驗證認證時，請使用。**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationPreventPriceRefresh=true
```

**範例2：取得具有指定區域最新價格的建議 (例如「UKWest」 ) 。**

```
.\DmaCmd.exe /Action=SkuRecommendation
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv"
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv"
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json"
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html"
/SkuRecommendationCurrencyCode=USD
/SkuRecommendationOfferName=MS-AZR-0044p
/SkuRecommendationRegionName=UKWest
/SkuRecommendationSubscriptionId=<Your Subscription Id>
/AzureAuthenticationInteractiveAuthentication=true
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId>
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

**範例3：取得特定資料庫的建議 (例如 "TPCDS1G，EDW_3G，TPCDS10G" ) 。**

```
.\DmaCmd.exe /Action=SkuRecommendation 
/SkuRecommendationInputDataFilePath="C:\TestOut\out.csv" 
/SkuRecommendationTsvOutputResultsFilePath="C:\TestOut\prices.tsv" 
/SkuRecommendationJsonOutputResultsFilePath="C:\TestOut\prices.json" 
/SkuRecommendationOutputResultsFilePath="C:\TestOut\prices.html" 
/SkuRecommendationCurrencyCode=USD 
/SkuRecommendationOfferName=MS-AZR-0044p 
/SkuRecommendationRegionName=UKWest 
/SkuRecommendationSubscriptionId=<Your Subscription Id> 
/SkuRecommendationDatabasesToRecommend=“TPCDS1G” “EDW_3G” “TPCDS10G” 
/AzureAuthenticationInteractiveAuthentication=true 
/AzureAuthenticationClientId=<Your AzureAuthenticationClientId> 
/AzureAuthenticationTenantId=<Your AzureAuthenticationTenantId>
```

針對單一資料庫建議，TSV 輸出檔案看起來會如下所示：

![DMA 資料夾中顯示的 PowerShell 單一資料庫檔案](../dma/media/dma-sku-recommend-single-db-recommendations.png)

針對受控實例的建議，TSV 輸出檔案看起來會如下所示：

![[DMA] 資料夾中顯示的 PowerShell 受控實例檔案](../dma/media/dma-sku-recommend-mi-recommendations.png)

輸出檔中每個資料行的描述如下。

- **DatabaseName** -您資料庫的名稱。
- **MetricType** -建議的效能層級。
- **MetricValue** -建議的 SKU。
- **PricePerMonth** –相對應 SKU 的每月預估價格。
- **RegionName** –對應 SKU 的地區名稱。 
- **IsTierRecommended** -我們會針對每一層提供最低 SKU 建議。 接著，我們會套用啟發學習法來判斷您資料庫的正確層級。 這會反映建議的資料庫層級。 
- **ExclusionReasons** -如果建議階層，此值為空白。 針對不建議的每一層，我們會提供未挑選它的原因。
- **AppliedRules** -套用之規則的簡短標記法。

最終的建議層 (，也就是 **MetricType**) 和值 (也就是在 **IsTierRecommended** 資料行為 TRUE 的情況下找到 **MetricValue**) -反映您的查詢在 Azure 中執行所需的最低 SKU，與您的內部部署資料庫類似的成功率。 針對 Azure SQL 受控執行個體，DMA 目前支援最常使用的8vcore 來 40vcore Sku 的建議。 例如，如果標準層的建議最小 SKU 為 S4，則選擇 S3 或以下會導致查詢超時或無法執行。

HTML 檔案以圖形格式包含這項資訊。 它提供了方便使用的方法，可讓您查看最終的建議並布建程式的下一個部分。 HTML 輸出的詳細資訊位於下一節。

## <a name="provision-recommended-skus-to-azure"></a>將建議的 Sku 布建至 Azure

只要按幾下滑鼠，您就可以使用識別的建議在 Azure 中布建目標 Sku，以便遷移您的資料庫。 您可以使用 HTML 檔案來輸入 Azure 訂用帳戶;為您的資料庫挑選定價層、計算層級和最大資料大小;並產生腳本來布建您的資料庫。 您可以使用 PowerShell 來執行此腳本。

您可以在單一電腦上執行此程式，也可以在多部電腦上執行，以判斷大規模的 SKU 建議。 DMA 目前透過命令列介面支援整個程式，讓它成為簡單且可擴充的體驗。

若要輸入布建資訊並對建議進行變更，請更新 HTML 檔案，如下所示。

**針對單一資料庫建議**

![Azure SQL Database SKU 建議畫面](../dma/media/dma-sku-recommend-single-db-recommendations1.png)

1. 開啟 HTML 檔案並輸入下列資訊：
    - **訂** 用帳戶識別碼-您想要布建資料庫之 Azure 訂用帳戶的訂用帳戶識別碼。
    - **資源群組** -您想要在其中部署資料庫的資源群組。 輸入存在的資源群組。
    - **區域** ：要在其中布建資料庫的區域。 請確定您的訂用帳戶支援選取區域。
    - **伺服器名稱** ：您要部署資料庫的 Azure SQL Database 伺服器。 如果您輸入的伺服器名稱不存在，則會加以建立。
    - **管理員使用者名稱** -伺服器管理員使用者名稱。
    - **管理員密碼** -伺服器管理員密碼。 密碼長度必須至少為8個字元，且長度不得超過128個字元。 您的密碼必須包含下列三個類別的字元 - 英文大寫字母、英文小寫字母、數字 (0-9) 和非英數字元 (!、$、#、% 等)。 密碼不能包含從使用者名稱) 的全部或部分 (3 + 連續字母。

2. 查看每個資料庫的建議，並視需要修改定價層、計算層級和最大資料大小。 請務必取消選取您目前不想要布建的任何資料庫。

3. 選取 [產生布建 **腳本**]、儲存腳本，然後在 PowerShell 中執行。

    此程式應該會建立您在 [HTML] 頁面中選取的所有資料庫。

**針對 Azure SQL 受控執行個體建議**

![Azure SQL MI SKU 建議畫面](../dma/media/dma-sku-recommend-mi-recommendations1.png)

1. 開啟 HTML 檔案並輸入下列資訊：
    - **訂** 用帳戶識別碼-您想要布建資料庫之 Azure 訂用帳戶的訂用帳戶識別碼。
    - **資源群組** -您想要在其中部署資料庫的資源群組。 輸入存在的資源群組。
    - **區域** ：要在其中布建資料庫的區域。 請確定您的訂用帳戶支援選取區域。
    - **實例名稱** –您想要將資料庫移轉至其中的 Azure SQL 受控執行個體實例。 實例名稱只能包含小寫字母、數位和 '-'，但開頭或結尾不能是 '-' 或超過63個字元。
    - **實例管理員使用者名稱** -實例管理員使用者名稱。 請確定您的登入名稱符合下列需求-它是 SQL 識別碼，不是一般的系統名稱 (例如 admin、administrator、sa、root、dbmanager、loginmanager 等 ) ，或是內建的資料庫使用者或角色 (例如 dbo、guest、public 等 ) 。 請確定您的名稱不包含空白字元、Unicode 字元或非英數位元，而且不是以數位或符號開頭。 
    - **實例管理員密碼** -實例系統管理員密碼。 您的密碼必須至少有16個字元，且長度不得超過128個字元。 您的密碼必須包含下列三個類別的字元 - 英文大寫字母、英文小寫字母、數字 (0-9) 和非英數字元 (!、$、#、% 等)。 密碼不能包含從使用者名稱) 的全部或部分 (3 + 連續字母。
    - **Vnet 名稱** –應布建受控實例的 vnet 名稱。 輸入現有的 VNet 名稱。
    - **子網名稱** –應布建受控實例的子網名稱。 輸入現有的子網名稱。

2. 請檢查每個實例的建議，並視需要修改定價層、計算層級和最大資料大小。 雖然建議目前僅限於8vcore 至 40vcore Sku，但如有需要，仍有提供64vcore 和 80vcore Sku 的選項。 請務必取消選取您目前不想要布建的任何實例。

    此程式應該會建立您在 [HTML] 頁面中選取的所有資料庫。

    > [!NOTE]
    > 在子網上建立受控實例 (特別是在第一次) 可能需要數小時才能完成。 透過 PowerShell 執行布建腳本之後，您可以在 Azure 入口網站上檢查部署的狀態。

## <a name="next-step"></a>後續步驟

- 如需從 CLI 執行 DMA 的完整命令清單，請參閱 [從命令列執行 Data Migration Assistant](./dma-commandline.md)一文。
