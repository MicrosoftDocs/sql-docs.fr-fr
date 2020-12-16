---
title: Vue d’ensemble de XEvents - SQL Server
description: L’architecture des événements étendus SQL Server vous permet de collecter les données nécessaires pour identifier et résoudre un problème de performances. Elle est configurable et scalable.
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: overview
helpviewer_keywords:
- extended events [SQL Server]
- xe
- XEvents
ms.assetid: bf3b98a6-51ed-4f2d-9c26-92f07f1fa947
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c837902edb7baa43d7fa07cfefc8ec335dc4183e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465530"
---
# <a name="extended-events-overview"></a>Vue d’ensemble des événements étendus

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] L’architecture des événements étendus permet aux utilisateurs de collecter autant de données que nécessaire pour dépanner ou identifier un problème de performances. Les événements étendus sont configurables et se mettent à l’échelle facilement.

Pour plus d’informations sur les événements étendus, consultez [Démarrage rapide : Événements étendus dans SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).

## <a name="benefits-of-ssnoversion-extended-events"></a>Avantages des événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Les événements étendus sont un système léger d’analyse des performances qui utilise très peu de ressources de performances. Les événements étendus fournissent deux interfaces utilisateur graphiques permettant de créer, de modifier, d’afficher et d’analyser vos données de session. Ces interfaces sont nommées comme suit :

- Assistant Nouvelle session
- Nouvelle session

## <a name="extended-events-concepts"></a>Concepts liés aux événements étendus  
 Les événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’appuient sur des concepts existants, tels qu’un événement ou un consommateur d’événements, utilisent les concepts provenant du suivi d’événements pour Windows et introduisent de nouveaux concepts.  
  
 Le tableau suivant décrit les concepts des événements étendus.  
  
|Rubrique|Description|  
|-----------|-----------------|  
|[Packages d’événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-packages.md)|Décrit les packages Événements étendus qui contiennent des objets. Ces objets sont utilisés pour obtenir et traiter les données lorsqu’une session d’événements étendus est en cours d’exécution.|  
|[Cibles des Événements étendus SQL Server](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130))|Décrit les consommateurs d'événements qui peuvent recevoir des données au cours d'une session d'événements.|  
|[Moteur des événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-engine.md)|Décrit le moteur qui implémente et gère une session Événements étendus.|  
|[Sessions d’événements étendus SQL Server](../../relational-databases/extended-events/sql-server-extended-events-sessions.md)|Décrit la session d'événements étendus.|  
| &nbsp; | &nbsp; |
  
## <a name="extended-events-architecture"></a>Architecture des événements étendus  

Les événements étendus représentent un système de gestion d’événements général pour les systèmes serveur. L'infrastructure des événements étendus prend en charge la corrélation de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et, sous certaines conditions, la corrélation de données provenant du système d'exploitation et d'applications de base de données. Dans le cas de données provenant du système d’exploitation, la sortie des Événements étendus doit être dirigée vers le Suivi d’événements pour Windows (ETW), afin de mettre en corrélation les données d’événement avec les données d’événement du système d’exploitation ou des applications.  

Toutes les applications ont des points d'exécution qui sont utiles aussi bien à l'intérieur qu'à l'extérieur d'une application. Au sein de l'application, le traitement asynchrone peut être mis en attente à l'aide d'informations collectées au cours de l'exécution initiale d'une tâche. En dehors de l’application, les points d’exécution fournissent des utilitaires d’analyse avec des informations sur les caractéristiques comportementales et de performances de l’application analysée.  

 Les événements étendus prennent en charge l'utilisation de données d'événement à l'extérieur d'un processus. Ces données sont utilisées en général par :  
  
-   des outils de suivi, tels que Trace SQL et le Moniteur système ;  
  
-   des outils de journalisation, tels que le journal des événements Windows ou le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ;  
  
-   des utilisateurs qui administrent un produit ou développent des applications sur un produit.  
  
 Les événements étendus présentent les aspects de conception clés suivants :  
  
