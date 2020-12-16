---
title: 從預存程序傳回資料 | Microsoft 文件
description: 了解如何使用結果集、輸出參數和傳回碼，將來自預存程序的資料傳回呼叫程式。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.technology: stored-procedures
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- stored procedures [SQL Server], returning data
- returning data from stored procedure
ms.assetid: 7a428ffe-cd87-4f42-b3f1-d26aa8312bf7
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5de3d43ebaef021697a1cc38a67a6e4eadcaf503
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473059"
---
# <a name="return-data-from-a-stored-procedure"></a>從預存程序傳回資料
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  將資料從程序傳回至呼叫端程式的方式有三種：結果集、輸出參數和傳回碼。 本主題提供有關這三種方法的資訊。  
  
## <a name="returning-data-using-result-sets"></a>使用結果集傳回資料
如果您的預存程序主體中包含 SELECT 陳述式 (但不是 SELECT ...INTO 或 INSERT...SELECT)，SELECT 陳述式所指定的資料列會直接傳送到用戶端。  針對大型結果集，在結果集完全傳送至用戶端之前，預存程序執行將不會繼續到下一個陳述式。  針對小型結果集，結果會進行多工緩衝處理以傳回至用戶端，而執行則會繼續進行。  如果在預存程序執行期間執行多個此類 SELECT 陳述式，則會將多個結果集傳送至用戶端。  此行為也適用於巢狀 TSQL 批次、巢狀預存程序和最上層 TSQL 批次。
 
 
 ### <a name="examples-of-returning-data-using-a-result-set"></a>使用結果傳回資料的範例 
  下列範例顯示的預存程序會針對也出現在 vEmployee 檢視中的所有 SalesPerson 資料列，傳回 LastName 和 SalesYTD 值。
  
 ```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetEmployeeSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetEmployeeSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetEmployeeSalesYTD  
AS    
  
    SET NOCOUNT ON;  
    SELECT LastName, SalesYTD  
    FROM Sales.SalesPerson AS sp  
    JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
    
RETURN  
GO  
  
```  

  
## <a name="returning-data-using-an-output-parameter"></a>使用輸出參數傳回資料  
 如果在程序定義中為參數指定 OUTPUT 關鍵字，程序就可以在結束時將參數目前的值傳回至呼叫端程式。 若要將參數值儲存在可供呼叫端程式使用的變數中，呼叫端程式在執行程序時必須使用 OUTPUT 關鍵字。 如需何種資料類型可做為輸出參數的詳細資訊，請參閱 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)。  
  
### <a name="examples-of-output-parameter"></a>輸出參數範例  
 下列範例示範的程序有一個輸入參數和一個輸出參數。 `@SalesPerson` 參數會接收呼叫端程式所指定的輸入值。 SELECT 陳述式使用傳入輸入參數的值，來取得正確的 `SalesYTD` 值。 SELECT 陳述式也會將值指派給 `@SalesYTD` 輸出參數，以便在程序結束時，將值傳回給呼叫端程式。  
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.uspGetEmployeeSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.uspGetEmployeeSalesYTD;  
GO  
CREATE PROCEDURE Sales.uspGetEmployeeSalesYTD  
@SalesPerson nvarchar(50),  
@SalesYTD money OUTPUT  
AS    
  
    SET NOCOUNT ON;  
    SELECT @SalesYTD = SalesYTD  
    FROM Sales.SalesPerson AS sp  
    JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
    WHERE LastName = @SalesPerson;  
RETURN  
GO  
  
```  
  
 下列範例會呼叫第一個範例中所建立的程序，並將被呼叫程序所傳回的輸出值儲存至呼叫端程式的區域變數 `@SalesYTD` 。  
  
```sql
-- Declare the variable to receive the output value of the procedure.  
DECLARE @SalesYTDBySalesPerson money;  
-- Execute the procedure specifying a last name for the input parameter  
-- and saving the output value in the variable @SalesYTDBySalesPerson  
EXECUTE Sales.uspGetEmployeeSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDBySalesPerson OUTPUT;  
-- Display the value returned by the procedure.  
PRINT 'Year-to-date sales for this employee is ' +   
    convert(varchar(10),@SalesYTDBySalesPerson);  
