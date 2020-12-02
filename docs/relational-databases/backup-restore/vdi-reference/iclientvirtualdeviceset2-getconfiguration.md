---
title: IClientVirtualDeviceSet2::GetConfiguration
titlesuffix: SQL Server VDI reference
description: 本文提供 IClientVirtualDeviceSet2::GetConfiguration 命令的參考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5d46efb493dc67c38affc25f99871f3ff4617ecb
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125393"
---
# <a name="iclientvirtualdeviceset2getconfiguration-vdi"></a>IClientVirtualDeviceSet2::GetConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

**GetConfiguration** 函式是用來等候伺服器設定虛擬裝置集。

## <a name="syntax"></a>語法

```c
HRESULT IClientVirtualDeviceSet2::GetConfiguration (
   DWORD         dwTimeOut,
   VDConfig*      pCfg
);
```

## <a name="parameters"></a>參數

*DwTimeOut* 這是逾時值 (以毫秒為單位)。 請使用 INFINITE 以避免逾時。

*pCfg* 成功執行時，這會包含伺服器所選取的設定。 如需詳細資訊，請參閱＜設定＞。

## <a name="return-value"></a>傳回值

|傳回值 | 說明 |
|---|---|
| NOERROR | 已傳回設定。 |
| VD_E_ABORT | 已叫用 SignalAbort。 |
| VD_E_TIMEOUT | 函數逾時。 |

## <a name="remarks"></a>備註

此函式會封鎖並處於「可警示」狀態。 成功叫用之後，即可開啟虛擬裝置集中的裝置。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱 [SQL Server 虛擬裝置介面參考概觀](reference-virtual-device-interface.md)。