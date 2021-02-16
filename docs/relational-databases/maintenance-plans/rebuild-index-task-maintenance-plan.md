---
title: Tâche Reconstruire l’index (Plan de maintenance) | Microsoft Docs
description: Découvrez comment recréer les index dans les tables de la base de données SQL Server en définissant un nouveau facteur de remplissage à l’aide de la tâche Reconstruire l’index.
ms.custom: ''
ms.date: 07/21/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
f1_keywords:
- reindex
- sql13.swb.maint.reindex.f1
helpviewer_keywords:
- Rebuild Index Task dialog box
ms.assetid: 33e2940b-139f-4563-b0cb-5683f08bd879
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 6c95f60259fb25b457e4349b6e0798ce4fa0e6fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351179"
---
# <a name="rebuild-index-task-maintenance-plan"></a>Tâche Reconstruire l'index (Plan de maintenance)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Utilisez la boîte de dialogue **Tâche Reconstruire l’index** pour recréer les index des tables de la base de données en définissant un nouveau facteur de remplissage. Le facteur de remplissage détermine la quantité d'espace disponible sur chaque page de l'index pour permettre l'extension ultérieure de l'index. Au fur et à mesure que des données sont ajoutées à la table, l'espace libre se remplit parce que le facteur de remplissage n'est pas conservé. Réorganiser les pages d'index et les données permet de recouvrer de l'espace libre.  
  
 La **Tâche Reconstruire l'index** utilise l'instruction ALTER INDEX. Pour plus d’informations sur les options décrites dans cette page, consultez [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md).  
  
## <a name="options"></a>Options  
 **Connection**  
 Sélectionnez la connexion serveur à utiliser pour exécuter la tâche.  
  
 **Nouveau**  
 Crée une nouvelle connexion serveur à utiliser pour exécuter la tâche. La boîte de dialogue **Nouvelle connexion** est décrite ci-dessous.  
  
 **Bases de données**  
 Spécifie les bases de données faisant l'objet de cette tâche.  
  
-   **Toutes les bases de données**  
  
     Génère un plan de maintenance qui exécute des tâches de maintenance sur toutes les bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à l'exception de tempdb.  
  
-   **Toutes les bases de données système**  
  
     Génère un plan de maintenance qui exécute des tâches de maintenance sur chaque base de données système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , à l'exception de tempdb. Aucune tâche de maintenance n'est exécutée sur les bases de données créées par l'utilisateur.  
  
-   **Toutes les bases de données utilisateur**  
  
     Génère un plan de maintenance qui exécute des tâches de maintenance sur toutes les bases de données créées par l'utilisateur. Aucune tâche de maintenance n'est exécutée sur les bases de données système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Ces bases de données**  
  
     Génère un plan de maintenance qui n'exécute les tâches de maintenance que sur les bases de données sélectionnées. Si vous choisissez cette option, sélectionnez au moins une base de données.  
  
    > [!NOTE]  
    >  Les plans de maintenance sont exécutés uniquement sur des bases de données définies au niveau de compatibilité 80 ou plus. Les bases de données définies au niveau de compatibilité 70 ou moins ne sont pas affichées.  
  
 **Object**  
 Limite la grille de **Sélection** à l’affichage des tables et/ou des vues.  
  
 **Sélection**  
 Spécifie les tables ou les index faisant l'objet de cette tâche. Non disponible quand **Tables et vues** est sélectionné dans la zone Objet.  
  
 **Espace disponible par page par défaut**  
 Permet de supprimer les index sur les tables de la base de données et de les recréer avec le facteur de remplissage spécifié lors de la création des index.  
  
 **Modifier l’espace disponible par page de**  
 Provoque la suppression des index des tables de la base de données et leur recréation avec un nouveau facteur de remplissage calculé automatiquement, la quantité d'espace libre spécifiée étant réservée dans les pages d'index. Plus le pourcentage est élevé, plus il y a d'espace libre réservé dans les pages d'index et plus l'index croît. Les valeurs valides sont comprises entre 0 et 100.  
  
 **Trier les résultats dans tempdb**  
 L’option `SORT_IN_TEMPDB` détermine l’emplacement de stockage temporaire des résultats intermédiaires du tri, générés pendant la création d’un index. Si aucune opération de tri n’est requise ou si le tri peut être effectué dans la mémoire, l’option `SORT_IN_TEMPDB`est ignorée.  
  
 **index de remplissage**  
 Spécifie le remplissage d’index.  
  
 **Conserver l’index en ligne**  
 Utilisez l'option `ONLINE` qui permet aux utilisateurs d'accéder à la table sous-jacente ou aux données d'index cluster, ainsi qu'à tous les index non-cluster associés au cours des opérations d'index.  
  
