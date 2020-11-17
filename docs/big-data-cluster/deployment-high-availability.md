---
title: 部署高可用性 SQL Server 巨量資料叢集
titleSuffix: Deploy SQL Server Big Data Cluster with high availability
description: 了解如何部署高可用性 SQL Server 巨量資料叢集。
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 09/18/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 08645672c1aa8b7b980b4ffe86b4029a691fa1cf
ms.sourcegitcommit: 275fd02d60d26f4e66f6fc45a1638c2e7cedede7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2020
ms.locfileid: "94447104"
---
# <a name="deploy-sql-server-big-data-cluster-with-high-availability"></a>部署高可用性 SQL Server 巨量資料叢集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

由於 SQL Server 巨量資料叢集在 Kubernetes 上是容器化應用程式，並使用具狀態集合和永續性儲存體等功能，因此這個基礎結構具有內建的狀況監控、失敗偵測和容錯移轉機制，可供叢集元件用來維護服務健全狀態。 為了提高可靠性，您也可以設定 SQL Server 主要執行個體和/或 HDFS 名稱節點和 Spark 共用服務，在高可用性設定中使用額外的複本進行部署。 監視、失敗偵測與自動容錯移轉是由巨量資料叢集管理服務 (即控制服務) 所管理。 此服務不需要使用者介入，其涵蓋範圍從可用性群組設定、設定資料庫鏡像端點，到將資料庫新增至可用性群組，或容錯移轉及升級協調。 

下圖表示如何在 SQL Server 巨量資料叢集中部署可用性群組：

:::image type="content" source="media/deployment-high-availability/contained-ag.png" alt-text="high-availability-ag-bdc":::

以下是可用性群組所啟用的一些功能：

