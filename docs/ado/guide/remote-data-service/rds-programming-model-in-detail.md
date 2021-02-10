---
description: Modèle de programmation RDS en détail
title: Modèle de programmation RDS en détail | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO], details
ms.assetid: 3e57af8d-519b-4467-a0bd-af468534cefd
author: rothja
ms.author: jroth
ms.openlocfilehash: 8421340798a308204a0961340e06216f3b0cb24a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031891"
---
# <a name="rds-programming-model-in-detail"></a>Modèle de programmation RDS en détail
Voici les éléments clés du modèle de programmation RDS :  
  
-   RDS.DataSpace  
  
-   RDSServer.DataFactory  
  
-   RDS.DataControl  
  
-   Événement  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
## <a name="rdsdataspace"></a>RDS.DataSpace  
 Votre application cliente doit spécifier le serveur et le programme serveur à appeler. En retour, votre application reçoit une référence au programme serveur et peut traiter la référence comme s’il s’agissait du programme serveur lui-même.  
  
 Le modèle objet RDS intègre cette fonctionnalité au [RDS. Objet DataSpace](../../reference/rds-api/dataspace-object-rds.md) .  
  
 Le programme serveur est spécifié avec un identificateur de programme, ou *ProgID*. Le serveur utilise le *ProgID* et le registre de l’ordinateur serveur pour trouver des informations sur le programme à lancer.  
  
 RDS effectue une distinction en interne selon que le programme serveur se trouve sur un serveur distant sur Internet ou sur un intranet. un serveur sur un réseau local ; ou non sur un serveur, mais plutôt sur une bibliothèque de liens dynamiques (DLL) locale. Cette distinction détermine la façon dont les informations sont échangées entre le client et le serveur, et fait une différence tangible dans le type de référence retourné à votre application cliente. Toutefois, à partir de votre point de vue, cette distinction n’a aucune signification particulière. Ce qui est important, c’est que vous recevez une référence de programme utilisable.  
  
## <a name="rdsserverdatafactory"></a>RDSServer.DataFactory  
 RDS fournit un programme serveur par défaut qui peut exécuter une requête SQL sur la source de données et retourner un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) ou accepter un objet **Recordset** et mettre à jour la source de données.  
  
 Le modèle objet RDS intègre cette fonctionnalité à l’objet [RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md) .  
  
 En outre, cet objet a une méthode pour créer un objet **Recordset** vide que vous pouvez remplir par programme ([CreateRecordset](../../reference/rds-api/createrecordset-method-rds.md)) et une autre méthode pour convertir un objet **Recordset** en chaîne de texte pour générer une page Web ([ConvertToString](../../reference/rds-api/converttostring-method-rds.md)).  
  
 Avec ADO, vous pouvez remplacer une partie du comportement standard de la connexion et de la commande de **RDSServer. DataFactory** par un gestionnaire **DataFactory** et un fichier de personnalisation qui contient des paramètres de connexion, de commande et de sécurité.  
  
 Le programme serveur est parfois appelé *objet métier*. Vous pouvez écrire votre propre objet métier personnalisé qui peut effectuer des opérations d’accès aux données complexes, des vérifications de validité, etc. Même lors de l’écriture d’un objet métier personnalisé, vous pouvez créer une instance d’un objet **RDSServer. DataFactory** et utiliser certaines de ses méthodes pour accomplir vos propres tâches.  
  
## <a name="rdsdatacontrol"></a>RDS.DataControl  
 RDS fournit un moyen de combiner les fonctionnalités de l' **objet RDS. DataSpace** et **RDSServer. DataFactory**, et permettent également aux contrôles visuels d’utiliser facilement l’objet **Recordset** renvoyé par une requête à partir d’une source de données. RDS tente, pour le cas le plus courant, de faire autant que possible d’accéder automatiquement aux informations sur un serveur et de les afficher dans un contrôle visuel.  
  
 Le modèle objet RDS intègre cette fonctionnalité au [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md) .  
  
 **RDS. DataControl** a deux aspects. Un aspect est lié à la source de données. Si vous définissez la commande et les informations de connexion à l’aide des propriétés **Connect** et **SQL** de l' **objet RDS. DataControl**, il utilisera automatiquement le **RDS. DataSpace** pour créer une référence à l’objet **RDSServer. DataFactory** par défaut. Ensuite, l’objet **RDSServer. DataFactory** utilise la valeur de la propriété **Connect** pour se connecter à la source de données, utilise la valeur de la propriété **SQL** pour obtenir un **jeu d’enregistrements** de la source de données et retourne l’objet **Recordset** à l’objet **RDS. DataControl**.  
  
 Le deuxième aspect concerne l’affichage des informations de l’ensemble d' **enregistrements** retourné dans un contrôle visuel. Vous pouvez associer un contrôle visuel à **RDS. DataControl** (dans un processus appelé Binding) et accéder aux informations contenues dans l’objet **Recordset** associé, affichant les résultats de la requête sur une page Web dans Microsoft® Internet Explorer. Chaque **RDS. DataControl** lie un objet **Recordset** , représentant les résultats d’une requête unique, à un ou plusieurs contrôles visuels (par exemple, une zone de texte, une zone de liste déroulante, un contrôle Grid, etc.). Il peut y avoir plusieurs **RDS. DataControl** sur chaque page. Chaque **RDS.** L’objet DataControl peut être connecté à une source de données différente et contenir les résultats d’une requête distincte.  
  
 **RDS. DataControl** possède également ses propres méthodes pour naviguer, trier et filtrer les lignes de l’objet **Recordset** associé. Ces méthodes sont similaires, mais pas les mêmes que les méthodes de l’objet **Recordset** ADO.  
  
## <a name="events"></a>Événements  
 RDS prend en charge deux de ses propres événements, qui sont indépendants du modèle d’événement ADO. L’événement [onreadystatechange](../../reference/rds-api/onreadystatechange-event-rds.md) est appelé chaque fois que le **RDS.** La propriété [ReadyState](../../reference/rds-api/readystate-property-rds.md) de DataControl change, ce qui vous avertit quand une opération asynchrone s’est terminée avec succès, s’est terminée ou a rencontré une erreur. L’événement [OnError](../../reference/rds-api/onerror-event-rds.md) est appelé chaque fois qu’une erreur se produit, même si l’erreur se produit pendant une opération asynchrone.  
  
> [!NOTE]
>  Microsoft Internet Explorer fournit deux événements supplémentaires à RDS : **ondatasetchanged**, qui indique que le **jeu d’enregistrements** est fonctionnel tout en récupérant les lignes, et **ondatasetcomplete**, qui indique que le **jeu d’enregistrements** a terminé la récupération de lignes.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation RDS avec des objets](./rds-programming-model-with-objects.md)   
 [DataControl, objet (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Objet DataFactory (RDSServer)](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [DataSpace, objet (RDS)](../../reference/rds-api/dataspace-object-rds.md)   
 [Scénario RDS](./rds-scenario.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Utilisation et sécurité de RDS](./rds-usage-and-security.md)