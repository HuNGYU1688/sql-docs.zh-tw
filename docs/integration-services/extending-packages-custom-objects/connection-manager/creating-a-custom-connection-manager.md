---
description: 建立自訂連接管理員
title: 建立自訂連線管理員 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom connection managers [Integration Services], creating
ms.assetid: e83f8e02-ace4-42e0-b979-2f6be1460985
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 55eafedfd05ce1dde2468bfda62b515057ea4bb6
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96123172"
---
# <a name="creating-a-custom-connection-manager"></a>建立自訂連接管理員

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  建立自訂連接管理員的步驟，與建立 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 之其他自訂物件的步驟類似：  
  
-   建立繼承自基底類別的新類別。 對於連接管理員而言，基底類別是 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase>。  
  
-   將可識別物件類型的屬性套用至類別。 對於連接管理員而言，屬性是 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute>。  
  
-   覆寫基底類別之方法與屬性的實作。 對於連接管理員而言，這些包括 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ConnectionString%2A> 屬性以及 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.AcquireConnection%2A> 與 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase.ReleaseConnection%2A> 方法。  
  
-   (選擇性) 開發自訂使用者介面。 對於連接管理員而言，這需要實作 <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionManagerUI> 介面的類別。  
  
> [!NOTE]  
>  已經建置到 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中的大多數工作、來源和目的地只能搭配特定類型的內建連接管理員一起使用。 因此，不能使用內建工作和元件來測試這些範例。  
  
## <a name="getting-started-with-a-custom-connection-manager"></a>開始使用自訂連接管理員  
  
### <a name="creating-projects-and-classes"></a>建立專案和類別  
 因為所有的 Managed 連接管理員都是從 <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManagerBase> 基底類別衍生，所以建立自訂連接管理員的第一個步驟是以慣用的 Managed 程式語言建立類別庫專案，並建立繼承自基底類別的類別。 在此衍生的類別中，您將覆寫基底類別的方法與屬性，以實作自訂功能。  
  
 在相同的方案中，為自訂使用者介面建立另一個類別庫專案。 之所以建議您為使用者介面建立個別的組件，是因為它可讓您分別更新和重新部署連接管理員或其使用者介面，從而簡化部署的工作。  
  
 透過使用強式名稱金鑰檔案，將兩個專案都設定成簽署將在建立時期產生的組件。  
  
### <a name="applying-the-dtsconnection-attribute"></a>套用 DtsConnection 屬性  
 將 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute> 屬性套用至您已建立的類別，以便將它識別為連接管理員。 此屬性會提供連接管理員的名稱、描述和連接類型等設計階段資訊。 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute.ConnectionType%2A> 與 **Description** 屬性會對應至顯示在 [加入 SSIS 連線管理員] 對話方塊中的 [類型] 與 [描述] 資料行，這個對話方塊將在為 [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] 中的封裝設定連接時顯示。  
  
 使用 <xref:Microsoft.SqlServer.Dts.Runtime.DtsConnectionAttribute.UITypeName%2A> 屬性將連接管理員連結至其自訂使用者介面。 如需取得此屬性所需的公開金鑰權杖，可以使用 **sn.exe -t**，從要用於簽署使用者介面組件的金鑰組 (.snk) 檔案顯示公開金鑰權杖。  
  
```vb  
<DtsConnection(ConnectionType:="SQLVB", _  
  DisplayName:="SqlConnectionManager (VB)", _  
  Description:="Connection manager for Sql Server", _  
  UITypeName:="SqlConnMgrUIVB.SqlConnMgrUIVB,SqlConnMgrUIVB,Version=1.0.0.0,Culture=neutral,PublicKeyToken=<insert public key token here>")> _  
Public Class SqlConnMgrVB  
  Inherits ConnectionManagerBase  
  . . .  
End Class  
```  
  
```csharp  
[DtsConnection(ConnectionType = "SQLCS",  
  DisplayName = "SqlConnectionManager (CS)",  
  Description = "Connection manager for Sql Server",  
  UITypeName = "SqlConnMgrUICS.SqlConnMgrUICS,SqlConnMgrUICS,Version=1.0.0.0,Culture=neutral,PublicKeyToken=<insert public key token here>")]  
public class SqlConnMgrCS :  
ConnectionManagerBase  
{  
  . . .  
}  
```  
  
## <a name="building-deploying-and-debugging-a-custom-connection-manager"></a>建立、部署和偵錯自訂連接管理員  
 在 [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] 中建立、部署和偵錯自訂連接管理員的步驟，類似於其他類型的自訂物件所需的步驟。 如需詳細資訊，請參閱[建立、部署和偵錯自訂物件](../../../integration-services/extending-packages-custom-objects/building-deploying-and-debugging-custom-objects.md)。    
  
## <a name="see-also"></a>另請參閱  
 [撰寫自訂連線管理員的程式碼](../../../integration-services/extending-packages-custom-objects/connection-manager/coding-a-custom-connection-manager.md)   
 [開發自訂連接管理員的使用者介面](../../../integration-services/extending-packages-custom-objects/connection-manager/developing-a-user-interface-for-a-custom-connection-manager.md)  
  
  
