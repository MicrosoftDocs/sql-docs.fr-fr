---
description: Utilisation de la messagerie de base de données
title: Utilisation de Database Mail | Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- e-mail [SMO]
- Database Mail [SMO]
- mail [SMO]
ms.assetid: 7605390f-b485-48cc-8d97-e364a066067b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 43a49f3ec79b75aeef6fe4282a2933616af723ae
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88420173"
---
# <a name="using-database-mail"></a>Utilisation de la messagerie de base de données
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  Dans SMO, le sous-système de la messagerie de base de données est représentée par l'objet <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> qui est référencé par la propriété <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A>. En utilisant l'objet SMO <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail>, vous pouvez configurer le sous-système de messagerie de base de données et gérer des profils et des comptes de messagerie. L' <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> objet SMO appartient à l’objet **serveur** , ce qui signifie que l’étendue des comptes de messagerie se situe au niveau du serveur.  
  
## <a name="examples"></a>Exemples  
 Pour utiliser un exemple de code qui est fourni, vous devrez choisir l'environnement de programmation, le modèle de programmation et le langage de programmation dans lequel créer votre application. Pour plus d’informations, consultez [créer un projet Visual C&#35; Smo dans Visual Studio .net](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md).  
  
 Pour les programmes qui utilisent la messagerie de base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous devez inclure l'instruction **Imports** pour qualifier l'espace de noms de la messagerie. Insérez l'instruction après les autres instructions **Imports** , avant toute autre déclaration dans l'application, par exemple :  
  
 `Imports Microsoft.SqlServer.Management.Smo`  
  
 `Imports Microsoft.SqlServer.Management.Common`  
  
 `Imports Microsoft.SqlServer.Management.Smo.Mail`  
  
## <a name="creating-a-database-mail-account-by-using-visual-basic"></a>Création d'un compte de messagerie de base de données à l'aide de Visual Basic  
 Cet exemple de code montre comment créer un compte de messagerie dans SMO. La messagerie de base de données est représentée par l'objet <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> et référencée par la propriété <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO peut être utilisé pour configurer la messagerie de base de données par programme, mais il ne permet pas l'envoi ou la réception de courrier électronique.  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server()
'Define the Database Mail service with a SqlMail object variable and reference it using the Server Mail property.
Dim sm As SqlMail
sm = srv.Mail
'Define and create a mail account by supplying the Database Mail service, name, description, display name, and email address arguments in the constructor.
Dim a As MailAccount
a = New MailAccount(sm, "AdventureWorks Administrator", "AdventureWorks Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com")
a.Create()
```
  
## <a name="creating-a-database-mail-account-by-using-visual-c"></a>Création d'un compte de messagerie de base de données à l'aide de Visual C#  
 Cet exemple de code montre comment créer un compte de messagerie dans SMO. La messagerie de base de données est représentée par l'objet <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> et référencée par la propriété <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO peut être utilisé pour configurer la messagerie de base de données par programme, mais il ne permet pas l'envoi ou la réception de courrier électronique.  
  
```csharp  
{  
         //Connect to the local, default instance of SQL Server.  
         Server srv = default(Server);   
           srv = new Server();   
           //Define the Database Mail service with a SqlMail object variable   
           //and reference it using the Server Mail property.   
           SqlMail sm;   
           sm = srv.Mail;   
           //Define and create a mail account by supplying the Database Mail  
           //service, name, description, display name, and email address  
           //arguments in the constructor.   
           MailAccount a = default(MailAccount);   
           a = new MailAccount(sm, "AdventureWorks2012 Administrator", "AdventureWorks2012 Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com");   
           a.Create();    
}  
```  
  
## <a name="creating-a-database-mail-account-by-using-powershell"></a>Création d'un compte de messagerie de base de données à l'aide de PowerShell  
 Cet exemple de code montre comment créer un compte de messagerie dans SMO. La messagerie de base de données est représentée par l'objet <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> et référencée par la propriété <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> de l'objet <xref:Microsoft.SqlServer.Management.Smo.Server>. SMO peut être utilisé pour configurer la messagerie de base de données par programme, mais il ne permet pas l'envoi ou la réception de courrier électronique.  
  
  
  
```powershell  
#Connect to the local, default instance of SQL Server.  
  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Define the Database Mail; reference it using the Server Mail property.  
$sm = $srv.Mail  
  
#Define and create a mail account by supplying the Database Mail service,  
#name, description, display name, and email address arguments in the constructor.  
$a = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Mail.MailAccount -argumentlist $sm, `  
"Adventure Works Administrator", "Adventure Works Automated Mailer",`  
 "Mail account for administrative e-mail.", "dba@Adventure-Works.com"  
$a.Create()  
```  
  
  
