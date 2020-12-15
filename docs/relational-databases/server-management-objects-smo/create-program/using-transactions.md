---
description: 使用交易
title: 使用交易 |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Objects, transactions
- transactions [SMO]
- SMO [SQL Server], transactions
ms.assetid: 399aded8-bee3-4cfb-a671-1877c7d0de9f
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 33d4884d179699cbefbbf4da4ceae4009feeb768
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467229"
---
# <a name="using-transactions"></a>使用交易
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理物件 (SMO) 中，交易處理是藉由使用 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 物件，透過與 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 執行個體的連接而達成。 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> 建立連接時，物件的屬性會參考物件 <xref:Microsoft.SqlServer.Management.Smo.Server> 。 <xref:Microsoft.SqlServer.Management.Common.DataTransferProgressEventType.StartTransaction>、<xref:Microsoft.SqlServer.Management.Common.ServerConnection.RollBackTransaction%2A> 和 <xref:Microsoft.SqlServer.Management.Common.ServerConnection.CommitTransaction%2A> 之類的方法屬於 <xref:Microsoft.SqlServer.Management.Smo.Server.ConnectionContext%2A> 物件屬性。  
  
## <a name="see-also"></a>另請參閱  
 [建立 SMO 程式](../../../relational-databases/server-management-objects-smo/create-program/creating-smo-programs.md)  
  
  
