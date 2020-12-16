---
title: 在 Docker 上搭配 SQL Server 使用分散式交易 (MSDTC)
description: 了解如何針對 Docker 上 SQL Server 容器中的分散式交易使用 Microsoft Distributed Transaction Coordinator (MSDTC)。
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 8f8487b4246a349169891f68e4068ad233f78d21
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471629"
---
# <a name="how-to-use-distributed-transactions-with-sql-server-on-docker"></a>如何在 Docker 上搭配 SQL Server 使用分散式交易

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

本文說明如何在 Docker 上設定 SQL Server Linux 容器以進行分散式交易。

SQL Server 容器映像可以使用分散式交易所需的 Microsoft Distributed Transaction Coordinator (MSDTC)。 若要了解 MSDTC 的通訊需求，請參閱[如何在 Linux 上設定 Microsoft 分散式交易協調器 (MSDTC)](sql-server-linux-configure-msdtc.md)。 本文說明與 SQL Server Docker 容器相關的特殊需求和案例。

## <a name="configuration"></a>組態

若要在 Docker 的容器中啟用 MSDTC 交易，您必須設定兩個新的環境變數：

- **MSSQL_RPC_PORT**：RPC 端點對應程式服務繫結至與接聽的 TCP 連接埠。  
- **MSSQL_DTC_TCP_PORT**：MSDTC 服務設定接聽的連接埠。

### <a name="pull-and-run"></a>提取並執行

<!--SQL Server 2017 on Linux -->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

下列範例示範如何使用這些環境變數來提取並執行針對 MSDTC 所設定的單一 SQL Server 2017 容器。 這可讓其與任何主機上的任何應用程式通訊。

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2017-latest
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2017-latest
```

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

下列範例示範如何使用這些環境變數來提取並執行針對 MSDTC 所設定的單一 SQL Server 2019 容器。 這可讓其與任何主機上的任何應用程式通訊。

```bash
docker run \
   -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=<YourStrong!Passw0rd>' \
   -e 'MSSQL_RPC_PORT=135' -e 'MSSQL_DTC_TCP_PORT=51000' \
   -p 51433:1433 -p 135:135 -p 51000:51000  \
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

```PowerShell
docker run `
   -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=<YourStrong!Passw0rd>" `
   -e "MSSQL_RPC_PORT=135" -e "MSSQL_DTC_TCP_PORT=51000" `
   -p 51433:1433 -p 135:135 -p 51000:51000  `
   -d mcr.microsoft.com/mssql/server:2019-GA-ubuntu-16.04
```

::: moniker-end

> [!IMPORTANT]
> 上一個命令僅適用於在 Linux 上執行的 Docker。 針對 Windows 上的 Docker，Windows 主機已接聽連接埠 135。 您可以移除 Windows 上 Docker 的 `-p 135:135` 參數，但有一些限制。 產生的容器，即無法用於與主機相關的分散式交易，而只能參與主機上 Docker 容器之間的分散式交易。

在此命令中，**RPC 端點對應程式服務** 已繫結至連接埠 135，而 **MSDTC** 服務則已繫結至 Docker 虛擬網路內的連接埠 51000。 SQL Server TDS 通訊會發生在 Docker 虛擬網路中的連接埠 1433 上。 這些連接埠已作為 TDS 連接埠 51433、RPC 端點對應程式連接埠 135 和 MSDTC 連接埠 51000 對外公開給主機。

> [!NOTE]
> RPC 端點對應程式和 MSDTC 連接埠在主機和容器上不一定要相同。 因此，雖然 RPC 端點對應程式連接埠在容器上設定為 135，但可能會對應到連接埠 13501 或主機伺服器上任何其他可用的連接埠。

## <a name="configure-the-firewall"></a>設定防火牆

為了與主機通訊，您也必須在主機伺服器上為容器設定防火牆。 為 Docker 容器公開的所有連接埠開啟防火牆，以進行外部通訊。 在上述範例中，這會是連接埠 135、51433 和 51000。 這些是主機本身的連接埠，而不是它們對應至容器中的連接埠。 因此，如果容器的 RPC 端點對應程式連接埠 51000 對應至主機連接埠 51001，則應在防火牆中開啟連接埠 51001 (而不是 51000)，以便與主機通訊。  

下列範例顯示如何在 Ubuntu 上建立這些規則。

```bash
sudo ufw allow from any to any port 51433 proto tcp
sudo ufw allow from any to any port 51000 proto tcp
sudo ufw allow from any to any port 135 proto tcp
```

下列範例示範如何在 Red Hat Enterprise Linux (RHEL) 上執行這項作業：

```bash
sudo firewall-cmd --zone=public --add-port=51999/tcp --permanent
sudo firewall-cmd --zone=public --add-port=51433/tcp --permanent
sudo firewall-cmd --zone=public --add-port=135/tcp --permanent
sudo firewall-cmd --reload
```

## <a name="configure-port-routing-on-the-host"></a>設定主機上的連接埠路由

在上述範例中，因為單一 Docker 容器會將 RPC 連接埠 135 對應至主機上的連接埠 135，所以搭配主機的分散式交易現在應該可以使用，不需要進一步設定。 請注意，您可以直接在以 root 身分執行的容器中使用連接埠 135，因為 SQL Server 會在這些容器中以較高的權限執行。 若是容器外的 SQL Server 或非根容器，您必須使用不同的暫時連接埠 (例如 13500)，然後將連接埠 135 的流量路由傳送至該連接埠。 您也必須將容器內的連接埠路由規則從容器連接埠 135 設定為暫時連接埠。

此外，如果您決定將容器的連接埠 135 對應至主機上不同連接埠 (例如 13500)，則您必須在主機上設定連接埠路由。 這可讓 Docker 容器與主機和其他外部伺服器參與分散式交易。

如需連接埠路由的詳細資訊，請參閱[設定連接埠路由](sql-server-linux-configure-msdtc.md#configure-port-routing)。

> [!NOTE]
> 根據預設，SQL Server 2017 會在根容器中執行，而 SQL Server 2019 容器則是以非根使用者的身分執行。

## <a name="next-steps"></a>後續步驟

如需 Linux 上 MSDTC 的詳細資訊，請參閱[如何在 Linux 上設定 Microsoft 分散式交易協調器 (MSDTC)](sql-server-linux-configure-msdtc.md)。
