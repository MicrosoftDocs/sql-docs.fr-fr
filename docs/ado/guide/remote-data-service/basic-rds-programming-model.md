---
description: Modèle de programmation RDS de base
title: Modèle de programmation RDS de base | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO]
ms.assetid: 0bdd236b-edff-4aac-94c3-93e1465ca6c5
author: rothja
ms.author: jroth
ms.openlocfilehash: 81d789c003921b4e8359df281cb9fa51dbdf1f26
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036578"
---
# <a name="basic-rds-programming-model"></a>Modèle de programmation RDS de base
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 RDS traite les applications qui existent dans l’environnement suivant : une application cliente spécifie un programme qui s’exécute sur un serveur et les paramètres requis pour retourner les informations souhaitées. Le programme appelé sur le serveur obtient l’accès à la source de données spécifiée, récupère les informations, traite éventuellement les données, puis retourne les informations obtenues à votre application cliente dans un format utilisable facilement. RDS vous offre la possibilité d’effectuer la séquence d’actions suivante :  
  
-   Spécifiez le programme à appeler sur le serveur et obtenez une méthode pour y faire référence à partir du client. (Cette référence est parfois appelée *proxy*. Il représente le programme serveur distant. L’application cliente « appellera » le proxy comme s’il s’agissait d’un programme local, mais il appelle en fait le programme serveur distant.)  
  
-   Appelez le programme serveur. Transmettez des paramètres au programme serveur qui identifient la source de données et la commande à émettre. (Le programme serveur utilise en fait ADO pour accéder à la source de données. ADO établit une connexion avec l’un des paramètres donnés, puis émet la commande spécifiée dans l’autre paramètre.)  
  
-   Le programme serveur obtient un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à partir de la source de données. Le cas échéant, l’objet **Recordset** est traité sur le serveur.  
  
-   Le programme serveur retourne l’objet **Recordset** final à l’application cliente.  
  
-   Sur le client, l’objet **Recordset** est placé dans un formulaire qui peut être facilement utilisé par les contrôles visuels.  
  
-   Toutes les modifications apportées à l’objet **Recordset** sont renvoyées au programme serveur, qui les utilise pour mettre à jour la source de données.  
  
 Ce modèle de programmation contient certaines fonctionnalités pratiques. Si vous n’avez pas besoin d’un programme serveur complexe pour accéder à la source de données, et si vous fournissez les paramètres de connexion et de commande requis, RDS récupère automatiquement les données spécifiées avec un programme serveur par défaut simple.  
  
 Si vous avez besoin d’un traitement plus complexe, vous pouvez spécifier votre propre programme serveur personnalisé. Par exemple, étant donné qu’un programme serveur personnalisé a la pleine puissance d’ADO, il peut se connecter à plusieurs sources de données différentes, combiner leurs données de manière complexe, puis renvoyer un résultat simple et traité à l’application cliente.  
  
 Enfin, si vos besoins se situent entre les deux, ADO prend désormais en charge la personnalisation du comportement du programme serveur par défaut.  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle de programmation RDS en détail](./rds-programming-model-in-detail.md)   
 [Scénario RDS](./rds-scenario.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Recordset, objet (ADO)](../../reference/ado-api/recordset-object-ado.md)   
 [Utilisation et sécurité de RDS](./rds-usage-and-security.md)