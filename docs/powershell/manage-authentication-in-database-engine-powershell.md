---
title: 在 PowerShell 中管理 SQL Server 的驗證
description: 了解如何在連線至資料庫引擎的執行個體時，使用 SQL Server 驗證，而非 Windows 驗證 (預設值)。
titleSuffix: SQL Server on Linux
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: ab9212a6-6628-4f08-a38c-d3156e05ddea
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 10/14/2020
ms.openlocfilehash: 28369cdd9f2336e9666f65bbaa99b13a31c77d13
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489798"
---
# <a name="manage-authentication-to-sql-server-in-powershell"></a>在 PowerShell 中管理 SQL Server 的驗證

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

依預設，連接至 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 的執行個體時， [!INCLUDE[ssDE](../includes/ssde-md.md)]PowerShell 元件會使用 Windows 驗證。 藉由定義 PowerShell 虛擬磁碟機，或指定 **Invoke-Sqlcmd** 的 **-Username** 和 **-Password** 參數，即可使用 SQL Server 驗證。

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

## <a name="permissions"></a>權限

您在 [!INCLUDE[ssDE](../includes/ssde-md.md)] 的執行個體中可執行的所有動作，都是透過授與用來連接至執行個體之驗證認證的權限所控制。 依預設， [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供者和 Cmdlet 會使用用來建立 [!INCLUDE[ssDE](../includes/ssde-md.md)]之 Windows 驗證連接的 Windows 帳戶。  

若要進行 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 驗證連接，您必須提供 SQL Server 驗證登入識別碼和密碼。 使用 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 提供者時，您必須將 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 登入認證與虛擬磁碟機產生關聯，然後使用變更目錄命令 (**cd**) 連接到該磁碟機。 在 Windows PowerShell 中，安全性認證只能與虛擬磁碟機產生關聯。  

## <a name="sql-server-authentication-using-a-virtual-drive"></a>使用虛擬磁碟機的 SQL Server 驗證

### <a name="to-create-a-virtual-drive-associated-with-a-sql-server-authentication-login"></a>建立與 SQL Server 驗證登入相關聯的虛擬磁碟機

1. 建立的函數：

    1. 具有參數可表示提供給虛擬磁碟機的名稱、登入識別碼，以及與虛擬磁碟機相關聯的提供者路徑。

    2. 使用 **read-host** 提示使用者輸入密碼。  

    3. 使用 **new-object** 建立認證物件。  

    4. 使用 **new-psdrive** 建立具有已提供認證的虛擬磁碟機。  

2. 呼叫此函數，以建立具有已提供認證的虛擬磁碟機。  

#### <a name="example-virtual-drive"></a>範例 (虛擬磁碟機)

此範例會建立名為 **sqldrive** 的函數，可讓您用來建立與指定之 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 驗證登入和執行個體相關聯的虛擬磁碟機。  
  
 **sqldrive** 函數會提示您輸入登入的密碼，並且在您輸入時遮罩密碼。 然後，每當您使用變更目錄命令 (**cd**) 連接到使用虛擬磁碟機名稱的路徑時，系統就會使用您在建立磁碟機時所提供的 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 驗證登入認證來執行所有作業。  
  
```powershell
## Create a function that specifies the login and prompts for the password.  
  
function sqldrive  
{  
    param( [string]$name, [string]$login = "MyLogin", [string]$root = "SQLSERVER:\SQL\MyComputer\MyInstance" )  
    $pwd = read-host -AsSecureString -Prompt "Password"  
    $cred = new-object System.Management.Automation.PSCredential -argumentlist $login,$pwd  
    New-PSDrive $name -PSProvider SqlServer -Root $root -Credential $cred -Scope 1  
}  
  
## Use the sqldrive function to create a SQLAuth virtual drive.  
sqldrive SQLAuth
  
## Set-Location to the virtual drive, which invokes the supplied authentication credentials.  
sl SQLAuth:
```

## <a name="sql-server-authentication-using-invoke-sqlcmd"></a>使用 Invoke-Sqlcmd 的 SQL Server 驗證

### <a name="to-use-invoke-sqlcmd-with-sql-server-authentication"></a>搭配使用 Invoke-Sqlcmd 與 SQL Server 驗證

1. 使用 **-Username** 參數可指定登入識別碼，而 **-Password** 參數則可指定相關聯的密碼。  

#### <a name="example-invoke-sqlcmd"></a>範例 (Invoke-Sqlcmd)

此範例使用 read-host Cmdlet 提示使用者輸入密碼，然後使用 SQL Server 驗證進行連接。  

```powershell
## Prompt the user for their password.  
$pwd = read-host -AsSecureString -Prompt "Password"  
  
Invoke-Sqlcmd -Query "SELECT GETDATE() AS TimeOfQuery;" -ServerInstance "MyComputer\MyInstance" -Username "MyLogin" -Password $pwd  
```

## <a name="see-also"></a>另請參閱

- [SQL Server PowerShell](sql-server-powershell.md)
- [SQL Server PowerShell 提供者](sql-server-powershell-provider.md)
- [Invoke-Sqlcmd Cmdlet](/powershell/module/sqlserver/invoke-sqlcmd)
- [使用 PowerShell 搭配 Azure Data Studio](../azure-data-studio/extensions/powershell-extension.md)
