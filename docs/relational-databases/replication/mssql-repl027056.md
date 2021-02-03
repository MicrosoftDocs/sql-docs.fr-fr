---
description: MSSQL_REPL027056
title: MSSQL_REPL027056 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_REPL027056 error
ms.assetid: 92d62f3c-b8ae-482e-a348-2e9a8ee9786e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 82c2afc30c295b390ba76ceed4c24c95b6b0d666
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206159"
---
# <a name="mssql_repl027056"></a>MSSQL_REPL027056
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Détails du message  
  
|Attribut|Valeur|  
|-|-|  
|Nom du produit|SQL Server|  
|ID de l’événement|27056|  
|Source de l’événement|MSSQLSERVER|  
|Composant|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nom symbolique||  
|Texte du message|Le processus de fusion n'a pas pu modifier l'historique de génération sur le '%1'. Lors de la résolution du problème, redémarrez la synchronisation avec un enregistrement d'historique détaillé et spécifiez un fichier de sortie dans lequel écrire.|  
  
## <a name="explanation"></a>Explication  
 Cette erreur résulte généralement d'une contention dans les tables système de réplication de fusion devenues trop volumineuses. Une longue période de rétention de publication est souvent à l'origine d'une croissance excessive des tables système car les métadonnées doivent être stockées dans ces tables jusqu'à la fin de la période de rétention.  
  
## <a name="user-action"></a>Action de l'utilisateur  
 **Résolution du problème :**  
  
1.  Diminuez la valeur des paramètres **-DownloadGenerationsPerBatch** et **- UploadGenerationsPerBatch** pour que l’Agent de fusion autorise la poursuite du traitement pendant que vous résolvez le problème à l’origine de l’erreur. Les paramètres des agents peuvent être spécifiés dans des profils d'agent et sur la ligne de commande. Pour plus d'informations, consultez les pages suivantes :  
  
    -   [Utiliser des profils d’agent de réplication](../../relational-databases/replication/agents/work-with-replication-agent-profiles.md)  
  
    -   [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Replication Agent Executables Concepts](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md).  
  
2.  Spécifiez une période de rétention de publication la plus courte possible. Pour plus d’informations, voir [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
3.  Dans le cadre de la gestion d'une réplication de fusion, contrôlez de temps en temps le développement des tables système associées à cette réplication : **MSmerge_contents**, **MSmerge_genhistory**, **MSmerge_tombstone**, **MSmerge_current_partition_mappings** et **MSmerge_past_partition_mappings**. Réindexez périodiquement ces tables. Pour plus d’informations, consultez [Réorganiser et reconstruire des index](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Guide de référence des erreurs et des événements &#40;réplication&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
