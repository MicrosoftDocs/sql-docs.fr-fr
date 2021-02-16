---
title: Configurer une stratégie de basculement automatique flexible pour un groupe de disponibilité
description: Décrit comment configurer une stratégie de basculement automatique flexible pour un groupe de disponibilité Always On à l’aide de Transact-SQL (T-SQL), PowerShell ou SQL Server Management Studio.
ms.date: 11/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- Availability Groups [SQL Server], flexible failover policy
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 1ed564b4-9835-4245-ae35-9ba67419a4ce
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.custom: seo-lt-2019
ms.openlocfilehash: 487b155561944e1303aac0fdb7453fc2d494802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342453"
---
# <a name="configure-a-flexible-automatic-failover-policy-for-an-always-on-availability-group"></a>Configurer une stratégie de basculement automatique flexible pour un groupe de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

  Cette rubrique explique comment configurer la stratégie de basculement flexible pour un groupe de disponibilité Always On à l’aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)] ou PowerShell dans SQL Server. Une stratégie de basculement flexible vous offre un contrôle granulaire sur les conditions qui entraînent le [basculement automatique](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) d’un groupe de disponibilité. En changeant les conditions d'échec qui déclenchent un basculement automatique et la fréquence des contrôles d'intégrité, vous pouvez augmenter ou diminuer la probabilité d'un basculement automatique pour assurer le contrat de niveau de service relatif à la haute disponibilité.  

   La stratégie de basculement flexible d'un groupe de disponibilité est définie par son niveau de condition et le seuil du délai d'attente de contrôle d'intégrité. Lorsque le dépassement du niveau de condition d'échec ou du seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité est détecté, la DLL de ressource du groupe de disponibilité répond au Clustering de basculement Windows Server (WSFC). Le cluster WSFC initialise un basculement automatique vers le réplica secondaire.  
 
  > [!NOTE]  
  > La stratégie de basculement flexible d'un groupe de disponibilité ne peut pas être configurée à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 
## <a name="limitations-on-automatic-failovers"></a><a name="Limitations"></a> Limitations relatives aux basculements automatiques  
  
-   Pour qu'un basculement automatique se produise, le réplica principal et un réplica secondaire doivent être configurés pour le mode de disponibilité avec validation synchrone et basculement automatique et le réplica secondaire doit être synchronisé avec le réplica principal.  
  
-   Seuls trois réplicas sont pris en charge dans le cadre du basculement automatique.  
  
-   Si un groupe de disponibilité dépasse le seuil d'échec WSFC, le cluster WSFC ne tente pas un basculement automatique du groupe de disponibilité. En outre, le groupe de ressources WSFC du groupe de disponibilité reste à l'état d'échec jusqu'à ce que l'administrateur de cluster mette manuellement le groupe de ressources en ligne ou jusqu'à ce que l'administrateur de base de données exécute un basculement manuel du groupe de disponibilité. Le *seuil d'échec WSFC* est le nombre maximal d'échecs autorisés pour le groupe de disponibilité au cours d'une période donnée. La période par défaut est de six heures, et la valeur par défaut du nombre maximal d’échecs au cours de cette période est *n*-1, où *n* est le nombre de nœuds de WSFC. Pour modifier les valeurs de seuil/d'échec pour un groupe de disponibilité donné, utilisez la console du gestionnaire de basculement WSFC.  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
  
-   Vous devez être connecté à l'instance de serveur qui héberge le réplica principal.  
   
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
  
|Tâche|Autorisations|  
|----------|-----------------|  
|Pour configurer la stratégie de basculement flexible pour un nouveau groupe de disponibilité|Requiert l’appartenance au rôle serveur fixe **sysadmin** et l’autorisation de serveur CREATE AVAILABILITY GROUP, l’autorisation ALTER ANY AVAILABILITY GROUP ou l’autorisation CONTROL SERVER.|  
|Pour modifier la stratégie d'un groupe de disponibilité existant|Requiert l'autorisation ALTER AVAILABILITY GROUP sur le groupe de disponibilité, l'autorisation CONTROL AVAILABILITY GROUP, l'autorisation ALTER ANY AVAILABILITY GROUP ou l'autorisation CONTROL SERVER.|  