GO  
  
```  
  
 執行程序時，也可以對 OUTPUT 參數指定輸入值。 這可讓程序接收來自呼叫端程式的值，變更值或對值執行作業，然後將新值傳回給呼叫端程式。 在上述範例中，在程式呼叫 `@SalesYTDBySalesPerson` 程序之前，可先指派 `Sales.uspGetEmployeeSalesYTD` 變數的值。 EXECUTE 陳述式就會將 `@SalesYTDBySalesPerson` 變數值傳入 `@SalesYTD` OUTPUT 參數。 然後在程序主體中，此值可用於產生新值的計算。 程序結束時，新值可透過 OUTPUT 參數從程序傳回，更新 `@SalesYTDBySalesPerson` 變數中的值。 這通常稱為「以傳址方式傳遞的能力」。  
  
 呼叫程序時，如果您對參數指定 OUTPUT，但該參數在程序定義中並未使用 OUTPUT 來定義，則會出現錯誤訊息。 不過，您可以用輸出參數來執行程序，但在執行程序時不指定 OUTPUT。 這樣不會傳回錯誤，但您不能在呼叫程式中使用輸出值。  
  
### <a name="using-the-cursor-data-type-in-output-parameters"></a>在 OUTPUT 參數中使用 Cursor 資料類型  
 [!INCLUDE[tsql](../../includes/tsql-md.md)] 程序只能針對 OUTPUT 參數使用 **cursor** 資料類型。 如果為參數指定 **cursor** 資料類型，程序定義中也必須為該參數指定 VARYING 和 OUTPUT 關鍵字。 參數可以指定為僅 OUTPUT，但如果在參數宣告中指定 VARYING 關鍵字，資料類型必須是 **cursor** ，同時也必須指定 OUTPUT 關鍵字。  
  
> [!NOTE]  
>  **cursor** 資料類型不可透過 OLE DB、ODBC、ADO 以及 DB-Library 之類的資料庫 API 繫結至應用程式變數。 因為必須先繫結 OUTPUT 參數，然後應用程式才能執行程序，所以不能從資料庫 API 呼叫具有 **cursor** OUTPUT 參數的程序。 只有當 [!INCLUDE[tsql](../../includes/tsql-md.md)] cursor **OUTPUT 變數指派給** 本機 [!INCLUDE[tsql](../../includes/tsql-md.md)] cursor **變數時，才可以從** 批次、程序或觸發程序呼叫這些程序。  
  
### <a name="rules-for-cursor-output-parameters"></a>Cursor 輸出參數的規則  
 以下規則是有關程序執行時的 **cursor** 輸出參數：  
  
-   順向資料指標的結果集之中只會傳回程序執行結束時，位於或超過資料指標所在位置的資料列，例如：  
  
    -   一個無法捲動的資料指標在 RS 結果集 (有 100 個資料列) 的程序中開啟。  
  
    -   此程序擷取 RS 結果集的前 5 個資料列。  
  
    -   此程序傳回呼叫者。  
  
    -   傳回呼叫者的 RS 結果集是由 RS 的第 6 到第 100 個資料列所構成，而呼叫者中的資料指標位於 RS 的第一個資料列之前。  
  
-   如果順向資料指標在程序結束時位於第一個資料列之前，整個結果集都會傳回至呼叫的批次、程序或觸發程序。 傳回時，資料指標的位置會設定在第一個資料列之前。  
  
-   如果順向資料指標在程序結束時位於最後一個資料列之後，便會將空的結果集傳回至呼叫的批次、程序或觸發程序。  
  
    > [!NOTE]  
    >  空的結果集與 Null 值是不同的。  
  
-   如果是可捲動的資料指標，結果集之中的所有資料列都會在程序結束時傳回至呼叫的批次、程序或觸發程序。 傳回時，資料指標會留在程序中執行最後一次擷取的位置。  
  
-   任何類型的資料指標關閉時，都會將 Null 值傳回至呼叫的批次、程序或觸發程序。 如果將資料指標指派給某個參數但該資料指標從未開啟時，也會發生這種情況。  
  
    > [!NOTE]  
    >  關閉的狀態只在傳回時有影響。 舉例來說，在程序執行到一半時關閉資料指標，之後又開啟，然後將該資料指標的結果集傳回至呼叫的批次、程序或觸發程序，這些都是有效的。  
  
### <a name="examples-of-cursor-output-parameters"></a>Cursor 輸出參數的範例  
 在下列範例中，會建立一個程序，其使用 **cursor** 資料類型指定輸出參數 `@currency_cursor`。 然後在批次中呼叫程序。  
 
 首先，建立在 Currency 資料表上宣告並隨後開啟資料指標的程序。  
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID ( 'dbo.uspCurrencyCursor', 'P' ) IS NOT NULL  
    DROP PROCEDURE dbo.uspCurrencyCursor;  
GO  
CREATE PROCEDURE dbo.uspCurrencyCursor   
    @CurrencyCursor CURSOR VARYING OUTPUT  
AS  
    SET NOCOUNT ON;  
    SET @CurrencyCursor = CURSOR  
    FORWARD_ONLY STATIC FOR  
      SELECT CurrencyCode, Name  
      FROM Sales.Currency;  
    OPEN @CurrencyCursor;  
GO  
  
```  
  
 接下來，執行一個宣告區域資料指標變數的批次、執行此程序將資料指標指派給區域變數，再從資料指標擷取資料列。  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @MyCursor CURSOR;  
