---
description: 通知操作員工作
title: 通知操作員工作 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.notifyoperatortask.f1
helpviewer_keywords:
- Notify Operator task
- notifications [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 6c816c68-c6d6-44e4-bb34-c8e060a958a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6bcdb94260a5fe1190e6f96ada085f004458c54
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194230"
---
# <a name="notify-operator-task"></a>通知操作員工作

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  「通知操作員」工作會傳送通知訊息給 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 操作員。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent 操作員是可接收電子通知的人員或群組使用的別名。 如需 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 運算子的詳細資訊，請參閱 [運算子](../../ssms/agent/operators.md)。  
  
 藉由使用「通知操作員」工作，封裝即可透過電子郵件、呼叫器或 **網路傳送** 通知一或多位操作員。 每一位操作員都可藉由不同的方法收到通知。 例如，OperatorA 可藉由電子郵件和呼叫器收到通知，而 OperatorB 則藉由呼叫器和 **網路傳送** 收到通知。 從工作接收通知的操作員必須為「通知操作員」工作上 **OperatorNotify** 集合的成員。  
  
 「通知操作員」工作是唯一不會封裝 Transact-SQL 陳述式或 DBCC 命令的資料庫維護工作。  
  
## <a name="configuration-of-the-notify-operator-task"></a>設定通知操作員工作  
 您可以透過 [ [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師] 設定屬性。 這項工作位於「 **設計師」中** [工具箱] **的** [維護計畫工作] [!INCLUDE[ssIS](../../includes/ssis-md.md)] 區段。  
  
 如需有關可在 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師中設定之屬性的詳細資訊，請按下列主題：  
  
-   [通知操作員工作 &#40;維護計畫&#41;](../../relational-databases/maintenance-plans/notify-operator-task-maintenance-plan.md)  
  
 如需有關如何在「 [!INCLUDE[ssIS](../../includes/ssis-md.md)] 設計師」中設定這些屬性的詳細資訊，請按下列主題：  
  
-   [設定工作或容器的屬性](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
