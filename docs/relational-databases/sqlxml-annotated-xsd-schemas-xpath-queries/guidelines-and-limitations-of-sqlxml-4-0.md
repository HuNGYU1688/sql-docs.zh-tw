---
title: SQLXML 4.0 的指導方針和限制
description: 瞭解使用 SQLXML 4.0 的指導方針和限制。
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, about SQLXML
ms.assetid: fe433d30-90a1-421e-85c6-af13294dc18d
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 305565d542144a7e6e24663aadba1fb56a67eddb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97431281"
---
# <a name="guidelines-and-limitations-of-sqlxml-40"></a>SQLXML 4.0 的指導方針和限制
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  當您使用 SQLXML 4.0 時，請記住以下事項：  
  
-   當做查詢結果傳回的 XML 不會針對產生此 XML 的對應結構描述進行驗證。  
  
-   SQLXML 4.0 同時包含與版本無關以及與版本相關的 PROGID。 建議您所有的實際執行應用程式都應該使用與版本相關的 PROGID。 這一點特別重要，因為 SQLXML 4.0 並非完全具有回溯相容性。 當您安裝更新的版本時，使用與版本相關的 PROGID 會防止您的實際執行可能發生的失敗。 在不同的版本之間，程式行為可能會因為各種原因而失敗，例如錯誤修正、可能的設計變更等等。 當您安裝更新的版本時，使用與版本相關的 PROGID 會防止發生非預期的失敗。 如果在您安裝更新的版本時使用與版本相關的 PROGID，您的應用程式將會繼續運作，而不會發生失敗。 如果您決定變更與之前版本相關的 PROGID，並在較新的版本中使用與最近版本相關的 PROGID，您必須先測試應用程式，然後才能實際執行它。 例如，使用與版本無關之 PROGID 的應用程式可能會在以下案例中發生失敗：  
  
     您正在執行使用 SQLXML 4.0 以及與版本無關之 PROGID 的應用程式，而且您決定安裝某個其他軟體程式。 此程式可能會安裝舊版的 SQLXML。 您的應用程式可能會失敗，因為應用程式內與版本無關的 PROGIDS 現在指向舊版的 SQLXML，它不一定擁有您的應用程式所使用的 SQLXML 功能。  
  
-   如果您基於任何原因而不想要使用 SQLXMLOLEDB 提供者，而想要使用適用于 SQLXML 功能的 SQLOLEDB 提供者，請將 [ **Sqlxml 版本** ] 屬性設定為 "sqlxml. 4.0"。  
  
  
