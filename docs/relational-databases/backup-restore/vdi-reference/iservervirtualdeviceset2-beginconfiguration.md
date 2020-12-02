---
title: IServerVirtualDeviceSet2::BeginConfiguration
titlesuffix: SQL Server VDI reference
description: 本文提供 IServerVirtualDeviceSet2::BeginConfiguration 命令的參考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d1a2bd52e923f64bac8b1865cee5e18aa2f6345e
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125356"
---
# <a name="iservervirtualdeviceset2beginconfiguration-vdi"></a>IServerVirtualDeviceSet2::BeginConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

伺服器會叫用 **BeginConfiguration** 函式，以開始設定虛擬裝置集。

## <a name="syntax"></a>語法

```c
HRESULT IServerVirtualDeviceSet2::BeginConfiguration (
   DWORD   dwFeatures,
   DWORD   dwAlignment,
   DWORD   dwBlockSize,
   DWORD   dwMaxTransferSize,
   DWORD   dwTimeout
);
```

## <a name="parameters"></a>參數

*dwFeatures* 經修改的功能遮罩。 VDF_WriteMedia 及/或 VDF_ReadMedia。

*dwAlignment* 最終的對齊方式。 如果是 0，則預設為 dwBlockSize。 必須是 2 的乘冪，>= dwBlockSize 且 <= 64 KB。

*dwBlockSize* 最小傳送單位 (以位元組為單位)。 必須是 2 的乘冪，>=512 且 <= 64 KB。

*dwMaxTransferSize* 將嘗試的最大傳送。 必須是 64 KB 的倍數。

*dwTimeout* 等待主要用戶端完成宣告其將提供之緩衝區的毫秒數。

## <a name="return-value"></a>傳回值

|傳回值 | 說明 |
|---|---|
| NOERROR | 虛擬裝置集處於可設定的狀態。 |
| VD_E_ABORT | 已叫用 SignalAbort。 |
| VD_E_PROTOCOL | 虛擬裝置集不是處於連線狀態。 |

## <a name="remarks"></a>備註

叫用此函式之後，虛擬裝置集就會移至可設定的狀態，並在其中決定緩衝區配置。
設定基本設定 (根據參數) 之後，這些值會在虛擬裝置集的存留期間保持固定。 虛擬裝置集對齊方式屬性是用來控制資料緩衝區的對齊方式。 這個值會設定可依緩衝區逐一覆寫的最小對齊值。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱 [SQL Server 虛擬裝置介面參考概觀](reference-virtual-device-interface.md)。