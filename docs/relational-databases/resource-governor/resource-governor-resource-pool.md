---
title: Pool de ressources du gouverneur de ressources
description: SQL Server Resource Governor spécifie des limites sur la quantité de processeur, les E/S physiques et la mémoire que les demandes d’application entrantes peuvent utiliser avec le pool de ressources.
ms.custom: ''
ms.date: 10/20/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Resource Governor, resource pool
- resource pool [SQL Server], overview
- resource pool [SQL Server]
ms.assetid: 306b6278-e54f-42e6-b746-95a9315e0cbe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4028f19dc9d7ac319b083b5e6077021da59e7a9b
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172311"
---
# <a name="resource-governor-resource-pool"></a>Pool de ressources du gouverneur de ressources
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Dans le gouverneur de ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , un pool de ressources représente un sous-ensemble des ressources physiques d'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Le gouverneur de ressources vous permet de spécifier des limites sur la quantité d'UC, les E/S physiques et la mémoire que les demandes d'application entrantes peuvent utiliser avec le pool de ressources. Chaque pool de ressources peut contenir un ou plusieurs groupes de charges de travail. Lorsqu'une session est démarrée, le classifieur du gouverneur de ressources affecte la session à un groupe de charges de travail spécifique, et la session doit s'exécuter à l'aide des ressources attribuées au groupe de charges de travail.  
  
## <a name="resource-pool-concepts"></a>Concepts relatifs au pool de ressources  
 Un pool de ressources, ou pool, représente les ressources physiques du serveur. Vous pouvez envisager un pool comme une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] virtuelle dans une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Un pool a deux parties. Une partie ne se chevauche pas avec les autres pools, ce qui permet de conserver un minimum de ressources. L'autre partie est partagée avec d'autres pools et prend en charge la consommation maximale de ressources possible. Les ressources de pool sont définies en spécifiant un ou plusieurs des paramètres suivants pour chaque ressource (UC, E/S physiques et mémoire) :  
  
-   **MIN_CPU_PERCENT et MAX_CPU_PERCENT**  
  
     Ces paramètres correspondent aux valeurs minimale et maximale garanties de la bande passante moyenne du processeur pour toutes les demandes dans le pool de ressources en cas de contention du processeur. Vous pouvez utiliser ces paramètres afin de déterminer l'utilisation prédictible des ressources du processeur pour plusieurs charges de travail, en fonction des besoins de chaque charge de travail. Supposons, par exemple, que les services des ventes et marketing d'une entreprise partagent la même base de données. Le service des ventes présente une charge de travail gourmande en ressources du processeur, avec des requêtes de priorité élevée. Le service marketing présente également une charge de travail gourmande en ressources du processeur, mais avec des requêtes de priorité plus faible. En créant un pool de ressources distinct pour chaque service, vous pouvez affecter un pourcentage *minimal* d'utilisation de l'UC de 70 pour le pool de ressources des ventes et un pourcentage *maximal* d'utilisation de l'UC de 30 pour le pool de ressources marketing. Cela garantit que la charge de travail Ventes reçoit les ressources du processeur dont elle a besoin et que la charge de travail Marketing est isolée des demandes d'UC de la charge de travail Ventes. Notez que le pourcentage maximal d'utilisation de l'UC est un maximum opportuniste. S'il existe une capacité de processeur disponible, la charge de travail l'utilise jusqu'à 100 %. La valeur maximale s'applique uniquement en cas de contention des ressources du processeur. Dans cet exemple, si la charge de travail Ventes est désactivée, la charge de travail Marketing peut utiliser 100 % de l'UC, si nécessaire.  
  
