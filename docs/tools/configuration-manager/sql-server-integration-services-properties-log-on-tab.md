---
title: SQL Server Integration Services 屬性 (登入索引標籤)
description: 了解 [SQL Server Integration Services 屬性] 對話方塊中的 [登入] 索引標籤。 了解如何指定帳戶，以及啟動或停止服務。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: c0eb1b87-6bb0-475e-8492-0fd3c3f910ea
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: dc1a5d6d5710d202d1bdc25a8e21cca550acf15f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97481519"
---
# <a name="sql-server-integration-services-properties-log-on-tab"></a>SQL Server Integration Services 屬性 (登入索引標籤)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  使用  [屬性][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]  對話方塊的 [登入] 索引標籤，指定要供 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服務使用的帳戶，以及啟動和停止該服務。  
  
## <a name="options"></a>選項。  
 **本機系統帳戶**  
 指定不需要密碼的本機系統帳戶。 但是，根據授與帳戶的權限，本機系統帳戶可能會限制服務與其他伺服器互動。  
  
 **這個帳戶**  
 指定使用 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 驗證的本機或網域使用者帳戶。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 建議您使用具有最少服務權限的網域使用者帳戶。 如需有關選取帳戶的資訊，請搜尋《線上叢書》的＜設定 Windows 服務帳戶＞主題。  
  
 **帳戶名稱**  
 指定本機或網域使用者帳戶名稱。  
  
 **密碼**  
 輸入帳戶的密碼。  
  
 **確認密碼**  
 再次輸入帳戶的密碼。  
  
 **啟動**  
 啟動服務。  
  
 **停止**  
 停止服務。  
  
 **暫停**  
 暫停服務。  
  
 **繼續**  
 繼續已暫停的服務。  
  
  