EXEC dbo.uspCurrencyCursor @CurrencyCursor = @MyCursor OUTPUT;  
WHILE (@@FETCH_STATUS = 0)  
BEGIN;  
     FETCH NEXT FROM @MyCursor;  
END;  
CLOSE @MyCursor;  
DEALLOCATE @MyCursor;  
GO  
  
```  
  
## <a name="returning-data-using-a-return-code"></a>使用傳回碼傳回資料  
 程序可以傳回稱為傳回碼的整數值，以指出程序的執行狀態。 若要為程序指定傳回碼，請使用 RETURN 陳述式。 就像使用 OUTPUT 參數一樣，若要在呼叫端程式中使用傳回碼，您必須在執行程序時將傳回碼儲存在變數中。 比方說，資料類型為 `@result` int **的指派變數** 會用於儲存來自程序 `my_proc`的傳回碼，例如：  
  
```sql
DECLARE @result int;  
EXECUTE @result = my_proc;  
```  
  
 傳回碼常用於程序的流程控制區塊中，以設定每個可能錯誤狀況的傳回碼值。 您可以在 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式之後使用 @@ERROR，來偵測陳述式執行過程中是否曾發生錯誤。  在 TSQL 傳回碼中引入 TRY/CATCH/THROW 錯誤處理之前，有時候必須先判斷預存程序成功或失敗。  預存程序應該一律使用錯誤 (必要時利用 THROW/RAISERROR 產生) 來表示失敗，而不仰賴傳回碼來表示失敗。  也請勿使用傳回碼來傳回應用程式資料。
  
### <a name="examples-of-return-codes"></a>傳回碼的範例  
 下列範例顯示具有可為各種錯誤設定特別傳回碼值之錯誤處理的 `usp_GetSalesYTD` 程序。 下表顯示由程序指派給每個可能錯誤的整數值，以及每個值對應的意義。  
  
|傳回碼值|意義|  
|-----------------------|-------------|  
|0|成功執行。|  
|1|未指定必要的參數值。|  
|2|指定的參數值無效。|  
|3|取得銷售值時發生錯誤。|  
|4|銷售人員有 NULL 銷售值。|  
  
```sql
USE AdventureWorks2012;  
GO  
IF OBJECT_ID('Sales.usp_GetSalesYTD', 'P') IS NOT NULL  
    DROP PROCEDURE Sales.usp_GetSalesYTD;  
