---
title: 資料庫引擎服務啟動選項 | Microsoft Docs
description: 熟悉 SQL Server 資料庫引擎啟動選項。 檢視如何使用這些選項的秘訣，並了解每個選項的用途。
ms.custom: ''
ms.date: 08/01/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- single-user mode [SQL Server], startup option
- overriding default startup options
- minimal configuration mode [SQL Server], startup option
- default startup options
- temporarily override default startup options [SQL Server]
- startup options [SQL Server]
- starting SQL Server, options
- single-user mode [SQL Server], startup parameter
- overriding default startup parameters
- minimal configuration mode [SQL Server], startup parameter
- default startup parameters
- temporarily override default startup parameters [SQL Server]
- startup parameters [SQL Server]
- starting SQL Server, parameters
ms.assetid: d373298b-f6cf-458a-849d-7083ecb54ef5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 614d630ae8e3ac85baaecb78b4232af7e6e687a0
ms.sourcegitcommit: 644223c40af7168f9d618526e9f4cd24e115d1db
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96328048"
---
# <a name="database-engine-service-startup-options"></a>Database Engine 服務啟動選項

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

啟動選項指定啟動期間所需的特定檔案位置，並指定一些整個伺服器範圍的條件。 除非您疑難排解 [!INCLUDE[ssDE](../../includes/ssde-md.md)] ，或是發生異常問題而「 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 客戶支援」指示您使用啟動選項，否則大部分使用者都不需要指定啟動選項。  
  
> [!WARNING]  
>  不正確使用啟動選項，可能會影響伺服器效能，而且可能會導致 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 無法啟動。  
>
>  以 "mssql" 使用者身分啟動 Linux 上的 SQL Server，以免未來發生啟動問題。 範例： `sudo -u mssql /opt/mssql/bin/sqlservr [STARTUP OPTIONS]` 
  
## <a name="about-startup-options"></a>關於啟動選項  
 安裝 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]時，安裝程式會在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 登錄中寫入一組預設啟動選項。 您可使用這些啟動選項來指定替代 master 資料庫檔案、master 資料庫記錄檔或錯誤記錄檔。 如果 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 找不到必要檔案，就不會啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
 您可以使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員來設定啟動選項。 如需相關資訊，請參閱[設定伺服器啟動選項 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)。  
  
## <a name="list-of-startup-options"></a>啟動選項清單  
### <a name="default-startup-options"></a>預設啟動選項  

|選項。|描述|  
|-----------------------------|-----------------|  
|**-d**  *master_file_path*|master 資料庫檔案的完整路徑 (通常是 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\master.mdf)。 如果不提供這個選項，會使用現有的登錄參數。|  
|**-e**  *error_log_path*|這是錯誤記錄檔的完整路徑 (通常是 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\LOG\ERRORLOG)。 如果不提供這個選項，會使用現有的登錄參數。|  
|**-l**  *master_log_path*|master 資料庫記錄檔的完整路徑 (通常是 C:\Program Files\Microsoft SQL Server\MSSQL.*n*\MSSQL\Data\mastlog.ldf)。 如果不指定這個選項，就會使用現有的登錄參數。|  
  
### <a name="other-startup-options"></a>其他啟動選項   

