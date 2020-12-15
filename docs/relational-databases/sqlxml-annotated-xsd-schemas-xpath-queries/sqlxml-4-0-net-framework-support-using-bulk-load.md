---
title: 在 .NET 環境中使用 SQLXML 大量載入
description: 瞭解如何在 .NET 環境中使用 SQLXML 4.0 大量載入 COM 物件，將 XML 資料大量載入資料庫中。
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- XML Bulk Load [SQLXML], .NET environment
- .NET Framework [SQLXML], XML Bulk Load
- bulk load [SQLXML], .NET environment
ms.assetid: b85df83b-ba56-43bf-bcdf-b2a6fca43276
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5bc731438d8691664b4db502ab05e3489e5faaa1
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461719"
---
# <a name="sqlxml-40-net-framework-support---using-bulk-load"></a>SQLXML 4.0 .NET Framework 支援 - 使用大量載入
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  本主題說明如何在 .NET 環境中使用 XML 大量載入功能。 如需 XML 大量載入的詳細資訊，請參閱 [&#40;SQLXML 4.0&#41;中執行 Xml 資料的大量載入 ](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)。  
  
 若要從 Managed 環境使用 SQLXML 大量載入 COM 物件，您需要將專案參考加入到此物件中。 這會在大量載入 COM 物件周圍產生 Managed 包裝函數介面。  
  
> [!NOTE]  
>  Managed XML 大量載入不會使用 Managed 資料流，而且在原生資料流周圍需要使用包裝函數。 SQLXML 大量載入元件將不會在多執行緒環境 ('[MTAThread]' 屬性) 下執行。 如果您嘗試在多執行緒環境中執行大量載入元件，則會收到 InvalidCastException 例外狀況，其中包含下列其他資訊：「介面 SQLXMLBULKLOADLib. Queryinterface 的 QueryInterface 失敗」。 因應措施是讓包含大量載入物件單一執行緒的物件可供存取 (例如，使用 **[STAThread]** 屬性，如範例) 所示。  
  
 本主題提供 C# 工作範例應用程式，將 XML 資料大量載入到資料庫中。 若要建立工作範例，按照下列步驟進行：  
  
1.  建立下列資料表：  
  
    ```  
    CREATE TABLE Ord (  
             OrderID     int identity(1,1)  PRIMARY KEY,  
             CustomerID  varchar(5))  
    GO  
    CREATE TABLE Product (  
             ProductID   int identity(1,1) PRIMARY KEY,  
             ProductName varchar(20))  
    GO  
    CREATE TABLE OrderDetail (  
           OrderID     int FOREIGN KEY REFERENCES Ord(OrderID),  
           ProductID   int FOREIGN KEY REFERENCES Product(ProductID),  
                       CONSTRAINT OD_key PRIMARY KEY (OrderID, ProductID))  
    GO  
    ```  
  
2.  將下列結構描述儲存在檔案 (schema.xml) 中：  
  
    ```  
    <xsd:schema xmlns:xsd="http://www.w3.org/2001/XMLSchema"  
                xmlns:sql="urn:schemas-microsoft-com:mapping-schema">  
    <xsd:annotation>  
      <xsd:appinfo>  
        <sql:relationship name="OrderOD"  
              parent="Ord"  
              parent-key="OrderID"  
              child="OrderDetail"  
              child-key="OrderID" />  
  
        <sql:relationship name="ODProduct"  
              parent="OrderDetail"  
              parent-key="ProductID"  
              child="Product"  
              child-key="ProductID"   
              inverse="true"/>  
      </xsd:appinfo>  
    </xsd:annotation>  
  
      <xsd:element name="Order" sql:relation="Ord"   
                                sql:key-fields="OrderID" >  
       <xsd:complexType>  
         <xsd:sequence>  
            <xsd:element name="Product" sql:relation="Product"   
                         sql:key-fields="ProductID"  
                         sql:relationship="OrderOD ODProduct">  
              <xsd:complexType>  
                 <xsd:attribute name="ProductID" type="xsd:int" />  
                 <xsd:attribute name="ProductName" type="xsd:string" />  
              </xsd:complexType>  
            </xsd:element>  
         </xsd:sequence>  
            <xsd:attribute name="OrderID"   type="xsd:integer" />   
            <xsd:attribute name="CustomerID"   type="xsd:string" />  
        </xsd:complexType>  
      </xsd:element>  
    </xsd:schema>  
    ```  
  
3.  將下列範例 XML 文件儲存在檔案 (data.xml) 中：  
  
    ```  
    <ROOT>    
      <Order OrderID="11" CustomerID="ALFKI">  
        <Product ProductID="11" ProductName="Chai" />  
        <Product ProductID="22" ProductName="Chang" />  
      </Order>  
      <Order OrderID="22" CustomerID="ANATR">  
         <Product ProductID="33" ProductName="Aniseed Syrup" />  
        <Product ProductID="44" ProductName="Gumbo Mix" />  
      </Order>  
    </ROOT>  
    ```  
  
4.  啟動 Visual Studio。  
  
5.  建立 C# 主控台應用程式。  
  
6.  從 [ **專案** ] 功能表選取 [ **加入參考**]。  
  
7.  在 [ **COM** ] 索引標籤中，選取 [ **Microsoft SQLXML 大量載入4.0 型別程式庫** ( # A0) ]，然後按一下 **[確定]**。 您將會看到在專案中建立的 **SQLXMLBULKLOADLib** 元件。  
  
8.  將 Main() 方法取代成下列程式碼。 將 **ConnectionString** 屬性和檔案路徑更新為架構和資料檔案。  
  
    ```  
    [STAThread]  
       static void Main(string[] args)  
       {     
             try  
             {  
                SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class objBL = new SQLXMLBULKLOADLib.SQLXMLBulkLoad4Class();  
                objBL.ConnectionString = "Provider=sqloledb;server=server;database=databaseName;integrated security=SSPI";  
                objBL.ErrorLogFile = "error.xml";  
                objBL.KeepIdentity = false;  
                objBL.Execute ("schema.xml","data.xml");  
             }  
             catch(Exception e)  
             {  
             Console.WriteLine(e.ToString());  
             }  
       }  
    ```  
  
9. 若要將 XML 載入到您所建立的資料表中，建立並執行專案。  
  
    > [!NOTE]  
    >  大量載入元件 (xblkld4.dll) 的參考也可以使用 tlbimp.exe 工具加入，該工具是 .NET Framework 的一部分。 此工具會針對原生 DLL (xblkld4.dll) 建立 Managed 包裝函數，然後就可以在任何 .NET 專案中使用。 例如：  
  
    ```  
    c:\>tlbimp xblkld4.dll  
    ```  
  
     這會建立您可以在 .NET Framework 專案中使用的 Managed 包裝函數 DLL (SQLXMLBULKLOADLib.dll)。 在 .NET Framework 中，您可以將專案參考加入到新建立的 DLL。  
  
## <a name="see-also"></a>另請參閱  
 [執行 XML 資料的大量載入 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  