-   **CAP_CPU_PERCENT**  
  
     Le paramètre CAP_CPU_PERCENT définit une limite maximale inconditionnelle d'utilisation de la bande passante de l'UC pour toutes les demandes exécutées dans le pool de ressources. Les charges de travail associées au pool peuvent utiliser la capacité de l'UC au-delà de la valeur de MAX_CPU_PERCENT si elle est disponible, mais pas au-delà de la valeur de CAP_CPU_PERCENT. En reprenant l'exemple ci-dessus, supposons que le service marketing soit facturé en fonction de son utilisation des ressources. Il souhaite une facturation prédictible et ne veut pas payer pour plus de 30 % de l'UC. Pour garantir cette exigence, il suffit de définir CAP_CPU_PERCENT à 30 pour le pool de ressources marketing.  
  
-   **MIN_MEMORY_PERCENT et MAX_MEMORY_PERCENT**  
  
     Ces paramètres spécifient la quantité de mémoire minimale et maximale réservée au pool de ressources qui ne peut pas être partagée avec d'autres pools de ressources. Pour des base de données sans tables à mémoire optimisée, la mémoire dont il est question ici correspond à la mémoire allouée à l'exécution des requêtes, et non à la mémoire du pool de mémoires tampons (des pages d'index et de données, par exemple). Définir une valeur minimale de mémoire pour un pool signifie que vous vous assurez que le pourcentage de mémoire spécifié est disponible pour toutes les demandes qui peuvent s'exécuter dans ce pool de ressources. Il s'agit d'un différentiateur important comparé à MIN_CPU_PERCENT, car dans ce cas, la mémoire peut rester dans le pool de ressources en question même lorsque celui-ci ne reçoit aucune demande dans les groupes de charges de travail associés. Par conséquent, il est essentiel de faire preuve de la plus grande vigilance lors de l'utilisation de ce paramètre, car cette mémoire ne pourra pas être utilisée par un autre pool, même en l'absence de demandes actives. Définir une valeur maximale de la mémoire pour un pool signifie que lorsque des requêtes s'exécutent dans ce pool, elles n'obtiennent jamais plus que ce pourcentage de mémoire globale.

     Afin de régir la mémoire pour les tables à mémoire optimisée avec Resource Governor, vous devez lier la base de données à un pool de ressources distinct. Pour plus d’informations, consultez [Lier une base de données avec des tables à mémoire optimisée à un pool de ressources](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md).
  
-   **AFFINITY**  
  
     Ce paramètre permet de définir l'affinité d'un pool de ressources en fonction d'un ou de plusieurs planificateurs ou nœuds NUMA pour un niveau d'isolation supérieur des ressources du processeur. Dans le scénario précédemment décrit, nous supposons que le service des ventes a besoin d'un environnement mieux isolé et souhaite disposer en permanence de 100 % d'un cœur d'UC. L'option AFFINITY permet de planifier les charges de travail Ventes et Marketing sur différentes UC. En supposant que la limite CAP_CPU_PERCENT est encore appliquée sur le pool marketing, la charge de travail Marketing continue à utiliser jusqu'à 30 % d'un cœur, tandis que la charge de travail Ventes utilise 100 % de l'autre cœur. Les charges de travail Ventes et Marketing s'exécutent sur deux ordinateurs isolés.  
  
-   **MIN_IOPS_PER_VOLUME et MAX_IOPS_PER_VOLUME**  
  
     Ces paramètres correspondent aux valeurs minimale et maximale d'opérations d'E/S physiques par seconde (IOPS) par volume disque pour un pool de ressources. Vous pouvez utiliser ces paramètres pour contrôler les entrées/sorties physiques pour les threads utilisateur d'un pool de ressources donné. Par exemple, le service des ventes génère plusieurs rapports de fin de mois par lots de grande taille. Les requêtes de ces lots peuvent générer des E/S susceptibles de saturer le volume disque et avoir un impact sur les performances d'autres charges de travail de priorité plus élevée dans la base de données. Pour isoler cette charge de travail, MIN_IOPS_PER_VOLUME est défini sur la valeur 20, tandis que MAX_IOPS_PER_VOLUME est défini sur la valeur 100 pour le pool de ressources du service des ventes, qui contrôle le niveau d'entrées/sorties qui est émis pour la charge de travail.  
  
Lors de la configuration de l'UC ou de la mémoire, la somme des valeurs MIN pour l'ensemble des pools ne peut pas dépasser 100 pour cent des ressources de serveur. En outre, les valeurs MAX et CAP définies doivent être comprises entre MIN et 100 % inclus.  
  
Si un pool est défini avec une valeur MIN différente de zéro, la valeur MAX effective des autres pools est réajustée. La valeur minimale de la valeur MAX d'un pool et de la somme des valeurs MIN des autres pools est soustraite de 100 %.  
  
Le tableau suivant illustre quelques-uns des concepts précédents. Le tableau présente les paramètres définis pour le pool interne, le pool par défaut et deux pools définis par l'utilisateur. 
  
|Nom du pool|Paramètre % MIN|Paramètre % MAX|% MAX effectif calculé|% partagé calculé|Commentaire|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interne|0|100|100|0|Les valeurs % MAX effectif et % partagé ne sont pas applicables au pool interne.|  
|default|0|100|30|30|La valeur MAX effectif est calculée comme suit : min(100,100-(20+50)) = 30. Le % partagé est calculé comme suit : MAX effectif - MIN = 30.|  
|Pool 1|20|100|50|30|La valeur MAX effectif est calculée comme suit : min(100,100-50) = 50. Le % partagé est calculé comme suit : MAX effectif - MIN = 30.|  
|Pool 2|50|70|70|20|La valeur MAX effectif est calculée comme suit : min(70,100-20) = 70. Le % partagé est calculé comme suit : MAX effectif - MIN = 20.|  

Les formules suivantes calculent le pourcentage MAX (% MAX) effectif et le pourcentage (%) partagé dans la table ci-dessus :  
  
-   Min(X,Y) représente la valeur plus petite de X et Y.  
  
-   Sum(X) représente la somme de la valeur X pour tous les pools.  
  
-   Total % partagé = 100 - sum(MIN %).  
  
-   % MAX effectif = min(X,Y).  
  
-   % partagé = % MAX effectif - % MIN.  

Sur la base du tableau précédent donné en exemple, détaillons les ajustements qui sont effectués lors de la création d’un pool supplémentaire. Ce pool (Pool 3) a un paramètre % MIN égal à 5.  
  
|Nom du pool|Paramètre % MIN|Paramètre % MAX|% MAX effectif calculé|% partagé calculé|Commentaire|  
|---------------|-------------------|-------------------|--------------------------------|-------------------------|-------------|  
|interne|0|100|100|0|Les valeurs % MAX effectif et % partagé ne sont pas applicables au pool interne.|  
|default|0|100|25|25|La valeur MAX effectif est calculée comme suit : min(100,100-(20+50+5)) = 25. Le % partagé est calculé comme suit : MAX effectif - MIN = 25.|  
|Pool 1|20|100|45|25|La valeur MAX effectif est calculée comme suit : min(100,100-55) = 45. Le % partagé est calculé comme suit : MAX effectif - MIN = 25.|  
|Pool 2|50|70|70|20|La valeur MAX effectif est calculée comme suit : min(70,100-25) = 70. Le % partagé est calculé comme suit : MAX effectif - MIN = 20.|  
|Pool 3|5|100|30|25|La valeur MAX effectif est calculée comme suit : min(100,100-70) = 30. Le % partagé est calculé comme suit : MAX effectif - MIN = 25.|  
  
 La partie partagée du pool est utilisée pour indiquer la destination des ressources disponibles si des ressources sont disponibles. Toutefois, lorsque les ressources sont consommées, elles sont acheminées vers le pool spécifié et ne sont pas partagées. Cette procédure peut améliorer l'utilisation des ressources en l'absence de demandes dans un pool donné et elle permet de libérer les ressources configurées du pool pour d'autres pools.  
  
 Dans certains cas extrêmes, les pools peuvent être configurés comme ci-dessous :  
  
-   Tous les pools sont définis avec des valeurs minimales qui représentent au total 100 pour cent des ressources de serveur. Dans ce cas, les valeurs maximales effectives sont égales aux valeurs minimales. Cela revient à diviser les ressources de serveur en portions qui ne se chevauchent pas, indépendamment des ressources consommées dans chaque pool.  
  
-   Tous les pools ont des valeurs minimales nulles. Tous les pools sont en concurrence pour les ressources disponibles et leurs tailles finales sont basées sur la consommation des ressources dans chaque pool. D'autres facteurs tels que les stratégies jouent un rôle dans la définition de la taille finale du pool.  
  
Le gouverneur de ressources prédéfinit deux pools de ressources, le pool interne et le pool par défaut. Vous pouvez ajouter des pools supplémentaires.  
  
**Pool interne**  
  
Le pool interne représente les ressources consommées par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ce pool contient toujours uniquement le groupe interne, et il est impossible de le modifier. La consommation des ressources par le pool interne n'est pas restreinte. Toutes les charges de travail dans le pool sont considérées comme critiques pour la fonction de serveur, et le gouverneur de ressources permet au pool interne de contraindre d'autres pools même si cela implique le non-respect des limites définies pour les autres pools.  
  
> [!NOTE]  
>  L'utilisation des ressources de pool interne et de groupe interne n'est pas soustraite de l'utilisation des ressources totales. Les pourcentages sont calculés à partir des ressources totales disponibles.  
  
**Pool par défaut**  
  
Le pool par défaut est le premier pool d'utilisateur prédéfini. Avant toute configuration, le pool par défaut contient seulement le groupe par défaut. Le pool par défaut ne peut pas être créé ni supprimé, mais il peut être modifié. Il peut contenir des groupes définis par l'utilisateur en plus du groupe par défaut. À partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] , il existe un pool de ressources par défaut pour les opérations [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] courantes et un pool de ressources externes par défaut pour les processus externes, tels que l’exécution de scripts R.  
  
