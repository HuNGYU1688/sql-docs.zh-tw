---
description: 使用 SQL 機器學習的 Python 和 R 教學課程鳶尾花示範資料
title: 教學課程的鳶尾花示範資料集
titleSuffix: SQL machine learning
Description: 建立包含鳶尾花資料集和預測模型的資料庫。 此資料集會用於使用 SQL 機器學習的 R 和 Python 教學課程。
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/26/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 76e746701aa331cf1720ea56befd5956fd65cfd4
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97470459"
---
# <a name="iris-demo-data-for-python-and-r-tutorials-with-sql-machine-learning"></a>使用 SQL 機器學習的 Python 和 R 教學課程鳶尾花示範資料
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

在此練習中，請建立資料庫，以根據相同的資料，儲存來自 [Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set) (鳶尾花資料集) 和模型的資料。 鳶尾花資料包含在 R 和 Python 發行版本中，並用於 SQL 機器學習的機器學習教學課程。

若要完成此練習，您應該具備 [SQL Server Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) 或可執行 T-SQL 查詢的其他工具。

使用此資料集的教學課程和快速入門包括下列內容：

+ [快速入門：以 Python 建立預測模型並為其評分](quickstart-python-train-score-model.md)

## <a name="create-the-database"></a>建立資料庫

1. 啟動 SQL Server Management Studio，然後開啟新的 [查詢]  視窗。  

2. 為此專案建立一個新的資料庫，並變更 [查詢]  視窗的內容，以使用新的資料庫。

    ```sql
    CREATE DATABASE irissql
    GO
    USE irissql
    GO
    ```

3. 加入一些空白資料表：一個用來儲存資料，一個用來儲存定型的模型。 **iris_models** 資料表用於儲存在其他練習中產生的序列化模型。

    下列程式碼會建立定型資料的資料表。

    ```sql
    DROP TABLE IF EXISTS iris_data;
    GO
    CREATE TABLE iris_data (
      id INT NOT NULL IDENTITY PRIMARY KEY
      , "Sepal.Length" FLOAT NOT NULL, "Sepal.Width" FLOAT NOT NULL
      , "Petal.Length" FLOAT NOT NULL, "Petal.Width" FLOAT NOT NULL
      , "Species" VARCHAR(100) NOT NULL, "SpeciesId" INT NOT NULL
    );
    ```

4. 執行下列程式碼，以建立用於儲存定型模型的資料表。 若要在 SQL Server 中儲存 Python (或 R) 模型，必須將其序列化，並儲存在 **varbinary(max)** 型別的資料行中。

    ```sql
    DROP TABLE IF EXISTS iris_models;
    GO

    CREATE TABLE iris_models (
      model_name VARCHAR(50) NOT NULL DEFAULT('default model') PRIMARY KEY,
      model VARBINARY(MAX) NOT NULL
    );
    GO
    ```

    除了模型內容以外，您通常也會為其他有用的中繼資料新增資料行，例如模型的名稱、定型的日期、來源演算法和參數、來源資料等等。 我們現在會保持其簡單性，只使用模型名稱。

## <a name="populate-the-table"></a>填入資料表

您可以從 R 或 Python 取得內建的鳶尾花資料。 您可以使用 Python 或 R，將資料載入資料框架，然後將其插入資料庫中的資料表。 將定型資料從外部工作階段移至資料表是多步驟的處理序：

+ 設計可取得所需資料的預存程序。
+ 執行預存程序以實際取得資料。
+ 建立 INSERT 陳述式，以指定擷取資料應該儲存的位置。

1. 在具有 Python 整合的系統上，建立以下使用 Python 程式碼來載入資料的預存程序。

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'Python', 
    @script = N'
    from sklearn import datasets
    iris = datasets.load_iris()
    iris_data = pandas.DataFrame(iris.data)
    iris_data["Species"] = pandas.Categorical.from_codes(iris.target, iris.target_names)
    iris_data["SpeciesId"] = iris.target
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

    當您執行此程式碼時，應該會出現「命令已順利完成。」訊息 這表示已根據您的規格建立了預存程序。

2. 或者，在具有 R 整合的系統上，改為建立使用 R 的程序。

    ```sql
    CREATE PROCEDURE get_iris_dataset
    AS
    BEGIN
    EXEC sp_execute_external_script @language = N'R', 
    @script = N'
    library(RevoScaleR)
    data(iris)
    iris$SpeciesID <- c(unclass(iris$Species))
    iris_data <- iris
    ', 
    @input_data_1 = N'', 
    @output_data_1_name = N'iris_data'
    WITH RESULT SETS (("Sepal.Length" float not null, "Sepal.Width" float not null, "Petal.Length" float not null, "Petal.Width" float not null, "Species" varchar(100) not null, "SpeciesId" int not null));
    END;
    GO
    ```

3. 若要實際填入資料表，請執行預存程序，並指定應該在其中寫入資料的資料表。 執行時，預存程序會執行 Python 或 R 程式碼，以載入內建的鳶尾花資料集，然後將資料插入 **iris_data** 資料表中。

    ```sql
    INSERT INTO iris_data ("Sepal.Length", "Sepal.Width", "Petal.Length", "Petal.Width", "Species", "SpeciesId")
    EXEC dbo.get_iris_dataset;
    ```

    如果您不熟悉 T-SQL，請注意 INSERT 陳述式只會新增新的資料；它不會檢查現有的資料，也不會刪除和重建資料表。 若要避免在資料表中取得多個相同資料的複本，您可以先執行此陳述式：`TRUNCATE TABLE iris_data`。 T-SQL [TRUNCATE TABLE](../../t-sql/statements/truncate-table-transact-sql.md) 陳述式會刪除現有的資料，但完整資料表的結構會保持不變。

## <a name="query-the-data"></a>查詢資料

執行查詢以確認資料已上傳，作為驗證步驟。

1. 在 [物件總管] 的 [資料庫] 下，以滑鼠右鍵按一下 **irissql** 資料庫，然後開始新的查詢。

2. 執行一些簡單的查詢：

    ```sql
    SELECT TOP(10) * FROM iris_data;
    SELECT COUNT(*) FROM iris_data;
    ```

## <a name="next-steps"></a>後續步驟

在下列快速入門中，您將建立機器學習模型，並將其儲存至資料表，然後使用模型以產生預測的結果。

+ [快速入門：以 Python 建立預測模型並為其評分](quickstart-python-train-score-model.md)