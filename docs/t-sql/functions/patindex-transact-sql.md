---
description: PATINDEX (Transact-SQL)
title: PATINDEX (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- PATINDEX
- PATINDEX_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- first occurrence of pattern [SQL Server]
- searches [SQL Server], pattern starting position
- starting position of patten search
- pattern searching [SQL Server]
- PATINDEX function
ms.assetid: c0dfb17f-2230-4e36-98da-a9b630bab656
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: c4d2ee21a4b2c2975fcead1e883cb28459c608dd
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "88363374"
---
# <a name="patindex-transact-sql"></a>PATINDEX (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  傳回指定之運算式中的模式，在所有有效文字和字元資料類型中第一次出現的起始位置，如果找不到模式，便傳回零。  
  
 ![主題連結圖示](../../database-engine/configure-windows/media/topic-link.gif "主題連結圖示") [Transact-SQL 語法慣例](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>語法  
  
```  
PATINDEX ( '%pattern%' , expression )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>引數
 *模式*  
 這是字元運算式，其中包含要尋找的順序。 此處可以使用萬用字元，但是 *pattern* 前後都必須加上 % 字元 (除非要搜尋第一個或最後一個字元)。 *pattern* 是字元字串資料類型類別目錄的運算式。 *pattern* 限制為 8000 個字元。

 > [!NOTE]
 > 雖然 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 原本就不支援傳統的規則運算式，但使用各種萬用字元運算式可以完成類似的複雜模式比對。 如需萬用字元語法的詳細資料，請參閱[字串運算子](../../t-sql/language-elements/string-operators-transact-sql.md)文件。
  
 *expression*  
 這是[運算式](../../t-sql/language-elements/expressions-transact-sql.md)，通常是搜尋指定之模式的資料行。 *expression* 屬於字元字串資料類型類別目錄。  
  
## <a name="return-types"></a>傳回型別  
若 *expression* 的資料類型為 **varchar(max)** 或 **nvarchar(max)**，則為 **bigint**，否則為 **int**。  
  
## <a name="remarks"></a>備註  
如果 *pattern* 或 *expression* 為 NULL，則 PATINDEX 會傳回 NULL。  
 
PATINDEX 的起始位置是 1。
 
PATINDEX 會以輸入的定序為基礎來執行比較。 若要執行指定定序的比較，您可以利用 COLLATE，將明確定序套用至輸入。  
  
## <a name="supplementary-characters-surrogate-pairs"></a>補充字元 (Surrogate 字組)  
使用 SC 定序時，傳回值會將 *expression* 參數中的任何 UTF-16 代理字組計算為單一字元。 如需詳細資訊，請參閱 [Collation and Unicode Support](../../relational-databases/collations/collation-and-unicode-support.md)。  
  
0x0000 (**char(0)**) 是 Windows 定序中未定義的字元，而且不得包含在 PATINDEX 中。  
  
## <a name="examples"></a>範例  
  
### <a name="a-simple-patindex-example"></a>A. 簡單的 PATINDEX 範例  
 下例範例會檢查 `ter`字元開頭位置的短字元字串 (`interesting data`)。  
  
```sql  
SELECT position = PATINDEX('%ter%', 'interesting data');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
3
```
  
### <a name="b-using-a-pattern-with-patindex"></a>B. 搭配 PATINDEX 使用模式  
下列範例會尋找 `ensure` 模式在 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 資料庫之 `DocumentSummary` 資料表中 `Document` 資料行之特定資料列中的起始位置。  
  
```sql  
SELECT position = PATINDEX('%ensure%',DocumentSummary)  
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
GO   
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
64  
```  
  
如果您沒有利用 `WHERE` 子句來限制要搜尋的資料列，查詢會傳回資料表的所有資料列，且針對找到模式的所有資料列報告非零值，找不到模式的所有資料列則會報告零。  
  
### <a name="c-using-wildcard-characters-with-patindex"></a>C. 搭配 PATINDEX 使用萬用字元  
 下列範例在指定的字串中 (索引從 1 開始)，使用 % 和 _ 萬用字元來尋找模式 `'en'` 後面接著任何一個字元和 `'ure'` 的開始位置：  
  
```sql  
SELECT position = PATINDEX('%en_ure%', 'Please ensure the door is locked!');  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
8  
```  
  
`PATINDEX` 的運作方式就像 `LIKE`，所以您可以使用任何萬用字元。 您不必用百分比括住模式。 `PATINDEX('a%', 'abc')` 會傳回 1 且 `PATINDEX('%a', 'cba')` 會傳回 3。  
  
 與 `LIKE` 不同的是，`PATINDEX` 會傳回位置，類似 `CHARINDEX`。  

### <a name="d-using-complex-wildcard-expressions-with-patindex"></a>D. 使用複雜的萬用字元運算式搭配 PATINDEX 
下列範例會使用 `[^]` [字串運算子](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)尋找不是數字、字母或空格的字元位置。

```sql
SELECT position = PATINDEX('%[^ 0-9A-z]%', 'Please ensure the door is locked!'); 
```
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
33
```

### <a name="e-using-collate-with-patindex"></a>E. 搭配 PATINDEX 使用 COLLATE  
 下列範例利用 `COLLATE` 函數來明確指定所搜尋之運算式的定序。  
  
```sql  
USE tempdb;  
GO  
SELECT PATINDEX ( '%ein%', 'Das ist ein Test'  COLLATE Latin1_General_BIN) ;  
GO  
```  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
position
--------
9
```

### <a name="f-using-a-variable-to-specify-the-pattern"></a>F. 使用變數來指定模式  
下列範例會使用變數，將值傳遞給 *pattern* 參數。 這個範例會使用 [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] 資料庫。  
  
```sql  
DECLARE @MyValue VARCHAR(10) = 'safety';   
SELECT position = PATINDEX('%' + @MyValue + '%', DocumentSummary)   
FROM Production.Document  
WHERE DocumentNode = 0x7B40;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
position
--------  
22
```  
  
## <a name="see-also"></a>另請參閱  
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [CHARINDEX &#40;Transact-SQL&#41;](../../t-sql/functions/charindex-transact-sql.md)  
 [LEN &#40;Transact-SQL&#41;](../../t-sql/functions/len-transact-sql.md)  
 [資料類型 &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [字串函數 &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)   
 [&#40;萬用字元 - 相符的字元&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-to-match-transact-sql.md)   
 [&#40;萬用字元 - 不相符的字元&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-character-s-not-to-match-transact-sql.md)   
 [_ &#40;萬用字元 - 符合單一字元&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/wildcard-match-one-character-transact-sql.md)   
 [百分比字元&#40;萬用字元 - 相符的字元&#41; &#40;Transact-SQL&#41;](../../t-sql/language-elements/percent-character-wildcard-character-s-to-match-transact-sql.md)  
  
  