-   Le moteur d'événements étendus est agnostique en termes d'événements, Cela permet au moteur de lier tout événement à toute cible, car le moteur n’est pas contraint par le contenu des événements. Pour plus d'informations sur le moteur d'événements étendus, consultez [SQL Server Extended Events Engine](../../relational-databases/extended-events/sql-server-extended-events-engine.md).  
  
-   Les événements sont séparés des consommateurs d'événements, appelés *cibles* dans les événements étendus. Cela signifie que toute cible peut recevoir tout événement. De plus, tout événement déclenché peut être automatiquement consommé par la cible, qui peut enregistrer dans le journal ou fournir un contexte d'événement supplémentaire. Pour plus d'informations, consultez [SQL Server Extended Events Targets](/previous-versions/sql/sql-server-2016/bb630339(v=sql.130)).  
  
-   Les événements sont distincts de l'action à entreprendre lorsqu'un événement se produit. Par conséquent, toute action peut être associée à tout événement.  
  
-   Les prédicats peuvent filtrer dynamiquement lorsque les données d'événement doivent être capturées. Le filtrage dynamique s’ajoute à la flexibilité de l’infrastructure Événements étendus. Pour plus d'informations, consultez [SQL Server Extended Events Packages](../../relational-databases/extended-events/sql-server-extended-events-packages.md).  
  
 Les événements étendus peuvent générer de façon synchrone des données d'événement (et traiter de façon asynchrone ces données), ce qui fournit une solution flexible de gestion des événements. De plus, les événements étendus fournissent les fonctionnalités suivantes :  
  
-   une approche unifiée de la gestion des événements sur le système serveur, tout en permettant aux utilisateurs d'isoler des événements spécifiques dans le but de résoudre des problèmes ;  
  
-   l'intégration avec les outils existants de suivi ETW et la prise en charge de ces derniers ;  
  
-   un mécanisme de gestion des événements entièrement configurable, basé sur [!INCLUDE[tsql](../../includes/tsql-md.md)];  
  
-   la capacité de surveiller dynamiquement les processus actifs tout en ayant un impact minime sur ces processus.  
  
-   une session de l'intégrité du système par défaut qui s'exécute sans effet de performance notable ; Elle recueille des données système qui peuvent vous aider à résoudre des problèmes de performances. Pour plus d’informations, consultez [Utiliser la session system_health](../../relational-databases/extended-events/use-the-system-health-session.md).  
  
## <a name="extended-events-tasks"></a>Tâches relatives aux événements étendus  

En utilisant [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)] pour exécuter des instructions DDL (Data Definition Language) [!INCLUDE[tsql](../../includes/tsql-md.md)] , des vues et des fonctions de gestion dynamiques ou des affichages catalogue, vous pouvez créer des solutions de dépannage des Événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] simples ou complexes pour votre environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Utilisez l' **Explorateur d'objets** pour gérer les sessions d'événements.|[Gérer les sessions d’événements dans l’Explorateur d’objets](../../relational-databases/extended-events/manage-event-sessions-in-the-object-explorer.md)|  
|Explique comment créer une session d'événements étendus.|[Créer une session d’événements étendus](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130))|  
|Explique comment afficher et actualiser des données cibles.| [Affichage avancé des données cibles d’événements étendus dans SQL Server](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md)|  
|Explique comment utiliser des outils d'événements étendus pour créer et gérer vos sessions d'événements étendus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Outils associés aux événements étendus](../../relational-databases/extended-events/extended-events-tools.md)|  
|Explique comment altérer une session d'événements étendus.|[Modifier une session d’événements étendus](../../relational-databases/extended-events/alter-an-extended-events-session.md)|  
|Explique comment obtenir des informations sur les champs associés aux événements.|[Obtenir les champs pour tous les événements](/previous-versions/sql/sql-server-2016/bb677249(v=sql.130))|  
|Explique comment déterminer quels sont les événements disponibles dans les packages enregistrés.|[Consulter les événements pour les packages enregistrés](./selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)|  
|Explique comment déterminer quelles cibles d'événements étendus sont disponibles dans les packages enregistrés.|[Afficher les cibles d’événements étendus pour les packages enregistrés](/previous-versions/sql/sql-server-2016/bb677247(v=sql.130))|  
|Explique comment afficher les événements Événements étendus et les actions qui sont équivalents à chaque événement SQL Trace et à ses colonnes associées.|[Consulter les événements étendus équivalents aux classes d’événements Trace SQL](../../relational-databases/extended-events/view-the-extended-events-equivalents-to-sql-trace-event-classes.md)|  
|Explique comment rechercher les paramètres que vous pouvez définir lorsque vous utilisez l'argument ADD TARGET dans CREATE EVENT SESSION ou ALTER EVENT SESSION.|[Obtenir les paramètres configurables pour l’argument ADD TARGET](/previous-versions/sql/sql-server-2016/bb677176(v=sql.130))|  
|Explique comment convertir un script Trace SQL existant en session d'événements étendus.|[Convertir un script Trace SQL existant en session d’événements étendus](../../relational-databases/extended-events/convert-an-existing-sql-trace-script-to-an-extended-events-session.md)|  
|Explique comment déterminer quelles requêtes détiennent le verrou, le plan de la requête et la pile [!INCLUDE[tsql](../../includes/tsql-md.md)] au moment où le verrou a été mis.|[Déterminer quelles requêtes détiennent des verrous](../../relational-databases/extended-events/determine-which-queries-are-holding-locks.md)|  
|Explique comment identifier la source des verrous qui gênent les performances de la base de données.|[Trouver les objets comportant le plus de verrous](../../relational-databases/extended-events/find-the-objects-that-have-the-most-locks-taken-on-them.md)|  
|Explique comment utiliser les événements étendus avec le suivi d'événements pour Windows pour surveiller l'activité système.|[Surveiller l’activité système à l’aide d’événements étendus](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)|  
|Utilisation des affichages catalogue et des vues de gestion dynamique pour les événements étendus | [Utilisation de SELECT et JOIN dans les vues système pour les événements étendus dans SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md) |
| &nbsp; | &nbsp; |

