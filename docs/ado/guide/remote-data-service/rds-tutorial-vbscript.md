---
description: Tutoriel RDS (VBScript)
title: Didacticiel RDS (VBScript) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- RDS tutorial [ADO], VBScript
ms.assetid: e2a48c4d-88b1-43ff-a202-9cdec54997d2
author: rothja
ms.author: jroth
ms.openlocfilehash: 693f5858a80d5bc986dc16f993aa5c5a78c54d7c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031871"
---
# <a name="rds-tutorial-vbscript"></a>Tutoriel RDS (VBScript)
Il s’agit du didacticiel RDS, écrit dans Microsoft Visual Basic Scripting Edition. Pour obtenir une description de l’objectif de ce didacticiel, consultez le didacticiel sur les [services Bureau à distance](./rds-tutorial.md).  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Dans ce didacticiel, [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) et [RDS. DataSpace](../../reference/rds-api/dataspace-object-rds.md) sont créés au moment de la conception, c’est-à-dire qu’ils sont définis avec des balises d’objet, comme suit : `<OBJECT>...</OBJECT>` . Elles peuvent également être créées au moment de l’exécution à l’aide de la méthode [CreateObject (RDS)](../../reference/rds-api/createobject-method-rds.md) . Par exemple, le **RDS.** L’objet DataControl a pu être créé comme suit :  
  
```vb
Set DC = Server.CreateObject("RDS.DataControl")  
   <!-- RDS.DataControl -->  
   <OBJECT   
      ID="DC1" CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E33">  
   </OBJECT>  
  
   <!-- RDS.DataSpace -->  
   <OBJECT   
      ID="DS1" WIDTH=1 HEIGHT=1  
      CLASSID="CLSID:BD96C556-65A3-11D0-983A-00C04FC29E36">  
   </OBJECT>  
  
   <SCRIPT LANGUAGE="VBScript">  
  
   Sub RDSTutorial()  
   Dim DF1   
```  
  
## <a name="step-1---specify-a-server-program"></a>Étape 1 : spécifier un programme serveur  
 VBScript peut découvrir le nom du serveur Web IIS sur lequel il s’exécute en accédant à la méthode VBScript **Request. ServerVariables** disponible pour Active Server pages :  
  
```vb
"https://<%=Request.ServerVariables("SERVER_NAME")%>"  
```  
  
 Toutefois, pour ce didacticiel, utilisez le serveur imaginaire, « yourServer ».  
  
> [!NOTE]
>  Faites attention au type de données des arguments **ByRef** . VBScript ne vous permet pas de spécifier le type de variable. vous devez donc toujours passer un **Variant**. Si vous utilisez le protocole HTTP, RDS vous permet de passer un variant à une méthode qui attend une valeur non variant si vous l’appelez avec le **RDS.** Méthode [CreateObject](../../reference/rds-api/createobject-method-rds.md) de l’objet DataSpace. Lorsque vous utilisez DCOM ou un serveur in-process, vous devez faire correspondre les types de paramètres côté client et serveur, sans quoi vous recevrez une erreur de type « incompatibilité de type ».  
  
```vb
Set DF1 = DS1.CreateObject("RDSServer.DataFactory", "https://yourServer")  
```  
  
## <a name="step-2a---invoke-the-server-program-with-rdsdatacontrol"></a>Étape 2a : appeler le programme serveur avec les services Bureau à distance. DataControl  
 Cet exemple est simplement un commentaire montrant que le comportement par défaut de l' **objet RDS. DataControl** consiste à exécuter la requête spécifiée.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial2A()  
   Dim RS  
   DC1.Refresh  
   Set RS = DC1.Recordset  
...  
```  
  
## <a name="step-2b---invoke-the-server-program-with-rdsserverdatafactory"></a>Étape 2b : appeler le programme serveur avec RDSServer. DataFactory  
  
## <a name="step-3---server-obtains-a-recordset"></a>Étape 3 : le serveur obtient un jeu d’enregistrements  
  
## <a name="step-4---server-returns-the-recordset"></a>Étape 4-le serveur retourne l’ensemble d’enregistrements  
  
```vb
Set RS = DF1.Query("DSN=Pubs;", "SELECT * FROM Authors")  
```  
  
## <a name="step-5---datacontrol-is-made-usable-by-visual-controls"></a>Étape 5 : l’DataControl est rendu utilisable par les contrôles visuels  
  
```vb
' Assign the returned recordset to the DataControl.  
  
DC1.SourceRecordset = RS  
```  
  
## <a name="step-6a---changes-are-sent-to-the-server-with-rdsdatacontrol"></a>Étape 6A-les modifications sont envoyées au serveur avec RDS. DataControl  
 Cet exemple est simplement un commentaire montrant comment le **RDS. DataControl** effectue des mises à jour.  
  
```vb
<OBJECT CLASSID="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33" ID="DC1">  
   <PARAM NAME="SQL" VALUE="SELECT * FROM Authors">  
   <PARAM NAME="Connect" VALUE="DSN=Pubs;">  
   <PARAM NAME="Server" VALUE="https://yourServer/">  
</OBJECT>  
...  
<SCRIPT LANGUAGE="VBScript">  
  
Sub RDSTutorial6A()  
Dim RS  
DC1.Refresh  
...  
Set RS = DC1.Recordset  
' Edit the Recordset object...  
' The SERVER and CONNECT properties are already set from Step 2A.  
Set DC1.SourceRecordset = RS  
...  
DC1.SubmitChanges  
```  
  
## <a name="step-6b---changes-are-sent-to-the-server-with-rdsserverdatafactory"></a>Étape 6B-les modifications sont envoyées au serveur avec RDSServer. DataFactory  
  
```vb
DF.SubmitChanges "DSN=Pubs", RS  
  
End Sub  
</SCRIPT>  
</BODY>  
</HTML>  
```  
  
 **Ceci est la fin du tutoriel.**  
  
## <a name="see-also"></a>Voir aussi  
 [Tutoriel RDS](./rds-tutorial.md)