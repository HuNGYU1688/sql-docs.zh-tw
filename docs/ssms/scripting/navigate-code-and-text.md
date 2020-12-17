---
title: 導覽程式碼與文字
description: 了解如何使用各種不同的技術來巡覽文件：為位置設定書籤，以更容易返回該位置；以累加方式搜尋；使用滑鼠和鍵盤；然後，使用 [移至] 命令，以藉由指定行號來移至某行。
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- searches [SQL Server Management Studio], incremental
- mouse [SQL Server Management Studio]
- bookmarks [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], navigating code
- Query Editor [SQL Server Management Studio], Go To Command
- incremental searches [SQL Server Management Studio]
- Query Editor [SQL Server Management Studio], bookmarks
- Query Editor [SQL Server Management Studio], mouse
- navigating code
- Go To command
ms.assetid: f63247ff-9751-4e99-8ee3-0772ad4009d0
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5c96185636472a1bf7cc512fa4947597289f38e5
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97474279"
---
# <a name="navigate-code-and-text"></a>導覽程式碼與文字

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

您可以利用下列方式在文字間移動：  
  
-   書籤。  
  
-   漸進式搜尋。  
  
-   滑鼠和導覽鍵。  
  
-   [移至] 命令。  
  
> [!NOTE]  
>  如需完整的鍵盤快速鍵清單，請參閱 [SQL Server Management Studio 鍵盤快速鍵](../../ssms/sql-server-management-studio-keyboard-shortcuts.md)。  
  
## <a name="navigating-with-bookmarks"></a>利用書籤導覽  
 若要在其他位置編輯文件，之後，再返回原始位置，請加入書籤。 您先設定好書籤，再利用鍵盤快速鍵來移到書籤。 請在書籤視窗中檢視書籤。  
  
## <a name="incremental-search"></a>漸進式搜尋  
 漸進式搜尋可協助您在輸入搜尋字元時，直接導覽到目前文件中的位置。 您可以利用鍵盤快速鍵來存取漸進式搜尋。  
  
## <a name="navigating-with-the-mouse-and-keyboard"></a>利用滑鼠和鍵盤來導覽  
 導覽文字最常見的方法是使用滑鼠和導覽鍵：  
  
-   利用向左鍵和向右鍵，每次移動一個字元，或結合 CTRL 鍵，每次移動一個單字。 向上鍵和向下鍵會每次移動一行。  
  
-   按一下某個位置，以將游標放在該處。  
  
-   利用捲軸或滑鼠滾輪，在文字間移動。  
  
-   利用 HOME、END、PAGE UP 和 PAGE DOWN 鍵，在文字間移動。  
  
-   利用 CTRL+PAGE UP 和 CTRL+PAGE DOWN，分別將插入點移到視窗頂端或底端。  
  
-   利用 CTRL+向上鍵和 CTRL+向下鍵，在不移動插入點的情況下，捲動檢視。  
  
## <a name="go-to-command"></a>移至命令  
 請使用 [移至] 命令移至特定行號。 若要顯示行號，請在 選項 對話方塊中，依序按一下 文字編輯器所有語言一般，然後選取 行號。  
  
 **移至特定行號**  
  
1.  在 [編輯] 功能表上，按一下 [移至]  
  
2.  輸入您想檢視的行號