Utilisez la requête Transact-SQL (T-SQL) suivante pour lister tous les événements étendus possibles et leurs descriptions :

```sql
SELECT
     obj1.name as [XEvent-name],
     col2.name as [XEvent-column],
     obj1.description as [Descr-name],
     col2.description as [Descr-column]
  FROM
               sys.dm_xe_objects        as obj1
      JOIN sys.dm_xe_object_columns as col2 on col2.object_name = obj1.name
  ORDER BY
    obj1.name,
    col2.name
```


## <a name="code-examples-can-differ-for-azure-sql-database"></a>Les exemples de code peuvent être différents pour Azure SQL Database

[!INCLUDE[sql-on-premises-vs-azure-similar-sys-views-include.](../../includes/paragraph-content/sql-on-premises-vs-azure-similar-sys-views-include.md)]

## <a name="see-also"></a>Voir aussi

[Applications de la couche Données](../../relational-databases/data-tier-applications/data-tier-applications.md)  
[Prise en charge DAC pour les objets et versions SQL Server](/previous-versions/sql/sql-server-2012/ee210549(v=sql.110))  
[Déployer une application de la couche Données](../../relational-databases/data-tier-applications/deploy-a-data-tier-application.md)  
[Analyser les applications de la couche Données](../../relational-databases/data-tier-applications/monitor-data-tier-applications.md)  
&nbsp;  
[Vues de gestion dynamique des Événements étendus](../../relational-databases/system-dynamic-management-views/extended-events-dynamic-management-views.md)  
[Affichages catalogue des événements étendus (Transact-SQL)](../../relational-databases/system-catalog-views/extended-events-catalog-views-transact-sql.md)  
&nbsp;  
[XELite : bibliothèque multiplateforme pour lire des événements XEvent à partir de fichiers XEL ou de flux SQL dynamiques](https://www.nuget.org/packages/Microsoft.SqlServer.XEvent.XELite/), publiée en mai 2019.  
[Applet de commande PowerShell Read-SQLXEvent](https://www.powershellgallery.com/packages/SqlServer.XEvent), publiée en juin 2019.  
[Les mystères du SQL : Suivi de causalité et séquence d’événements pour les sessions XEvent (article de blog publié le 1er avril 2019)](https://bobsql.com/sql-mysteries-causality-tracking-vs-event-sequence-for-xevent-sessions/)