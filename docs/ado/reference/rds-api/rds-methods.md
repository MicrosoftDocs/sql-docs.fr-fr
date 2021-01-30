---
description: Méthodes RDS
title: Méthodes RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- RDS methods [ADO]
- methods [ADO], RDS
ms.assetid: c2c6af1a-3c44-4c9d-ad33-b381552c71af
author: rothja
ms.author: jroth
ms.openlocfilehash: c5eb9926646d247bc252232d3fab155337f03890
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168834"
---
# <a name="rds-methods"></a>Méthodes RDS
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
|Méthode|Description|  
|-|-|  
|[Annuler (RDS)](./cancel-method-rds.md)|Annule l’exécution d’un appel de méthode asynchrone en attente.|  
|[CancelUpdate (RDS)](./cancelupdate-method-rds.md)|Annule toutes les modifications apportées à la ligne actuelle ou nouvelle d’un objet **Recordset** .|  
|[ConvertToString (RDS)](./converttostring-method-rds.md)|Convertit un **Recordset** en une chaîne MIME qui représente les données du Recordset.|  
|[CreateObject (RDS)](./createobject-method-rds.md)|Crée le proxy pour l’objet métier cible et retourne un pointeur vers celui-ci.|  
|[CreateRecordset (RDS)](./createrecordset-method-rds.md)|Crée un **jeu d’enregistrements** vide et déconnecté.|  
|[Execute, méthode (RDS)](./execute-method-rds.md)|Exécutez la demande et créez un ensemble de lignes de données avancé (pour une utilisation avec ADO 2,5 et versions ultérieures).|  
|[Execute21, méthode (RDS)](./execute21-method-rds.md)|Exécutez la demande et créez un ensemble de lignes de données avancé (pour une utilisation avec ADO 2,1).|  
|[InvokeService (RDS)](./invokeservice-rds.md)|Retourne un pointeur vers l’interface demandée sur une version plus puissante de l’objet.|  
|[MoveFirst, MoveLast, MoveNext, MovePrevious (RDS)](./movefirst-movelast-movenext-and-moveprevious-methods-rds.md)|Passe au premier enregistrement, dernier, suivant ou précédent dans un objet **Recordset** spécifié.|  
|[Requête (RDS)](./query-method-rds.md)|Utilise une chaîne de requête SQL valide pour retourner un **jeu d’enregistrements**.|  
|[Actualiser (RDS)](./refresh-method-rds.md)|Interroge à jour la source de données spécifiée dans la propriété **Connect** et met à jour les résultats de la requête.|  
|[Réinitialisation (RDS)](./reset-method-rds.md)|Exécute le tri ou le filtre sur un **jeu d’enregistrements** côté client, en fonction des propriétés de tri et de filtre spécifiées.|  
|[SubmitChanges (RDS)](./submitchanges-method-rds.md)|Soumet les modifications en attente de l' **objet Recordset** mis à jour et mis en cache localement à la source de données spécifiée dans la propriété **Connect** .|  
|[Synchronize, méthode (RDS)](./synchronize-method-rds.md)|Synchronise le Recordset donné avec la base de données spécifiée par la chaîne de connexion (pour une utilisation avec ADO 2,5 et versions ultérieures).|  
|[Synchronize21, méthode (RDS)](./synchronize21-method-rds.md)|Synchronise le Recordset donné avec la base de données spécifiée par la chaîne de connexion (pour une utilisation avec ADO 2,1).|