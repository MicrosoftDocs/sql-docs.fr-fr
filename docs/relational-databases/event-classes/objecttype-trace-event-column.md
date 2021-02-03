---
title: ObjectType, colonne d’événements de trace | Microsoft Docs
description: Reportez-vous aux valeurs possibles de la colonne d’événements de trace Object Type, qui est utilisée dans divers événements de trace dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- SQL Server event classes, Object Type column values
- events [SQL Server], Object Type column values
- event classes [SQL Server], Object Type column values
- Object Type column values [SQL Server]
ms.assetid: 42f85c50-34c9-49ca-955f-af9595e2707f
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 63b888ac84ff567be7b72eb09cd6271853ab69f9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183247"
---
# <a name="objecttype-trace-event-column"></a>Colonne d'événements de trace ObjectType
[!INCLUDE [SQL Server - ASDB](../../includes/applies-to-version/sql-asdb.md)]
  La colonne d'événements de trace Object Type est utilisée dans divers événements de trace. Cette rubrique décrit les valeurs admises dans cette colonne, ainsi que leur définition associée.  
  
## <a name="object-type-column-values"></a>Valeurs de la colonne Object Type  
  
|Valeur|Définition|  
|-----------|----------------|  
|8259|Contrainte CHECK|  
|8260|Valeur par défaut (contrainte ou autonome)|  
|8262|Contrainte FOREIGN KEY|  
|8272|Procédure stockée|  
|8274|Règle|  
|8275|Table système|  
|8276|Déclencheur sur le serveur|  
|8277|Table (définie par l'utilisateur)|  
|8278|Affichage|  
|8280|Procédure stockée étendue|  
|16724|Déclencheur CLR|  
|16964|Base de données|  
|16975|Object|  
|17222|Catalogue de texte intégral|  
|17232|Procédure stockée CLR|  
|17235|schéma|  
|17475|Informations d'identification|  
|17491|Événement DDL|  
|17741|Événement de gestion|  
|17747|Événement de sécurité|  
|17749|Événement d'utilisateur|  
|17985|Fonction Aggregate CLR|  
|17993|Fonction SQL table inline|  
|18000|Fonction de partition|  
|18002|Procédure de filtre de réplication|  
|18004|Fonction SQL table|  
|18259|Rôle serveur|  
|18263|[!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows|  
|19265|Clé asymétrique|  
|19277|Clé principale|  
|19280|Clé primaire|  
|19283|ObfusKey|  
|19521|Connexion à clé asymétrique|  
|19523|Connexion à certificat|  
|19538|Role|  
|19539|Connexion SQL|  
|19543|Connexion Windows|  
|20034|Liaisons de service distant|  
|20036|Notification d'événement de base de données|  
|20037|Notification d'événement|  
|20038|Fonction scalaire SQL|  
|20047|Notification d'événement d'objet|  
|20051|Synonyme|  
|20307|Séquence|  
|20549|Point de terminaison|  
|20801|Requêtes ad hoc pouvant être mises en cache|  
|20816|Requêtes préparées pouvant être mises en cache|  
|20819|File d'attente du service Service Broker|  
|20821|Contrainte unique|  
|21057|Rôle d'application|  
|21059|Certificat|  
|21075|Serveur|  
|21076|Déclencheur Transact-SQL|  
|21313|Assembly|  
|21318|Fonction scalaire CLR|  
|21321|Fonction scalaire SQL inline|  
|21328|Schéma de partition|  
|21333|Utilisateur|  
|21571|Contrat de service Service Broker|  
|21572|Déclencheur de base de données|  
|21574|Fonction table CLR|  
|21577|Table interne (par exemple, table des nœuds XML, table des files d'attente).|  
|21581|Type de message Service Broker|  
|21586|Itinéraire Service Broker|  
|21587|Statistiques|  
|21825<br /><br /> 21827<br /><br /> 21831<br /><br /> 21843<br /><br /> 21847|Utilisateur|  
|22099|Service Service Broker|  
|22601|Index|  
|22604|Connexion à certificat|  
|22611|XMLSchema|  
|22868|Type|  
  
## <a name="see-also"></a>Voir aussi  
 [sp_trace_setevent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-trace-setevent-transact-sql.md)  
  
  
