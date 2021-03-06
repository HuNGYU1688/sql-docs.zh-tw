---
description: 變更商務規則名稱 (Master Data Services)
title: 變更商務規則名稱
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: conceptual
helpviewer_keywords:
- business rules [Master Data Services], changing name
ms.assetid: cffcae43-a208-443f-9f43-a0ec9e05f79c
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: c575f408d0682554eb3ead69e3915d226d4975b0
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88500702"
---
# <a name="change-a-business-rule-name-master-data-services"></a>變更商務規則名稱 (Master Data Services)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  在 [!INCLUDE[ssMDSshort](../includes/ssmdsshort-md.md)]中，當指派的商務規則名稱不符合業務需求時，變更此名稱。  
  
## <a name="prerequisites"></a>先決條件  
 若要執行此程序：  
  
-   您必須擁有存取 **[系統管理]** 功能區域的權限。  
  
-   您必須是模型管理員。 如需詳細資訊，請參閱系統 [管理員 &#40;Master Data Services&#41;](../master-data-services/administrators-master-data-services.md)。  
  
-   商務規則必須存在。 如需詳細資訊，請參閱[建立及發行商務規則 &#40;Master Data Services&#41;](../master-data-services/create-and-publish-a-business-rule-master-data-services.md)。  
  
### <a name="to-change-the-name-of-a-business-rule"></a>若要變更商務規則的名稱  
  
1.  在 [ [!INCLUDE[ssMDSmdm](../includes/ssmdsmdm-md.md)]] 中，按一下 **[系統管理]**。  
  
2.  從功能表列，指向 **[管理]** ，然後按一下 **[商務規則]**。  
  
3.  在 [ **商務規則** ] 頁面上，從 [ **模型** ] 下拉式清單中選取模型。  
  
4.  從 [實體] **** 下拉式清單選取實體。  
  
5.  從 [成員類型]**** 下拉式清單中，選取成員的類型。  
  
6.  在方格中，選取您想要變更名稱之商務規則的資料列，然後按一下 [編輯]****。  
  
7.  輸入商務規則的新名稱。  
  
8.  按一下 [檔案] 。  
  
9. 按一下 [全部發行] ****。  
  
10. 在確認對話方塊中按一下 **[確定]**。 [商務規則狀態]**** 資料行中的值是 [使用中]****。  
  
## <a name="see-also"></a>另請參閱  
 [商務規則 &#40;Master Data Services&#41;](../master-data-services/business-rules-master-data-services.md)  
  
  
