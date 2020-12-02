---
description: 設定資料收集器的屬性
title: 設定資料收集器的屬性 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- sql13.swb.dc.datacollectionprop.general.f1
- sql13.swb.dc.datacollectionprop.advanced.f1
ms.assetid: cf98f57d-5a6d-4bc3-bf10-783e460fc63d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 19d0107e21e71c8330e3990d4fe937e030ecd0e7
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128841"
---
# <a name="configure-properties-of-a-data-collector"></a>設定資料收集器的屬性
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  本主題討論如何設定資料收集器的屬性。  
  
## <a name="data-collection-properties-general-tab"></a>資料收集屬性 (一般索引標籤)  
 使用這個頁面可設定管理資料倉儲的設定，並指定在收集而來的資料上傳到資料倉儲之前，應該將這些資料儲存在哪裡。  
  
 **啟用資料收集**  
 選取此選項可啟用資料收集。 這與執行 sp_syscollector_enable_collector 預存程序有相同的效果。 清除此核取方塊會停用資料收集，而且與執行 sp_syscollector_disable_collector 預存程序有相同的效果。  
  
 **Server**  
 顯示將主控管理資料倉儲的伺服器名稱。  
  
 **資料庫名稱**  
 顯示用於管理資料倉儲的關聯式資料庫名稱。  
  
 **測試連接**  
 使用設定資料收集時所提供的資訊，測試與指定之 [伺服器] 的連接。  
  
 **快取目錄**  
 指定在收集而來的資料上傳到管理資料倉儲之前，應該將這些資料儲存在收集資料之系統上的哪一個目錄。 如果未指定 [快取目錄]，資料收集器會嘗試尋找 %TEMP% 和 %TMP% 環境變數的位置，並使用其中一個位置當做暫存區的預設位置。 如果未設定這些環境變數，就會發生錯誤，而且系統會提示您建立快取目錄。  
  
## <a name="data-collection-properties-advanced-tab"></a>資料收集屬性 (進階索引標籤)  
 使用這個頁面可針對管理資料倉儲的連接來設定重試設定。  
  
 **上傳失敗時的重試次數**  
 指定當上傳失敗時，重試上傳到管理資料倉儲的次數。 預設值是 1。  
  
## <a name="see-also"></a>另請參閱  
 [管理資料收集](../../relational-databases/data-collection/manage-data-collection.md)   
 [設定管理資料倉儲 &#40;SQL Server Management Studio&#41;](../../relational-databases/data-collection/configure-the-management-data-warehouse-sql-server-management-studio.md)  
  
  
