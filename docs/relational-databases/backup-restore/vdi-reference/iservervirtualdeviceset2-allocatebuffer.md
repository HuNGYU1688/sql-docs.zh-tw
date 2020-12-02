---
title: IServerVirtualDeviceSet2::AllocateBuffer
titlesuffix: SQL Server VDI reference
description: 本文提供 IServerVirtualDeviceSet2::AllocateBuffer 命令的參考。
ms.date: 08/30/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.technology: backup-restore
ms.topic: reference
author: cawrites
ms.author: chadam
ms.openlocfilehash: d311b2253e9083c78207d1443782096b4c82a491
ms.sourcegitcommit: 5a1ed81749800c33059dac91b0e18bd8bb3081b1
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96128946"
---
# <a name="iservervirtualdeviceset2allocatebuffer-vdi"></a>IServerVirtualDeviceSet2::AllocateBuffer (VDI)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

**AllocateBuffer** 函式會從虛擬裝置集取得共用記憶體緩衝區。

## <a name="syntax"></a>語法

```c
HRESULT IServerVirtualDeviceSet2::AllocateBuffer (
   LPVOID*      ppBuffer,
   UINT32      dwSize,
   UINT32      dwAlignment
);
```

## <a name="parameters"></a>參數

*ppBuffer* 這會傳回緩衝區開頭的指標。

*dwSize* 這是緩衝區的大小 (以位元組為單位)。 這不包含用戶端要求的任何前置詞區域。 伺服器會隱藏任何這類區域，且在傳回緩衝區位址之前，會有可用的空間。

*dwAlignment* 這會指定緩衝區的對齊界限。 例如，值 4096 會確保緩衝區在 4096 位元組的界限上對齊。 這表示傳回的位址會將低序位 12 位元設為零。 這個參數必須是 2 的乘冪。

## <a name="return-value"></a>傳回值

|傳回值 | 說明 |
|---|---|
| NOERROR | 會傳回緩衝區。 |
| VD_E_MEMORY | 發生記憶體不足的狀況。 |
| VD_E_INVALID | 參數無效。 |

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱 [SQL Server 虛擬裝置介面參考概觀](reference-virtual-device-interface.md)。