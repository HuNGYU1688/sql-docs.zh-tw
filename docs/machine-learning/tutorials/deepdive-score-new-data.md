---
title: 使用 RevoScaleR 對資料評分
description: 使用在上一個教學課程中建立的羅吉斯迴歸模型，為另一個使用相同自變數作為輸入的資料集評分。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 11/27/2018
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 314520a54bb9052fb091932b63b9cf4c817a0f16
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470499"
---
# <a name="score-new-data-sql-server-and-revoscaler-tutorial"></a>對新資料評分 (SQL Server 和 RevoScaleR 教學課程)
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

這是 [RevoScaleR 教學課程系列](deepdive-data-science-deep-dive-using-the-revoscaler-packages.md)的教學課程 8；此教學課程系列說明如何搭配 SQL Server 使用 [RevoScaleR 函式](/machine-learning-server/r-reference/revoscaler/revoscaler) \(英文\)。

在此教學課程中，您將會使用在上一個教學課程中建立的羅吉斯迴歸模型，為另一個使用相同獨立變數作為輸入的資料集評分。

> [!div class="checklist"]
> * 為新資料評分
> * 建立分數的長條圖

> [!NOTE]
> 您需要具備 DDL 系統管理員權限才能執行某些步驟。

## <a name="generate-and-save-scores"></a>產生並儲存分數
  
1. 更新 sqlScoreDS 資料來源 (在[教學課程二](deepdive-create-sql-server-data-objects-using-rxsqlserverdata.md)中建立)，以使用在上一個教學課程中建立的資料行資訊。
  
    ```R
    sqlScoreDS <- RxSqlServerData(
        connectionString = sqlConnString,
        table = sqlScoreTable,
        colInfo = ccColInfo,
        rowsPerRead = sqlRowsPerRead)
    ```
  
2. 為了確保您不會遺失結果，請建立新的資料來源物件。 然後，使用新的資料來源物件來填入 RevoDeepDive 資料庫中的新資料表。
  
    ```R
    sqlServerOutDS <- RxSqlServerData(table = "ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead )
    ```
    此時，資料表尚未建立。 此陳述式只會定義資料的容器。
     
3. 使用 **rxGetComputeContext()** 檢查目前的計算內容，並視需要將計算內容設定為伺服器。
  
    ```R
    rxSetComputeContext(sqlCompute)
    ```
  
4. 基於謹慎起見，請檢查輸出資料表是否存在。 如果已經有同名的資料表存在，當您嘗試寫入新的資料表時，就會收到錯誤。
  
    若要這樣做，請呼叫函數 [rxSqlServerTableExists](/machine-learning-server/r-reference/revoscaler/rxsqlserverdroptable) 和 [rxSqlServerDropTable](/machine-learning-server/r-reference/revoscaler/rxsqlserverdroptable)，並傳遞資料表名稱作為輸入。
  
    ```R
    if (rxSqlServerTableExists("ccScoreOutput"))     rxSqlServerDropTable("ccScoreOutput")
    ```
  
    + **rxSqlServerTableExists** 會查詢 ODBC 驅動程式，如果資料表存在便傳回 TRUE，否則為 FALSE。
    + **rxSqlServerDropTable** 會執行 DDL，如果資料表順利卸除便傳回 TRUE，否則為 FALSE。

