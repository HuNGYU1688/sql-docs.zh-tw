---
title: '將批註式 XDR 架構轉換為 XSD (SQLXML) '
description: 瞭解如何在 SQLXML 4.0 中使用 XDR 至 XSD 轉換器工具，將批註式 XDR 架構轉換為相等的 XSD 架構。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- annotated XDR schemas, converting schemas
- annotated XSD schemas, converting schemas
- XDR to XSD Converter tool [SQLXML]
- XDR schemas [SQLXML], converting
- converting annotated schemas
- mapping schema [SQLXML], conversions
- XSD schemas [SQLXML], converting schemas
ms.assetid: 151c94a8-66d3-4c46-a5ff-a22df456940a
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7936bf8d9d9c37620ef9b692c125ff16f1cea85
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467109"
---
# <a name="converting-annotated-xdr-schemas-to-equivalent-xsd-schemas-sqlxml-40"></a>將註解式 XDR 結構描述轉換為等效 XSD 結構描述 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  XML 結構描述定義 (XSD) 語言是 XML-Data Reduced (XDR) 結構描述定義語言的後續版本。 透過在 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] SQLXML 4.0 中導入的 XSD 支援，系統會假設新的註解式結構描述是使用 XSD 建立的。 SQLXML 4.0 包含 XDR 轉成 XSD 轉換器工具，這項工具是設計來協助您將現有的註解式 XDR 結構描述轉換成對等的 XSD 結構描述。  
  
> [!IMPORTANT]  
>  只有當您想要將註解式 XDR 結構描述轉換成 XSD 以便搭配 SQLXML 4.0 使用時，才應該使用這項工具。 這不是一般用途的 XDR 轉成 XSD 轉換器工具。 在其他環境中使用時，轉換的 XSD 結構描述可能會與原始 XDR 結構描述具有不同的行為。  
  
 如果輸入 XDR 檔案在 XML 宣告內部指定了編碼，這就會成為所產生之 XSD 輸出檔案的編碼。  
  
 轉換器工具 (Cvtschema.exe) 會安裝在 Program Files\SQLXML 4.0\bin 資料夾中，而且它是在命令提示字元中執行。  
  
 下面是一般語法：  
  
```  
cvtschema XDRFileName, [-y], [-w] [-?]  
```  
  
 其中：  
  
 XDRFileName  
 這是要轉換成 XSD 之 XDR 檔案的名稱。 此工具會讀取輸入 XDR 檔案並且在目前的工作目錄中建立 XSD 輸出檔案。 如果輸入檔案具有 .xdr 或 .xml 副檔名，系統就會使用相同的名稱與 .xsd 副檔名來建立輸出 XSD 檔案。 如果輸入檔的副檔名不是 .xml 或 xdr (或遺漏延伸模組) ，則會使用相同的名稱建立輸出檔，並將 .xsd 副檔名附加至輸入檔名稱。 例如，如果輸入 XDR 檔案名稱為 SampleFile.abc，產生的 XSD 就會儲存成 SampleFile.abc.xsd。  
  
 -y  
 (選擇性) 使用轉換器工具所產生的 XSD 檔案來覆寫現有的 XSD 檔案。 如果沒有指定旗標，此工具會提示您指定是否要覆寫現有的 XSD 檔案，並提供變更輸出檔案名稱的選項。  
  
 -w  
 (選擇性) 傳回在轉換程序中由此工具產生的非嚴重警告。 根據預設，此工具只會針對嚴重錯誤顯示訊息。  
  
 -?  
 傳回可使用 **cvtschema** 指定的選項清單，以及說明。  
  
## <a name="see-also"></a>另請參閱  
 [將 XSD 資料類型對應到 XPath 資料類型 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/mapping-xsd-data-types-to-xpath-data-types-sqlxml-4-0.md)   
 [&#40;SQLXML 4.0&#41;的 XSD 批註 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-using/xsd-annotations-sqlxml-4-0.md)  
  
  
