---
title: 設定 Windows 防火牆
description: 了解如何設定 Windows 防火牆，以允許透過防火牆存取 SQL Server 的執行個體。
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- Windows Firewall ports
- WMI firewall ports
- Windows Firewall [Database Engine]
- firewall systems, configuring
- advfirewall
- firewall systems
- rules firewall
- firewall systems, overview and port list
- 1433 TCP port
- portopening using netsh
- ports [SQL Server], TCP
- netsh to open firewall ports
ms.assetid: f55c6a0e-b6bd-4803-b51a-f3a419803024
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1459c50d87f2f7ccc58e20bd7e21d27ace700f66
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96120876"
---
# <a name="configure-the-windows-firewall-to-allow-sql-server-access"></a>Configure the Windows Firewall to Allow SQL Server Access
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

防火牆系統有助於預防未經授權存取電腦資源。 如果防火牆已開啟，但是設定不正確，則嘗試連接至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的行為可能會被封鎖。  
  
若要透過防火牆存取 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的執行個體，您必須在執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 防火牆是 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 的元件。 您也可以安裝來自其他公司的防火牆。 本文討論 Windows 防火牆的設定方法；其中的基本原則也適用於其他防火牆程式。  
  
> [!NOTE]  
>  本文提供防火牆設定的概觀，並且摘要說明 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 管理員有興趣的資訊。 如需防火牆的詳細資訊及授權防火牆資訊，請參閱防火牆文件集，例如 [Windows 防火牆安全性部署指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)。  
  
 使用者熟悉如何管理 **Windows 防火牆**，而且知道自己想要設定的哪些防火牆設定可直接移至更進階的發行項：  
  
