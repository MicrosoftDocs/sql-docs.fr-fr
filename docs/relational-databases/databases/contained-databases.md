---
title: Bases de données autonomes | Microsoft Docs
description: Découvrez les bases de données autonomes, les avantages et les limitations des bases de données partiellement autonomes, ainsi que la façon dont SQL Server 2019 isole les bases de données.
ms.custom: ''
ms.date: 08/24/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- contained database
- database_uncontained_usage event
- partially contained database
- contained database, understanding
ms.assetid: 36af59d7-ce96-4a02-8598-ffdd78cdc948
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: bbad843d482945a29cb03fde9545b1c19fb354f9
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98098391"
---
# <a name="contained-databases"></a>Bases de données autonomes
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Une *base de données autonome* est une base de données qui est isolée d’autres bases de données et de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui héberge cette base de données.  [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] aide l'utilisateur à isoler sa base de données de l'instance de 4 manières différentes.  
  
-   Une grande partie des métadonnées qui décrivent une base de données est conservée dans la base de données. (En plus ou en remplacement de la conservation des métadonnées dans la base de données MASTER.)  
  
-   Toutes les métadonnées sont définies à l'aide du même classement.  
  
-   L'authentification utilisateur peut être exécutée par la base de données, ce qui réduit la dépendance sur les connexions de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L'environnement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (DMV, XEvents, etc.) signale des informations relatives à la relation contenant-contenu et peut interagir sur celles-ci.  
  
 Certaines fonctionnalités de bases de données partiellement autonomes, telles que le stockage des métadonnées dans la base de données, s'appliquent à toutes les bases de données [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Certains avantages des bases de données partiellement autonomes, tels que l'authentification de niveau base de données et le classement de catalogue, doivent être activés pour pouvoir être disponibles. La relation contenant-contenu partielle est activée à l’aide des instructions **CREATE DATABASE** et **ALTER DATABASE** ou de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Pour plus d'informations sur l'activation de l’autonomie de base de données partielle, consultez [Migrer vers une base de données partiellement autonome](../../relational-databases/databases/migrate-to-a-partially-contained-database.md).  
  
