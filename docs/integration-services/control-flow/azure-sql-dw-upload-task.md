---
description: Azure SQL DW 上傳工作
title: Azure SQL DW 上傳工作 | Microsoft Docs
ms.custom: ''
ms.date: 12/16/2016
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPDWUPTASK.F1
- sql14.dts.designer.afpdwuptask.f1
ms.assetid: eef82c89-228a-4dc7-9bd0-ea00f57692f5
author: Lingxi-Li
ms.author: lingxl
ms.openlocfilehash: 5510fb2a0a4760b5465dad7f44eed8c85ef36251
ms.sourcegitcommit: ece151df14dc2610d96cd0d40b370a4653796d74
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96297956"
---
# <a name="azure-sql-dw-upload-task"></a>Azure SQL DW 上傳工作

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



**Azure SQL DW 上傳工作** 可讓 SSIS 套件將表格式資料從檔案系統或 Azure Blob 儲存體複製到 Azure Synapse Analytics (DW)。
該工作會利用 PolyBase 來改善效能，如 [Azure Synapse Analytics 載入模式及策略](/archive/blogs/sqlcat/azure-sql-data-warehouse-loading-patterns-and-strategies)一文所述。
目前支援的來源資料檔案格式為使用 UTF8 編碼的分隔文字。
從檔案系統複製時，資料會先上傳到 Azure Blob 儲存體暫存，再到 Azure SQL DW。 因此，會需要 Azure Blob 儲存體。

> [!NOTE]
> 不支援具有 Data Lake Gen2 服務類型的 Azure 儲存體連線管理員。
>
> 若要針對暫存或來源使用 Azure Data Lake Gen2，您可透過具有 Blob 儲存體服務類型的 Azure 儲存體連線管理員進行連線。

**Azure SQL DW 上傳工作** 是 [SQL Server Integration Services (SSIS) Feature Pack for Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md) 的元件。

若要新增 **Azure SQL DW 上傳工作**，請將其從 SSIS 工具箱拖放到設計工具畫布，然後按兩下或在其上按一下滑鼠右鍵，再按一下 [編輯]  ，即可看到工作編輯器對話方塊。

在 [一般]  頁面上設定下列屬性。

**SourceType** 指定來源資料存放區的類型。 選取下列其中一種類型：

* **檔案系統：** 來源資料所在的本機檔案系統。
* **BlobStorage：** 來源資料所在的 Azure Blob 儲存體。

以下是每個來源類型的屬性。

### <a name="filesystem"></a>FileSystem

欄位|描述
-----|-----------
LocalDirectory|指定包含要上傳之資料檔案的本機目錄。
Recursively|指定是否要以遞迴方式搜尋子目錄。
FileName|指定名稱篩選，以選取具有特定名稱模式的檔案。 例如 MySheet*.xsl\* 會包含 MySheet001.xsl 及 MySheetABC.xslx 這類的檔案。
RowDelimiter|指定標示各資料列結尾的字元。
ColumnDelimiter|指定一或多個標示各資料行結尾的字元。 例如 &#124; (縱線字元)、\t (定位字元)、' (單引號)、" (雙引號) 和 0x5c (反斜線)。
IsFirstRowHeader|指定每個資料檔案中的第一個資料列是否包含資料行名稱，而非實際資料。
AzureStorageConnection|指定 Azure 儲存體連線管理員。
BlobContainer|指定要透過 PolyBase 將本機資料上傳及轉送到 Azure DW 的目的 Blob 容器名稱。 如果容器不存在，系統將會建立新容器。
BlobDirectory|指定要透過 PolyBase 將本機資料上傳及轉送到 Azure DW 的目的 Blob 目錄 (虛擬階層式結構)。
RetainFiles|指定是否要保留上傳到 Azure 儲存體的檔案。
CompressionType|指定在將檔案上傳到 Azure 儲存體時要使用的壓縮格式。 本機來源不會受到影響。
CompressionLevel|指定要用於壓縮格式的壓縮層級。
AzureDwConnection|指定 Azure SQL DW 的 ADO.NET 連線管理員。
TableName|指定目的資料表的名稱。 選擇現有的資料表名稱，或選擇 **\<New Table ...>** 建立新的資料表。
TableDistribution|指定新資料表的發佈方法。 如果為 **TableName** 指定了新的資料表名稱即適用。
HashColumnName|指定用於雜湊表發佈的資料行。 如果為 **TableDistribution** 指定了 **HASH** 即適用。

### <a name="blobstorage"></a>BlobStorage

欄位|描述
-----|-----------
AzureStorageConnection|指定 Azure 儲存體連線管理員。
BlobContainer|指定來源資料所在的 Blob 容器名稱。
BlobDirectory|指定來源資料所在的 Blob 目錄 (虛擬階層式結構)。
RowDelimiter|指定標示各資料列結尾的字元。
ColumnDelimiter|指定一或多個標示各資料行結尾的字元。 例如 &#124; (縱線字元)、\t (定位字元)、' (單引號)、" (雙引號) 和 0x5c (反斜線)。
CompressionType|指定來源資料使用的壓縮格式。
AzureDwConnection|指定 Azure SQL DW 的 ADO.NET 連線管理員。
TableName|指定目的資料表的名稱。 選擇現有的資料表名稱，或選擇 **\<New Table ...>** 建立新的資料表。
TableDistribution|指定新資料表的發佈方法。 如果為 **TableName** 指定了新的資料表名稱即適用。
HashColumnName|指定用於雜湊表發佈的資料行。 如果為 **TableDistribution** 指定了 **HASH** 即適用。

根據您複製到新資料表或現有資料表，看到的 [對應]  頁面會有所不同。
如果是前者，請設定要對應哪些來源資料行，及其在要建立的目的資料表中對應名稱為何。
如果是後者，請設定來源與目的資料行之間的對應關係。

在 [資料行]  頁面上，為各來源資料行設定資料類型屬性。

[T-SQL]  頁面會顯示用來將資料從 Azure Blob 儲存體上傳到 Azure SQL DW 的 T-SQL。
T-SQL 從其他頁面上的組態自動產生，會在工作執行過程中執行。
您也可以按一下 [編輯]  按鈕以手動編輯產生的 T-SQL，使其符合您的特定需求。
您之後可以按一下 [重設]  按鈕，還原成自動產生的 T-SQL。