5. 執行 [rxPredict](/machine-learning-server/r-reference/revoscaler/rxpredict) 來建立分數，並將它們儲存在資料來源 sqlScoreDS 中定義的新資料表。
  
    ```R
    rxPredict(modelObject = logitObj,
        data = sqlScoreDS,
        outData = sqlServerOutDS,
        predVarNames = "ccFraudLogitScore",
          type = "link",
        writeModelVars = TRUE,
        overwrite = TRUE)
    ```
  
    **rxPredict** 函數是支援在遠端計算內容中執行的另一個函數。 您可以使用 **rxPredict** 函式，從根據 [rxLinMod](/machine-learning-server/r-reference/revoscaler/rxlinmod)、[rxLogit](/machine-learning-server/r-reference/revoscaler/rxlogit)或 [rxGlm](/machine-learning-server/r-reference/revoscaler/rxglm) 的模型建立分數。
  
    - 在此，參數 *writeModelVars* 設為 **TRUE** 。 這表示估計所用的變數將會包含在新的資料表中。
  
    - 參數 *predVarNames* 指定儲存結果的變數。 您在這裡傳遞新的變數，`ccFraudLogitScore`。
  
    - *rxPredict* 的 **type** 參數定義您要如何計算預測。 指定關鍵字 **回應**，根據回應變數的小數位數來產生分數。 或者，使用關鍵字 **連結**，根據基礎連結函式產生分數，在此情況下，會使用羅吉斯小數位數來建立預測。

6. 在一段時間之後，您可以在 Management Studio 中重新整理資料表清單，以查看新的資料表及其資料。

7. 若要新增其他變數到輸出預測，請使用 *extraVarsToWrite* 引數。  例如，在下列程式碼中，來自評分資料表的變數 *custID* 會新增到預測的輸出資料表。
  
    ```R
    rxPredict(modelObject = logitObj,
            data = sqlScoreDS,
            outData = sqlServerOutDS,
            predVarNames = "ccFraudLogitScore",
              type = "link",
            writeModelVars = TRUE,
            extraVarsToWrite = "custID",
            overwrite = TRUE)
    ```

## <a name="display-scores-in-a-histogram"></a>以長條圖顯示分數

在建立新的資料表之後，計算並顯示 10000 個預測分數的長條圖。 指定下限和上限值可加快計算，您將可從資料庫取得計算結果，並新增到您的工作資料中。

1. 建立新的資料來源 sqlMinMax，查詢資料庫以取得下限和上限值。
  
    ```R
    sqlMinMax <- RxSqlServerData(
        sqlQuery = paste("SELECT MIN(ccFraudLogitScore) AS minVal,",
        "MAX(ccFraudLogitScore) AS maxVal FROM ccScoreOutput"),
        connectionString = sqlConnString)
    ```

     此範例中，您可以看到使用 **RxSqlServerData** 資料來源物件，根據 SQL 查詢、函數或預存程序定義任意資料集，然後在您的 R 程式碼中使用它們有多容易。 變數不會儲存實際的值，而只會儲存資料來源定義。唯有在您將它用於例如 **rxImport** 的函數中時，才會執行查詢來產生值。
      
2. 呼叫 [rxImport](/machine-learning-server/r-reference/revoscaler/rximport) 函式將值放入可跨計算內容共用的資料框架。
  
    ```R
    minMaxVals <- rxImport(sqlMinMax)
    minMaxVals <- as.vector(unlist(minMaxVals))
    ```

    **結果**
     
    ```R
    > minMaxVals
     
    [1] -23.970256   9.786345
    ```

3. 現在可以使用最大值和最小值，請使用這些值來為產生的分數建立另一個資料來源。
  
    ```R
    sqlOutScoreDS <- RxSqlServerData(sqlQuery = "SELECT ccFraudLogitScore FROM ccScoreOutput",
        connectionString = sqlConnString,
        rowsPerRead = sqlRowsPerRead,
            colInfo = list(ccFraudLogitScore = list(
                low = floor(minMaxVals[1]),
                        high = ceiling(minMaxVals[2]) ) ) )
    ```

4. 使用資料來源物件 sqlOutScoreDS 來取得分數，並計算及顯示長條圖。 新增程式碼，視需要設定計算內容。
  
    ```R
    # rxSetComputeContext(sqlCompute)
    rxHistogram(~ccFraudLogitScore, data = sqlOutScoreDS)
    ```
  
    **結果**
  
    ![R 所建立的複雜長條圖](media/rsql-sue-complex-histogram.png "R 所建立的複雜長條圖")
  
## <a name="next-steps"></a>後續步驟

> [!div class="nextstepaction"]
> [使用 R 轉換資料](../../machine-learning/tutorials/deepdive-transform-data-using-r.md)