> [!NOTE]  
>  Le groupe par défaut est modifiable, mais il ne peut pas être déplacé hors du pool par défaut.  
  
**Pool externe**  
  
Les utilisateurs peuvent définir un pool externe pour définir des ressources pour les processus externes. Pour R Services, il gouverne plus particulièrement `rterm.exe`, `BxlServer.exe` et les autres processus engendrés par ces derniers.  
  
**Pools de ressources définis par l'utilisateur**  
  
Les pools de ressources définis par l'utilisateur sont ceux que vous créez pour les charges de travail spécifiques dans votre environnement. Le gouverneur de ressources fournit des instructions DDL pour créer, modifier et supprimer des pools de ressources.  
  
## <a name="resource-pool-tasks"></a>Tâches du pool de ressources  
  
|Description de la tâche|Article|  
|----------------------|-----------|  
|Décrit comment créer un pool de ressources.|[Créer un pool de ressources](../../relational-databases/resource-governor/create-a-resource-pool.md)|  
|Décrit comment modifier les paramètres du pool de ressources.|[Modifier les paramètres de pool de ressources](../../relational-databases/resource-governor/change-resource-pool-settings.md)|  
|Décrit comment supprimer un pool de ressources.|[Supprimer un pool de ressources](../../relational-databases/resource-governor/delete-a-resource-pool.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Resource Governor](../../relational-databases/resource-governor/resource-governor.md)   
 [Groupe de charge de travail de Resource Governor](../../relational-databases/resource-governor/resource-governor-workload-group.md)   
 [Fonction classifieur de Resource Governor](../../relational-databases/resource-governor/resource-governor-classifier-function.md)   
 [Configurer le gouverneur de ressources à l'aide d'un modèle](../../relational-databases/resource-governor/configure-resource-governor-using-a-template.md)   
 [Afficher les propriétés du gouverneur de ressources](../../relational-databases/resource-governor/view-resource-governor-properties.md)  
  
  