##  <a name="partially-contained-database-concepts"></a><a name="Concepts"></a> Concepts relatifs aux bases de données partiellement autonomes  
 Une base de données autonome complète inclut tous les paramètres et métadonnées requis pour définir la base de données ; sa configuration ne dépend pas de l'instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] où la base de données est installée. Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la séparation d'une base de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pouvait être fastidieuse et nécessiter une connaissance détaillée de la relation entre la base de données et l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les bases de données partiellement autonomes simplifient la séparation d'une base de données de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des autres bases de données.  
  
 La base de données autonome considère les fonctionnalités en termes d’autonomie. Toute entité définie par l'utilisateur qui ne s'appuie que sur des fonctions résidant dans la base de données est considérée comme entièrement à relation contenant-contenu. Toute entité définie par l'utilisateur qui s'appuie sur des fonctions résidant hors de la base de données est considérée sans relation contenant-contenu. (Pour plus d’informations, consultez la section [Relation contenant-contenu](#containment) , plus loin dans cette rubrique.)  
  
 Les termes suivants s'appliquent au modèle de base de données autonome.  
  
 Limite de base de données  
 Limite entre une base de données et l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Limite entre une base de données et d'autres bases de données.  
  
 À relation contenant-contenu  
 Élément qui existe entièrement dans la limite de base de données.  
  
 Sans relation contenant-contenu  
 Élément qui dépasse la limite de base de données.  
  
 Base de données non autonome  
 Base de données dont la relation contenant-contenu a la valeur **NONE**. Toutes les bases de données de versions antérieures à [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] sont sans relation contenant-contenu. Par défaut, toutes les bases de données [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] et de versions ultérieures ont une relation contenant-contenu définie avec la valeur **NONE**.  
  
 Base de données partiellement autonome  
 Une base de données partiellement autonome est une base de données autonome qui peut autoriser certaines fonctionnalités qui dépassent la limite de base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet de déterminer si la limite de la relation contenant-contenu est dépassée.  
  
 Utilisateur contenu  
 Il existe deux types d'utilisateurs pour les bases de données autonomes.  
  
-   **Utilisateur de base de données autonome avec mot de passe**  
  
     Les utilisateurs de base de données autonome avec mots de passe sont authentifiés par la base de données. Pour plus d’informations, voir [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md).  
  
-   **Principaux Windows**  
  
     Les utilisateurs Windows autorisés et les membres de groupes Windows autorisés peuvent se connecter directement à la base de données et n'ont pas besoin de connexions dans la base de données **master** . La base de données fait confiance au processus d'authentification effectué par Windows.  
  
 Les utilisateurs, selon les connexions dans la base de données **master** , peuvent bénéficier de l'accès à une base de données autonome, mais cela créerait une dépendance sur l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ainsi, la création d’utilisateurs sur la base des connexions nécessite donc une autonomie partielle.
  
> [!IMPORTANT]  
>  L'activation de bases de données autonomes partielle transfère le contrôle de l'accès à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux propriétaires de la base de données. Pour plus d'informations, consultez [Meilleures pratiques de sécurité recommandées avec les bases de données autonomes](../../relational-databases/databases/security-best-practices-with-contained-databases.md).  
  
 Limite de base de données  
 Comme les bases de données partiellement autonomes séparent les fonctionnalités de base de données de celles de l'instance, une frontière clairement définie existe entre ces deux éléments, appelée *limite de base de données*.  
  
 À l'intérieur de la limite de base de données se trouve le *modèle de base de données*, où les bases de données sont développées et gérées. Les entités situées dans la base de données sont, par exemple, des tables système telles que **sys.tables**, des utilisateurs de base de données autonome avec mots de passe et des tables utilisateur dans la base de données actuelle référencées par un nom en deux parties.  
  
 En dehors de la limite de base de données se trouve le *modèle de gestion*, qui se rapporte aux fonctions et à la gestion au niveau de l’instance. Les entités situées en dehors de la limite de base de données sont, par exemple, des tables système telles que **sys.endpoints**, des utilisateurs mappés aux connexions et des tables utilisateur dans une autre base de données référencées par un nom en trois parties.  
  
##  <a name="containment"></a><a name="containment"></a> Relation contenant-contenu  
 Les entités d'utilisateur qui résident entièrement dans la base de données sont considérées *avec relation contenant-contenu*. Toutes les entités qui résident en dehors de la base de données, ou qui s'appuient sur une interaction avec des fonctions situées en dehors de la base de données, sont considérées *sans relation contenant-contenu*.  
  
 En général, les entités d'utilisateur sont classées dans les catégories de relation contenant-contenu suivantes :  
  
-   Entités d’utilisateur à relation contenant-contenu entière (celles qui ne dépassent jamais la limite de base de données), par exemple sys.indexes. Tout code qui utilise ces fonctionnalités ou tout objet qui référence uniquement ces entités est également entièrement à relation contenant-contenu.  
  
-   Entités d’utilisateur sans relation contenant-contenu (celles qui dépassent la limite de base de données), par exemple sys.server_principals ou un principal de serveur (connexion) lui-même. Tout code qui utilise ces entités ou toute fonction qui les référence est sans relation contenant-contenu.  
  
###  <a name="partially-contained-database"></a><a name="partial"></a> Base de données partiellement autonome  
 La fonctionnalité de base de données autonome est actuellement disponible uniquement dans un état partiel. Une base de données partiellement autonome est une base de données autonome qui autorise l'utilisation de fonctionnalités sans autonomie.  
  
 Utilisez les vues [sys.dm_db_uncontained_entities](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) et [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) pour retourner des informations sur des objets ou des fonctionnalités sans relation contenant-contenu. En déterminant l'état de la relation contenant-contenu des éléments de votre base de données, vous pouvez découvrir les objets ou les fonctionnalités qui doivent être remplacés ou modifiés pour promouvoir la relation contenant-contenu.  
  
> [!IMPORTANT]  
>  Étant donné que certains objets ont pour paramètre de relation contenant-contenu **NONE** par défaut, cette vue peut retourner des faux positifs.  
  
 Le comportement des bases de données partiellement autonomes est très différent de celui des bases de données non autonomes en termes de classement. Pour plus d'informations sur les problèmes de classement, consultez [Classements de base de données autonome](../../relational-databases/databases/contained-database-collations.md).  
  
##  <a name="benefits-of-using-partially-contained-databases"></a><a name="benefits"></a> Avantages de l'utilisation de bases de données partiellement autonomes  
 Les problèmes et les complications associés aux bases de données non autonomes peuvent être résolus à l'aide d'une base de données partiellement autonome.  
  
### <a name="database-movement"></a>Déplacement de base de données  
 Un des problèmes qui se produit lors du déplacement de bases de données est que certaines informations importantes peuvent être indisponibles lorsqu'une base de données est déplacée d'une instance à l'autre. Par exemple, les informations de connexion sont stockées dans l'instance, et non dans la base de données. Lorsque vous déplacez une base de données non autonome d'une instance vers une autre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ces informations sont abandonnées. Vous devez identifier les informations manquantes et les déplacer avec votre base de données vers la nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le processus peut nécessiter du temps et s'avérer difficile.  
  
 La base de données partiellement autonome peut stocker des informations importantes dans la base de données afin que celle-ci dispose toujours des informations après le déplacement.  
  
> [!NOTE]  
>  Une base de données partiellement autonome peut fournir de la documentation décrivant ces fonctionnalités utilisées par une base de données qui ne peut pas être séparée de l'instance. Cela inclut une liste d'autres bases de données reliées entre elles, des paramètres système que la base de données requiert mais sans relation contenant-contenu, et ainsi de suite.  
  
### <a name="benefit-of-contained-database-users-with-always-on"></a>Avantage des utilisateurs de base de données autonome avec Always On  
 En réduisant les liens à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les bases de données partiellement autonomes peuvent être utiles pendant le basculement lorsque vous utilisez [!INCLUDE[ssHADR](../../includes/sshadr-md.md)].  
  
 La création d'utilisateurs autonomes permet à l'utilisateur de se connecter directement à la base de données autonome. Il s’agit d’une fonctionnalité très importante dans les scénarios de haute disponibilité et de récupération d’urgence, notamment dans le cadre d’une solution Always On. Si les utilisateurs sont des utilisateurs à relation contenant-contenu, en cas de basculement, les personnes peuvent se connecter au serveur secondaire sans créer de connexions sur l'instance hébergeant le serveur secondaire. Ceci constitue un avantage immédiat. Pour plus d’informations, consultez [Vue d’ensemble des groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md) et [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On (SQL Server)](../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="initial-database-development"></a>Développement initial de la base de données  
 Étant donné qu'un développeur ne peut pas savoir où une nouvelle base de données sera déployée, diminuer les impacts environnementaux déployés sur la base de données réduit le travail et les difficultés du développeur. Dans le modèle sans relation contenant-contenu, le développeur doit prendre en considération les impacts environnementaux potentiels sur la nouvelle base de données et le nouveau programme. Toutefois, en utilisant des bases de données partiellement autonomes, les développeurs peuvent détecter les impacts au niveau de l'instance sur la base de données et les difficultés au niveau de l'instance pour le développeur.  
  
### <a name="database-administration"></a>Administration de bases de données  
 La gestion des paramètres de base de données dans la base de données, plutôt que dans la base de données master, permet à chaque propriétaire de la base de données d'avoir plus de contrôle sur sa base de données, sans donner au propriétaire de la base de données l'autorisation **sysadmin** .  
  
##  <a name="limitations"></a><a name="Limitations"></a> Limitations  
 Les bases de données partiellement autonomes n'autorisent pas les fonctionnalités suivantes.  
  
-   Réplication, capture des changements de données ou suivi des modifications.  
  
-   Procédures numérotées  
  
-   les objets liés par schéma qui dépendent de fonctions intégrées avec des modifications de classement ;  
  
-   la modification de liaison résultant des modifications de classement, notamment les références aux objets, les colonnes, les symboles ou les types.
  
> [!WARNING]  
>  Les procédures stockées temporaires sont actuellement autorisées. Étant donné que les procédures stockées temporaires violent l’autonomie, il ne faut pas s'attendre à leur prise en charge dans les futures versions d'une base de données autonome.  
  
##  <a name="identifying-database-containment"></a><a name="Identifying"></a> Identification de base de données à relation contenant-contenu  
 Il existe deux outils qui permettent d'identifier l'état de la relation contenant-contenu de la base de données. [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md) est une vue qui montre toutes les entités potentiellement sans relation contenant-contenu dans la base de données. L'événement database_uncontained_usage se produit lorsqu'une entité sans relation contenant-contenu réelle est identifiée au moment de l'exécution.  
  
### <a name="sysdm_db_uncontained_entities"></a>sys.dm_db_uncontained_entities  
 Cette vue affiche les entités de la base de données susceptibles d'être sans relation contenant-contenu, telles que celles qui dépassent la limite de base de données. Cela inclut les entités d'utilisateur qui peuvent utiliser des objets à l'extérieur du modèle de base de données. Cependant, comme la relation contenant-contenu de certaines entités (par exemple, celles utilisant SQL dynamique) ne peut être déterminée qu'au moment de l'exécution, la vue peut afficher des entités qui ne sont pas réellement sans relation contenant-contenu. Pour plus d’informations, consultez [sys.dm_db_uncontained_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
### <a name="database_uncontained_usage-event"></a>database_uncontained_usage, événement  
 Ce XEvent se produit chaque fois qu'une entité sans relation contenant-contenu est identifiée lors de l'exécution. Cela inclut des entités provenant du code client. Ce XEvent ne se produit que pour les entités sans relation contenant-contenu réelles. Toutefois, l'événement ne se produit qu'au moment de l'exécution. Par conséquent, toutes les entités d'utilisateur sans relation contenant-contenu que vous n'avez pas exécutées ne seront pas identifiées par ce XEvent  
  
## <a name="see-also"></a>Voir aussi  
 [Fonctionnalités modifiées &#40;base de données à relation contenant-contenu&#41;](../../relational-databases/databases/modified-features-contained-database.md)   
 [Classements de base de données autonome](../../relational-databases/databases/contained-database-collations.md)   
 [Meilleures pratiques de sécurité recommandées avec les bases de données autonomes](../../relational-databases/databases/security-best-practices-with-contained-databases.md)   
 [Migrer vers une base de données partiellement autonome](../../relational-databases/databases/migrate-to-a-partially-contained-database.md)   
 [Utilisateurs de base de données autonome - Rendre votre base de données portable](../../relational-databases/security/contained-database-users-making-your-database-portable.md)  
  
  