GO  
CREATE PROCEDURE Sales.usp_GetSalesYTD  
@SalesPerson nvarchar(50) = NULL,  -- NULL default value  
@SalesYTD money = NULL OUTPUT  
AS    
  
-- Validate the @SalesPerson parameter.  
IF @SalesPerson IS NULL  
   BEGIN  
       PRINT 'ERROR: You must specify a last name for the sales person.'  
       RETURN(1)  
   END  
ELSE  
   BEGIN  
   -- Make sure the value is valid.  
   IF (SELECT COUNT(*) FROM HumanResources.vEmployee  
          WHERE LastName = @SalesPerson) = 0  
      RETURN(2)  
   END  
-- Get the sales for the specified name and   
-- assign it to the output parameter.  
SELECT @SalesYTD = SalesYTD   
FROM Sales.SalesPerson AS sp  
JOIN HumanResources.vEmployee AS e ON e.BusinessEntityID = sp.BusinessEntityID  
WHERE LastName = @SalesPerson;  
-- Check for SQL Server errors.  
IF @@ERROR <> 0   
   BEGIN  
      RETURN(3)  
   END  
ELSE  
   BEGIN  
   -- Check to see if the ytd_sales value is NULL.  
     IF @SalesYTD IS NULL  
       RETURN(4)   
     ELSE  
      -- SUCCESS!!  
        RETURN(0)  
   END  
-- Run the stored procedure without specifying an input value.  
EXEC Sales.usp_GetSalesYTD;  
GO  
-- Run the stored procedure with an input value.  
DECLARE @SalesYTDForSalesPerson money, @ret_code int;  
-- Execute the procedure specifying a last name for the input parameter  
-- and saving the output value in the variable @SalesYTD  
EXECUTE Sales.usp_GetSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDForSalesPerson OUTPUT;  
PRINT N'Year-to-date sales for this employee is ' +  
    CONVERT(varchar(10), @SalesYTDForSalesPerson);  
  
```  
  
 下列範例會建立程式以處理從 `usp_GetSalesYTD` 程序傳回的傳回碼。  
  
```sql
-- Declare the variables to receive the output value and return code   
-- of the procedure.  
DECLARE @SalesYTDForSalesPerson money, @ret_code int;  
  
-- Execute the procedure with a title_id value  
-- and save the output value and return code in variables.  
EXECUTE @ret_code = Sales.usp_GetSalesYTD  
    N'Blythe', @SalesYTD = @SalesYTDForSalesPerson OUTPUT;  
--  Check the return codes.  
IF @ret_code = 0  
BEGIN  
   PRINT 'Procedure executed successfully'  
   -- Display the value returned by the procedure.  
   PRINT 'Year-to-date sales for this employee is ' + CONVERT(varchar(10),@SalesYTDForSalesPerson)  
END  
ELSE IF @ret_code = 1  
   PRINT 'ERROR: You must specify a last name for the sales person.'  
ELSE IF @ret_code = 2   
   PRINT 'ERROR: You must enter a valid last name for the sales person.'  
ELSE IF @ret_code = 3  
   PRINT 'ERROR: An error occurred getting sales value.'  
ELSE IF @ret_code = 4  
   PRINT 'ERROR: No sales recorded for this employee.'     
GO  
  
```  
  
## <a name="see-also"></a>另請參閱  
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [PRINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/print-transact-sql.md)   
 [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [資料指標](../../relational-databases/cursors.md)   
 [RETURN &#40;Transact-SQL&#41;](../../t-sql/language-elements/return-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)  
  
  
