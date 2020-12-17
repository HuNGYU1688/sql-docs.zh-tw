---
title: 使用 R 建立 SSIS 和 SSRS 工作流程
description: 結合 SQL Server 機器學習服務與 R 服務、Reporting Services (SSRS) 和 SQL Server Integration Services (SSIS) 的整合案例。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 08/28/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 12088a5d5a8413f76176da09e2f8aa94e72e63f0
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470909"
---
# <a name="create-ssis-and-ssrs-workflows-with-r-on-sql-server"></a>使用 R 在 SQL Server 上建立 SSIS 和 SSRS 工作流程
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

本文說明如何搭配兩種重要 SQL Server 功能、透過 SQL Server 機器學習服務的語言與資料科學功能，來使用內嵌的 R 和 Python 指令碼：SQL Server Integration Services (SSIS) 和 SQL Server Reporting Services SSRS。 SQL Server 中的 R 和 Python 程式庫可提供統計與預測功能。 而 SSIS 和 SSRS 則分別提供協調的 ETL 轉換與視覺效果。 本文說明如何將上述所有功能一起放入此工作流程模式中：

> [!div class="checklist"]
> * 建立包含了可執行的 R 或 Python 預存程序
> * 從 SSIS 或 SSRS 執行預存程序

本文中的範例大多與 R 和 SSIS 相關，但其概念和步驟同樣適用於 Python。 而本文的第二區段則提供了 SSRS 視覺效果的指引和連結。

<a name="bkmk_ssis"></a> 

## <a name="use-ssis-for-automation"></a>使用 SSIS 進行自動化

資料科學工作流程的互動性很高，並涉及許多資料的轉換，包括縮放、彙總、機率計算，以及重新命名與合併屬性。 資料科學家已習慣以 R、Python 或其他語言執行許多這些工作；不過，在企業資料上執行這類工作流程需要與 ETL 工具和程序的緊密整合。

由於 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] 可讓您透過 Transact-SQL 和預存程序在 R 中執行複雜的作業，因此您可以將資料科學工作與現有的 ETL 程序整合。 您可以使用最有效率的工具 (包括 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 和 [!INCLUDE[tsql](../../includes/tsql-md.md)]) 將資料準備最佳化，而不需執行一連串耗用大量記憶體的工作。 

以下為如何使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]，將資料處理和模型化管線自動化的一些概念：

+ 從內部部署或雲端來源擷取資料，以建立訓練資料 
+ 建立並執行 R 或 Python 模型，以作為資料整合工作流程的一部分
+ 定期 (已排程) 重新訓練模型
+ 將 R 或 Python 指令碼的執行結果載入至其他目的地 (例如 Excel、Power BI、Oracle 和 Teradata 等等)
+ 使用 SSIS 工作在 SQL 資料庫中建立資料功能
+ 使用條件式分支切換 R 與 Python 工作的計算內容

## <a name="ssis-example"></a>SSIS 範例

下列範例源自於已淘汰的 MSDN 部落格文章，作者為 Jimmy Wong，URL：`https://blogs.msdn.microsoft.com/ssis/2016/01/11/operationalize-your-machine-learning-project-using-sql-server-2016-ssis-and-r-services/`

這個範例會示範如何使用 SSIS 將工作自動化。 您可以使用 SQL Server Management Studio 來建立具有內嵌 R 的預存程序，然後從 SSIS 套件中的 [執行 T-SQL 工作](../../integration-services/control-flow/execute-t-sql-statement-task.md) 執行這些預存程序。

若要逐步執行此範例，您需要熟悉 Management Studio、SSIS、SSIS 設計師、套件設計以及 T-SQL。 SSIS 套件使用了三個[執行 T-SQL 工作](../../integration-services/control-flow/execute-t-sql-statement-task.md)，其分別為將訓練資料插入資料表、建立資料模型，以及對資料進行評分以取得預測輸出。

### <a name="load-training-data"></a>載入訓練資料

在 SQL Server Management Studio 中執行下列指令碼，以建立用來儲存資料的資料表。 在此練習中，您應該建立並使用測試資料庫。 

```T-SQL
Use test-db
GO

Create table ssis_iris (
    id int not null identity primary key
    , "Sepal.Length" float not null, "Sepal.Width" float not null
    , "Petal.Length" float not null, "Petal.Width" float not null
    , "Species" varchar(100) null
);
GO
```

建立預存程序，使其將訓練資料載入資料框架中。 此範例使用了內建的 Iris 資料集。 

```T-SQL
Create procedure load_iris
as
begin
    execute sp_execute_external_script
        @language = N'R'
        , @script = N'iris_df <- iris;'
        , @input_data_1 = N''
        , @output_data_1_name = N'iris_df'
    with result sets (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100)));
end;
```

在 SSIS 設計師中，建立 [執行 SQL 工作](../../integration-services/control-flow/execute-sql-task.md)，以執行您剛才定義的預存程序。 **SQLStatement** 的指令碼會移除現有的資料、指定要插入的資料，然後呼叫預存程序來提供資料。

```T-SQL
truncate table ssis_iris;
insert into ssis_iris("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species")
exec dbo.load_iris;
```

![插入資料](../media/create-workflows-using-r-in-sql-server/ssis-exec-sql-insert-data.png "插入資料")

### <a name="generate-a-model"></a>產生模型

