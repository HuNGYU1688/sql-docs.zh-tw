---
description: Current (MDX)
title: 目前的 (MDX) |Microsoft Docs
ms.date: 06/04/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: mdx
ms.topic: reference
ms.author: owend
ms.reviewer: owend
author: minewiskan
ms.openlocfilehash: 105bb306cb84f151024a288f5aa15eb09ddbf5c2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/17/2020
ms.locfileid: "88387544"
---
# <a name="current-mdx"></a>Current (MDX)


  反覆運算時從一個集合傳回目前的 Tuple。  
  
## <a name="syntax"></a>語法  
  
```  
  
Set_Expression.Current   
```  
  
## <a name="arguments"></a>引數  
 *Set_Expression*  
 傳回集合的有效多維度運算式 (MDX) 運算式。  
  
## <a name="remarks"></a>備註  
 在反覆運算期間的每個步驟，進行運算的 Tuple 就是目前的 Tuple。 **目前**的函式會傳回該元組。 只有在集合上反覆運算時，這個函數才是有效的。  
  
 逐一查看集合的 MDX 函數包含 [產生](../mdx/generate-mdx.md) 函數。  
  
> [!NOTE]  
>  這個函數只能搭配已命名的集合 (使用集合別名或定義命名集) 使用。  
  
## <a name="examples"></a>範例  
 下列範例顯示如何在**產生**中使用**目前**的函式：  
  
 `WITH`  
  
 `//Creates a set of tuples consisting of all Calendar Years crossjoined with`  
  
 `//all Product Categories`  
  
 `SET MyTuples AS CROSSJOIN(`  
  
 `[Date].[Calendar Year].[Calendar Year].MEMBERS,`  
  
 `[Product].[Category].[Category].MEMBERS)`  
  
 `//Iterates through each tuple in the set and returns the name of the Calendar`  
  
 `//Year in each tuple`  
  
 `MEMBER MEASURES.CURRENTDEMO AS`  
  
 `GENERATE(MyTuples, MyTuples.CURRENT.ITEM(0).NAME, ", ")`  
  
 `SELECT MEASURES.CURRENTDEMO ON 0`  
  
 `FROM [Adventure Works]`  
  
## <a name="see-also"></a>另請參閱  
 [MDX 函數參考 &#40;MDX&#41;](../mdx/mdx-function-reference-mdx.md)  
  
  
