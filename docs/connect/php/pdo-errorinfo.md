---
title: PDO::errorInfo
description: Microsoft PDO_SQLSRV Driver for PHP for SQL Server 中的 PDO::errorInfo 函式適用的 API 參考。
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9d5481d5-13bc-4388-b3aa-78676c0fc709
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 579b516481c6349396a342e2c98200d70d731d25
ms.sourcegitcommit: 331b8495e4ab37266945c81ff5b93d250bdaa6da
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/20/2020
ms.locfileid: "88646168"
---
# <a name="pdoerrorinfo"></a>PDO::errorInfo
[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

擷取資料庫控制代碼上最近作業的延伸錯誤資訊。  
  
## <a name="syntax"></a>語法  
  
```  
  
array PDO::errorInfo();  
```  
  
## <a name="return-value"></a>傳回值  
關於資料庫控制代碼上最近作業的錯誤資訊陣列。 此陣列包含下列欄位：  
  
-   SQLSTATE 錯誤碼。  
  
-   驅動程式特有的錯誤碼。  
  
-   驅動程式特有的錯誤訊息。  
  
如果沒有發生錯誤，或如果未設定 SQLSTATE，則驅動程式特定欄位會是 NULL。  
  
## <a name="remarks"></a>備註  
PDO::errorInfo 只會針對直接在資料庫上執行的作業擷取錯誤資訊。 使用 PDO::prepare 或 PDO::query 建立 PDOStatement 執行個體時，請使用 PDOStatement::errorInfo。  
  
[!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]2.0 版已加入 PDO 支援。  
  
## <a name="example"></a>範例  
在此範例中，資料行名稱的拼字錯誤 (`Cityx` 而不是 `City`)，因而導致錯誤並隨之進行報告。  
  
```  
<?php  
$conn = new PDO( "sqlsrv:server=(local) ; Database = AdventureWorks ", "");  
$query = "SELECT * FROM Person.Address where Cityx = 'Essen'";  
  
$conn->query($query);  
print $conn->errorCode();  
echo "\n";  
print_r ($conn->errorInfo());  
?>  
```  
  
## <a name="see-also"></a>另請參閱  
[PDO 類別](../../connect/php/pdo-class.md)

[PDO](https://php.net/manual/book.pdo.php)  
  
