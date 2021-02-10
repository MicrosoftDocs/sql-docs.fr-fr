---
description: Boutons de navigation de l’application Carnet d’adresses
title: Boutons de navigation dans le carnet d’adresses | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], navigation buttons
- address book application scenario [ADO], navigation buttons
ms.assetid: f0dd84c6-5c33-4ab9-82b4-4c42dfdd2277
author: rothja
ms.author: jroth
ms.openlocfilehash: e9ee0bb698c64de6fb04f776caa4e34c27b8d49a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032059"
---
# <a name="address-book-navigation-buttons"></a>Boutons de navigation de l’application Carnet d’adresses
L’application Carnet d’adresses affiche les boutons de navigation en bas de la page Web. Vous pouvez utiliser les boutons de navigation pour parcourir les données dans l’affichage de grille HTML en sélectionnant la première ou la dernière ligne de données ou les lignes adjacentes à la sélection actuelle.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="navigation-sub-procedures"></a>Procédures Sub de navigation  
 L’application Carnet d’adresses contient plusieurs procédures qui permettent aux utilisateurs de cliquer sur les boutons **premier**, **suivant**, **précédent** et **dernier** pour se déplacer dans les données.  
  
 Par exemple, un clic sur le **premier** bouton active la procédure Sub First_OnClick VBScript. La procédure exécute une méthode [MoveFirst](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , qui fait de la première ligne de données la sélection actuelle. Le fait de cliquer sur le **dernier** bouton active la procédure Last_OnClick Sub, qui appelle la méthode [MoveLast](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md) , en faisant de la dernière ligne de données la sélection actuelle. Les autres boutons de navigation fonctionnent de la même manière.  
  
```vb
' Move to the first record in the bound Recordset.  
Sub First_OnClick  
   DC1.Recordset.MoveFirst  
End Sub  
  
' Move to the next record from the current position   
' in the bound Recordset.  
Sub Next_OnClick  
   If Not DC1.Recordset.EOF Then    ' Cannot move beyond bottom record.  
      DC1.Recordset.MoveNext  
      Exit Sub  
   End If     
End Sub  
  
' Move to the previous record from the current position in the bound   
' Recordset.  
Sub Prev_OnClick  
   If Not DC1.Recordset.BOF Then    ' Cannot move beyond top record.  
      DC1.Recordset.MovePrevious  
      Exit Sub  
   End If  
End Sub  
  
' Move to the last record in the bound Recordset.  
Sub Last_OnClick  
   DC1.Recordset.MoveLast  
End Sub  
```  
  
## <a name="see-also"></a>Voir aussi  
 [DataControl, objet (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [MoveFirst, MoveLast, MoveNext et MovePrevious, méthodes (RDS)](../../reference/rds-api/movefirst-movelast-movenext-and-moveprevious-methods-rds.md)