---
title: 設定編輯器 (SQL Server Management Studio)
description: 了解如何透過在 [選項] 對話方塊中設定選項，以自訂 SQL Server Management Studio 編輯器的作業。
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: e7c7a8ef-f561-4258-a7b6-c445dba69f87
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/01/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6da583c30bae7c58c463a9518545f36274e19ce6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476909"
---
# <a name="configure-editors-sql-server-management-studio"></a>設定編輯器 (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

您可以設定各個 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 編輯器的選項，以自訂其作業。  
  
## <a name="setting-editor-options"></a>設定編輯器選項  
 使用 [工具] 功能表並選取 [選項...] 來顯示 [選項] 對話方塊，即可設定大部分的編輯器選項。 在 **[選項]** 對話方塊於左窗格中開啟 **[文字編輯器]** 節點，可設定編碼與文字編輯選項。 文字編輯器下的節點適用於特定的編輯器：  
  
1.  **所有語言** - 使用此節點設定的選項會套用至所有 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 編輯器。 使用其他節點為特定的編輯器設定不同之選項，可以覆寫這些設定。  
  
2.  **純文字** - 使用此節點設定的選項會套用至 MDX、DMX 與文字編輯器。  
  
3.  **Transact-SQL** - 使用此節點設定的選項會套用至資料庫引擎查詢編輯器。  
  
4.  **XML** 使用此節點設定的選項會套用至 XML for Analysis 編輯器。  
  
 開啟 **[查詢執行]** 或 **[查詢結果]** 節點，以自訂查詢的執行方式與結果的顯示方式。  
  
## <a name="editor-configuration-tasks"></a>編輯器配置工作  
  
|工作描述|主題|  
|----------------------|-----------|  
|描述如何指定在 [Windows 檔案總管] 中按兩下特定副檔名的檔案，自動開啟編輯器。|[使副檔名與程式碼編輯器相關聯](./associate-file-extensions-to-a-code-editor.md)|  
|描述如何自訂字型，讓程式碼與文字更容易閱讀。|[變更字型色彩、大小與樣式](./change-font-color-size-and-style.md)|  
|描述如何檢視屬性。|[在 Management Studio 中使用屬性視窗](./use-the-properties-window-in-management-studio.md)|  
|編輯器選項對話方塊的 F1 說明頁面之位置。|[查詢選項頁面 F1 說明](../f1-help/f1-help-for-server-connections-sql-server-management-studio.md)|