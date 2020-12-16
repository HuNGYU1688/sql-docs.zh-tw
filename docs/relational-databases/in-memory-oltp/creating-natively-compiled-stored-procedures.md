---
title: 建立原生編譯的預存程序 | Microsoft Docs
description: 了解僅針對原生編譯的預存程序支援的 Transact-SQL 功能。 查看如何在 SQL Server 中建立原生編譯的預存程序。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: e6b34010-cf62-4f65-bbdf-117f291cde7b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 398e9dee801465cffd85d3ce65f0e344318b4cf1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481239"
---
# <a name="creating-natively-compiled-stored-procedures"></a>建立原生編譯的預存程序
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

原生編譯預存程序不會實作完整 [!INCLUDE[tsql](../../includes/tsql-md.md)] 可程式性和查詢介面區。 某些 [!INCLUDE[tsql](../../includes/tsql-md.md)] 建構無法在原生編譯的預存程序內使用。 如需詳細資訊，請參閱 [原生編譯的 T-SQL 模組支援的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md)。  
  
只有原生編譯預存程序支援下列 [!INCLUDE[tsql](../../includes/tsql-md.md)] 功能：  
  
-   不可部分完成的區塊。 如需詳細資訊，請參閱 [Atomic Blocks](../../relational-databases/in-memory-oltp/atomic-blocks-in-native-procedures.md)。  
  
-   參數和變數上的 `NOT NULL` 限制式。 您不能指派 **NULL** 值給宣告為 **NOT NULL** 的參數或變數。 如需詳細資訊，請參閱 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)。  
  
    -   `CREATE PROCEDURE dbo.myproc (@myVarchar VARCHAR(32) NOT NULL) AS (...)`  
  
    -   `DECLARE @myVarchar VARCHAR(32) NOT NULL = "Hello"; -- Must initialize to a value.`  
  
    -   `SET @myVarchar = NULL; -- Compiles, but fails during run time.`  
  
-   原生編譯預存程序的結構描述繫結。  
  
使用 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)建立原生編譯的預存程序。 下列範例會顯示記憶體最佳化的資料表以及用來將資料列插入資料表中的原生編譯預存程序。  
  
```sql  
CREATE TABLE [dbo].[T2] (  
  [c1] [int] NOT NULL, 
  [c2] [datetime] NOT NULL,
  [c3] nvarchar(5) NOT NULL, 
  CONSTRAINT [PK_T1] PRIMARY KEY NONCLUSTERED ([c1])  
  ) WITH ( MEMORY_OPTIMIZED = ON , DURABILITY = SCHEMA_AND_DATA )  
GO  
  
CREATE PROCEDURE [dbo].[usp_2] (@c1 int, @c3 nvarchar(5)) 
WITH NATIVE_COMPILATION, SCHEMABINDING  
AS BEGIN ATOMIC WITH  
(  
 TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE = N'us_english'  
)  
  DECLARE @c2 datetime = GETDATE();  
  INSERT INTO [dbo].[T2] (c1, c2, c3) values (@c1, @c2, @c3);  
END  
GO  
```  
 
在程式碼範例中， **NATIVE_COMPILATION** 表示這個 [!INCLUDE[tsql](../../includes/tsql-md.md)] 預存程序是原生編譯的預存程序。 以下是必要的選項：  
  
|選項|描述|  
|------------|-----------------|  
|**SCHEMABINDING**|原生編譯的預存程序必須繫結至其所參考之物件的結構描述。 這表示，此程序所參考的資料表將無法卸除。 此程序中參考的資料表必須包含其結構描述名稱，而且查詢中不允許使用萬用字元 (\*) (表示沒有 `SELECT * from...`)。 只有這個 **版本中的原生編譯預存程序才支援** SCHEMABINDING [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。|  
|**BEGIN ATOMIC**|原生編譯的預存程序主體必須剛好由一個不可部分完成的區塊所組成。 不可部分完成的區塊保證會以不可部分完成的方式執行預存程序。 如果此程序在使用中交易的內容之外叫用，它將會開始新的交易，該交易會在不可部分完成的區塊結尾認可。 原生編譯預存程序中不可部分完成的區塊有兩個必要選項：<br /><br /> **TRANSACTION ISOLATION LEVEL** 如需支援的隔離等級，請參閱 [記憶體最佳化資料表的交易隔離等級](/previous-versions/sql/sql-server-2016/dn133175(v=sql.130)) 。<br /><br /> **LANGUAGE**。 預存程序的語言必須設定為其中一個可用語言或語言別名。|  
  
## <a name="see-also"></a>另請參閱  
 [原生編譯的預存程序](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
