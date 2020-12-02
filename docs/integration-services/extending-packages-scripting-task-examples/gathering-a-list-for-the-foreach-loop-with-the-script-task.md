---
description: 以指令碼工作蒐集 ForEach 迴圈的清單
title: 以指令碼工作蒐集 ForEach 迴圈的清單 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- Foreach Loop containers
- Script task [Integration Services], Foreach loops
- Script task [Integration Services], examples
- SSIS Script task, Foreach loops
ms.assetid: 694f0462-d0c5-4191-b64e-821b1bdef055
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e8ff797225d3a97673bf0f1b108ce15eb6e8bd52
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "92193098"
---
# <a name="gathering-a-list-for-the-foreach-loop-with-the-script-task"></a>以指令碼工作蒐集 ForEach 迴圈的清單

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Foreach from Variable 列舉值會透過以變數傳遞給它的清單中之項目來列舉，並針對每個項目執行相同的工作。 您可以在指令碼工作中使用自訂程式碼，針對此目的填入清單。 如需列舉值的詳細資訊，請參閱 [Foreach 迴圈容器](../../integration-services/control-flow/foreach-loop-container.md)。  
  
> [!NOTE]  
>  如果您想要建立可更輕鬆地在多個封裝之間重複使用的工作，請考慮使用此指令碼工作範例中的程式碼做為自訂工作的起點。 如需詳細資訊，請參閱 [開發自訂工作](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md)。  
  
## <a name="description"></a>描述  
 下列範例使用 **System.IO** 命名空間的方法，蒐集電腦上的 Excel 活頁簿清單，這些活頁簿比使用者在變數中所指定的天數還要新或是還要舊。 它會在磁碟 C 上以遞迴方式搜尋目錄中有 .xls 副檔名的檔案，並檢查檔案最後修改的日期，以判斷檔案是否應歸屬於清單中。 它會將符合的檔案加入 **ArrayList**，並將 **ArrayList** 儲存到變數，以供稍後用於 Foreach 迴圈容器。 Foreach Loop 容器是設定為從 Variable 列舉值使用 Foreach。  
  
> [!NOTE]  
>  從 Variable 列舉值與 Foreach 搭配使用的變數必須是 **Object** 類型。 您放置在變數中的物件必須實作下列其中一個介面：**System.Collections.IEnumerable**、**System.Runtime.InteropServices.ComTypes.IEnumVARIANT**、**System.ComponentModel IListSource** 或 **Microsoft.SqlServer.Dts.Runtime.Wrapper.ForEachEnumeratorHost**。 經常會使用 **Array** 或 **ArrayList**。 **ArrayList** 需要參考以及 **System.Collections** 命名空間的 **Imports** 陳述式。  
  
 您可以透過為 `FileAge` 封裝變數使用不同的正值與負值，來試驗這項工作。 例如，您可以輸入 5 以搜尋在過去五天內建立的檔案，或是輸入 3 搜尋已建立超過三天的檔案。 這項工作可能需要花費一兩分鐘，以搜尋磁碟機上的許多資料夾。  
  
#### <a name="to-configure-this-script-task-example"></a>設定此指令碼工作範例  
  
1.  建立名為 `FileAge` 且類型為整數的封裝孌數，並輸入正整數值或負整數值。 當值為正數時，程式碼會搜尋比指定天數還要新的檔案，當值為負數時，會搜尋比指定的天數還要舊的檔案。  
  
2.  建立類型為 **Object** 且名為 `FileList` 的變數，以接收指令碼工作蒐集的檔案清單，以便稍後供 Variable 列舉值的 Foreach 使用。  
  
3.  將 `FileAge` 變數加入指令碼工作的 **ReadOnlyVariables** 屬性，然後將 `FileList` 變數加入 **ReadWriteVariables** 屬性。  
  
4.  在您的程式碼中，匯入 **System.Collections** 和 **System.IO** 命名空間。  
  
### <a name="code"></a>程式碼  
  