##  <a name="health-check-timeout-threshold"></a><a name="HCtimeout"></a> Seuil du délai d'attente de contrôle d'intégrité  
 La DLL de ressource WSFC du groupe de disponibilité exécute un *contrôle d’intégrité* du réplica principal en appelant la procédure stockée [sp_server_diagnostics](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) sur l’instance de SQL Server qui héberge le réplica principal. **sp_server_diagnostics** retourne les résultats à un intervalle égal à 1/3 du seuil du délai d’attente de vérification d’intégrité pour le groupe de disponibilité. Le seuil par défaut du délai d’attente de vérification d’intégrité est de 30 secondes, ce qui signifie que **sp_server_diagnostics** répond à un intervalle de 10 secondes. Si la procédure **sp_server_diagnostics** est lente ou ne renvoie aucune information, la DLL de ressource attend la fin de l’intervalle du seuil du délai d’attente de vérification d’intégrité avant de déterminer que le réplica principal ne répond pas. Si le réplica principal ne répond pas, un basculement automatique est initialisé, si actuellement pris en charge.  
  
> [!IMPORTANT]  
>  **sp_server_diagnostics** n’exécute pas de vérifications d’intégrité au niveau de la base de données.  
  
##  <a name="failure-condition-level"></a><a name="FClevel"></a> Niveau de condition d'échec  
 Le fait que les données de diagnostic et les informations d’intégrité renvoyées par **sp_server_diagnostics** justifient ou non un basculement automatique dépend du niveau de condition d’échec du groupe de disponibilité. Le *niveau de condition d’échec* spécifie les conditions d’échec qui déclenchent un basculement automatique. Il existe cinq niveaux de condition d'échec, allant du moins restrictif (niveau 1) au plus restrictif (le niveau 5). Chaque niveau comprend les niveaux moins restrictifs. Par conséquent, le niveau de condition le plus strict, le niveau 5, inclut les quatre conditions moins restrictives (1 à 4), et ainsi de suite.  
  
> [!IMPORTANT]  
>  Les bases de données endommagées et suspectes ne sont détectées par aucun niveau de condition d'échec. Par conséquent, une base de données qui est endommagée ou suspecte (que ce soit en raison d'une défaillance matérielle, de l'altération des données ou de tout autre problème) ne déclenche jamais de basculement automatique.  
  
 Le tableau suivant décrit les conditions d'échec qui correspondent à chaque niveau.  
  
