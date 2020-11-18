---
title: 連接到 DB2 資料庫 (DB2ToSQL) |Microsoft Docs
description: 瞭解如何連接到 DB2 資料庫的目標實例，以遷移 DB2 資料庫。 SSMA 會取得所有 DB2 架構的相關中繼資料。
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5eb5801d-f0c3-4127-97c0-0b1ef49f4844
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d0ac703c8ea155f33ecb713b98a26f0c39b5a695
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870069"
---
# <a name="connecting-to-db2-database-db2tosql"></a>連接到 DB2 資料庫 (DB2ToSQL) 

若要將 DB2 資料庫移轉至 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，您必須連接到您想要遷移的 db2 資料庫。 當您連接時，SSMA 會取得所有 DB2 架構的相關中繼資料，然後將它顯示在 [DB2 中繼資料瀏覽器] 窗格中。 SSMA 會儲存資料庫伺服器的相關資訊，但不會儲存密碼。

您與資料庫的連接會保持作用中，直到您關閉專案為止。 當您重新開啟專案時，如果您想要使用資料庫的使用中連接，則必須重新連接。

與 DB2 資料庫相關的中繼資料不會自動更新。 相反地，如果您想要更新 DB2 中繼資料瀏覽器中的中繼資料，則必須手動加以更新。 如需詳細資訊，請參閱本主題稍後的「重新整理 DB2 中繼資料」一節。

## <a name="required-db2-permissions"></a>需要的 DB2 許可權

使用者授權會定義可供使用者使用的命令和物件清單。 這份清單會控制使用者動作。 在 DB2 中，在實例層級和 DB2 資料庫層級都有預先決定的授權許可權群組。 這可讓 SSMA 從連接的使用者所擁有的架構取得中繼資料。 若要取得其他架構中物件的中繼資料，然後轉換這些架構中的物件，帳戶必須具有下列許可權：

- 除非在 CREATE 中使用 RESTRICT 關鍵字，否則架構遷移的架構存取通常會授與 PUBLIC
- 資料移轉的資料存取需要 DATAACCESS

## <a name="establishing-a-connection-to-db2"></a>建立 DB2 的連接

當您連接到資料庫時，SSMA 會讀取資料庫中繼資料，然後將此中繼資料加入至專案檔。 當 SSMA 將物件轉換成 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 語法，以及將資料移轉至時，會使用此中繼資料 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 您可以在 [DB2 中繼資料瀏覽器] 窗格中流覽此中繼資料，並查看個別資料庫物件的屬性。  

> [!IMPORTANT]
> 在您嘗試連接之前，請確定資料庫伺服器正在執行，而且可以接受連接。

**連接到 DB2**

1. **在 [檔案**] 功能表上，選取 **[連接到 DB2]**。

   如果您先前已連線到 DB2，命令名稱將會 **重新連接至 db2**。

2. 在 [ **提供者** ] 方塊中，您會看到目前唯一的 DB2 用戶端存取提供者 **OLE DB 提供者** 。

3. 在 [ **管理員** ] 方塊中，您可以選取 **db2 for zOs**、 **db2 for LUW** 或 **db2 for i**

4. 在 [ **模式]** 方塊中，選取 [ **標準模式]** 或 [ **連接字串模式]**。

   使用標準模式來指定伺服器名稱和埠。 使用服務名稱模式，以手動方式指定 DB2 服務名稱。 使用連接字串模式來提供完整的連接字串。

5. 如果您選取 [ **標準] 模式**，請提供下列值：

   - 在 [ **伺服器名稱** ] 方塊中，輸入或選取資料庫伺服器的名稱或 IP 位址。
   - 如果資料庫伺服器未設定為接受預設通訊埠 (1521) 的連接，請在 [ **伺服器埠** ] 方塊中輸入 DB2 連接所使用的通訊埠編號。
   - 在 [ **伺服器埠** ] 方塊中，輸入 Tcp/ip 通訊埠編號。
   - 在 [ **初始目錄** ] 方塊中，輸入資料庫名稱。
   - 在 [ **使用者名稱** ] 方塊中，輸入具有必要許可權的 DB2 帳戶。
   - 在 [ **密碼** ] 方塊中，輸入指定之使用者名稱的密碼。

6. 如果您選取 **連接字串模式**，請在 [ **連接字串** ] 方塊中提供連接字串。

   下列範例顯示 OLE DB 連接字串：

   `Provider=OraOLEDB.DB2;Data Source=MyDB2DB;User Id=myUsername;Password=myPassword;`

   下列範例顯示使用整合式安全性的 DB2 用戶端連接字串：
  
   `Data Source=MyDB2DB;Integrated Security=yes;`

   如需詳細資訊，請參閱 [連接到 Oracle &#40;OracleToSQL&#41;](../../ssma/oracle/connect-to-oracle-oracletosql.md)。
  
## <a name="reconnecting-to-db2"></a>重新連接到 DB2

您與資料庫伺服器的連接會保持作用中，直到您關閉專案為止。 當您重新開啟專案時，如果您想要使用資料庫的使用中連接，則必須重新連接。 您可以離線工作，直到您想要更新中繼資料、將資料庫物件載入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，以及遷移資料為止。

## <a name="refreshing-db2-metadata"></a>重新整理 DB2 中繼資料

DB2 資料庫的相關中繼資料不會自動重新整理。 DB2 中繼資料瀏覽器中的中繼資料是您第一次連接時的中繼資料快照，或是上次手動重新整理中繼資料的時間。 您可以手動更新所有架構、單一架構或個別資料庫物件的中繼資料。

**重新整理中繼資料**

1. 請確定您已連線到資料庫。
2. 在 [DB2 中繼資料瀏覽器] 中，選取您要更新的每個架構或資料庫物件旁的核取方塊。
3. 以滑鼠右鍵按一下 [ **架構**] 或個別的架構或資料庫物件，然後選取 [ **從資料庫** 重新整理]。

   如果您沒有使用中的連接，SSMA 會顯示 [ **連接到 DB2** ] 對話方塊，讓您可以連接。
  
4. 在 [從資料庫重新整理] 對話方塊中，指定要重新整理的物件。
   - 若要重新整理物件，請按一下物件旁的 **現用** 欄位，直到出現箭號為止。
   - 若要防止重新整理物件，請按一下物件旁的 **現用** 欄位，直到出現 **X** 為止。
   - 若要重新整理或拒絕物件的類別目錄，請按一下 [類別目錄] 資料夾旁的 **[使用中** ] 欄位。

     若要查看色彩編碼的定義，請按一下 [ **圖例** ] 按鈕。

5. [!INCLUDE[click OK](../../includes/clickok-md.md)]

## <a name="next-step"></a>後續步驟

- 遷移程式的下一個步驟是 [連接到 SQL Server](./connecting-to-sql-server-db2tosql.md)。

## <a name="see-also"></a>另請參閱

- [將 DB2 資料庫移轉至 SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)