```vb  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports System.Collections  
Imports System.IO  
  
Public Class ScriptMain  
  
  Private Const FILE_AGE As Integer = -50  
  
  Private Const FILE_ROOT As String = "C:\"  
  Private Const FILE_FILTER As String = "*.xls"  
  
  Private isCheckForNewer As Boolean = True  
  Dim fileAgeLimit As Integer  
  Private listForEnumerator As ArrayList  
  
  Public Sub Main()  
  
    fileAgeLimit = DirectCast(Dts.Variables("FileAge").Value, Integer)  
  
    ' If value provided is positive, we want files NEWER THAN n days.  
    '  If negative, we want files OLDER THAN n days.  
    If fileAgeLimit < 0 Then  
      isCheckForNewer = False  
    End If  
    ' Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit)  
  
    listForEnumerator = New ArrayList  
  
    GetFilesInFolder(FILE_ROOT)  
  
    ' Return the list of files to the variable  
    '  for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: " & listForEnumerator.Count.ToString, "Results", Windows.Forms.MessageBoxButtons.OK, Windows.Forms.MessageBoxIcon.Information)  
    Dts.Variables("FileList").Value = listForEnumerator  
  
    Dts.TaskResult = ScriptResults.Success  
  
  End Sub  
  
  Private Sub GetFilesInFolder(ByVal folderPath As String)  
  
    Dim localFiles() As String  
    Dim localFile As String  
    Dim fileChangeDate As Date  
    Dim fileAge As TimeSpan  
    Dim fileAgeInDays As Integer  
    Dim childFolder As String  
  
    Try  
      localFiles = Directory.GetFiles(folderPath, FILE_FILTER)  
      For Each localFile In localFiles  
        fileChangeDate = File.GetLastWriteTime(localFile)  
        fileAge = DateTime.Now.Subtract(fileChangeDate)  
        fileAgeInDays = fileAge.Days  
        CheckAgeOfFile(localFile, fileAgeInDays)  
      Next  
  
      If Directory.GetDirectories(folderPath).Length > 0 Then  
        For Each childFolder In Directory.GetDirectories(folderPath)  
          GetFilesInFolder(childFolder)  
        Next  
      End If  
  
    Catch  
      ' Ignore exceptions on special folders such as System Volume Information.  
    End Try  
  
  End Sub  
  
  Private Sub CheckAgeOfFile(ByVal localFile As String, ByVal fileAgeInDays As Integer)  
  
    If isCheckForNewer Then  
      If fileAgeInDays <= fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    Else  
      If fileAgeInDays > fileAgeLimit Then  
        listForEnumerator.Add(localFile)  
      End If  
    End If  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using System.Data;  
using System.Math;  
using Microsoft.SqlServer.Dts.Runtime;  
using System.Collections;  
using System.IO;  
  
public partial class ScriptMain : Microsoft.SqlServer.Dts.Tasks.ScriptTask.VSTARTScriptObjectModelBase  
    {  
  
        private const int FILE_AGE = -50;  
  
        private const string FILE_ROOT = "C:\\";  
        private const string FILE_FILTER = "*.xls";  
  
        private bool isCheckForNewer = true;  
        int fileAgeLimit;  
        private ArrayList listForEnumerator;  
  
        public void Main()  
  {  
  
    fileAgeLimit = (int)(Dts.Variables["FileAge"].Value);  
  
    // If value provided is positive, we want files NEWER THAN n days.  
    // If negative, we want files OLDER THAN n days.  
    if (fileAgeLimit<0)  
    {  
      isCheckForNewer = false;  
    }  
    // Extract number of days as positive integer.  
    fileAgeLimit = Math.Abs(fileAgeLimit);  
  
    listForEnumerator = new ArrayList();  
  
    GetFilesInFolder(FILE_ROOT);  
  
    // Return the list of files to the variable  
    // for later use by the Foreach from Variable enumerator.  
    System.Windows.Forms.MessageBox.Show("Matching files: "+ listForEnumerator.Count, "Results",   
MessageBoxButtons.OK, MessageBoxIcon.Information);  
    Dts.Variables["FileList"].Value = listForEnumerator;  
  
    Dts.TaskResult = (int)ScriptResults.Success;  
  
  }  
  
        private void GetFilesInFolder(string folderPath)  
        {  
  
            string[] localFiles;  
            DateTime fileChangeDate;  
            TimeSpan fileAge;  
            int fileAgeInDays;  
  
            try  
            {  
                localFiles = Directory.GetFiles(folderPath, FILE_FILTER);  
                foreach (string localFile in localFiles)  
                {  
                    fileChangeDate = File.GetLastWriteTime(localFile);  
                    fileAge = DateTime.Now.Subtract(fileChangeDate);  
                    fileAgeInDays = fileAge.Days;  
                    CheckAgeOfFile(localFile, fileAgeInDays);  
                }  
  
                if (Directory.GetDirectories(folderPath).Length > 0)  
                {  
                    foreach (string childFolder in Directory.GetDirectories(folderPath))  
                    {  
                        GetFilesInFolder(childFolder);  
                    }  
                }  
  
            }  
            catch  
            {  
                // Ignore exceptions on special folders, such as System Volume Information.  
            }  
  
        }  
  
        private void CheckAgeOfFile(string localFile, int fileAgeInDays)  
        {  
  
            if (isCheckForNewer)  
            {  
                if (fileAgeInDays <= fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
            else  
            {  
                if (fileAgeInDays > fileAgeLimit)  
                {  
                    listForEnumerator.Add(localFile);  
                }  
            }  
  
        }  
  
    }  
```  
  
## <a name="see-also"></a>另請參閱  
 [Foreach 迴圈容器](../../integration-services/control-flow/foreach-loop-container.md)   
 [設定 Foreach 迴圈容器](../control-flow/foreach-loop-container.md)  
  