在 SQL Server Management Studio 中執行下列指令碼，以建立用來儲存模型的資料表。 

```T-SQL
Use test-db
GO

Create table ssis_iris_models (
    model_name varchar(30) not null default('default model') primary key,
    model varbinary(max) not null
);
GO
```

使用 [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)，建立可產生線性模型的預存程序。 在 SQL Server 中，RevoScaleR 和 revoscalepy 程式庫會自動在 R 和 Python 工作階段中啟用，因此您不需要匯入程式庫。

```T-SQL
Create procedure generate_iris_rx_model
as
begin
    execute sp_execute_external_script
        @language = N'R'
        , @script = N'
          irisLinMod <- rxLinMod(Sepal.Length ~ Sepal.Width + Petal.Length + Petal.Width + Species, data = ssis_iris);
          trained_model <- data.frame(payload = as.raw(serialize(irisLinMod, connection=NULL)));'
        , @input_data_1 = N'select "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from ssis_iris'
        , @input_data_1_name = N'ssis_iris'
        , @output_data_1_name = N'trained_model'
    with result sets ((model varbinary(max)));
end;
GO
```

在 SSIS 設計師中，建立 [執行 SQL 工作](../../integration-services/control-flow/execute-sql-task.md)，以執行 **generate_iris_rx_model** 預存程序。 該模型會序列化並儲存至 ssis_iris_models 資料表。 **SQLStatement** 的指令碼如下所示：

```T-SQL
insert into ssis_iris_models (model)
exec generate_iris_rx_model;
update ssis_iris_models set model_name = 'rxLinMod' where model_name = 'default model';
```

![產生線性模型](../media/create-workflows-using-r-in-sql-server/ssis-exec-rxlinmod.png "產生線性模型")

作為檢查點，在這項工作完成之後，您可以查詢 ssis_iris_models，以查看其是否包含了一個二進位模型。

### <a name="predict-score-outcomes-using-the-trained-model"></a>使用「已訓練」模型來預測 (評分) 結果

您現在已經有載入訓練資料並產生模型的程式碼，剩下的唯一步驟就是使用模型來產生預測。 

若要這麼做，請將 R 指令碼放入 SQL 查詢中，以觸發 ssis_iris_model 上的 [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) 內建 R 函式。 則名為 **predict_species_length** 的預存程序便會完成這項工作。

```T-SQL
Create procedure predict_species_length (@model varchar(100))
as
begin
    declare @rx_model varbinary(max) = (select model from ssis_iris_models where model_name = @model);
    -- Predict based on the specified model:
    exec sp_execute_external_script
        @language = N'R'
        , @script = N'
irismodel <-unserialize(rx_model);
irispred <- rxPredict(irismodel, ssis_iris[,2:6]);
OutputDataSet <- cbind(ssis_iris[1], irispred$Sepal.Length_Pred, ssis_iris[2]);
colnames(OutputDataSet) <- c("id", "Sepal.Length.Actual", "Sepal.Length.Expected");
#OutputDataSet <- subset(OutputDataSet, Species.Length.Actual != Species.Expected);
'
    , @input_data_1 = N'
    select id, "Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species" from ssis_iris
    '
    , @input_data_1_name = N'ssis_iris'
    , @params = N'@rx_model varbinary(max)'
    , @rx_model = @rx_model
    with result sets ( ("id" int, "Species.Length.Actual" float, "Species.Length.Expected" float)
        );
end;
```

在 SSIS 設計師中，建立 [執行 SQL 工作](../../integration-services/control-flow/execute-sql-task.md)，以執行 **predict_species_length** 預存程序，以產生預測的花瓣長度。

```T-SQL
exec predict_species_length 'rxLinMod';
```

![產生預測](../media/create-workflows-using-r-in-sql-server/ssis-exec-predictions.png "產生預測")

### <a name="run-the-solution"></a>執行解決方案

在 SSIS 設計師中，按下 F5 執行套件。 您應該會看到類似下列螢幕擷取畫面的結果。

![按下 F5 以在偵錯模式中執行](../media/create-workflows-using-r-in-sql-server/ssis-exec-F5-run.png "按下 F5 以在偵錯模式中執行")

<a name="bkmk_ssrs"></a> 

## <a name="use-ssrs-for-visualizations"></a>使用 SSRS 進行視覺效果

雖然 R 可以建立圖表和有趣的視覺效果，但其與外部資料來源的整合度並不高，這表示您必須個別產生每個圖表或圖形。 此外，可能也很難進行共用。

藉由使用 [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)]，您便可以透過各種企業報告工具 (包括 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] 和 Power BI) 都能輕鬆取用的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 預存程序，在 R 中執行複雜的作業。

## <a name="next-steps"></a>後續步驟

本文中的 SSIS 和 SSRS 範例說明了執行包含內嵌 R 或 Python 指令碼之預存程序的兩個案例。 重要的是，您可以將 R 或 Python 指令碼提供給任何應用程式或工具使用，以便在預存程序上傳送執行要求。 SSIS 的另一個重點是，您可以透過作業鏈中的 R 或 Python 資料科學功能建立套件，以自動化和排程各種作業，例如資料擷取、清理、操作等等。 如需詳細資訊和想法，請參閱[在 SQL Server 機器學習服務中，使用預存程序以讓 R 程式碼能夠運作](operationalizing-your-r-code.md)。