---
description: 驗證訂閱
title: 驗證訂閱 | Microsoft 文件
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.validateandresynch.f1
helpviewer_keywords:
- Validate Subscription dialog box
ms.assetid: 74bdf5e1-b886-4284-b5fb-332bf79ae083
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: e66d2b6c088126285f52b462ec2bf9f125f81cc9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468689"
---
# <a name="validate-subscription"></a>驗證訂閱
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  使用 **[驗證訂閱]** 對話方塊以指定下次執行訂閱的合併代理程式時，應驗證合併式發行集的訂閱。 驗證的結果會在複寫監視器中顯示。 如需詳細資訊，請參閱 [Validate Data at the Subscriber](../../relational-databases/replication/validate-data-at-the-subscriber.md)。  
  
 也可以在 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 中，以滑鼠右鍵按一下發行集，然後按一下 [驗證所有訂閱] 來驗證合併式發行集的所有訂閱。  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>選項  
 **上一次嘗試驗證的日期**  
 合併代理程式工作階段上一次進行訂閱驗證的日期，無論驗證成功與否。  
  
 **上一次成功驗證的日期**  
 合併代理程式工作階段上一次成功驗證訂閱的日期。  
  
 **驗證此訂閱**  
 選取即可驗證訂閱。  
  
 **選項**  
 按一下以存取 **[訂閱驗證選項]** 對話方塊，其中可讓您指定是否要使用資料列計數驗證或二進位總和檢查碼驗證。  
  
## <a name="see-also"></a>另請參閱  
 [驗證複寫的資料](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
