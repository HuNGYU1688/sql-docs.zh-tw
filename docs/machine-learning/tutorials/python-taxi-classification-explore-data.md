---
title: Python 教學課程：探索及視覺化資料
titleSuffix: SQL machine learning
description: 探索範例資料並產生一些繪圖，以準備使用 Python 的二元分類搭配 SQL 機器學習。
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 0f52df34bb04a94baf14076a9ea345712166ae5a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470319"
---
# <a name="python-tutorial-explore-and-visualize-data"></a>Python 教學課程：探索及視覺化資料
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

在這個五部分教學課程系列的第二部分中，您已探索範例資料並產生一些繪圖。 稍後，您將了解如何在 Python 中序列化繪圖物件，然後將這些物件還原序列化並製作繪圖。

在本文中，您將：

> [!div class="checklist"]
> + 檢閱範例資料
> + 在 T-SQL 中使用 Python 建立繪圖

在[第一部分](python-taxi-classification-introduction.md)中，您已安裝必要條件並還原範例資料庫。

在[第三部分](python-taxi-classification-create-features.md)中，您將了解如何使用 Transact-SQL 函式，從未經處理的資料建立特徵。 接著您將從預存程序呼叫該函數，以建立包含特徵值的資料表。

在[第四部分](python-taxi-classification-train-model.md)中，您將載入模組，並呼叫所需的函式，以使用 SQL Server 預存程序來建立和定型模型。

在[第五部分](python-taxi-classification-deploy-model.md)中，您將了解如何運作您在第四部分中定型並儲存的模型。

## <a name="review-the-data"></a>檢閱資料

首先，請花一點時間瀏覽資料結構描述，因為我們已進行一些變更，好讓您能更輕鬆地使用 NYC 計程車資料

+ 使用的原始資料集將計程車識別碼和車程記錄的檔案分開。 我們已經在資料行 _medallion_、_hack_license_ 和 _pickup_datetime_ 上聯結兩個原始資料集。  
+ 原始資料集跨越許多檔案，而且相當大。 我們將抽樣減少，只取得 1% 的原始記錄數目。 目前的資料表有 1,703,957 個資料列和 23 個資料行。

**計程車識別碼**

_medallion_ 資料行代表計程車的唯一識別碼。

_hack_license_ 資料行包含計程車司機駕照號碼 (匿名)。

**車程和小費記錄**

每筆車程記錄都會包含上車和下車位置與時間，以及車程距離。

每筆費用記錄都會包括付款資訊，例如付款類型、總付款金額和小費金額。

最後三個資料行可以用於各種機器學習工作。  _tip_amount_ 資料行包含連續數值，而且可以當成 **label** 資料行來進行迴歸分析。 _tipped_ 資料行只有是/否值，並且用於二元分類。 _tip_class_ 資料行有多個 **類別標籤** ，因此可以當成多類別分類工作的標籤使用。

使用這些商務規則，用於 label 資料行的值都是根據 `tip_amount` 資料行︰

+ 標籤資料行 `tipped` 有可能的值 0 和 1

    如果 `tip_amount` > 0，`tipped` = 1；否則 `tipped` = 0

+ 標籤資料行 `tip_class` 有可能的類別值 0-4

    類別 0：`tip_amount` = $0

    類別 1：`tip_amount` > $0 和 `tip_amount` < = $5
    
    類別 2：`tip_amount` > $5 和 `tip_amount` < = $10
    
    類別 3：`tip_amount` > $10 和 `tip_amount` < = $20
    
    類別 4：`tip_amount` > $20

## <a name="create-plots-using-python-in-t-sql"></a>在 T-SQL 中使用 Python 建立繪圖

開發資料科學方案通常會包含大量資料瀏覽和資料視覺化。 因為視覺效果是可了解資料和極端值分佈的功能強大工具，所以 Python 提供許多套件可以將資料視覺化。 **matplotlib** 模組是視覺效果較熱門的程式庫之一，其中包含許多用來建立長條圖、散佈圖、盒狀圖和其他資料瀏覽圖形的功能。

在本節中，您將瞭解如何使用預存程序來處理繪圖。 您不需要在伺服器上開啟影像，您可以將 Python 物件 `plot` 儲存為 **varbinary** 資料，然後將該資料寫入可以在其他位置共用或檢視的檔案。

### <a name="create-a-plot-as-varbinary-data"></a>建立繪圖成為 varbinary 資料

預存程序會將序列化的 Python `figure` 物件當做 **varbinary** 資料的資料流傳回。 您無法直接檢視二進位資料，但是您可以在用戶端上使用 Python 程式碼來還原序列化和檢視圖形，然後將影像檔案儲存在用戶端電腦上。

