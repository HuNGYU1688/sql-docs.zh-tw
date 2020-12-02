---
title: IServerVirtualDeviceSet2::OpenDevice
titlesuffix: SQL Server VDI reference
description: 本文提供 IServerVirtualDeviceSet2::OpenDevice 命令的參考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4dcec84bd4e46eead1ee558a1b2e0c4013ae9fed
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125362"
---
# <a name="iservervirtualdeviceset2opendevice-vdi"></a>IServerVirtualDeviceSet2::OpenDevice (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

**OpenDevice** 函式會從虛擬裝置集取得虛擬裝置介面。

## <a name="syntax"></a>語法

```c
HRESULT IServerVirtualDeviceSet2::OpenDevice (
   LPCWSTR                     lpName,
   IServerVirtualDevice**      ppVirtualDevice
);
```

## <a name="parameters"></a>參數

*lpName* 這是由 BACKUP 或 RESTORE 命令的第一個 VIRTUAL_DEVICE= 子句所提供。 此名稱可作為索引鍵，以存取用戶端所建立的虛擬裝置集。

*ppVirtualDevice* 這可用來傳回虛擬裝置介面。

## <a name="return-value"></a>傳回值

|傳回值 | 說明 |
|---|---|
| NOERROR | 此函數已成功。 |
| VD_E_OPEN |所有裝置都已開啟。 |

## <a name="remarks"></a>備註

每個呼叫都會傳回下一個未開啟的裝置。 函式呼叫次數只能與虛擬裝置集設定中指定的裝置數目相等。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱 [SQL Server 虛擬裝置介面參考概觀](reference-virtual-device-interface.md)。