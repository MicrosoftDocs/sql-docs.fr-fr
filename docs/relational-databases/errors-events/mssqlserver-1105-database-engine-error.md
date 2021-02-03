---
description: MSSQLSERVER_1105
title: MSSQLSERVER_1105 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1105 (Database Engine error)
ms.assetid: e7f4ad02-8c7f-4bb9-9781-2c86253f2138
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 26b6adb58381b9dd9e02f378743405f602795bbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197198"
---
# <a name="mssqlserver_1105"></a>MSSQLSERVER_1105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|1105|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|NO_MORE_SPACE_IN_FG|  
|Texte du message|Impossible d’allouer de l’espace pour l’objet ’%.*ls’%.\*ls dans la base de données ’%.\*ls’, car le groupe de fichiers ’%.\*ls’ est plein. Créez de l'espace disque en supprimant des fichiers qui ne sont pas indispensables, en supprimant des objets dans le groupe de fichiers, en ajoutant des fichiers supplémentaires au groupe de fichiers ou en définissant Autogrowth à ON pour les fichiers existants dans le groupe de fichiers.|  
  
## <a name="explanation"></a>Explication  
Il n'y a pas d'espace disque disponible dans un groupe de fichiers.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Les actions ci-dessous peuvent éventuellement créer de l'espace disponible dans le groupe de fichiers :  
  
-   Activez la croissance automatique.  
  
-   Ajoutez d'autres fichiers au groupe de fichiers.  
  
-   Libérez de l'espace disque en supprimant des index ou des tables qui ne sont plus nécessaires.  
  
-   Pour plus d'informations, consultez la section « Résolution des problèmes d'espace disque insuffisant » dans la documentation en ligne de SQL Server.  
  
> [!NOTE]  
> Si un index se trouve sur plusieurs fichiers, **ALTER INDEX REORGANIZE** peut retourner une erreur 1105 quand l’un des fichiers est plein. Le processus de réorganisation est bloqué quand il tente de déplacer des lignes vers le fichier plein. Pour contourner cette limitation, effectuez une opération **ALTER INDEX REBUILD** au lieu de **ALTER INDEX REORGANIZE** ou augmentez la limite de croissance des fichiers qui sont pleins.  
  
