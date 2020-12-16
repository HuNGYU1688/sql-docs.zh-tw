---
title: 記憶體內部 OLTP 的 Transact-SQL 支援 | Microsoft 文件
description: 了解包含語法選項以支援記憶體內部 OLTP 的 Transact-SQL 陳述式。 使用連結以取得關於支援功能的其他參考。
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: b1cc7c30-1747-4c21-88ac-e95a5e58baac
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7dc86572c8a1ee4b8d63e9705a9a5295565c3808
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97438718"
---
# <a name="transact-sql-support-for-in-memory-oltp"></a>記憶體中 OLTP 的 Transact-SQL 支援
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  以下 [!INCLUDE[tsql](../../includes/tsql-md.md)] 陳述式包含語法選項，可支援記憶體內部 OLTP：  
  
-   [ALTER DATABASE 檔案及檔案群組選項 &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-file-and-filegroup-options.md) (已加入 **MEMORY_OPTIMIZED_DATA**)  
  
-   [ALTER TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/alter-trigger-transact-sql.md)  
  
-   [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md) (已加入 **MEMORY_OPTIMIZED_DATA**)  
  
-   [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)  
  
-   [CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
  
-   [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)  
  
-   [CREATE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-type-transact-sql.md)  
  
-   [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
    在原生編譯的預存程序中，您可以將變數宣告為 **NOT NULL**。 您無法在一般的預存程序中進行。  
  
 從 SQL Server 2016 開始，**AUTO_UPDATE_STATISTICS** 可以是記憶體最佳化資料表的 **ON**。 如需詳細資訊，請參閱 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)。  
  
 原生編譯的預存程序不支援 [SET STATISTICS XML &#40;Transact-SQL&#41;](../../t-sql/statements/set-statistics-xml-transact-sql.md) ON。  
  
 如需不支援功能的詳細資訊，請參閱[記憶體內部 OLTP 不支援 Transact-SQL 建構](../../relational-databases/in-memory-oltp/transact-sql-constructs-not-supported-by-in-memory-oltp.md)。  
  
 如需原生編譯預存程序內和上方支援之建構的相關資訊，請參閱 [原生編譯 T-SQL 模組支援的功能](../../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md) 和 [原生編譯 T-SQL 模組支援的 DDL](../../relational-databases/in-memory-oltp/supported-ddl-for-natively-compiled-t-sql-modules.md)。  
  
## <a name="see-also"></a>另請參閱  
 [記憶體內部 OLTP &#40;記憶體內部最佳化&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)   
 [原生編譯預存程序的移轉問題](./a-guide-to-query-processing-for-memory-optimized-tables.md)   
 [記憶體內部 OLTP 不支援的 SQL Server 功能](../../relational-databases/in-memory-oltp/unsupported-sql-server-features-for-in-memory-oltp.md)   
 [原生編譯的預存程序](./a-guide-to-query-processing-for-memory-optimized-tables.md)  
  
