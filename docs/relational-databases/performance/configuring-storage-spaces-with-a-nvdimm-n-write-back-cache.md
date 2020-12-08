---
title: 設定儲存空間 - NVDIMM-N 回寫式快取
description: 了解如何設定具有鏡像 NVDIMM N 回寫式快取的鏡像儲存空間，以作為虛擬磁碟機來儲存 SQL Server 交易記錄。
ms.custom: seo-dt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 861862fa-9900-4ec0-9494-9874ef52ce65
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8cde267305ee7e7666443d0cf9c7f96dee92d072
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505336"
---
# <a name="configuring-storage-spaces-with-a-nvdimm-n-write-back-cache"></a>設定具有 NVDIMM-N 回寫式快取的儲存空間
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Windows Server 2016 支援 NVDIMM-N 裝置，以大幅加快輸入/輸出 (I/O) 作業的速度。 這類裝置一個吸引人的用法是作為回寫式快取，以取得低寫入延遲。 本主題討論如何設定具有鏡像 NVDIMM N 回寫式快取的鏡像儲存空間，以作為虛擬磁碟機，來儲存 SQL Server 交易記錄。 如果您也想使用它來儲存資料表或其他資料，您可以將更多磁碟加入存放集區，或建立多個集區 (若隔離很重要)。  
  
 若要檢視使用這項技術的 Channel 9 影片，請參閱 [Using Non-volatile Memory (NVDIMM-N) as Block Storage in Windows Server 2016](https://channel9.msdn.com/Events/Build/2016/P466)(在 Windows Server 2016 中使用靜態記憶體 (NVDIMM-N) 作為區塊存放裝置)。  
  
## <a name="identifying-the-right-disks"></a>識別正確的磁碟  
 在 Windows Server 2016 中設定儲存空間，特別是使用進階功能 (例如回寫式快取)，最簡單的方法是透過 PowerShell。 第一個步驟是識別應該作為儲存空間集區一部分的磁碟，此集區會是建立虛擬磁碟的來源。 NVDIMM-N 的媒體類型和匯流排類型為 SCM (儲存類別記憶體)，可透過 Get-PhysicalDisk PowerShell Cmdlet 來查詢。  
  
```  
Get-PhysicalDisk | Select FriendlyName, MediaType, BusType  
```  
  
 ![Windows Powershell 視窗的螢幕擷取畫面，其中顯示 Get-PhysicalDisk Cmdlet 的輸出。](../../relational-databases/performance/media/get-physicaldisk.png "Get-PhysicalDisk")  
  
> [!NOTE]  
>  使用 NVDIMM-N 裝置時，您不再需要明確選取可作為回寫式快取目標的裝置。  
  
 為了建立具有鏡像回寫式快取的鏡像虛擬磁碟，至少需要 2 部 NVDIMM-N 和 2 個其他磁碟。 將所需的實體磁碟指派給變數再建立集區可簡化程序。  
  
```  
$pd =  Get-PhysicalDisk | Select FriendlyName, MediaType, BusType | WHere-Object {$_.FriendlyName -like 'MK0*' -or $_.FriendlyName -like '2c80*'}  
```  
  
 此螢幕擷取畫面顯示 $pd 變數，以及使用下列 PowerShell Cmdlet 傳回之指派給該變數的 2 個 SSD 和 2 部 NVDIMM-N。  
  
```  
$pd | Select FriendlyName, MediaType, BusType  
```  
  
 ![Windows Powershell 視窗的螢幕擷取畫面，其中顯示 $pd Cmdlet 的輸出。](../../relational-databases/performance/media/select-friendlyname.png "選取 FriendlyName")  
  
## <a name="creating-the-storage-pool"></a>建立存放集區  
 您可以透過包含 PhysicalDisks 的 $pd 變數，輕鬆地使用 New-StoragePool PowerShell Cmdlet 建立存放集區。  
  
```  
New-StoragePool -StorageSubSystemFriendlyName "Windows Storage*" -FriendlyName NVDIMM_Pool -PhysicalDisks $pd  
```  
  
 ![Windows Powershell 視窗的螢幕擷取畫面，其中顯示 New-StoragePool Cmdlet 的輸出。](../../relational-databases/performance/media/new-storagepool.png "New-StoragePool")  
  
## <a name="creating-the-virtual-disk-and-volume"></a>建立虛擬磁碟和磁碟區  
 建立集區之後，下一個步驟是對虛擬磁碟進行切割及格式化。 在本例中，只會建立一個虛擬磁碟，而且可以使用 New-Volume PowerShell Cmdlet 簡化此程序：  
  
```  
New-Volume -StoragePool (Get-StoragePool -FriendlyName NVDIMM_Pool) -FriendlyName Log_Space -Size 300GB -FileSystem NTFS -AccessPath S: -ResiliencySettingName Mirror  
```  
  
 ![Windows Powershell 視窗的螢幕擷取畫面，其中顯示 New-Volume Cmdlet 的輸出。](../../relational-databases/performance/media/new-volume.png "New-Volume")  
  
 虛擬磁碟已建立、初始化並以 NTFS 格式化。 下列螢幕擷取畫面顯示它具有 300 GB 的大小與 1 GB 的寫入快取大小，並將裝載於 NVDIMM-N。  
  
 ![Windows Powershell 視窗的螢幕擷取畫面，其中顯示 Get-VirtualDisk Cmdlet 的輸出。](../../relational-databases/performance/media/get-virtualdisk.png "Get-VirtualDisk")  
  
 您現在可以檢視伺服器中顯示的這個新磁碟區。 您現在可以使用此磁碟機來儲存 SQL Server 交易記錄。  
  
 ![[這部電腦] 頁面上檔案總管視窗的螢幕擷取畫面，其中顯示 Log_Space 磁碟機。](../../relational-databases/performance/media/log-space-drive.png "Log_Space Drive")  
  
## <a name="see-also"></a>另請參閱  
 [Windows 10 中的 Windows 儲存空間](https://windows.microsoft.com/windows-10/storage-spaces-windows-10)   
 [Windows 2012 R2 中的 Windows 儲存空間](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831739(v=ws.11))   
 [交易記錄 &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)   
 [檢視或變更資料及記錄檔的預設位置 &#40;SQL Server Management Studio&#41;](../../database-engine/configure-windows/view-or-change-the-default-locations-for-data-and-log-files.md)  
  
