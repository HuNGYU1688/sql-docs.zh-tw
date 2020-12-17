---
title: SQL Server 屬性 (進階索引標籤)
description: 了解 [SQL Server 屬性] 對話方塊中 [進階] 索引標籤上的選項，例如資料路徑、執行個體識別碼，以及自訂屬性。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 2ffd10fd-bac1-478f-9cff-96ed6c8b787f
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: ab2a821d2c7d14f16caefb7f881f69962b3eaa94
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465769"
---
# <a name="sql-server-properties-advanced-tab"></a>SQL Server 屬性 (進階索引標籤)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  下列屬性預設會出現在 **[進階]** 索引標籤。 如果定義了自訂屬性，這些屬性與其值也會在這個索引標籤上顯示。  
  
## <a name="options"></a>選項。  
 **叢集**  
 指出這項服務是否安裝為叢集伺服器的資源。  
  
 **客戶回函報表**  
 指出是否已在這項服務上啟用「服務品質監控」。 如需有關客戶回函報表的詳細資訊，請搜尋「線上叢書」的＜錯誤報告和使用方式報告設定＞主題。  
  
 **資料路徑**  
 顯示這個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝之 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]二進位編碼檔案的路徑。  
  
 **傾印目錄**  
 顯示在發生錯誤時放置記憶體傾印的位置。  
  
 **錯誤報告**  
 若設定為 [是]，一旦發生嚴重錯誤，Dr. Watson 程式會將資訊轉送至 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 或您的錯誤伺服器。 如需錯誤報表的詳細資訊，請搜尋《線上叢書》的＜錯誤報告和使用方式報告設定＞主題。 若要變更此值，請在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 物件總管中，在伺服器上按一下滑鼠右鍵，然後依序按一下 [屬性] **和 [其他伺服器設定]** 頁面。 這個選項會出現在 **[資訊報告]** 區域中。  
  
 **檔案版本**  
 顯示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可執行檔的版本。  
  
 **安裝路徑**  
 顯示這個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安裝之 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]二進位編碼檔案的路徑。  
  
 **執行個體識別碼**  
 指出使用此服務的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體。  
  
 **語言**  
 顯示伺服器訊息的預設語言。  
  
 **登錄根目錄**  
 顯示此應用程式使用之登錄機碼的位置。  
  
 **Service Pack 層級**  
 顯示這個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體的 Service Pack 層級。  
  
 **SKU 名稱**  
 顯示產品存貨保持單元 (SKU)，有時也稱作產品版本 (product edition)。  
  
 **啟動參數**  
 列出這個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體所使用的任何啟動參數。 參數是由分號區隔開來的。 預設的參數包含 master 資料庫 (`master.mdf`)、master 資料庫的記錄檔 (`mastlog.ldf`) 以及錯誤記錄檔的資料檔案路徑。 如需啟動參數的語法，請搜尋《線上叢書》的 **＜使用 SQL 伺服器啟動選項＞** 主題。  
  
 **存貨保持單元**  
 顯示產品存貨保持單元 (SKU) 號碼。  
  
 **版本**  
 顯示這個 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體的版本號碼。  
  
 **虛擬伺服器名稱**  
 當 **安裝在叢集伺服器時的** 虛擬伺服器名稱 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
  
