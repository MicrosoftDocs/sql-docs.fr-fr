---
description: Modèle de programmation RDS avec des objets
title: Modèle de programmation RDS avec objets | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS programming model [ADO]
- RDS objects [ADO]
ms.assetid: 07ce0ef0-72f1-48f4-823d-1b65d28c0926
author: rothja
ms.author: jroth
ms.openlocfilehash: 282e272d1d830bfb20f407053a04a6d0b598c490
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036359"
---
# <a name="rds-programming-model-with-objects"></a>Modèle de programmation RDS avec des objets
L’objectif de RDS est d’accéder aux sources de données et de les mettre à jour par le biais d’un intermédiaire tel qu’IIS. Le modèle de programmation spécifie la séquence d’activités nécessaire pour atteindre cet objectif. Le modèle objet spécifie les objets dont les méthodes et les propriétés affectent le modèle de programmation.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Les services Bureau à distance permettent d’effectuer la séquence d’actions suivante :  
  
-   Spécifiez le programme à appeler sur le serveur et obtenez une méthode (proxy) pour y faire référence à partir du client ([RDS. DataSpace](../../reference/rds-api/dataspace-object-rds.md)).  
  
-   Appelez le programme serveur. Transmettez des paramètres au programme serveur qui identifie la source de données et la commande à émettre (proxy ou [RDS. DataControl](../../reference/rds-api/datacontrol-object-rds.md)).  
  
-   Le programme serveur obtient un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à partir de la source de données, généralement à l’aide d’ADO. Si vous le souhaitez, l’objet **Recordset** est traité sur le serveur ([RDSServer. DataFactory](../../reference/rds-api/datafactory-object-rdsserver.md)).  
  
-   Le programme serveur retourne l’objet **Recordset** final à l’application cliente (proxy).  
  
-   Sur le client, l’objet **Recordset** est placé dans un formulaire qui peut être facilement utilisé par les contrôles visuels (contrôle visuel et **RDS). DataControl**).  
  
-   Les modifications apportées à l’objet **Recordset** sont renvoyées au serveur et utilisées pour mettre à jour la source de données (**RDS. DataControl** ou **RDSServer. DataFactory**).  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du modèle objet RDS](./rds-object-model-summary.md)   
 [DataControl, objet (RDS)](../../reference/rds-api/datacontrol-object-rds.md)   
 [Objet DataFactory (RDSServer)](../../reference/rds-api/datafactory-object-rdsserver.md)   
 [DataSpace, objet (RDS)](../../reference/rds-api/dataspace-object-rds.md)   
 [Scénario RDS](./rds-scenario.md)   
 [Didacticiel RDS](./rds-tutorial.md)   
 [Recordset, objet (ADO)](../../reference/ado-api/recordset-object-ado.md)   
 [Utilisation et sécurité de RDS](./rds-usage-and-security.md)