> [!NOTE]
>  Les opérations d’index en ligne ne sont pas disponibles dans toutes les éditions de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour obtenir la liste des fonctionnalités prises en charge par les éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prise en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
 **Ne pas reconstruire les index | Reconstruire des index en mode hors connexion**  
 Indiquez de quelle manière le système doit traiter les types d’index qui ne peuvent pas être recréés lorsqu’ils sont en ligne.  
  
 **MAXDOP**  
 Affectez une valeur pour limiter le nombre de processeurs utilisés dans une exécution de plans parallèles.  
  
 **Faible priorité utilisée**  
 Sélectionnez cette option pour attendre les verrouillages de faible priorité.  
  
 **Abandonner après délai d’attente**  
 Indiquez de quelle manière le système doit se comporter une fois que le temps défini par le paramètre **Durée maximale** est écoulé.  
  
 **Durée maximale**  
 Précisez le délai d’attente des verrouillages de faible priorité.  
  
 **Vue T-SQL**  
 Affiche les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutées sur le serveur pour cette tâche, selon les options sélectionnées.  
  
> [!NOTE]  
>  Si le nombre d'objets impliqués est élevé, l'affichage des instructions peut prendre un temps considérable.  


[!INCLUDE[index-stats-options-reorg-5589131-2999104](../../includes/paragraph-content/index-stats-options-reorganize-maintenance-plan-include.md)]

  
## <a name="new-connection-dialog-box"></a>Boîte de dialogue Nouvelle connexion  
 **Nom de connexion**  
 Entrez un nom pour la nouvelle connexion.  
  
 **Sélectionnez ou entrez un nom de serveur.**  
 Sélectionnez un serveur auquel établir la connexion pour exécuter la tâche.  
  
 **Actualiser**  
 Actualise la liste des serveurs disponibles.  
  
 **Entrez des informations pour vous connecter au serveur**  
 Spécifiez le mode d'authentification sur le serveur.  
  
 **Utiliser la sécurité intégrée à Windows NT**  
 Se connecte à une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] à l'aide de l'authentification Windows.  
  
 **Utiliser un nom d'utilisateur et un mot de passe spécifiques**  
 Se connecte à une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] à l'aide de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option n'est pas disponible.  
  
 **Nom d'utilisateur**  
 Fournit le nom d'utilisateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
 **Mot de passe**  
 Fournit un mot de passe à utiliser pour l'authentification. Cette option n'est pas disponible.  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)   
 [DBCC DBREINDEX &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-dbreindex-transact-sql.md)   
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)   
 [Option SORT_IN_TEMPDB pour les index](../../relational-databases/indexes/sort-in-tempdb-option-for-indexes.md)   
 [Instructions pour les opérations d'index en ligne](../../relational-databases/indexes/guidelines-for-online-index-operations.md)   
 [Fonctionnement des opérations d'index en ligne](../../relational-databases/indexes/how-online-index-operations-work.md)   
 [Exécuter des opérations en ligne sur les index](../../relational-databases/indexes/perform-index-operations-online.md)  
  
  