|選項。 |描述|   
|---------------------------|-----------------|  
|**-c**|縮短從命令提示字元啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的啟動時間。 一般而言， [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 會呼叫「服務控制管理員」，以服務方式啟動。 因為 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 從命令提示字元啟動時不會以服務方式啟動，所以請使用 **-c** 略過這個步驟。|  
|**-f**|啟動只含最小組態的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體。 如果組態值設定 (如過度調配記憶體) 造成伺服器無法啟動，這就很有用。 以最低組態模式啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 會將 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 放在單一使用者模式下。 如需詳細資訊，請參閱後續的 **-m** 描述。|  
|**-kDecimalNumber**| 這個啟動參數會限制每秒的檢查點 I/O 要求數目，其中 **DecimalNumber** 代表每秒的檢查點速度 (MB)。  變更此值可能會影響備份的速度或進行復原程序，因此請謹慎執行。 也就是說，如果針對參數指定非常低的值，則可能會經歷較長的復原時間，且備份也可能需要稍長時間才能完成，因為備份起始的檢查點程序也延遲了。 您可不使用此參數，改用下列方法協助消除系統的 I/O 瓶頸：<br /> - 提供適當的硬體來維持 SQL Server 所張貼的 I/O 要求 <br /> - 執行足夠的應用程式微調 | 
|**-m**|在單一使用者模式中啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體。 以單一使用者模式啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體時，只有單一使用者可以進行連接，且不會啟動 CHECKPOINT 處理序。 CHECKPOINT 會保證將交易規律地從磁碟快取區寫到資料庫裝置。 (一般而言，如果遇到一些應該修復系統資料庫的問題時，就會使用這個選項)。這個選項會啟用 sp_configure allow updates 選項。 根據預設，allow updates 是停用的。 在單一使用者模式下啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可讓電腦本機管理員群組的任何成員以 sysadmin 固定伺服器角色的成員身分，連接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的執行個體。 如需詳細資訊，請參閱 [當系統管理員遭到鎖定時連接到 SQL Server](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)。如需單一使用者模式的詳細資訊，請參閱 [以單一使用者模式啟動 SQL Server](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)。|  
|**-m用戶端應用程式名稱**|限制與所指定用戶端應用程式的連接。 例如， `-mSQLCMD`  會將連接限制為單一連接，而且該連接必須將自己識別為 SQLCMD 用戶端程式。 當您在單一使用者模式下啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 而且有未知的用戶端應用程式佔用唯一可用的連接時，請使用這個選項。 使用 `"Microsoft SQL Server Management Studio - Query"` 與 SSMS 查詢編輯器連接。 SSMS 查詢編輯器選項無法透過 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] Configuration Manager 進行設定，因為它包含此工具拒絕的虛線字元。<br /><br /> 用戶端應用程式名稱區分大小寫。 如果應用程式名稱包含空格或特殊字元，則需要以雙引號括住。<br /><br />**從命令列啟動時的範例：**<br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlservr -s MSSQLSERVER -m"Microsoft SQL Server Management Studio - Query"` <br /><br />`C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\sqlservr -s MSSQLSERVER -mSQLCMD` <br /><br /> **安全性注意事項：** 請勿將這個選項當做安全性功能使用。 用戶端應用程式會提供用戶端應用程式名稱，而且可能會在連接字串中提供假的名稱。|  
|**-n**|請不要使用 Windows 應用程式記錄檔來記錄 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。 若您使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -n **啟動** 執行個體，建議您同時使用 **-e** 啟動選項。 否則，系統不會記錄 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 事件。|  
|**-s**|可讓您啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的具名執行個體。 若未設定 **-s** 參數，則會嘗試啟動預設執行個體。 您必須先在命令提示字元處切換至該執行個體的適當 BINN 目錄，才能啟動 **sqlservr.exe**。 例如，如果 Instance1 的二進位檔原本要使用 `\mssql$Instance1`，使用者就必須位於 `\mssql$Instance1\binn` 目錄才能啟動 **sqlservr.exe -s instance1**。|  
|**-T**  *trace#*|指出啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 執行個體時，應該已啟用指定的追蹤旗標 (*trace#* )。 追蹤旗標用來啟動具有非標準行為的伺服器。 如需詳細資訊，請參閱[追蹤旗標 &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)。<br /><br /> **重要：** 指定具有 **-T** 選項的追蹤旗標時，請使用大寫 "T" 來傳送追蹤旗標號碼。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]會接受小寫 "t"，但這會設定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 支援工程師才需要的其他內部追蹤旗標 (不會讀取控制台啟動視窗中所指定的參數)。|  
|**-x**|停用下列監視功能：<br /> - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 效能監視器計數器<br /> - 保留 CPU 時間及快取命中比率統計資料<br /> - 收集 DBCC SQLPERF 命令的資訊<br /> - 收集某些動態管理檢視的資訊<br /> - 許多擴充事件的事件點<br /><br /> **警告︰** 當您使用 **-x** 啟動選項時，就會大幅減少可讓您用來診斷 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 之效能與運作問題的資訊。|  
|**-E**|增加針對檔案群組內每一個檔案所配置的範圍數。 這個選項對於有限制執行索引或資料掃描之使用者數目的資料倉儲應用程式可能會很有幫助。 其他應用程式內不應該使用這個選項，因為它對於效能可能有負面影響。 32 位元的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]版本中不支援這個選項。|  
  
## <a name="using-startup-options-for-troubleshooting"></a>使用啟動選項進行疑難排解  
 有些啟動選項 (例如單一使用者模式和最低組態模式) 主要是在疑難排解期間使用。 手動啟動 sqlservr.exe 時，使用 **-m** 或 **-f** 選項來啟動伺服器進行疑難排解是在命令列中最容易的作業。  
  
> [!NOTE]  
>  使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] net start **啟動** 時，啟動選項會使用斜線 (/)，而非連字號 (-)。  
  
## <a name="using-startup-options-during-normal-operations"></a>在正常作業期間使用啟動選項  
 在您每次啟動 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]時，可能都會想要使用一些啟動選項。 使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 組態管理員來設定啟動參數，很容易就可以完成這些選項 (例如，使用追蹤旗標來啟動)。 這些工具會將啟動選項儲存成登錄機碼，這樣 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 就一定會使用這些啟動選項來啟動。  
  
## <a name="compatibility-support"></a>相容性支援  

如需已從先前版本移除的選項，請參閱 [ 應用程式](../../tools/sqlservr-application.md#compatibility-support)。

## <a name="related-tasks"></a>相關工作  
[設定 scan for startup procs 伺服器組態選項](../../database-engine/configure-windows/configure-the-scan-for-startup-procs-server-configuration-option.md)  
[啟動、停止、暫停、繼續、重新啟動資料庫引擎、SQL Server Agent 或 SQL Server Browser 服務](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)
[設定伺服器啟動選項 &#40;SQL Server 組態管理員&#41;](../../database-engine/configure-windows/scm-services-configure-server-startup-options.md)
  
## <a name="see-also"></a>另請參閱  
 [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)   
 [sqlservr 應用程式](../../tools/sqlservr-application.md)  
  
  
