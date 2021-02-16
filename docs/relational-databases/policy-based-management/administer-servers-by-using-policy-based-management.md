---
title: Administrer des serveurs à l’aide de la gestion basée sur des stratégies
description: Découvrez comment utiliser la gestion basée sur des stratégies pour manager une ou plusieurs instances de SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/12/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- facet See facets
- Declarative Management Framework See Policy-Based Management
- surface area configuration [SQL Server], Policy-Based Management
- Policy-Based Management
- facets [Policy-Based Management]
- Policy-Based Management, administering
- conditions [Policy-Based Management]
- facets [Policy-Based Management], about facets
- PolicyAdministratorRole role
ms.assetid: ef2a7b3b-614b-405d-a04a-2464a019df40
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 721f2d692d95f8bdccbdf351193fb63c36ad146b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345523"
---
# <a name="administer-servers-by-using-policy-based-management"></a>Administrer des serveurs à l'aide de la Gestion basée sur des stratégies
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   La Gestion basée sur des stratégies est un système de stratégies permettant de gérer une ou plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez-la pour créer des conditions contenant des expressions conditionnelles. Ensuite, créez des stratégies qui appliquent les conditions à des objets cibles de base de données.  

Par exemple, en tant qu’administrateur de la base de données, vous souhaiterez peut-être vous assurer que la messagerie de base de données n’est pas activée sur certains serveurs afin de créer une condition et une stratégie qui définit cette option de serveur. 
   
 > **IMPORTANT** Les stratégies peuvent affecter le fonctionnement des fonctionnalités. Par exemple, la capture des données modifiées et la réplication transactionnelle utilisent toutes deux la table systranschemas, qui n'a pas d'index. Si vous activez une stratégie stipulant que toutes les tables doivent avoir un index, la mise en conformité à cette stratégie entraînera l'échec de ces fonctionnalités.  
  
 Utilisez SQL Server Management Studio pour créer et gérer des stratégies pour effectuer les actions suivantes :
  
1.  Sélectionner une facette de la Gestion basée sur des stratégies qui contient les propriétés à configurer.  
  
2.  Définir une condition qui spécifie l'état d'une facette de gestion.  
  
3.  Définir une stratégie qui contient la condition, des conditions supplémentaires qui filtrent les jeux de cibles et le mode d'évaluation.  
  
4.  Vérifier si une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est conforme à la stratégie.  
  
 Pour les stratégies qui échouent, l'Explorateur d'objets indique un état critique sous forme d'icône rouge à côté de la cible et des nœuds situés plus hauts dans l'arborescence de l'Explorateur d'objets.  
  
> **REMARQUE :** lorsque le système calcule le jeu d’objets pour une stratégie, les objets système sont exclus par défaut.  Par exemple, si le jeu d'objets de la stratégie fait référence à toutes les tables, la stratégie ne s'applique pas aux tables système. Si les utilisateurs souhaitent évaluer une stratégie sur les objets système, ils peuvent les ajouter explicitement au jeu d'objets. Toutefois, bien que toutes les stratégies soient prises en charge pour le mode d'évaluation **vérifier la planification** , pour des raisons de performances, toutes les stratégies comportant des jeux d'objets arbitraires ne sont pas prises en charge pour le mode d'évaluation **vérifier la planification** . Pour plus d’informations, consultez [https://docs.microsoft.com/archive/blogs/sqlpbm/policy-evaluation-modes](/archive/blogs/sqlpbm/policy-evaluation-modes)  
  
## <a name="three-policy-based-management-components"></a>Trois composants de la Gestion basée sur des stratégies  
 La Gestion basée sur des stratégies a trois composants :  
  
-   Gestion de la stratégie. Les administrateurs de stratégie créent des stratégies.  
  
-   Administration explicite. Les administrateurs sélectionnent une ou plusieurs cibles gérées et vérifient de manière explicite que celles-ci sont conformes à une stratégie spécifique, ou rendent de manière explicite les cibles conformes à une stratégie.  
  
-   Modes d’évaluation. Il existe quatre modes d’évaluation, dont trois peuvent être automatisés :  
  
    -   **À la demande**. Ce mode évalue la stratégie lorsqu'elle est spécifiée directement par l'utilisateur.  
  
    -   **Sur modification : empêcher**. Ce mode automatisé utilise des déclencheurs DDL pour empêcher les violations de stratégie.  
  
        > **IMPORTANT !** Si l’option de configuration serveur relative aux déclencheurs imbriqués (nested triggers) est désactivée, le mode **Sur modification : empêcher** ne fonctionne pas correctement. La Gestion basée sur des stratégies repose sur des déclencheurs DDL pour détecter et restaurer les opérations DDL qui ne sont pas conformes aux stratégies qui utilisent ce mode d'évaluation. Si les déclencheurs DDL de la Gestion basée sur des stratégies est supprimée ou si les déclencheurs imbriqués sont désactivés, ce mode d'évaluation échouera ou se comportera de façon inattendue.  
  
    -   **Sur modification : Journal uniquement**. Ce mode automatisé utilise la notification d'événements pour évaluer une stratégie lorsqu'une modification pertinente est apportée.  
  
    -   **Selon planification**. Ce mode automatisé utilise un travail de l'agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour évaluer périodiquement une stratégie.  
  
     Lorsque les stratégies automatisées ne sont pas activées, la Gestion basée sur des stratégies n'affecte pas les performances système.  
  
## <a name="terms"></a>Termes  
 **Cible gérée de la Gestion basée sur des stratégies** Entités gérées par la Gestion basée sur des stratégies, telles qu’une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], une base de données, une table ou un index. Toutes les cibles dans une instance de serveur forment une hiérarchie cible. Un jeu de cibles est l'ensemble des cibles qui résulte de l'application d'un jeu de filtres cibles à la hiérarchie cible, par exemple toutes les tables de la base de données détenues par le schéma HumanResources.  
  
 **Facette de la Gestion basée sur des stratégies** Ensemble de propriétés logiques qui modèlent le comportement ou les caractéristiques de certains types de cibles gérées. Le nombre et les caractéristiques des propriétés sont intégrés à la facette et peuvent être ajoutés ou supprimés uniquement par le créateur de la facette. Un type de cible peut implémenter une ou plusieurs facettes de gestion et une facette de gestion peut être implémentée par un ou plusieurs types de cibles. Certaines propriétés d'une facette peuvent s'appliquer uniquement à une version spécifique.  
  
 **Condition de la Gestion basée sur des stratégies**  
 Expression booléenne qui spécifie un ensemble d'états autorisés pour une cible gérée par la Gestion basée sur des stratégies en ce qui concerne une facette de gestion. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tente d’observer les classements au moment de l’évaluation d’une condition. Si les classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne correspondent pas exactement aux classements Windows, testez votre condition afin de déterminer la façon dont l'algorithme résout les conflits.  
  
 **Stratégie de la Gestion basée sur des stratégies**  
 Condition de la Gestion basée sur des stratégies et comportement attendu, par exemple mode d'évaluation, filtres de cibles et planification. Une stratégie ne peut contenir qu'une seule condition. Les stratégies peuvent être activées ou désactivées. Les stratégies sont stockées dans la base de données msdb.  
  
 **Catégorie de la stratégie de la Gestion basée sur des stratégies**  
 Catégorie définie par l'utilisateur afin d'aider à gérer les stratégies. Les utilisateurs peuvent classifier les stratégies en différentes catégories de stratégies. Une stratégie appartient à une seule catégorie de stratégie. Les catégories de stratégies s'appliquent aux bases de données et aux serveurs. Au niveau de la base de données, les conditions suivantes s'appliquent :  
  
