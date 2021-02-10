---
description: 'Étape 1 : Spécifier un programme serveur (tutoriel RDS)'
title: 'Étape 1 : spécifier un programme serveur (didacticiel RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], specifying server program
ms.assetid: d8bb35b1-c02a-4231-8d55-016e56e53b95
author: rothja
ms.author: jroth
ms.openlocfilehash: d0e59c52ee311c5172b3cdc15f58c5e9448f7110
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036398"
---
# <a name="step-1-specify-a-server-program-rds-tutorial"></a>Étape 1 : Spécifier un programme serveur (tutoriel RDS)
Dans le cas le plus général, utilisez le [RDS. ](../../reference/rds-api/dataspace-object-rds.md) La méthode [CreateObject](../../reference/rds-api/createobject-method-rds.md) de l’objet DataSpace permet de spécifier le programme serveur par défaut, [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md), ou votre propre programme serveur personnalisé (objet métier). Un programme serveur est instancié sur le serveur, et une référence au programme serveur, ou *proxy*, est retournée.  
  
 Ce didacticiel utilise le programme serveur par défaut :  
  
```vb
Sub RDSTutorial1()  
   Dim DS as New RDS.DataSpace  
   Dim DF as Object  
   Set DF = DS.("RDSServer.DataFactory", "https://yourServer")  
...  
```  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 2 : appeler le programme serveur (didacticiel RDS)](./step-2-invoke-the-server-program-rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](./rds-tutorial-vbscript.md)