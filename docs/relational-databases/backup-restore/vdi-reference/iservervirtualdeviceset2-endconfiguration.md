---
title: IServerVirtualDeviceSet2::EndConfiguration
titlesuffix: SQL Server VDI reference
description: 本文提供 IServerVirtualDeviceSet2::EndConfiguration 命令的參考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7e182626a39df3ad1303592cda9744897861b182
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96125332"
---
# <a name="iservervirtualdeviceset2endconfiguration-vdi"></a>IServerVirtualDeviceSet2::EndConfiguration (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

**EndConfiguration** 函式會通知 VDI，伺服器已完成其設定。

## <a name="syntax"></a>語法

```c
HRESULT IServerVirtualDeviceSet2::EndConfiguration ();
```

## <a name="return-value"></a>傳回值

|傳回值 | 說明 |
|---|---|
| NOERROR | 此函數已成功。 |
| VD_E_ABORT | 已要求中止。 |
| VD_E_PROTOCOL | 集合未處於可設定的狀態。 |
| VD_E_MEMORY | 無法取得透過 'RequestBuffers' 呼叫所要求的記憶體。 集合會維持可設定的狀態，且沒有可用的緩衝區空間。 伺服器可以減少其緩衝區需求，或中止作業。 |

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱 [SQL Server 虛擬裝置介面參考概觀](reference-virtual-device-interface.md)。