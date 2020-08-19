---
description: MSSQLSERVER_846
title: MSSQLSERVER_846 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 846 (Database Engine error)
ms.assetid: ccf367eb-06b0-42b8-b4d6-2b88f4a502d3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 950147fbfabfe41e7c643d586991c8d130f17ea2
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88420973"
---
# <a name="mssqlserver_846"></a>MSSQLSERVER_846
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|846|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|N/A|  
|Texte du message|Un dépassement de délai s’est produit lors de l’attente du verrou de tampon -- type %d, bp %p, page %d:%d, état %#x, ID de base de données : %d, ID d’unité d’allocation : %I64d%ls, tâche 0x%p : %d, temps d’attente %d, indicateurs 0x%I64x, tâche propriétaire 0x%p. Pas de poursuite de l'attente.|  
  
## <a name="explanation"></a>Explication  
Un ordinateur peut ne plus répondre, ou bien un dépassement de délai ou une autre perturbation peuvent se produire, au même moment où [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] écrit des erreurs de verrous de tampon dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Si le champ des statistiques du message a la valeur 0x04 activée, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] attend une opération d'E/S. Vous pouvez également recevoir le message [MSSQLSERVER_833](~/relational-databases/errors-events/mssqlserver-833-database-engine-error.md) dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Si le champ des statistiques du message a la valeur 0x04 désactivée, cela signifie qu'il existe une contention importante pour une page. Si l'objet est une page de données, une conception de code inefficace peut être à l'origine du problème. Si la page ne contient pas de données, l'erreur peut être causée par des goulots d'étranglement de serveurs tels qu'une insuffisance au niveau des ressources matérielles.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Pour contourner ce problème, une ou plusieurs des étapes suivantes (selon votre environnement) peuvent réduire ou éliminer les messages d'erreur :  
  
-   Déterminez la présence de goulots d'étranglement matériels. Si nécessaire, procédez à une mise à niveau de votre matériel afin que vous puissiez répondre aux exigences de configuration, requête et chargement de votre environnement. Pour plus d’informations sur les goulots d’étranglement, consultez [Identifier les goulots d’étranglement](~/relational-databases/performance/identify-bottlenecks.md).  
  
-   Vérifiez si vous avez des erreurs dans le journal et exécutez tous les diagnostics que votre fournisseur de matériel vous a procurés.  
  
-   Assurez-vous que vos lecteurs de disque ne sont pas compressés. Le stockage de données ou de fichiers journaux sur des disques compressés n'est pas pris en charge. Pour plus d’informations sur les fichiers physiques, consultez [Groupes de fichiers et fichiers de base de données](~/relational-databases/databases/database-files-and-filegroups.md).  
  
-   Vérifiez que les messages d'erreur disparaissent lorsque vous désactivez les options suivantes :  
  
    -   Option permettant de configurer le renforcement de la priorité SQL Server  
  
    -   Option « Regroupement léger » (mode fibre)  
  
    -   Option « set working set size »  
  
    > [!NOTE]  
    > Les anciens paramètres peuvent souvent s'avérer contre productifs si vous modifiez leur valeur par défaut, OFF. Pour plus d’informations sur les paramètres, consultez [Options de configuration de serveur &#40;SQL Server&#41;](~/database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
-   Ajustez les requêtes pour réduire les ressources utilisées sur le système. Le réglage des performances permettra de réduire l'effort système et d'améliorer le temps de réponse de chaque requête.  
  
-   Définissez l'option AUTO_SHRINK avec la valeur OFF pour réduire la surcharge qu'entraînent les modifications de taille des bases de données.  
  
-   Assurez-vous de définir l'option FILEGROWTH avec des incréments suffisamment espacés. Planifiez une tâche pour vérifier l'espace disponible dans les bases de données, puis augmentez la taille des bases de données pendant les heures creuses.  
  
