---
description: 'Étape 6 : Les changements sont envoyés au serveur (tutoriel RDS)'
title: 'Étape 6 : les modifications sont envoyées au serveur (didacticiel RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], changes sent to server
ms.assetid: b1e927d6-7d50-4978-9eef-045043cdce7a
author: rothja
ms.author: jroth
ms.openlocfilehash: d6a769a22a86e145727079af6fc067b1552f2bbf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031711"
---
# <a name="step-6-changes-are-sent-to-the-server-rds-tutorial"></a>Étape 6 : Les changements sont envoyés au serveur (tutoriel RDS)
Si l’objet **Recordset** est modifié, toute modification (c’est-à-dire les lignes ajoutées, modifiées ou supprimées) peut être renvoyée au serveur.  
  
> [!NOTE]
>  Le comportement par défaut de RDS peut être appelé implicitement avec les objets ADO et le fournisseur Microsoft OLE DB Remoting. Les requêtes peuvent retourner des **jeux d’enregistrements**, et les **jeux d’enregistrements** modifiés peuvent mettre à jour la source de données. Ce didacticiel n’appelle pas RDS avec des objets ADO, mais c’est là qu’il s’agit d’un résultat :  
  
```vb
Dim rs as New ADODB.Recordset  
rs. "SELECT * FROM Authors","=MS Remote;=Pubs;" & _  
=https://yourServer;=SQLOLEDB;"  
...              ' Edit the Recordset.  
rs.   ' The equivalent of   
...  
```  
  
 **Partie A** Supposons que vous avez utilisé uniquement les [services Bureau à distance. DataControl](../../reference/rds-api/datacontrol-object-rds.md) et qu’un objet **Recordset** est maintenant associé à **RDS. DataControl**. La méthode [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) met à jour la source de données avec toutes les modifications apportées à l’objet **Recordset** si les propriétés [Server](../../reference/rds-api/server-property-rds.md) et [Connect](../../reference/rds-api/connect-property-rds.md) sont toujours définies.  
  
```vb
Sub RDSTutorial6A()  
Dim DC as New RDS.DataControl  
Dim RS as ADODB.Recordset  
DC. = "https://yourServer"  
DC. = "DSN=Pubs"  
DC. = "SELECT * FROM Authors"  
DC.  
...  
Set RS = DC.  
   ' Edit the Recordset.  
...  
DC.  
...  
```  
  
 **Partie B** Vous pouvez également mettre à jour le serveur avec l’objet [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) , en spécifiant une connexion et un objet **Recordset** .  
  
```vb
Sub RDSTutorial6B()  
Dim DS As New RDS.DataSpace  
Dim RS As ADODB.Recordset  
Dim DC As New RDS.DataControl  
Dim DF As Object  
Dim blnStatus As Boolean  
Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
Set RS = DF. ("DSN=Pubs", "SELECT * FROM Authors")  
DC. = RS    ' Visual controls can now bind to DC.  
    ' Edit the Recordset.  
blnStatus = DF."DSN=Pubs", RS  
End Sub  
```  
  
 **Ceci est la fin du tutoriel.**  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Voir aussi  
 [Fournisseur Microsoft OLE DB Remoting (fournisseur de services ADO)](../appendixes/microsoft-ole-db-remoting-provider-ado-service-provider.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)