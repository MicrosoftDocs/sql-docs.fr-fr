---
description: Boutons de commande de l’application Carnet d’adresses
title: Boutons de commande du carnet d’adresses | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO], command buttons
- RDS scenarios [ADO], command buttons
ms.assetid: 80676831-6488-4dad-a558-c47c52256a22
author: rothja
ms.author: jroth
ms.openlocfilehash: dccdb22981bb1662cd50da1e28db8fa0a2ce47d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032044"
---
# <a name="address-book-command-buttons"></a>Boutons de commande de l’application Carnet d’adresses
L’application Carnet d’adresses contient les boutons de commande suivants :  
  
-   Bouton **Rechercher** pour envoyer une requête à la base de données.  
  
-   Bouton **Effacer** pour effacer les zones de texte avant de commencer une nouvelle recherche.  
  
-   Bouton **mettre à jour le profil** pour enregistrer les modifications apportées à un enregistrement d’employé.  
  
-   Bouton **annuler les modifications** pour abandonner les modifications.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="find-button"></a>Bouton Rechercher  
 Le fait de cliquer sur le bouton **Rechercher** active la sous-procédure VBScript Find_OnClick, qui génère et envoie la requête SQL. Cliquer sur ce bouton remplit la grille de données.  
  
## <a name="building-the-sql-query"></a>Génération de la requête SQL  
 La première partie de la procédure Find_OnClick Sub génère la requête SQL, une expression à la fois, en ajoutant des chaînes de texte à une instruction SQL SELECT globale. Elle commence par définir la variable `myQuery` sur une instruction SQL SELECT qui demande toutes les lignes de données de la table de source de données. Ensuite, la procédure Sub analyse chacune des quatre zones d’entrée de la page.  
  
 Étant donné que le programme utilise le mot `like` dans la création des instructions SQL, les requêtes sont des recherches sous-chaînes plutôt que des correspondances exactes.  
  
 Par exemple, si la zone **nom de famille** contenait l’entrée « Berge » et que la zone de **titre** contenait l’entrée « gestionnaire de programmes », l’instruction SQL (valeur de `myQuery` ) lira :  
  
```sql
Select FirstName, LastName, Title, Email, Building, Room, Phone from Employee where lastname like 'Berge%' and title like 'Program Manager%'  
```  
  
 Si la requête a réussi, toutes les personnes dont le nom contient le texte « berge » (par exemple, berge et Berger) et dont le titre contient les mots « responsable de programme » (par exemple, chef de programme, technologies avancées) s’affichent dans la grille de données HTML.  
  
## <a name="preparing-and-sending-the-query"></a>Préparation et envoi de la requête  
 La dernière partie de la procédure Find_OnClick Sub est constituée de deux instructions. La première instruction affecte la propriété [SQL](../../reference/rds-api/sql-property.md) de l' [objet RDS. Objet DataControl](../../reference/rds-api/datacontrol-object-rds.md) égal à la requête SQL générée dynamiquement. La deuxième instruction provoque l' **objet RDS. DataControl** ( `DC1` ) pour interroger la base de données, puis afficher les nouveaux résultats de la requête dans la grille.  
  
```vb
Sub Find_OnClick  
   '...  
   DC1.SQL = myQuery  
   DC1.Refresh  
End Sub  
```  
  
## <a name="update-profile-button"></a>Bouton mettre à jour le profil  
 En cliquant sur le bouton **mettre à jour le profil** , vous activez la sous-procédure VBScript Update_OnClick, qui exécute les [services Bureau à distance.](../../reference/rds-api/datacontrol-object-rds.md) `DC1` Méthodes [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) et [Refresh](../../reference/rds-api/refresh-method-rds.md) de l’objet DataControl.  
  
```vb
Sub Update_OnClick  
   DC1.SubmitChanges  
   DC1.Refresh  
End Sub  
```  
  
 Lors de `DC1.SubmitChanges` l’exécution, le service de données distant transmet toutes les informations de mise à jour et les envoie au serveur via http. La mise à jour est tout-ou-rien ; Si une partie de la mise à jour échoue, aucune des modifications n’est apportée et un message d’État est renvoyé. `DC1.Refresh` n’est pas nécessaire après **SubmitChanges** avec le service de données distant, mais elle garantit l’actualisation des données.  
  
## <a name="cancel-changes-button"></a>Bouton Annuler les modifications  
 Le fait de cliquer sur **annuler les modifications** active la sous-procédure VBScript Cancel_OnClick, qui exécute les [services Bureau à distance. DataControl](../../reference/rds-api/datacontrol-object-rds.md) de l’objet ( `DC1)` méthode [CancelUpdate](../../reference/rds-api/cancelupdate-method-rds.md) .  
  
```vb
Sub Cancel_OnClick  
   DC1.CancelUpdate  
End Sub  
```  
  
 Lorsque `DC1.CancelUpdate` s’exécute, il ignore toutes les modifications apportées par un utilisateur à un enregistrement d’employé sur la grille de données depuis la dernière requête ou mise à jour. Il restaure les valeurs d’origine.  
  
## <a name="see-also"></a>Voir aussi  
 [Boutons de navigation dans le carnet d’adresses](./address-book-navigation-buttons.md)   
 [DataControl, objet (RDS)](../../reference/rds-api/datacontrol-object-rds.md)