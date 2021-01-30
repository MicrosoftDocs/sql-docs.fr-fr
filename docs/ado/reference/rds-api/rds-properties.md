---
description: Propriétés RDS
title: Propriétés RDS | Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- RDS properties [ADO]
- properties [ADO], RDS
ms.assetid: e4e04cbd-21fc-44a1-9f21-49aa68746934
author: rothja
ms.author: jroth
ms.openlocfilehash: 6bf9a232f2dadfc562cd6e7e183858c3a7f64e9c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168831"
---
# <a name="rds-properties"></a>Propriétés RDS
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
|Propriété|Description|  
|-|-|  
|[Connexion (RDS)](./connect-property-rds.md)|Indique le nom de la base de données à partir de laquelle les opérations de requête et de mise à jour sont exécutées.|  
|[ExecuteOptions (RDS)](./executeoptions-property-rds.md)|Indique si l’exécution asynchrone est activée.|  
|[FetchOptions (RDS)](./fetchoptions-property-rds.md)|Indique le type d’extraction asynchrone.|  
|[FilterColumn (RDS)](./filtercolumn-property-rds.md)|Indique la colonne sur laquelle les critères de filtre doivent être évalués.|  
|[FilterCriterion (RDS)](./filtercriterion-property-rds.md)|Indique l’opérateur d’évaluation à utiliser dans la valeur de filtre.|  
|[FilterValue (RDS)](./filtervalue-property-rds.md)|Indique la valeur pour filtrer les enregistrements.|  
|[Gestionnaire (RDS)](./handler-property-rds.md)|Indique le nom d’un programme de personnalisation côté serveur (*Gestionnaire*) qui étend les fonctionnalités de **RDSServer. DataFactory** et de tous les paramètres utilisés par le *Gestionnaire*.|  
|[InternetTimeout (RDS)](./internettimeout-property-rds.md)|Indique le nombre de millisecondes à attendre avant l’expiration d’une demande.|  
|[ReadyState (RDS)](./readystate-property-rds.md)|Indique la progression d’un objet **DataControl** lors de l’extraction de données dans son objet **Recordset** .|  
|[Recordset et SourceRecordset (RDS)](./recordset-sourcerecordset-properties-rds.md)|Indique l’objet **Recordset** renvoyé à partir d’un objet métier personnalisé.|  
|[Serveur (RDS)](./server-property-rds.md)|Indique le nom du Internet Information Services (IIS) et le protocole de communication.|  
|[SortColumn (RDS)](./sortcolumn-property-rds.md)|Indique la colonne dans laquelle trier les enregistrements.|  
|[SortDirection (RDS)](./sortdirection-property-rds.md)|Indique si un ordre de tri est croissant ou décroissant.|  
|[SQL (RDS)](./sql-property.md)|Indique la chaîne de requête utilisée pour récupérer le **jeu d’enregistrements**.|  
|[URL (RDS)](./url-property-rds.md)|Indique une chaîne qui contient une URL relative ou absolue.|