|Level|Condition d'échec|[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valeur|Valeur PowerShell|  
|-----------|-----------------------|------------------------------|----------------------|  
|Une|Le serveur est arrêté. Spécifie qu’un basculement automatique est initialisé lorsque l’une des situations suivantes se produit :<br /><br /> Le service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] est fermé.<br /><br /> Le bail du groupe de disponibilité pour la connexion au cluster WSFC expire car aucun accusé de réception n'est reçu de l'instance de serveur. Pour plus d’informations, consultez [Fonctionnement : délai d’expiration de bail Always On SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> <br /><br /> Il s'agit du niveau le moins restrictif.|1|**OnServerDown**|  
|Deux|Le serveur ne répond pas. Spécifie qu’un basculement automatique est initialisé lorsque l’une des situations suivantes se produit :<br /><br /> L'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne se connecte pas au cluster et le seuil du délai d'attente de contrôle d'intégrité spécifié par l'utilisateur pour le groupe de disponibilité est dépassé.<br /><br /> Le réplica de disponibilité est dans un état d'échec.|2|**OnServerUnresponsive**|  
|Trois|Erreur critique du serveur. Spécifie qu'un basculement automatique est initialisé en cas d'erreurs internes critiques [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telles que les verrouillages spinlock orphelins, les violations graves d'accès en écriture, ou en cas de trop de vidages.<br /><br /> C'est le niveau par défaut.|3|**OnCriticalServerError**|  
|Quatre|Erreur de serveur modérée. Spécifie qu'un basculement automatique est initialisé en cas d'erreurs internes modérées [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , telles qu'une condition persistante de mémoire insuffisante dans le pool de ressources interne [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|4|**OnModerateServerError**|  
|Cinq|Conditions d'échec qualifiées. Spécifie qu'un basculement automatique est initialisé pour toutes les conditions d'échec qualifiées, notamment :<br /><br /> Détection d’un interblocage de Scheduler.<br /><br /> Détection d'un blocage insoluble.<br /><br /> <br /><br /> Il s'agit du niveau le plus restrictif.|5|**OnAnyQualifiedFailureConditions**|  
  
> [!NOTE]  
>  L'absence de réponse par une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] aux demandes des clients n'est pas pertinente pour les groupes de disponibilité.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
 **Pour configurer la stratégie de basculement flexible**  
  
1.  Connectez-vous à l'instance de serveur qui héberge le réplica principal.  
  
2.  Pour un nouveau groupe de disponibilité, utilisez l’instruction [CREATE AVAILABILITY GROUP](../../../t-sql/statements/create-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] . Si vous modifiez un groupe de disponibilité existant, utilisez l’instruction [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md)[!INCLUDE[tsql](../../../includes/tsql-md.md)] .  
  
    -   Pour définir le niveau de condition de basculement, utilisez l’option FAILURE_CONDITION_LEVEL = *n* , où *n* est un entier compris entre 1 et 5.  
  
         Par exemple, l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante modifie le niveau de condition d'échec d'un groupe de disponibilité existant, `AG1`, sur le niveau un :  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (FAILURE_CONDITION_LEVEL = 1);  
        ```  
  
         La relation de ces valeurs entières avec les niveaux de condition d'échec est la suivante :  
  
        |[!INCLUDE[tsql](../../../includes/tsql-md.md)] Valeur|Level|Le basculement automatique démarre lorsque…|  
        |------------------------------|-----------|-------------------------------------------|  
        |1|Une|Le serveur est arrêté. Le service SQL Server s'arrête à cause d'un basculement ou d'un redémarrage.|  
        |2|Deux|Le serveur ne répond pas. Toutes les conditions qui correspondent à une valeur inférieure sont remplies, le service SQL Server est connecté au cluster et le seuil du délai d'attente de contrôle d'intégrité est dépassé, ou le réplica principal actuel est dans un état d'échec.|  
        |3|Trois|Erreur critique du serveur. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur interne et critique de serveur est survenue.<br /><br /> C'est le niveau par défaut.|  
        |4|Quatre|Erreur de serveur modérée. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur modérée de serveur s'est produite.|  
        |5|Cinq|Conditions d'échec qualifiées. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une condition d'échec qualifiée s'est produite.|  
  
         Pour plus d’informations sur les niveaux de condition de basculement, consultez [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;SQL Server&#41;]().  
  
    -   Pour configurer le seuil du délai d’attente de contrôle d’intégrité, utilisez l’optionHEALTH_CHECK_TIMEOUT = *n* , où *n* est un entier compris entre 15000 millisecondes (15 secondes) et 4294967295 millisecondes. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).  
  
         Par exemple, l'instruction [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivante modifie le seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité existant, `AG1`, sur 60 000 millisecondes (une minute).  
  
        ```  
  
        ALTER AVAILABILITY GROUP AG1 SET (HEALTH_CHECK_TIMEOUT = 60000);  
        ```  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Utilisation de PowerShell  
 **Pour configurer la stratégie de basculement flexible**  
  
1.  Définissez la valeur par défaut (**cd**) sur l’instance de serveur qui héberge le réplica principal.  
  
2.  Quand vous ajoutez un réplica de disponibilité à un groupe de disponibilité, utilisez l’applet de commande **New-SqlAvailabilityGroup** . Quand vous modifiez un réplica de disponibilité existant, utilisez l’applet de commande **Set-SqlAvailabilityGroup** .  
  
    -   Pour définir le niveau de condition de basculement, utilisez le paramètre **FailureConditionLevel**_niveau_ , où *niveau* a l’une des valeurs suivantes :  
  
        |Valeur|Level|Le basculement automatique démarre lorsque…|  
        |-----------|-----------|-------------------------------------------|  
        |**OnServerDown**|Une|Le serveur est arrêté. Le service SQL Server s'arrête à cause d'un basculement ou d'un redémarrage.|  
        |**OnServerUnresponsive**|Deux|Le serveur ne répond pas. Toutes les conditions qui correspondent à une valeur inférieure sont remplies, le service SQL Server est connecté au cluster et le seuil du délai d'attente de contrôle d'intégrité est dépassé, ou le réplica principal actuel est dans un état d'échec.|  
        |**OnCriticalServerError**|Trois|Erreur critique du serveur. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur interne et critique de serveur est survenue.<br /><br /> C'est le niveau par défaut.|  
        |**OnModerateServerError**|Quatre|Erreur de serveur modérée. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une erreur modérée de serveur s'est produite.|  
        |**OnAnyQualifiedFailureConditions**|Cinq|Conditions d'échec qualifiées. Toutes les conditions qui correspondent à une valeur inférieure sont remplies ou une condition d'échec qualifiée s'est produite.|  
  
         Pour plus d’informations sur les niveaux de condition de basculement, consultez [Stratégie flexible pour le basculement automatique d’un groupe de disponibilité &#40;SQL Server&#41;]().  
  
         Par exemple, la commande suivante modifie le niveau de condition d'échec d'un groupe de disponibilité existant, `AG1`, sur le niveau un.  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `   
        -FailureConditionLevel OnServerDown  
        ```  
  
    -   Pour définir le seuil du délai d’attente de vérification d’intégrité, utilisez le paramètre **HealthCheckTimeout**_n_ , où *n* est un entier compris entre 15000 millisecondes (15 secondes) et 4294967295 millisecondes. La valeur par défaut est 30 000 millisecondes (ou 30 secondes).  
  
         Par exemple, la commande suivante modifie le seuil du délai d'attente de contrôle d'intégrité d'un groupe de disponibilité existant, `AG1`, sur 120 000 millisecondes (deux minutes).  
  
        ```  
        Set-SqlAvailabilityGroup `   
        -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAG `   
        -HealthCheckTimeout 120000  
        ```  
  
> [!NOTE]  
>  Pour voir la syntaxe d’une applet de commande, utilisez l’applet de commande **Get-Help** dans l’environnement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Pour en savoir plus, voir [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Pour configurer et utiliser le fournisseur SQL Server PowerShell**  
  
-   [Fournisseur SQL Server PowerShell](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  

##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer un basculement automatique**  
  
-   [Modifier le mode de disponibilité d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md) (le basculement automatique nécessite le mode de disponibilité avec validation synchrone)  
  
-   [Modifier le mode de basculement d’un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurer la stratégie de basculement flexible pour contrôler les conditions du basculement automatique &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenu associé  
  
-   [Fonctionnement : délai d’expiration de bail Always On SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268)  
  
## <a name="see-also"></a>Voir aussi  
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modes de disponibilité &#40;groupes de disponibilité Always On&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Basculement et modes de basculement &#40;groupes de disponibilité AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Clustering de basculement Windows Server &#40;WSFC&#41; avec SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Stratégie de basculement pour les instances de cluster de basculement](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [sp_server_diagnostics &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md)  
  
