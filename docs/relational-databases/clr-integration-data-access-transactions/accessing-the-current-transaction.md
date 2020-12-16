---
title: 存取目前的交易 |Microsoft Docs
description: 在 SQL Server CLR 整合中，system.string 類別的 Current 屬性可讓您存取目前的交易。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- current transaction access
- Current property
- Transaction class
ms.assetid: 1a4e2ce5-f627-4c81-8960-6a9968cefda2
author: rothja
ms.author: jroth
ms.openlocfilehash: b9220519a926ca75a00843dba54dfad797064aee
ms.sourcegitcommit: 52252e8b4c9b50d3915aecd3135e8901d345e7e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/16/2020
ms.locfileid: "97599845"
---
# <a name="accessing-the-current-transaction"></a>存取目前交易
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果在輸入執行的 common language runtime (CLR) 程式碼的時間點，交易會 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 透過 system.string 類別公開。  **Transaction. current** 屬性是用來存取目前的交易。 在大部分情況下，您不需要明確存取交易。 針對資料庫連接，ADO.NET 會在 **連接** 時自動檢查 **Transaction。** 呼叫 Open 方法時，會在該交易中明確地登記連接， (除非連接字串) 中的 [**登錄關鍵字]** 設為 false。  
  
 在下列案例中，您可能會想要直接使用 **交易** 物件：  
  
-   如果您想要編列不會自動編列的資源，或由於某些原因，無法在初始化期間編列的資源。  
  
-   如果您想要在交易中明確編列資源。  
  
-   如果您想要從預存程序或函數內部結束外部交易。 在此情況下，您可以使用 <xref:System.Transactions.TransactionScope>。 例如，下列程式碼將回復目前的交易：  
  
    ```  
    using(TransactionScope transactionScope = new TransactionScope(TransactionScopeOptions.Required)) { }  
    ```  
  
 本主題的其餘部分將描述取消外部交易的其他方式。  
  
## <a name="canceling-an-external-transaction"></a>取消外部交易  
 您可以使用下列方式，從 Managed 程序或函數取消外部交易：  
  
-   Managed 程序或函數可以使用輸出參數來傳回值。 呼叫程式 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可以檢查傳回的值，並在適當的情況下執行 **回復交易**。  
  
-   Managed 程序或函數可以擲回自訂例外狀況。 呼叫 [!INCLUDE[tsql](../../includes/tsql-md.md)] 程式可以攔截 try/catch 區塊中 managed 程式或函數所擲回的例外狀況，並執行 **回復交易**。  
  
-   若符合特定條件，則 managed 程式或函數可以藉由呼叫 **transaction** 來取消目前的交易。  
  
 在 managed 程式或函數中呼叫它時， **交易. Rollback** 方法會擲回例外狀況，其中包含不明確的錯誤訊息，而且可以包裝在 try/catch 區塊中。 錯誤訊息如下所示：  
  
```  
Msg 3994, Level 16, State 1, Procedure uspRollbackFromProc, Line 0  
Transaction is not allowed to roll back inside a user defined routine, trigger or aggregate because the transaction is not started in that CLR level. Change application logic to enforce strict transaction nesting.  
```  
  
 這項例外狀況是在預期中，而必須有 try/catch 區塊，程式碼才能繼續執行。 如果沒有 try/catch 區塊，例外狀況就會立即擲回給呼叫 [!INCLUDE[tsql](../../includes/tsql-md.md)] 程序，而且 Managed 程式碼執行將完成。 當 Managed 程式碼完成執行時，會引發另一項例外狀況：  
  
```  
Msg 3991, Level 16, State 1, Procedure uspRollbackFromProc, Line 1   
The context transaction which was active before entering user defined routine, trigger or aggregate " uspRollbackFromProc " has been ended inside of it, which is not allowed. Change application logic to enforce strict transaction nesting. The statement has been terminated.  
```  
  
 這項例外狀況也在預期中，而且您必須在執行引發觸發程序之動作的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式前後設有 try/catch 區塊，才能繼續執行。 儘管會擲回兩項例外狀況，交易仍會回復，而且不會認可變更。  
  
