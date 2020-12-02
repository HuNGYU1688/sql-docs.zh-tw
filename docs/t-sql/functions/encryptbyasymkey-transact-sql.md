---
description: ENCRYPTBYASYMKEY (Transact-SQL)
title: ENCRYPTBYASYMKEY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- ENCRYPTBYASYMKEY
- ENCRYPTBYASYMKEY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ENCRYPTBYASYMKEY function
- encryption [SQL Server], asymmetric keys
- asymmetric keys [SQL Server], ENCRYPTBYASYMKEY function
ms.assetid: 86bb2588-ab13-4db2-8f3c-42c9f572a67b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 42ffb61535beefeee149124adf7873cce78c1535
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128499"
---
# <a name="encryptbyasymkey-transact-sql"></a>ENCRYPTBYASYMKEY (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

此函式使用非對稱金鑰為資料加密。  
  
 ![文章連結圖示](../../database-engine/configure-windows/media/topic-link.gif "文章連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```syntaxsql
EncryptByAsymKey ( Asym_Key_ID , { 'plaintext' | @plaintext } )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
*asym_key_ID*  
資料庫中的非對稱金鑰識別碼。 *asym_key_ID* 具有 **int** 資料類型。  
  
*cleartext*  
`ENCRYPTBYASYMKEY` 會使用以非對稱金鑰來加密的資料字串。 *cleartext* 可以具有
 
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
或
  
+ **varchar**
 
資料類型。  
  
**\@plaintext**  
`ENCRYPTBYASYMKEY` 會使用非對稱金鑰來加密其值的變數。 **\@plaintext** 可以是
  
+ **binary**
+ **char**
+ **nchar**
+ **nvarchar**
+ **varbinary**
  
或
  
+ **varchar**
 
資料類型。  
  
## <a name="return-types"></a>傳回型別  
**varbinary**，大小上限為 8,000 個位元組。  
  
## <a name="remarks"></a>備註  
相較於對稱金鑰加密和解密，使用非對稱金鑰的加密和解密作業會耗用大量資源，因此變得非常昂貴。 建議開發人員避免在大型資料集上執行非對稱金鑰加密和解密作業；例如，儲存在資料庫資料表中的使用者資料集。 相反地，建議開發人員先使用強式對稱金鑰將該資料加密，再使用非對稱金鑰將該對稱金鑰加密。  
  
根據演算法，如果輸入超過特定位元組數目，`ENCRYPTBYASYMKEY` 會傳回 **NULL**。 特定限制：

+ 512 位元 RSA 金鑰最多可以加密 53 個位元組
+ 1024 位元金鑰最多可以加密 117 個位元組
+ 2048 位元金鑰最多可以加密 245 個位元組

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中，憑證和非對稱金鑰都可作為 RSA 金鑰上的包裝函式。  
  
## <a name="examples"></a>範例  
此範例會使用非對稱金鑰 `JanainaAsymKey02` 將儲存在 `@cleartext` 中的文字加密。 陳述式會將加密資料插入 `ProtectedData04` 資料表中。  
  
```sql  
INSERT INTO AdventureWorks2012.Sales.ProtectedData04   
    VALUES( N'Data encrypted by asymmetric key ''JanainaAsymKey02''',  
    EncryptByAsymKey(AsymKey_ID('JanainaAsymKey02'), @cleartext) );  
GO  
```  
  
## <a name="see-also"></a>另請參閱  
 [DECRYPTBYASYMKEY &#40;Transact-SQL&#41;](../../t-sql/functions/decryptbyasymkey-transact-sql.md)   
 [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-asymmetric-key-transact-sql.md)   
 [加密階層](../../relational-databases/security/encryption/encryption-hierarchy.md)  
  
