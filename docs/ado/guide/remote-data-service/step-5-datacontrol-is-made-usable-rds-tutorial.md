---
description: 'Étape 5 : DataControl devient utilisable (tutoriel RDS)'
title: 'Étape 5 : le DataControl est rendu utilisable (didacticiel RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], datacontrol made usable
ms.assetid: ed5c4a24-9804-4c85-817e-317652acb9b4
author: rothja
ms.author: jroth
ms.openlocfilehash: 6453043514657b03e72eeea032451f647f6c2eec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036089"
---
# <a name="step-5-datacontrol-is-made-usable-rds-tutorial"></a>Étape 5 : DataControl devient utilisable (tutoriel RDS)
L’objet **Recordset** retourné peut être utilisé. Vous pouvez l’examiner, le parcourir ou le modifier comme n’importe quel autre **jeu d’enregistrements**. Ce que vous pouvez faire avec le **Recordset** dépend de votre environnement. Visual Basic et Visual C++ disposent de contrôles visuels qui peuvent utiliser un **jeu d’enregistrements** directement ou indirectement avec l’aide d’un contrôle d’activation des données.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Par exemple, si vous affichez une page Web dans Microsoft Internet Explorer, vous souhaiterez peut-être afficher les données de l’objet **Recordset** dans un contrôle visuel. Les contrôles visuels d’une page Web ne peuvent pas accéder directement à un objet **Recordset** . Toutefois, ils peuvent accéder à l’objet **Recordset** via le [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md). **RDS. DataControl** devient utilisable par un contrôle visuel lorsque sa propriété [SourceRecordset](../../reference/rds-api/recordset-sourcerecordset-properties-rds.md) est définie sur l’objet **Recordset** .  
  
 Le paramètre **dataSrc** de l’objet de contrôle visuel doit être défini sur **RDS. DataControl**, et sa propriété **dataFld** définie sur un champ d’objet **Recordset** (colonne).  
  
 Dans ce didacticiel, définissez la propriété **SourceRecordset** :  
  
```vb
Sub RDSTutorial5()  
   Dim DS as New RDS.DataSpace  
   Dim RS as ADODB.Recordset  
   Dim DC as New RDS.DataControl  
   Dim DF as Object  
   Set DF = DS.CreateObject("RDSServer.DataFactory", "https://yourServer")  
   Set RS = DF.Query ("DSN=Pubs", "SELECT * FROM Authors")  
   DC.SourceRecordset = RS         ' Visual controls can now bind to DC.  
...  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 6 : les modifications sont envoyées au serveur (didacticiel RDS)](./step-6-changes-are-sent-to-the-server-rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)