### <a name="example"></a>範例  
 下列範例是使用 **transaction** 方法，從 managed 程式復原的交易。 請注意交易前後的 try/catch 區塊。在 managed 程式碼中， **Rollback** 方法。 [!INCLUDE[tsql](../../includes/tsql-md.md)] 指令碼會建立組件和 Managed 預存程序。 請注意， **EXEC uspRollbackFromProc** 語句會包裝在 try/catch 區塊中，因此攔截到 managed 程式完成執行時所擲回的例外狀況。  
  
```csharp  
using System;  
using System.Data;  
using System.Data.SqlClient;  
using System.Data.SqlTypes;  
using Microsoft.SqlServer.Server;  
using System.Transactions;  
  
public partial class StoredProcedures  
{  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void uspRollbackFromProc()  
{  
   using (SqlConnection connection = new SqlConnection(@"context connection=true"))  
   {  
      // Open the connection.  
      connection.Open();  
  
      bool successCondition = true;  
  
      // Success condition is met.  
      if (successCondition)  
      {  
         SqlContext.Pipe.Send("Success condition met in procedure.");   
         // Perform other actions here.  
      }  
  
      //  Success condition is not met, the transaction will be rolled back.  
      else  
      {  
         SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...");  
         try  
         {  
               // Get the current transaction and roll it back.  
               Transaction trans = Transaction.Current;  
               trans.Rollback();  
         }  
         catch (SqlException ex)  
         {  
            // Catch the expected exception.   
            // This allows the connection to close correctly.                      
         }    
      }  
  
      // Close the connection.  
      connection.Close();  
   }  
}  
};  
```  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Data.SqlClient  
Imports System.Data.SqlTypes  
Imports Microsoft.SqlServer.Server  
Imports System.Transactions  
  
Partial Public Class StoredProcedures  
<Microsoft.SqlServer.Server.SqlProcedure()> _  
Public Shared Sub  uspRollbackFromProc ()  
   Using connection As New SqlConnection("context connection=true")  
  
   ' Open the connection.  
   connection.Open()  
  
   Dim successCondition As Boolean  
   successCondition = False  
  
   ' Success condition is met.  
   If successCondition Then  
  
      SqlContext.Pipe.Send("Success condition met in procedure.")  
  
      ' Success condition is not met, the transaction will be rolled back.  
  
   Else  
      SqlContext.Pipe.Send("Success condition not met in managed procedure. Transaction rolling back...")  
      Try  
         ' Get the current transaction and roll it back.  
         Dim trans As Transaction  
         trans = Transaction.Current  
         trans.Rollback()  
  
      Catch ex As SqlException  
         ' Catch the exception instead of throwing it.    
         ' This allows the connection to close correctly.                      
      End Try  
  
   End If  
  
   ' Close the connection.  
   connection.Close()  
  
End Using  
End Sub  
End Class  
```  
  
 **Transact-SQL**  
  
```  
--Register assembly.  
CREATE ASSEMBLY TestProcs FROM 'C:\Programming\TestProcs.dll'   
Go  
CREATE PROCEDURE uspRollbackFromProc AS EXTERNAL NAME TestProcs.StoredProcedures.uspRollbackFromProc  
Go  
  
-- Execute procedure.  
BEGIN TRY  
BEGIN TRANSACTION   
-- Perform other actions.  
Exec uspRollbackFromProc  
-- Perform other actions.  
PRINT N'Commiting transaction...'  
COMMIT TRANSACTION  
END TRY  
  
BEGIN CATCH  
SELECT ERROR_NUMBER() AS ErrorNum, ERROR_MESSAGE() AS ErrorMessage  
PRINT N'Exception thrown, rolling back transaction.'  
ROLLBACK TRANSACTION  
PRINT N'Transaction rolled back.'   
END CATCH  
Go  
  
-- Clean up.  
DROP Procedure uspRollbackFromProc;  
Go  
DROP ASSEMBLY TestProcs;  
Go  
```  
  
## <a name="see-also"></a>另請參閱  
 [CLR 整合和交易](../../relational-databases/clr-integration-data-access-transactions/clr-integration-and-transactions.md)  
  
  
