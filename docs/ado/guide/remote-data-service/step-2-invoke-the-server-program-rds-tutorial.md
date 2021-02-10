---
description: 'Étape 2 : Appeler le programme serveur (tutoriel RDS)'
title: 'Étape 2 : appeler le programme serveur (didacticiel RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], invoking server program
ms.assetid: 5e74c2da-65ee-4de4-8b41-6eac45c3632e
author: rothja
ms.author: jroth
ms.openlocfilehash: ccccbbc0d634b1044569c4787b8e7bb60c2c3275
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031721"
---
# <a name="step-2-invoke-the-server-program-rds-tutorial"></a>Étape 2 : Appeler le programme serveur (tutoriel RDS)
Lorsque vous appelez une méthode sur le *proxy* client, le programme réel sur le serveur exécute la méthode. Dans cette étape, vous allez exécuter une requête sur le serveur.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 **Partie A** Si vous n’avez pas utilisé [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) dans ce didacticiel, la méthode la plus pratique pour effectuer cette étape consiste à utiliser les [services Bureau à distance. DataControl](../../reference/rds-api/datacontrol-object-rds.md) . **RDS. DataControl** combine l’étape précédente de la création d’un proxy, avec cette étape, émettant la requête.  
  
 Définissez le **RDS.** Propriété du [serveur](../../reference/rds-api/server-property-rds.md) d’objets DataControl pour identifier l’emplacement où le programme serveur doit être instancié ; la propriété [Connect](../../reference/rds-api/connect-property-rds.md) pour spécifier la chaîne de connexion permettant d’accéder à la source de données ; et la propriété [SQL](../../reference/rds-api/sql-property.md) pour spécifier le texte de la commande de requête. Ensuite, émettez la méthode [Refresh](../../reference/rds-api/refresh-method-rds.md) pour faire en sorte que le programme serveur se connecte à la source de données, récupère les lignes spécifiées par la requête et retourne un objet **Recordset** au client.  
  
 Ce didacticiel n’utilise pas les **services Bureau à distance. DataControl**, mais il s’agit de la façon dont elle se présenterait :  
  
```vb
Sub RDSTutorial2A()  
   Dim DC as New RDS.DataControl  
   DC.Server = "https://yourServer"  
   DC.Connect = "DSN=Pubs"  
   DC.SQL = "SELECT * FROM Authors"  
   DC.Refresh  
...  
```  
  
 Le didacticiel n’appelle pas non plus RDS avec les objets ADO, mais c’est le cas :  
  
```vb
Dim rs as New ADODB.Recordset  
rs.Open "SELECT * FROM Authors","Provider=MS Remote;Data Source=Pubs;" & _  
        "Remote Server=https://yourServer;Remote Provider=SQLOLEDB;"  
```  
  
 **Partie B** La méthode générale pour effectuer cette étape consiste à appeler la méthode de [requête](../../reference/rds-api/query-method-rds.md) d’objet **RDSServer. DataFactory** . Cette méthode prend une chaîne de connexion, qui est utilisée pour se connecter à une source de données, et un texte de commande, qui est utilisé pour spécifier les lignes à retourner à partir de la source de données.  
  
 Ce didacticiel utilise la méthode de **requête** d’objet **DataFactory** :  
  
```vb
Sub RDSTutorial2B()  
   Dim DS as New RDS.DataSpace  
   Dim DF  
   Dim RS as ADODB.Recordset  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 3 : le serveur obtient un Recordset (didacticiel RDS)](./step-3-server-obtains-a-recordset-rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)