1. 如果 PowerShell 指令碼尚未建立預存程序，請建立預存程序 **PyPlotMatplotlib**。

    - 變數 `@query` 會定義查詢文字 `SELECT tipped FROM nyctaxi_sample`，以當成指令碼輸入變數 `@input_data_1` 的引數傳遞給 Python 程式碼區塊。
    - Python 指令碼相當簡單：**matplotlib** `figure` 物件會用來建立長條圖和散佈圖，然後使用 `pickle` 程式庫將這些物件序列化。
    - Python 圖形物件會序列化為 **pandas** DataFrame 以進行輸出。
  
    ```sql
    DROP PROCEDURE IF EXISTS PyPlotMatplotlib;
    GO

    CREATE PROCEDURE [dbo].[PyPlotMatplotlib]
    AS
    BEGIN
        SET NOCOUNT ON;
        DECLARE @query nvarchar(max) =
        N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'
        EXECUTE sp_execute_external_script
        @language = N'Python',
        @script = N'
    import matplotlib
    matplotlib.use("Agg")
    import matplotlib.pyplot as plt
    import pandas as pd
    import pickle

    fig_handle = plt.figure()
    plt.hist(InputDataSet.tipped)
    plt.xlabel("Tipped")
    plt.ylabel("Counts")
    plt.title("Histogram, Tipped")
    plot0 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.tip_amount)
    plt.xlabel("Tip amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Tip amount")
    plot1 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.hist(InputDataSet.fare_amount)
    plt.xlabel("Fare amount ($)")
    plt.ylabel("Counts")
    plt.title("Histogram, Fare amount")
    plot2 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    plt.scatter( InputDataSet.fare_amount, InputDataSet.tip_amount)
    plt.xlabel("Fare Amount ($)")
    plt.ylabel("Tip Amount ($)")
    plt.title("Tip amount by Fare amount")
    plot3 = pd.DataFrame(data =[pickle.dumps(fig_handle)], columns =["plot"])
    plt.clf()

    OutputDataSet = plot0.append(plot1, ignore_index=True).append(plot2, ignore_index=True).append(plot3, ignore_index=True)
    ',
    @input_data_1 = @query
    WITH RESULT SETS ((plot varbinary(max)))
    END
    GO
    ```

2. 現在，請執行不含引數的預存程序，以便從硬式編碼為輸入查詢的資料產生繪圖。

    ```sql
    EXEC [dbo].[PyPlotMatplotlib]
    ```

3. 結果應該類似這樣：
  
    ```sql
    plot
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    0xFFD8FFE000104A4649...
    ```

4. 從 [Python 用戶端](../python/setup-python-client-tools-sql.md)中，您現在可以連線到產生二進位繪圖物件的 SQL Server 執行個體，並檢視繪圖。 

    若要這麼做，請執行下列 Python 程式碼，並適當地替換伺服器名稱、資料庫名稱和認證 (使用 Windows 驗證時，請將 `UID` 和 `PWD` 參數取代為 `Trusted_Connection=True`)。 請確定用戶端和伺服器上的 Python 版本相同。 此外，也請確定您用戶端上的 Python 程式庫 (例如 matplotlib) 與伺服器上安裝程式庫的版本相同或更高。
  
    ```python
    %matplotlib notebook
    import pyodbc
    import pickle
    import os
    cnxn = pyodbc.connect('DRIVER=SQL Server;SERVER={SERVER_NAME};DATABASE={DB_NAME};UID={USER_NAME};PWD={PASSWORD}')
    cursor = cnxn.cursor()
    cursor.execute("EXECUTE [dbo].[PyPlotMatplotlib]")
    tables = cursor.fetchall()
    for i in range(0, len(tables)):
        fig = pickle.loads(tables[i][0])
        fig.savefig(str(i)+'.png')
    print("The plots are saved in directory: ",os.getcwd())
    ```

5. 如果連線成功，您應該會看到如下所示的訊息：
  
   *繪圖儲存在目錄：xxxx*
  
6. 輸出檔案會在 Python 工作目錄中建立。 若要檢視繪圖，請找出 Python 工作目錄，然後開啟檔案。 下圖顯示在用戶端電腦上儲存的繪圖。
  
   ![小費金額與車資金額](media/sqldev-python-sample-plot.png "小費金額與車資金額") 

## <a name="next-steps"></a>後續步驟

在本文章中，您將：

> [!div class="checklist"]
> + 已檢閱範例資料
> + 已在 T-SQL 中使用 Python 建立繪圖

> [!div class="nextstepaction"]
> [Python 教學課程：使用 T-SQL 建立資料特徵](python-taxi-classification-create-features.md)