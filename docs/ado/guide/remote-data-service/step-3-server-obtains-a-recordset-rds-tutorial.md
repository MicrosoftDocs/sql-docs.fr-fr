---
description: 'Étape 3 : Le serveur obtient un recordset (tutoriel RDS)'
title: 'Étape 3 : le serveur obtient un Recordset (didacticiel RDS) | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS tutorial [ADO], server obtains Recordset
ms.assetid: 9c6779c9-1208-4696-ac51-c39f3a6d9240
author: rothja
ms.author: jroth
ms.openlocfilehash: f8da4957f44c481e2a89ca1c8c964880f1ca93fd
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88451931"
---
# <a name="step-3-server-obtains-a-recordset-rds-tutorial"></a>Étape 3 : Le serveur obtient un recordset (tutoriel RDS)
Le programme serveur utilise la chaîne de connexion et le texte de commande pour interroger la source de données pour les lignes souhaitées. ADO est généralement utilisé pour récupérer ce **Recordset**, bien que d’autres interfaces d’accès aux données Microsoft, telles que OLE DB, puissent être utilisées.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Un programme serveur personnalisé peut se présenter comme suit :  
  
```vb
Public Function ServerProgram(cn as String, qry as String) as Object  
Dim rs as New ADODB.Recordset  
   rs.CursorLocation = adUseClient  
   rs.Open qry, cn   
   rs.ActiveConnection = Nothing  
   Set ServerProgram = rs  
End Function  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Étape 4 : le serveur retourne le recordset (didacticiel RDS)](../../../ado/guide/remote-data-service/step-4-server-returns-the-recordset-rds-tutorial.md)   
 [Tutoriel RDS (VBScript)](../../../ado/guide/remote-data-service/rds-tutorial-vbscript.md)   