- 如果在部署設定檔中指定高可用性設定，就會建立名為 `containedag` 的單一可用性群組。 根據預設，`containedag` 具有三個複本，包括主要複本。 可用性群組的所有 CRUD 作業都是在內部進行管理，包括建立可用性群組，或將複本聯結至所建立的可用性群組。 您無法在巨量資料叢集的 SQL Server 主要執行個體中建立其他可用性群組。
- 所有資料庫都會自動新增至可用性群組，包括所有使用者和系統資料庫 (例如 `master` 和 `msdb`)。 此功能提供跨可用性群組複本的單一系統檢視。 其他模型資料庫 `model_replicatedmaster` 和 `model_msdb` 可用來植入系統資料庫的複寫部分。 除了這些資料庫，如果您直接連接到執行個體，還會看到 `containedag_master` 和 `containedag_msdb` 資料庫。 `containedag` 資料庫代表可用性群組內的 `master` 和 `msdb`。

  > [!IMPORTANT]
  > 因工作流程 (例如附加資料庫) 而在執行個體上建立的資料庫不會自動新增至可用性群組，且巨量資料叢集管理員必須手動執行此作業。 如需如何啟用 SQL Server 執行個體 master 資料庫之暫存端點的指示，請參閱[連線到 SQL Server 執行個體](#instance-connect)一節。 在 SQL Server 2019 CU2 版本之前，因 restore 陳述式而建立的資料庫具有相同行為，且需要手動將資料庫新增至包含的可用性群組。
  >
- Polybase 設定資料庫由於包含每個複本特定的執行個體層級中繼資料，因此不會包含在可用性群組中。
- 系統會自動佈建外部端點來連接到可用性群組中的資料庫。 `master-svc-external` 這個端點扮演可用性群組接聽程式的角色。
- 佈建第二個外部端點，對次要複本進行唯讀連接以擴增讀取工作負載。

## <a name="deploy"></a>部署

若要在可用性群組中部署 SQL Server 主要：

1. 啟用 `hadr` 功能
1. 指定 AG 的複本數目 (最大值為 3)
1. 設定針對連接到唯讀次要複本所建立第二個外部端點的詳細資料

您可以使用 `aks-dev-test-ha` 或 `kubeadm-prod` 內建組態設定檔來開始自訂巨量資料叢集。 這些設定檔包含為資源設定額外高可用性時所需的設定。 例如，以下是 `bdc.json` 設定檔中與針對 SQL Server 主要執行個體啟用可用性群組有關的區段。  

```json
{
  ...
    "spec": {
      "type": "Master",
      "replicas": 3,
      "endpoints": [
        {
          "name": "Master",
          "serviceType": "LoadBalancer",
          "port": 31433
        },
        {
          "name": "MasterSecondary",
          "serviceType": "LoadBalancer",
          "port": 31436
        }
      ],
      "settings": {
        "sql": {
          "hadr.enabled": "true"
        }
      }
    }
  ...
}
```

下列步驟會逐步示範如何從 `aks-dev-test-ha` 設定檔開始自訂您的巨量資料叢集部署設定。 若要在 `kubeadm` 叢集上進行部署，請套用類似步驟，但確定在 `endpoints` 區段中使用 `NodePort` 作為 `serviceType`。

1. 複製您的目標設定檔

    ```bash
    azdata bdc config init --source aks-dev-test-ha --target custom-aks-ha
    ```

1. 您可以視需要選擇性地對自訂設定檔進行任何編輯。 
1. 使用以上所建立的叢集組態設定檔來啟動叢集部署

    ```bash
    azdata bdc create --config-profile custom-aks-ha --accept-eula yes
    ```

## <a name="connect-to-sql-server-databases-in-the-availability-group"></a>連接到可用性群組中 SQL Server 資料庫

根據您想要針對 SQL Server 主要執行的工作負載類型，您可以連接到主要複本 (適用於讀寫工作負載) 或次要複本中的資料庫 (適用於唯讀類型的工作負載)。 以下概述每種連接類型：

### <a name="connect-to-databases-on-the-primary-replica"></a>連接到主要複本上的資料庫

若要連接到主要複本，請使用 `sql-server-master` 端點。 此端點也是 AG 的接聽程式。 使用此端點時，所有連接都會在可用性群組的資料庫內容中進行。 例如，使用此端點的預設連接會導致連接到可用性群組中 `master` 資料庫，而不是 SQL Server 執行個體 `master` 資料庫。 執行此命令來尋找端點：

```bash
azdata bdc endpoint list -e sql-server-master -o table
```

```
Description                           Endpoint             Name               Protocol
------------------------------------  -------------------  -----------------  ----------
SQL Server Master Instance Front-End  11.11.111.111,11111  sql-server-master  tds
```

> [!NOTE]
> 在從遠端資料來源 (例如 HDFS 或資料集區) 存取資料的分散式查詢執行期間，可能會發生容錯移轉事件。 基於最佳做法，應用程式應該設計成在容錯移轉造成中斷連接時具有連接重試邏輯。  
>

### <a name="connect-to-databases-on-the-secondary-replicas"></a>連接到次要複本上的資料庫

若要對次要複本中的資料庫進行唯讀連接，請使用 `sql-server-master-readonly` 端點。 此端點可作為所有次要複本之間的負載平衡器。  使用此端點時，所有連接都會在可用性群組的資料庫內容中進行。 例如，使用此端點的預設連接會導致連接到可用性群組中 `master` 資料庫，而不是 SQL Server 執行個體 `master` 資料庫。 

```bash
azdata bdc endpoint list -e sql-server-master-readonly -o table
```

```
Description                                    Endpoint            Name                        Protocol
---------------------------------------------  ------------------  --------------------------  ----------
SQL Server Master Readable Secondary Replicas  11.11.111.11,11111  sql-server-master-readonly  tds
```

## <a name="connect-to-sql-server-instance"></a><a id="instance-connect"></a> 連接到 SQL Server 執行個體

針對設定伺服器層級設定或手動將資料庫新增至可用性群組等特定作業，您必須連接到 SQL Server 執行個體。 在 SQL Server 2019 CU2 版本之前，`sp_configure`、`RESTORE DATABASE` 或任何可用性群組 DDL 等作業都需要這種連線類型。 根據預設，巨量資料叢集不會包含啟用執行個體連接的端點，您必須手動公開此端點。 

> [!IMPORTANT]
> 針對 SQL Server 執行個體連接所公開的端點只支援 SQL 驗證，即使在已啟用 Active Directory 的叢集中也一樣。 根據預設，在巨量資料叢集部署期間，會停用 `sa` 登入，並根據部署時為 `AZDATA_USERNAME` 和 `AZDATA_PASSWORD` 環境變數所提供值佈建新的 `sysadmin` 登入。

> [!IMPORTANT]
> 包含的可用性群組 DDL 在 BDC 中是以獨佔方式自我管理的。 不支援卸除包含的可用性或資料庫鏡像端點的任何 (外部使用者) 嘗試，而且可能導致無法復原的 BDC 狀態。

下列範例示範如何公開此端點，然後將使用還原工作流程建立的資料庫新增至可用性群組。 當您想要使用 `sp_configure` 變更伺服器設定時，也適用設定 SQL Server 主要執行個體連接的類似指示。

> [!NOTE]
> 從 SQL Server 2019 CU2 版本開始，因還原工作流程而建立的資料庫會自動新增至所包含可用性群組。

- 判斷裝載主要複本的 Pod，方法是連接到 `sql-server-master` 端點並執行：

    ```sql
    SELECT @@SERVERNAME
   ```

- 建立新的 Kubernetes 服務來公開外部端點

    針對 `kubeadm` 叢集，請執行下列命令。 將 `podName` 取代為上一個步驟所傳回的伺服器名稱、將 `serviceName` 取代為所建立 Kubernetes 服務的慣用名稱，並將 `namespaceName`* 取代為您的 BDC 叢集名稱。

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=NodePort
    ```

    針對 aks 叢集執行，請執行相同的命令，但所建立的服務類型會是 `LoadBalancer`。 例如： 

    ```bash
    kubectl -n <namespaceName> expose pod <podName> --port=1533  --name=<serviceName> --type=LoadBalancer
    ```

    以下是這個命令針對 aks 執行的範例，其中裝載主要複本的 Pod 是 `master-0`：

    ```bash
    kubectl -n mssql-cluster expose pod master-0 --port=1533  --name=master-sql-0 --type=LoadBalancer
    ```

    取得所建立 Kubernetes 服務的 IP：

    ```bash
    kubectl get services -n <namespaceName>
    ```

> [!IMPORTANT]
> 基於最佳做法，您應該藉由執行此命令來刪除以上所建立的 Kubernetes 服務以進行清理：
>
>```bash
>kubectl delete svc master-sql-0 -n mssql-cluster
>```

- 將資料庫新增至可用性群組。

    若要將資料庫新增至 AG，則必須以完整復原模式執行，且必須進行記錄備份。 使用以上所建立 Kubernetes 服務的 IP 並連接到 SQL Server 執行個體，然後執行 TSQL 陳述式，如下所示。

    ```sql
    ALTER DATABASE <databaseName> SET RECOVERY FULL;
    BACKUP DATABASE <databaseName> TO DISK='<filePath>'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE <databaseName>
    ```

    下列範例會新增已在執行個體上還原的資料庫 `sales`：

    ```sql
    ALTER DATABASE sales SET RECOVERY FULL;
    BACKUP DATABASE sales TO DISK='/var/opt/mssql/data/sales.bak'
    ALTER AVAILABILITY GROUP containedag ADD DATABASE sales
    ```

## <a name="known-limitations"></a>已知限制

巨量資料叢集中 SQL Server master 的內含可用性群組有已知問題與限制：

- 部署巨量資料叢集之後，必須建立高可用性設定。 您無法在可用性群組部署後啟用高可用性設定。 此時，唯一啟用的設定是針對同步認可複本。

> [!WARNING]
> 針對仲裁認可中的任何複本將同步處理模式更新為非同步認可，將會導致無效的高可用性設定。 以此組態執行可能會有資料遺失風險，因為萬一發生影響主要複本的失敗事件時，並不會觸發自動容錯移轉，而且使用者在發出手動容錯移轉時，必須接受資料遺失的風險。

- 若要從另一部伺服器上所建立備份成功還原已啟用 TDE 的資料庫，則必須確定已同時在 SQL Server 執行個體主機以及包含的 AG 主機上還原[必要憑證](../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md)。 如需如何備份和還原憑證的範例，請參閱[此處](https://www.sqlshack.com/restoring-transparent-data-encryption-tde-enabled-databases-on-a-different-server/)。
- 使用 `sp_configure` 執行伺服器組態設定等特定作業會需要連接到 SQL Server 執行個體 `master` 資料庫，而不是可用性群組 `master`。 您無法使用對應的主要端點。 請遵循[指示](#instance-connect)來公開端點並連接到 SQL Server 執行個體，然後執行 `sp_configure`。 當您手動公開端點來連接到 SQL Server 執行個體 `master` 資料庫時，只能使用 SQL 驗證。
- 雖然可用性群組中包含 msdb 資料庫，而且 SQL Agent 作業會複寫到整個群組，作業只會根據主要複本上的排程執行。
- 不支援針對自主可用性群組使用複寫功能。 作為自主 AG 之一部分的 SQL Server 執行個體，無論是在執行個體層級或自主 AG 層級，皆無法當作散發者或發行者運作。
- 不支援在建立資料庫期間新增檔案群組。 作為因應措施，您可以先建立資料庫，然後發出 ALTER DATABASE 陳述式以新增任何檔案群組。
- 在 SQL Server 2019 CU2 版本之前，因 `CREATE DATABASE` 與 `RESTORE DATABASE` 以外的工作流程 (例如 `CREATE DATABASE FROM SNAPSHOT`) 而建立的資料庫不會自動新增至可用性群組。 請[連接到執行個體](#instance-connect)，然後手動將資料庫新增至可用性群組。

## <a name="next-steps"></a>後續步驟

- 如需在巨量資料叢集部署中使用設定檔的詳細資訊，請參閱[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md#configfile)。
- 如需 SQL Server 可用性群組功能的詳細資訊，請參閱 [Always On 可用性群組概觀 (SQL Server)](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)。
