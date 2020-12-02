---
description: SQL Server 匯入和匯出精靈的資料類型對應
title: SQL Server 匯入和匯出精靈中的資料類型對應 | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 669be403-cb17-4b12-bbbf-e7a74003c4b6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 946bb57a3d821186ebcca132539713cf515ab20f
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/25/2020
ms.locfileid: "96122806"
---
# <a name="data-type-mapping-in-the-sql-server-import-and-export-wizard"></a>SQL Server 匯入和匯出精靈的資料類型對應

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 在 [ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 匯入和匯出精靈] 中，您可以在新的目的地資料表和檔案中設定資料行的名稱、資料類型和資料類型屬性，但無法針對資料行值指定自訂轉換。 所以，從來源到目的地的資料類型內建對應極為重要。  
  
##  <a name="how-does-the-wizard-map-data-types-between-source-and-destination"></a><a name="wizardMapping"></a> 精靈如何對應來源和目的地之間的資料類型？
此精靈會使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 所安裝對應檔案來對應兩個資料庫系統或版本之間的資料類型。 例如，它會比對 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 資料類型與 Oracle 資料類型。 XML 格式的對應檔案預設安裝在下列資料夾中。
-   **C:\Program Files\Microsoft SQL Server\130\DTSMappingFiles\\** (64 位元)
-   **C:\Program Files (x86)\Microsoft SQL Server\130\DTSMappingFiles\\** (32 位元)。  
  
 如果您編輯現有的對應檔案，或將新的對應檔案加入資料夾，就必須關閉再重新開啟 [ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 匯入和匯出精靈] 或 [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ，才能載入新的或變更後的對應檔案。  
 
## <a name="you-can-change-an-existing-mapping-file"></a>您可以變更現有的對應檔案
如果貴公司在資料類型間需要不同的對應，您可以更新對應檔案，變更精靈使用的對應。 例如，當您將資料從 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 傳送到 DB2 時，如果想要讓 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **nchar** 資料類型對應到 DB2 **GRAPHIC** 資料類型，而非 DB2 **VARGRAPHIC** 資料類型，則可在 **SqlClientToIBMDB2.xml** 對應檔案中變更 **nchar** 對應，以便使用 **GRAPHIC** 而非 **VARGRAPHIC**。  
  
## <a name="you-can-add-a-new-mapping-file"></a>您可以加入新的對應檔案
[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 會安裝多種來源和目的地常用之組合的對應。 您也可以在 **MappingFiles** 目錄中加入新的對應檔案，支援其他的來源與目的地。 新的對應檔必須符合已發行的 XSD 結構描述，也必須對應來源和目的地的唯一組合。 對應檔案結構描述 **DataTypeMapping.xsd** 的發行位置在 [這裡](https://schemas.microsoft.com/sqlserver/2008/07/IntegrationServices/DataTypeMapping/DataTypeMapping.xsd)。
 
## <a name="sample-mapping-file"></a>範例對應檔案
以下是從 SQL Server 資料類型 (或者更具體地說，從 SQL Server .Net Framework 資料提供者使用的資料類型) 對應至 Oracle 資料類型之 XML 對應檔案的一部分。 如您所見，SQL Server **int** 資料類型對應到 Oracle **INTEGER** 資料類型即為一例。
  
```xml  
  
<dtm:DataTypeMappings  
    xmlns:dtm="https://www.microsoft.com/SqlServer/Dts/DataTypeMapping.xsd"   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    SourceType="System.Data.SqlClient.SqlConnection"   
    MinSourceVersion="*"   
    MaxSourceVersion="*"   
    DestinationType="MSDAORA;OraOLEDB.Oracle;System.Data.OracleClient.OracleConnection"   
    MinDestinationVersion="08.*"   
    MaxDestinationVersion="*">  
  
    <!-- smallint -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>smallint</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
    <!-- int -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>int</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
        ...  
  
</dtm:DataTypeMappings>  
  
```  

