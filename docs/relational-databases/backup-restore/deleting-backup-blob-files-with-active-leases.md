---
title: 刪除擁有使用中租用的備份 Blob 檔案 | Microsoft 文件
description: 如果 SQL Server 備份或還原失敗，則 Azure 儲存體中的 Blob 可能會成為孤立。 了解如何刪除孤立的 Blob。
ms.custom: ''
ms.date: 08/17/2017
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 13a8f879-274f-4934-a722-b4677fc9a782
author: cawrites
ms.author: chadam
ms.openlocfilehash: df3d0b503ab61d00d098a47ef0bb2a85c32dc99f
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96127000"
---
# <a name="delete-backup-blob-files-with-active-leases"></a>刪除擁有使用中租用的備份 Blob 檔案

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

備份至 Microsoft Azure 儲存體或從中還原時，SQL Server 會取得無限期租用，以鎖定 Blob 的獨佔存取權。 當備份或還原程序順利完成時，就會釋放租用。 如果備份或還原失敗，備份程序會嘗試清除任何無效的 Blob。 不過，如果由於過長或持續性網路連線失敗而無法備份，備份程序可能無法存取 Blob，而該 Blob 可能仍然是被遺棄狀態。 這表示在釋放租用之前，無法寫入或刪除 Blob。 本主題描述如何釋放 (中斷) 租用及刪除 Blob。
  
如需租用類型的詳細資訊，請閱讀這篇[文章](/rest/api/storageservices/Lease-Blob)。  
  
如果備份作業失敗，可能會產生無效的備份檔案。 備份 Blob 檔案可能也擁有使用中租用，導致無法刪除或覆寫此檔案。 若要刪除或覆寫這些 Blob，必須先釋放 (中斷) 租用。 如果備份失敗，建議您清除租用及刪除 Blob。 您也可以在儲存體管理工作之中，定期清除租用及刪除 Blob。  
  
如果還原失敗，系統並不會封鎖後續還原，因此使用中的租用就不會是問題。 只有當您必須覆寫或刪除 Blob 時，才需要中斷租用。  
  
## <a name="manage-orphaned-blobs"></a>管理孤立的 Blob

下列步驟將說明如何在備份或還原活動失敗之後進行清除。 您可以使用 PowerShell 指令碼來完成所有步驟。 下一個章節包含範例 PowerShell 指令碼：  
  
1. **識別擁有租用的 Blob：** 如果您有執行備份程序的指令碼或處理序，就可以在指令碼或處理序內部擷取失敗，並將其用於清除 Blob。  您也可以使用 LeaseStats 和 LeastState 屬性來識別擁租用的 Blob。 識別出 Blob 之後，請在刪除 Blob 之前先驗證備份檔案的有效性。  
  
1. **中斷租用：** 授權的要求不必提供租用識別碼就可以中斷租用。 如需詳細資訊，請參閱 [此處](/rest/api/storageservices/Lease-Blob) 。  
  
    > [!TIP]  
    > SQL Server 會發出租用識別碼，以便在還原作業期間確立獨佔存取權。 還原租用識別碼為 BAC2BAC2BAC2BAC2BAC2BAC2BAC2BAC2。  
  
1. **刪除 Blob：** 若要刪除擁有使用中租用的 Blob，您必須先中斷租用。  

###  <a name="powershell-script-example"></a><a name="Code_Example"></a> PowerShell 指令碼範例  
  
> [!IMPORTANT]
> 如果您執行的是 PowerShell 2.0，載入 Microsoft WindowsAzure.Storage.dll 組件時可能會發生問題。 建議您升級 [PowerShell](/powershell/) 以解決這項問題。 您也可以使用下列因應措施來建立或修改 powershell.exe.config 檔案，以便在執行階段載入 .NET 2.0 和 .NET 4.0 組件：  
>
> ```xml
> <?xml version="1.0"?>
>     <configuration>
>         <startup useLegacyV2RuntimeActivationPolicy="true">
>             <supportedRuntime version="v4.0.30319"/>
>             <supportedRuntime version="v2.0.50727"/>
>         </startup>
>     </configuration>  
> ```  
  
 下列範例指令碼會識別擁有使用中租用的 Blob，並予以中斷。 此範例也會示範如何篩選釋放租用識別碼。  
  
#### <a name="tips-on-running-this-script"></a>執行這個指令碼的秘訣
  
> [!WARNING]  
> 如果 Azure Blob 儲存體服務的備份與此指令碼同時執行，備份可能會失敗，因為此指令碼會中斷備份嘗試同時取得的租用。 建議在維護時段或未執行或安排備份時執行此指令碼。  
  
- 執行此指令碼之前，您應新增儲存體帳戶、儲存體金鑰、容器與 Azure 儲存體組件路徑的值，以及名稱參數。 儲存體的路徑是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]執行個體的安裝目錄。 儲存體組件的檔案名稱是 Microsoft.WindowsAzure.Storage.dll。
  
- 如果沒有任何鎖定租用的 Blob，您應該會看見下列訊息：`There are no blobs with locked lease status`
  
- 如果有鎖定租用的 Blob，您應該會看見下列訊息：`Breaking Leases`、`The lease on <URL of the Blob> is a restore lease: You will see this message only if you have a blob with a restore lease that is still active.`，以及 `The lease on <URL of the Blob> is not a restore lease Breaking lease on <URL of the Bob>.`
  
```powershell
$storageAccount = "<myStorageAccount>"
$storageKey = "<myStorageKey>"
$blobContainer = "<myBlobContainer>"
$storageAssemblyPathName = "<myStorageAssemblyPathName>"
  
# well known Restore Lease ID  
$restoreLeaseId = "BAC2BAC2BAC2BAC2BAC2BAC2BAC2BAC2"  
  
# load the storage assembly without locking the file for the duration of the PowerShell session  
$bytes = [System.IO.File]::ReadAllBytes($storageAssemblyPath)  
[System.Reflection.Assembly]::Load($bytes)  
  
$cred = New-Object 'Microsoft.WindowsAzure.Storage.Auth.StorageCredentials' $storageAccount, $storageKey  
$client = New-Object 'Microsoft.WindowsAzure.Storage.Blob.CloudBlobClient' "https://$storageAccount.blob.core.windows.net", $cred  
$container = $client.GetContainerReference($blobContainer)  
  
# list all the blobs  
$blobs = $container.ListBlobs($null,$true)
  
# filter blobs that are have Lease Status as "locked"
$lockedBlobs = @()  
foreach($blob in $blobs)  
{  
    $blobProperties = $blob.Properties
    if($blobProperties.LeaseStatus -eq "Locked")  
    {  
        $lockedBlobs += $blob  
    }  
}  

if($lockedBlobs.Count -gt 0)  
{  
    Write-Host "Breaking leases..."
    foreach($blob in $lockedBlobs )
    {  
        try  
        {  
            $blob.AcquireLease($null, $restoreLeaseId, $null, $null, $null)  
            Write-Host "The lease on $($blob.Uri) is a restore lease."  
        }  
        catch [Microsoft.WindowsAzure.Storage.StorageException]  
        {  
            if($_.Exception.RequestInformation.HttpStatusCode -eq 409)  
            {  
                Write-Host "The lease on $($blob.Uri) is not a restore lease."  
            }  
        }  
  
        Write-Host "Breaking lease on $($blob.Uri)."  
        $blob.BreakLease($(New-TimeSpan), $null, $null, $null) | Out-Null  
    }  
} else { Write-Host " There are no blobs with locked lease status." }
```  
  
## <a name="see-also"></a>另請參閱

[SQL Server 備份至 URL 的最佳做法和疑難排解](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)