-   Les propriétaires de base de données peuvent abonner une base de données à un jeu de catégories de stratégies.  
  
-   Seules les stratégies de ses catégories abonnées peuvent gouverner une base de données.  
  
-   Toutes les bases de données sont abonnées implicitement à la catégorie de stratégie par défaut.  
  
 Au niveau du serveur, les catégories de stratégies peuvent être appliquées à toutes les bases de données.  
  
 **Stratégie actuelle**  
 Les stratégies actuelles d'une cible sont celles qui gouvernent cette cible. Une stratégie est actuelle en ce qui concerne une cible uniquement si toutes les conditions suivantes sont remplies :  
  
-   La stratégie est activée.  
  
-   La cible appartient au jeu de cibles de la stratégie.  
  
-   La cible ou l'un de ses ancêtres s'abonne au groupe de stratégies qui contient cette stratégie.  
  
## <a name="links-to-specific-tasks"></a>Liens vers des tâches spécifiques 

 - [Stocker des stratégies de la Gestion basée sur des stratégies.](policy-based-management-storage.md)
 - [Configurer des alertes pour informer les administrateurs de stratégie en cas d’échec de stratégie](../../relational-databases/policy-based-management/configure-alerts-to-notify-policy-administrators-of-policy-failures.md)
 - [Créer une condition de gestion basée sur des stratégies](../../relational-databases/policy-based-management/create-a-new-policy-based-management-condition.md)
 - [Supprimer une condition de gestion basée sur des stratégies](../../relational-databases/policy-based-management/delete-a-policy-based-management-condition.md)
 - [Afficher ou modifier les propriétés d’une condition de gestion basée sur des stratégies](../../relational-databases/policy-based-management/view-or-modify-the-properties-of-a-policy-based-management-condition.md)
 - [Exporter une stratégie de gestion basée sur des stratégies](../../relational-databases/policy-based-management/export-a-policy-based-management-policy.md)
 - [Importer une stratégie de gestion basée sur des stratégies](../../relational-databases/policy-based-management/import-a-policy-based-management-policy.md)
 - [Évaluer une stratégie de gestion basée sur des stratégies à partir d’un objet](../../relational-databases/policy-based-management/evaluate-a-policy-based-management-policy-from-an-object.md)
 - [Utiliser les facettes de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/working-with-policy-based-management-facets.md)
 - [Contrôler et appliquer les meilleures pratiques à l’aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)

## <a name="see-also"></a>Voir aussi  
 
 - [Tutoriel : Créer et appliquer une stratégie désactivée par défaut](lesson-1-create-and-apply-an-off-by-default-policy.md)
 - [Tutoriel : Créer et appliquer une stratégie de standards de nommage](lesson-2-create-and-apply-a-naming-standards-policy.md)
 - [Vues de la Gestion basée sur des stratégies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)