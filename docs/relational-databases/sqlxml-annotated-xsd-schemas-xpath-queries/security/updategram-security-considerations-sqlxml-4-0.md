---
title: Updategram (SQLXML) 的安全性考慮
description: 瞭解在 SQLXML 4.0 中使用 updategram 的安全性指導方針。
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, updategrams
- security [SQLXML], updategrams
- updategrams [SQLXML], security
ms.assetid: 00dc6cf4-a2e8-4cca-bdd6-d5122102a82d
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 23214cd1120061f6da9c6df46f08fd1c7a0df04a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479229"
---
# <a name="updategram-security-considerations-sqlxml-40"></a>Updategram 安全性考量 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  下面是使用 Updategram 的安全性指導方針：  
  
-   當您使用 Updategram 來更新資料時，請避免使用預設對應。 當您使用預設對應時，Updategram 中的元素名稱會對應至資料表名稱，而且屬性名稱會對應至資料行。 這樣會公開資料庫中的資料庫資料表和資料行資訊，因而可能產生安全性風險。 不過，如果您指定不同的對應結構描述 (將 Updategram 中的元素和屬性對應至資料庫資料表和資料行)，您的 Updategram 元素和屬性名稱可能就是任意的，而且此結構描述會針對這些名稱與資料庫資料表和資料行進行必要的對應。 因此，資料庫資訊就不會在 Updategram 中公開。  
  
-   請勿允許使用者建立和執行其 Updategram。 建議您將 Updategram 放置於伺服器上成為範本，而非以動態方式在 ASP 類型的應用程式中建立它們，因為這樣會讓資料庫中的資料面臨風險。 僅允許使用者透過當做範本提供的 Updategram 來存取資料可以降低這項風險。  
  
## <a name="see-also"></a>另請參閱  
 [使用 Updategram 來修改 SQLXML 4.0 中的資料](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/using-updategrams-to-modify-data-in-sqlxml-4-0.md)  
  
  