-   [設定用於 Database Engine 存取的 Windows 防火牆](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)    
-   [設定 Windows 防火牆以允許 Analysis Services 存取](https://docs.microsoft.com/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)    
-   [設定供報表伺服器存取的防火牆](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)  
  
##  <a name="basic-firewall-information"></a><a name="BKMK_basic"></a> 基本防火牆資訊  
 防火牆的運作方式是檢查內送封包，以及針對一組規則比較這些封包。 如果封包符合規則所規定的標準，則防火牆會將該封包傳遞給 TCP/IP 通訊協定以進行其他處理。 如果封包不符合這些規則所指定的標準，防火牆接著會捨棄該封包，而且如果已啟用記錄，即會在防火牆記錄檔中建立項目。  
  
 允許傳輸的清單是以下列其中一種方式填入：  

- **自動**：當已啟用防火牆的電腦起始通訊時，防火牆就會在清單中建立項目，以便允許回應。 此回應會被視為要求流量，而且不需設定任何項目。 
  
-   **手動**：管理員設定防火牆的例外。 這允許存取您電腦上指定的程式或連接埠。 在此情況下，當此電腦當做伺服器、接聽程式或對等運作時，它就會接受未經要求的內送傳輸。 這是必須完成才能連接至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的組態類型。  
  
 選擇防火牆策略比單獨決定給定的通訊埠應該開啟或關閉更複雜。 為企業設計防火牆策略時，請務必考慮所有適用的規則和組態選項。 本文不會檢閱所有可能的防火牆選項。 我們建議您檢閱下列文件：  
  
 [Windows 防火牆部署指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-deployment-guide)    
 [Windows 防火牆設計指南](/windows/security/threat-protection/windows-firewall/windows-firewall-with-advanced-security-design-guide)    
 [伺服器及網域隔離簡介](/windows/security/threat-protection/windows-firewall/domain-isolation-policy-design)  
  
##  <a name="default-firewall-settings"></a><a name="BKMK_default"></a> 預設防火牆設定  
 規劃防火牆組態的第一個步驟是判斷作業系統之防火牆的目前狀態。 如果作業系統是從舊版升級，先前的防火牆設定可能已經保留下來。 此外，其他管理員或網域中的「群組原則」可能已經變更了防火牆設定。  
  
> [!NOTE]  
>  開啟防火牆將會影響存取此電腦的其他程式，例如檔案及列印共用，以及遠端桌面連接。 調整防火牆設定之前，管理員應該考慮在電腦上執行的所有應用程式。  
  
##  <a name="programs-to-configure-the-firewall"></a><a name="BKMK_programs"></a> 設定防火牆的程式  
您可以使用 **Microsoft Management Console** 或 **netsh** 來設定 Windows 防火牆設定。  

-  **Microsoft Management Console (MMC)**  
  
     [具有進階安全性的 Windows 防火牆] MMC 嵌入式管理單元可讓您設定更多進階的防火牆設定。 這個嵌入式管理單元會以容易使用的方式呈現大部分防火牆選項，而且它會呈現所有防火牆設定檔。 如需詳細資訊，請參閱本文稍後的[使用具有進階安全性的 Windows 防火牆嵌入式管理單元](#BKMK_WF_msc)。  
  
-   **netsh**  
  
     系統管理員可以使用 **netsh.exe** 工具，在命令提示字元中設定及監視 Windows 電腦，或是利用批次檔執行此作業 **。** 藉由使用 **netsh** 工具，您可以指示輸入適當協助程式的內容命令，讓協助程式執行該命令。 協助程式為一動態連結程式庫 (.dll) 檔案，可為一或多項服務、公用程式或通訊協定提供組態、監視與支援，從而延伸 **netsh** 工具的功能。 所有支援 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的作業系統都有防火牆協助程式。 [!INCLUDE[firstref_longhorn](../../includes/firstref-longhorn-md.md)] 也提供進階防火牆協助程式，稱為 **advfirewall**。 本文不會討論使用 **netsh** 的詳細資料。 不過，您可以使用 **netsh** 設定本文所描述的許多組態選項。 例如，在命令提示字元中執行下列指令碼，即可開啟  
  
    ```  
    netsh firewall set portopening protocol = TCP port = 1433 name = SQLPort mode = ENABLE scope = SUBNET profile = CURRENT  
    ```  
  
     針對進階安全性協助程式使用 Windows 防火牆的類似範例：  
  
    ```  
    netsh advfirewall firewall add rule name = SQLPort dir = in protocol = tcp action = allow localport = 1433 remoteip = localsubnet profile = DOMAIN  
    ```  
  
     如需有關 **netsh** 的詳細資訊，請參閱下列連結：  
  
    -   [Netsh 命令語法、內容及格式設定](/windows-server/networking/technologies/netsh/netsh-contexts)    
    -   [How to use the "netsh advfirewall firewall" context instead of the "netsh firewall" context to control Windows Firewall behavior in Windows Server 2008 and in Windows Vista](https://support.microsoft.com/kb/947709) (如何使用 "netsh advfirewall firewall" 內容而非 "netsh firewall" 內容來控制 Windows Server 2008 和 Windows Vista 的 Windows 防火牆行為)    

    
- **針對 Linux**：在 Linux 上，您也必須開啟與您必須存取之服務關聯的連接埠。 不同的 Linux 散發套件與不同的防火牆各有自己的程序。 請參閱 [Red Hat 上的 SQL Server](../../linux/quickstart-install-connect-red-hat.md) 與 [SUSE 上的 SQL Server](../../linux/quickstart-install-connect-suse.md) 兩個範例。 
  
## <a name="ports-used-by-ssnoversion"></a>使用的通訊埠 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
 下列表格可以協助您識別 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]所使用的通訊埠。  
  
###  <a name="ports-used-by-the-database-engine"></a><a name="BKMK_ssde"></a> Ports Used By the Database Engine  
 

根據預設，SQL Server 使用的一般連接埠和相關聯的資料庫引擎服務為：TCP **1433**、**4022**、**135**、**1434**、UDP **1434**。 下表會詳細說明這些連接埠。 具名執行個體會使用[動態連接埠](#BKMK_dynamic_ports)。
 
 下表將列出 [!INCLUDE[ssDE](../../includes/ssde-md.md)]常用的通訊埠。  
  
|狀況|連接埠|註解|  
|--------------|----------|--------------|  
|透過 TCP 執行的預設執行個體|TCP 通訊埠 1433|這是允許通過防火牆最常見的通訊埠。 它適用於 [!INCLUDE[ssDE](../../includes/ssde-md.md)]預設安裝的一般連接，或在電腦上唯一執行之執行個體的具名執行個體 (具名執行個體具有特殊考量。 請參閱本文稍後的[動態通訊埠](#BKMK_dynamic_ports)。)|  
|含預設連接埠的具名執行個體|此 TCP 通訊埠是在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 啟動時決定的動態通訊埠。|請參閱下面「 [動態通訊埠](#BKMK_dynamic_ports)」一節的討論。 當您使用具名執行個體時， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務可能會需要 UDP 通訊埠 1434。|  
| 含固定連接埠的具名執行個體 |管理員所設定的通訊埠編號。|請參閱下面「 [動態通訊埠](#BKMK_dynamic_ports)」一節的討論。|  
|專用管理員連接|TCP 通訊埠 1434 (預設執行個體)。 其他通訊埠則用於具名執行個體。 請檢查錯誤記錄檔，以取得通訊埠編號。|根據預設，系統不會啟用專用管理員連接 (DAC) 的遠端連接。 若要啟用遠端 DAC，請使用介面區組態 Facet。 如需詳細資訊，請參閱＜ [Surface Area Configuration](../../relational-databases/security/surface-area-configuration.md)＞。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務|UDP 通訊埠 1434|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務會接聽具名執行個體的內送連接，並且將對應至該具名執行個體的 TCP 通訊埠編號提供給用戶端。 每當使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的具名執行個體時，通常就會啟動 [!INCLUDE[ssDE](../../includes/ssde-md.md)] Browser 服務。 如果用戶端設定成連接至具名執行個體的特定通訊埠，就不需要啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務。|  
|含 HTTP 端點的執行個體。|可以在建立 HTTP 端點時指定。 預設值為 TCP 通訊埠 80 (用於 CLEAR_PORT 傳輸) 和 443 (用於 SSL_PORT 傳輸)。|用於透過 URL 進行 HTTP 連接。|  
|含 HTTP 端點的預設執行個體 |TCP 通訊埠 443|用於透過 URL 進行 HTTPS 連接。 HTTPS 是使用傳輸層安全性 (TLS) (先前稱為安全通訊端層 (SSL)) 的 HTTP 連線。|  
|[!INCLUDE[ssSB](../../includes/sssb-md.md)]|TCP 通訊埠 4022。 若要確認使用的通訊埠，請執行下列查詢：<br /><br /> `SELECT name, protocol_desc, port, state_desc`<br /><br /> `FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'SERVICE_BROKER'`|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssSB](../../includes/sssb-md.md)]沒有預設連接埠，但這是線上叢書範例中的傳統組態。|  
|資料庫鏡像|管理員所選擇的通訊埠。 若要判斷此通訊埠，請執行下列查詢：<br /><br /> `SELECT name, protocol_desc, port, state_desc FROM sys.tcp_endpoints`<br /><br /> `WHERE type_desc = 'DATABASE_MIRRORING'`|雖然資料庫鏡像沒有預設通訊埠，不過《線上叢書》範例會使用 TCP 通訊埠 5022 或 7022。 請務必避免中斷使用中的鏡像端點，尤其是在具有自動容錯移轉的高安全性模式中。 您的防火牆組態必須避免中斷仲裁。 如需詳細資訊，請參閱 [指定伺服器網路位址 &#40;資料庫鏡像&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)。|  
|複寫|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的複寫連接會使用一般的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 通訊埠 (例如，預設執行個體的 TCP 通訊埠 1433)。<br /><br /> 複寫快照集的 Web 同步處理和 FTP/UNC 存取需要在防火牆上開啟其他通訊埠。 為了將初始資料和結構描述從某個位置傳送至另一個位置，複寫可能會使用 FTP (TCP 通訊埠 21)、透過 HTTP 同步處理 (TCP 通訊埠 80) 或檔案共用。 檔案共用會使用 UDP 通訊埠 137 和 138，以及 TCP 通訊埠 139 (如果使用 NetBIOS)。 檔案共用使用 TCP 通訊埠 445。|若為透過 HTTP 同步處理，複寫會使用 IIS 端點 (其通訊埠可設定，但預設為通訊埠 80)，不過 IIS 處理序會透過預設執行個體的標準通訊埠 (1433) 連接至後端 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。<br /><br /> 在使用 FTP 進行 Web 同步處理期間，FTP 傳送是介於 IIS 與 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 發行者之間，而非介於訂閱者與 IIS 之間。|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] 偵錯工具|TCP 通訊埠 135<br /><br /> 請參閱「 [通訊埠 135 的特殊考量](#BKMK_port_135)」<br /><br /> 可能也需要「 [IPsec](#BKMK_IPsec) 」例外。|如果您正在使用 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]，也必須在 [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] 主機電腦上，將 **Devenv.exe** 加入至「例外」清單並開啟 TCP 通訊埠 135。<br /><br /> 如果您正在使用 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]，也必須在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 主機電腦上，將 **ssms.exe** 加入至「例外」清單並開啟 如需詳細資訊，請參閱 [先設定防火牆規則再執行 TSQL 偵錯工具](../../relational-databases/scripting/configure-firewall-rules-before-running-the-tsql-debugger.md)。|  
  
 如需為 [!INCLUDE[ssDE](../../includes/ssde-md.md)]設定 Windows 防火牆的逐步解說指示，請參閱 [設定用於 Database Engine 存取的 Windows 防火牆](../../database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access.md)。  
  
####  <a name="dynamic-ports"></a><a name="BKMK_dynamic_ports"></a> 動態通訊埠  
 根據預設，具名執行個體 (包括 [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]) 會使用動態通訊埠。 這表示每次 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 啟動時，它就會識別可用的通訊埠並使用該通訊埠編號。 如果具名執行個體是唯一安裝的 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 執行個體，它可能會使用 TCP 通訊埠 1433。 如果安裝了其他 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 執行個體，它可能會使用不同的 TCP 通訊埠。 由於選取的通訊埠可能會在每次 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 啟動時變更，所以很難將防火牆設定成允許存取正確的通訊埠編號。 因此，如果已使用防火牆，我們建議您將 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 重新設定成每次都使用相同的通訊埠編號。 這個通訊埠就稱為固定通訊埠或靜態通訊埠。 如需詳細資訊，請參閱[設定伺服器接聽特定 TCP 通訊埠 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port.md)。  
  
 另一種設定具名執行個體接聽固定通訊埠的方法，是在防火牆中為 **sqlservr.exe** (適用於[!INCLUDE[ssDE](../../includes/ssde-md.md)]) 一類的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 程式建立例外狀況。 雖然這樣很方便，但當您使用具有進階安全性的 Windows 防火牆 MMC 嵌入式管理單元時，通訊埠編號將不會顯示在 [輸入規則]  頁面的 [本機通訊埠]  資料行中。 如此一來可能會讓您更難以稽核哪些通訊埠已開啟。 其他考量是 Service Pack 或累積更新可能會變更 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可執行檔的路徑，因而使防火牆規則失效。  
  
##### <a name="to-add-a-program-exception-to-the-firewall-using-windows-defender-firewall-with-advanced-security"></a>使用具有進階安全性的 Windows Defender 防火牆，將程式例外新增至防火牆
  
1. 從 [開始] 功能表，鍵入 *wf.msc*。 按 Enter 或選取搜尋結果 wf.msc，以開啟 **具有進階安全性的 Windows Defender 防火牆**。
1. 在左窗格中，選取 [輸入規則]  。
1. 在右窗格的 [動作] 下方，選取 [新增規則]。[新增輸入規則精靈] 隨即開啟。
1. 在 [規則類型]  上，選取 [程式]  。 選取 [下一步] 。
1. 在 [程式]  上，選取 [這個程式路徑]  。 選取 [瀏覽]  以找出您的 SQL Server 執行個體。 此程式稱為 sqlservr.exe。 它通常位於：

   `C:\Program Files\Microsoft SQL Server\MSSQL15.<InstanceName>\MSSQL\Binn\sqlservr.exe`

   選取 [下一步] 。

1. 在 [動作] 上，選取 [允許連線]。 選取 [下一步] 。
1. 在 [設定檔] 上，加入所有三個設定檔。 選取 [下一步]  。
1. 在 [名稱]  上，輸入規則的名稱。 選取 [完成]  。

如需端點的詳細資訊，請參閱[設定 Database Engine 接聽多個 TCP 通訊埠](../../database-engine/configure-windows/configure-the-database-engine-to-listen-on-multiple-tcp-ports.md)和[端點目錄檢視 &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/endpoints-catalog-views-transact-sql.md)。 
  
###  <a name="ports-used-by-analysis-services"></a><a name="BKMK_ssas"></a> Analysis Services 所使用的通訊埠  
 
根據預設，SQL Server Analysis Services 使用的一般連接埠和相關聯的服務為：TCP **2382**、**2383**、**80**、**443**。 下表會詳細說明這些連接埠。  
 
 下表將列出 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]常用的通訊埠。  
  
|功能|連接埠|註解|  
|-------------|----------|--------------|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]|TCP 通訊埠 2383 (預設執行個體)|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]預設執行個體的標準通訊埠。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務|TCP 通訊埠 2382 (只有 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 具名執行個體需要)|沒有指定通訊埠編號之 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 具名執行個體的用戶端連接要求會被導向至通訊埠 2382，亦即 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 所接聽的通訊埠。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 會將要求重新導向至具名執行個體所使用的通訊埠。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 設定成可透過 IIS/HTTP 使用<br /><br /> (PivotTable® 服務會使用 HTTP 或 HTTPS)|TCP 通訊埠 80|用於透過 URL 進行 HTTP 連接。|  
|[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] 設定成可透過 IIS/HTTPS 使用<br /><br /> (PivotTable® 服務會使用 HTTP 或 HTTPS)|TCP 通訊埠 443|用於透過 URL 進行 HTTPS 連接。 HTTPS 是使用 TLS 的 HTTP 連線。|  
  
 如果使用者透過 IIS 和網際網路存取 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ，您就必須開啟 IIS 所接聽的通訊埠，並且在用戶端連接字串中指定該通訊埠。 在此情況下，針對直接存取 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]，則不必開啟任何通訊埠。 不過，您應該限制預設通訊埠 2389 和通訊埠 2382 與所有不需要的其他通訊埠。  
  
 如需設定 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的 Windows 防火牆之逐步解說指示，請參閱 [Configure the Windows Firewall to Allow Analysis Services Access](https://docs.microsoft.com/analysis-services/instances/configure-the-windows-firewall-to-allow-analysis-services-access)(設定 Windows 防火牆以允許 Analysis Services 存取)。  
  
###  <a name="ports-used-by-reporting-services"></a><a name="BKMK_ssrs"></a> Reporting Services 所使用的通訊埠  

根據預設，SQL Server Reporting SErvices 使用的一般連接埠和相關聯的服務為：TCP **80**、**443**。 下表會詳細說明這些連接埠。 


下表將列出 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]常用的通訊埠。  
  
|功能|連接埠|註解|  
|-------------|----------|--------------|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Web 服務|TCP 通訊埠 80|用於透過 URL 進行 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 的 HTTP 連接。 建議您不要使用預先設定的規則 **World Wide Web 服務 (HTTP)** 。 如需詳細資訊，請參閱下面的「 [與其他防火牆規則的互動](#BKMK_other_rules) 」一節。|  
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 設定成可透過 HTTPS 使用|TCP 通訊埠 443|用於透過 URL 進行 HTTPS 連接。 HTTPS 是使用 TLS 的 HTTP 連線。 建議您不要使用預先設定的規則 **Secure World Wide Web 服務 (HTTPS)** 。 如需詳細資訊，請參閱下面的「 [與其他防火牆規則的互動](#BKMK_other_rules) 」一節。|  
  
當 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 連接至 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 或 [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]的執行個體時，您也必須針對這些服務開啟適當的通訊埠。 如需設定 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]Windows 防火牆的逐步指示， [Configure a Firewall for Report Server Access](../../reporting-services/report-server/configure-a-firewall-for-report-server-access.md)(設定報表伺服器存取的防火牆)。  
  
###  <a name="ports-used-by-integration-services"></a><a name="BKMK_ssis"></a> Integration Services 所使用的通訊埠  
 下表將列出 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服務所使用的通訊埠。  
  
|功能|連接埠|註解|  
|-------------|----------|--------------|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 遠端程序呼叫 (MS RPC)<br /><br /> 由 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 執行階段所使用。|TCP 通訊埠 135<br /><br /> 請參閱「 [通訊埠 135 的特殊考量](#BKMK_port_135)」|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服務會在通訊埠 135 上使用 DCOM。 服務控制管理員會使用通訊埠 135 來執行一些工作，例如啟動和停止 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服務，以及將控制要求傳送至執行中服務。 您無法變更此通訊埠編號。<br /><br /> 只有當您要從 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 或自訂應用程式連接至 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 服務的遠端執行個體時，才需要開啟這個通訊埠。|  
  
如需針對 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 設定「Windows 防火牆」的逐步指示，請參閱 [Integration Services 服務 &#40;SSIS Service&#41;](/previous-versions/sql/sql-server-2012/ms137861(v=sql.110))。  
  
###  <a name="additional-ports-and-services"></a><a name="BKMK_additional_ports"></a> 其他通訊埠和服務  
下表將列出 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可能會相依的通訊埠和服務。  
  
|狀況|連接埠|註解|  
|--------------|----------|--------------|  
|Windows Management Instrumentation<br /><br /> 如需有關 WMI 的詳細資訊，請參閱＜ [WMI Provider for Configuration Management Concepts](../../relational-databases/wmi-provider-configuration/wmi-provider-for-configuration-management.md)＞。|WMI 會使用透過 DCOM 所指派的通訊埠，當做共用服務主機執行。 WMI 可能正在使用 TCP 通訊埠 135。<br /><br /> 請參閱「 [通訊埠 135 的特殊考量](#BKMK_port_135)」|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員會使用 WMI 來列出並管理服務。 建議您使用預先設定的規則群組 **Windows Management Instrumentation (WMI)** 。 如需詳細資訊，請參閱下面的「 [與其他防火牆規則的互動](#BKMK_other_rules) 」一節。|  
|[!INCLUDE[msCoName](../../includes/msconame-md.md)] 分散式交易協調器 (MS DTC)|TCP 通訊埠 135<br /><br /> 請參閱「 [通訊埠 135 的特殊考量](#BKMK_port_135)」|如果應用程式使用分散式交易，您可能必須將防火牆設定成允許 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 分散式交易協調器 (MS DTC) 傳輸在個別 MS DTC 執行個體之間，以及在 MS DTC 與資源管理員 (例如 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 之間流動。 我們建議您使用預先設定的規則群組 **[分散式交易協調器]** 。<br /><br /> 針對個別資源群組中的整個叢集設定單一共用 MS DTC 時，您應該將 sqlservr.exe 當成例外新增至防火牆。|  
|[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中的瀏覽按鈕會使用 UDP 來連接至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser 服務。 如需詳細資訊，請參閱 [SQL Server Browser 服務 &#40;Database Engine and SSAS&#41;](../../database-engine/configure-windows/sql-server-browser-service-database-engine-and-ssas.md)。|UDP 通訊埠 1434|UDP 是一種無連接的通訊協定。<br /><br /> 防火牆有一個設定 ([INetFwProfile 介面的 UnicastResponsesToMulticastBroadcastDisabled 屬性](https://go.microsoft.com/fwlink/?LinkId=118371) \(英文\))，可在廣播 (或多點傳送) UDP 要求的單點傳送回應方面控制防火牆的行為。  它有兩種行為：<br /><br /> 如果此設定為 TRUE，就完全不允許廣播的任何單點傳送回應。 列舉服務將會失敗。<br /><br /> 如果此設定為 FALSE (預設值)，就允許單點傳送回應 3 秒。 您無法設定時間的長度。 在擁塞或高延遲的網路中，或針對負載繁重的伺服器，嘗試列舉 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的執行個體可能會傳回部分清單，因而誤導使用者。|  
|<a name="BKMK_IPsec"></a> IPsec 傳輸|UDP 通訊埠 500 和 UDP 通訊埠 4500|如果網域原則要求透過 IPsec 完成網路通訊，您也必須將 UDP 通訊埠 4500 和 UDP 通訊埠 500 加入至例外清單。 IPsec 是在 Windows 防火牆嵌入式管理單元中使用 [新增輸入規則精靈]  的選項。 如需詳細資訊，請參閱稍後的 [使用具有進階安全性嵌入式管理單元的 Windows 防火牆](#BKMK_WF_msc) 。|  
|使用 Windows 驗證搭配信任的網域|防火牆必須設定成允許驗證要求。|如需詳細資訊，請參閱＜ [如何設定網域和信任的防火牆](https://support.microsoft.com/kb/179442/)＞。|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 叢集|叢集需要與 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]沒有直接相關的其他通訊埠。|如需詳細資訊，請參閱＜ [啟用可供叢集使用的網路](https://go.microsoft.com/fwlink/?LinkId=118372)＞。|  
|保留在 HTTP 伺服器 API (HTTP.SYS) 中的 URL 命名空間|可能是 TCP 通訊埠 80，但是可以設定成其他通訊埠。 如需一般資訊，請參閱＜ [設定 HTTP 和 HTTPS](https://go.microsoft.com/fwlink/?LinkId=118373)＞。|如需有關使用 HttpCfg.exe 保留 HTTP.SYS 端點的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 特定資訊，請參閱[關於 URL 保留項目和註冊 &#40;SSRS 組態管理員&#41;](../../reporting-services/install-windows/about-url-reservations-and-registration-ssrs-configuration-manager.md)。|  
  
##  <a name="special-considerations-for-port-135"></a><a name="BKMK_port_135"></a> 通訊埠 135 的特殊考量  
 當您使用 RPC 搭配 TCP/IP 或 UDP/IP 當做傳輸時，系統通常會視需要以動態方式將傳入通訊埠指派給系統服務。所使用的是大於通訊埠 1024 的 TCP/IP 和 UDP/IP 通訊埠。 這些通訊埠通常非正式地稱為「隨機 RPC 通訊埠」。 在這些情況下，RPC 用戶端會仰賴 RPC 端點對應程式來告知它們哪些動態通訊埠已經指派給伺服器。 對於某些以 RPC 為基礎的服務而言，您可以設定特定通訊埠，而非讓 RPC 以動態方式指派通訊埠。 不論服務為何，您都可以將 RPC 以動態方式指派的通訊埠範圍限制成小範圍。 由於連接埠 135 用於許多服務，所以它經常會受到惡意使用者的攻擊。 開啟通訊埠  
  
 如需有關通訊埠 135 的詳細資訊，請參閱下列參考：  
  
-   [Windows Server 系統的服務概觀和網路通訊埠需求 (機器翻譯)](https://support.microsoft.com/kb/832017)   
-   [使用產品光碟的 Windows Server 2003 支援工具對 RPC 端點對應程式錯誤進行疑難排解](https://support.microsoft.com/kb/839880)  
-   [遠端程序呼叫 (RPC)](https://go.microsoft.com/fwlink/?LinkId=118375)    
-   [如何設定 RPC 動態連接埠配置以使用防火牆](https://support.microsoft.com/kb/154596/)  
  
##  <a name="interaction-with-other-firewall-rules"></a><a name="BKMK_other_rules"></a> 與其他防火牆規則的互動  
 [Windows 防火牆] 會使用規則和規則群組來建立其組態。 每個規則或規則群組通常會與特定的程式或服務相關聯，而且該程式或服務可能會不經通知而修改或刪除該項規則。 例如，規則群組 **World Wide Web 服務 (HTTP)** 和 **World Wide Web 服務 (HTTPS)** 會與 IIS 相關聯。 啟用這些規則將會開啟通訊埠 80 和 443，而且如果您啟用了這些規則，相依於通訊埠 80 和 443 的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 功能將會正常運作。 不過，設定 IIS 的管理員可能會修改或停用這些規則。 因此，如果您要針對 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]使用通訊埠 80 或通訊埠 443，就應該建立維護所需通訊埠組態的自訂規則或規則群組 (獨立於其他 IIS 規則)。  
  
 [具有進階安全性的 Windows 防火牆] MMC 嵌入式管理單元允許符合任何適用允許規則的任何傳輸。 因此，如果有兩項同時套用至通訊埠 80 的規則 (具有不同的參數)，系統就會允許符合任何一項規則的傳輸。 所以，如果其中一項規則允許來自區域子網路而且透過通訊埠 80 的傳輸，而另一項規則允許來自任何位址的傳輸，其結果就是允許通訊埠 80 的所有傳輸，不論來源為何。 若要有效管理 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的存取權，管理員應該定期檢閱在伺服器上啟用的所有防火牆規則。  
  
##  <a name="overview-of-firewall-profiles"></a><a name="BKMK_profiles"></a> 防火牆設定檔的概觀  
 作業系統會使用防火牆設定檔，在連線能力、連線和類別方面識別並記憶它們所連線的每個網路。  
  
 [具有進階安全性的 Windows 防火牆] 具有三種網路位置類型：  
  
-   **定義域**：Windows 可以針對電腦所加入的網域，驗證網域控制站的存取權。
-   **公用**：除了網域網路以外，所有網路一開始都會分類成公用。 代表直接連接至網際網路或位於公共場所 (例如機場和咖啡店) 的網路應該會保持公用。
-   **私人**：由使用者或應用程式識別成私人的網路。 只有受信任的網路才應該識別成私人網路。 使用者可能會想要將家庭或小型企業網路識別成私人。  
  
 管理員可以針對每種網路位置類型建立設定檔，而且每個設定檔都含有不同的防火牆原則。 不過，一次只會套用一個設定檔。 設定檔的套用順序如下所示：  
  
1.  如果所有介面都經過電腦屬於其成員之網域的網域控制站驗證，就會套用網域設定檔。    
2.  如果所有介面都經過網域控制站驗證或連接至分類成私人網路位置的網路，就會套用私人設定檔。    
3.  否則，就會套用公用設定檔。  
  
 您可以使用 [具有進階安全性的 Windows 防火牆] MMC 嵌入式管理單元來檢視和設定所有防火牆設定檔。 [控制台] 中的 **[Windows 防火牆]** 項目只會設定目前的設定檔。  
  
##  <a name="additional-firewall-settings-using-the-windows-firewall-item-in-control-panel"></a><a name="BKMK_additional_settings"></a> 使用控制台中的 Windows 防火牆項目進行其他防火牆設定  
 您加入至防火牆的例外可以限制針對來自特定電腦或區域子網路的內送連接開啟通訊埠。 這種通訊埠開啟範圍的限制可以減少電腦遭受惡意使用者攻擊的風險，而且建議使用這種限制。  
  
> [!NOTE]  
>  使用 [控制台] 中的 [Windows 防火牆]  項目只會設定目前的防火牆設定檔。  
  
### <a name="to-change-the-scope-of-a-firewall-exception-using-the-windows-firewall-item-in-control-panel"></a>使用控制台中的 Windows 防火牆項目來變更防火牆例外的範圍  
  
1.  在 [控制台] 的 **[Windows 防火牆]** 中，選取 **[例外]** 索引標籤上的程式或通訊埠，然後按一下 **[內容]** 或 **[編輯]** 。  
  
2.  在 **[編輯程式]** 或 **[編輯連接埠]** 對話方塊中，按一下 **[變更領域]** 。  
  
3.  選擇下列其中一個選項：  
  
    -   **任何電腦 (包括在網際網路上的)** ：不建議使用。 這個選項會允許可設定您電腦位址的任何電腦連接至指定的程式或通訊埠。 雖然您可能需要這項設定才能將資訊呈現給網際網路上的匿名使用者，不過這樣做會增加遭受惡意使用者攻擊的風險。 如果您啟用了這項設定，而且也允許網路位址轉譯 (NAT) 周遊 (例如 [允許邊緣周遊] 選項)，就會進一步增加遭受攻擊的風險。  
  
    -   **只有我的網路 (子網路)** ：這是比 **[任何電腦]** 更安全的設定。 只有位於網路之區域子網路的電腦才能連接至程式或通訊埠。  
  
    -   **自訂清單**：只有具備已列出 IP 位址的電腦才能連線。 這可能是比 [只有我的網路 (子網路)]  更安全的設定。不過，使用 DHCP 的用戶端電腦可能偶爾會變更其 IP 位址。 接著，預期的電腦將無法連接。 您不想要授權的其他電腦可能會接受列出的 IP 位址，然後就能夠連接。 [自訂清單]  選項可能適用於列出已設定為使用固定 IP 位址的其他伺服器；不過，入侵者可能會假冒這些 IP 位址。 限制性防火牆規則只會與您的網路基礎結構同樣強固。  
  
##  <a name="using-the-windows-firewall-with-advanced-security-snap-in"></a><a name="BKMK_WF_msc"></a> 使用具有進階安全性的 Windows 防火牆嵌入式管理單元  
 您可以使用具有進階安全性的 Windows 防火牆 MMC 嵌入式管理單元來設定其他進階的防火牆設定。 此嵌入式管理單元包含規則精靈，而且它會公開 [控制台] 中 [Windows 防火牆]  項目無法使用的額外設定。 這些設定包括：  
  
-   加密設定  
-   服務限制   
-   依據名稱限制電腦的連接    
-   限制特定使用者或設定檔的連接  
-   允許傳輸通過網路位址轉譯 (NAT) 路由器的邊緣周遊    
-   設定輸出規則    
-   設定安全性規則    
-   針對內送連接要求 IPsec  
  
### <a name="to-create-a-new-firewall-rule-using-the-new-rule-wizard"></a>使用新增規則精靈來建立新的防火牆規則  
  
1.  在 [開始] 功能表上，選取 [執行]  、輸入 **WF.msc**，然後選取 [確定]  。    
2.  在 [具有進階安全性的 Windows 防火牆]  的左窗格中，以滑鼠右鍵按一下 [輸入規則]  ，然後選取 [新增規則]  。   
3.  使用您想要的設定來完成 **[新增輸入規則精靈]** 。  
  
##  <a name="troubleshooting-firewall-settings"></a><a name="BKMK_troubleshooting"></a> 疑難排解防火牆設定  
 下列工具和技巧可用於疑難排解防火牆問題：  
  
-   有效的通訊埠狀態是與該通訊埠相關之所有規則的聯集。 嘗試封鎖透過某個連接埠存取時，檢閱引用連接埠號碼的所有規則可能很實用。 若要這樣做，請使用 [具有進階安全性的 Windows 防火牆] MMC 嵌入式管理單元，然後依據通訊埠編號來排序輸入和輸出規則。  
  
-   檢閱在執行 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之電腦上作用中的通訊埠。 此檢閱程序包括確認哪些是接聽的 TCP/IP 通訊埠，並確認通訊埠的狀態。  
  
     若要驗證接聽的通訊埠，請使用 **netstat** 命令列公用程式。 除了顯示使用中的 TCP 連接之外， **netstat** 公用程式也會顯示各種 IP 統計資料與資訊。  
  
    #### <a name="to-list-which-tcpip-ports-are-listening"></a>列出哪些是接聽的 TCP/IP 通訊埠  
  
    1.  開啟命令提示字元視窗。  
  
    2.  請在命令提示字元之下，輸入 **netstat -n -a**。  
  
         **-n** 參數會指示 **netstat** 以數值方式顯示使用中 TCP 連線的位址與通訊埠號碼。 **-a** 參數會指示 **netstat** 顯示電腦所接聽之電腦上的 TCP 與 UDP 通訊埠。  
  
-   **PortQry** 公用程式可用於將 TCP/IP 通訊埠的狀態回報為接聽中、未接聽或已篩選。 (若為已篩選狀態，表示通訊埠不一定是接聽中。此狀態會指出公用程式未接收到通訊埠的回應)。**PortQry** 公用程式可從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=17148)下載。  
  
## <a name="see-also"></a>另請參閱  
 [Windows Server 系統的服務概觀和網路通訊埠需求 (機器翻譯)](https://support.microsoft.com/kb/832017)   
 [操作說明：設定防火牆設定 (Azure SQL Database)](https://azure.microsoft.com/documentation/articles/sql-database-configure-firewall-settings/)  
  
  
