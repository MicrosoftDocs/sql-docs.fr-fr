---
title: Points de contrôle de base de données (SQL Server) | Microsoft Docs
description: Apprenez-en davantage sur les points de contrôle. Ces points de référence connus et fiables permettent au Moteur de base de données SQL Server de commencer à appliquer les modifications contenues dans le journal dans le cadre de la récupération.
ms.date: 04/23/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.custom: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- automatic checkpoints
- transaction logs [SQL Server], checkpoints
- logs [SQL Server], active
- pages [SQL Server], dirty
- MinLSN
- checkpoints [SQL Server]
- pages [SQL Server], flushing
- dirty pages
- transaction logs [SQL Server], active logs
- recovery interval option [SQL Server]
- buffer cache [SQL Server]
- logs [SQL Server], checkpoints
- Minimum Recovery LSN
- flushing pages
- active logs
ms.assetid: 98a80238-7409-4708-8a7d-5defd9957185
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 259e58dcf9ef713a9bb8787a63ac8cbf3a349c88
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237732"
---
# <a name="database-checkpoints-sql-server"></a>Points de contrôle de base de données (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
 Un *point de contrôle* permet la création d'un point de référence connu et fiable à partir duquel le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] peut, lors d'une récupération faisant suite à une panne ou à un arrêt imprévu, commencer à appliquer les modifications contenues dans le journal.

##  <a name="overview"></a><a name="Overview"></a> Vue d'ensemble   
Pour des raisons de performances, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] procède aux modifications des pages de base de données en mémoire (dans le cache des tampons) sans écrire ces pages sur le disque après chaque modification. En revanche, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] publie périodiquement un point de contrôle dans chaque base de données. Un *point de contrôle* écrit les pages modifiées en mémoire actuelles (appelées *pages de modifications*) et les informations du journal des transactions de la mémoire vers le disque, et il enregistre également les informations dans le journal des transactions.  
  
 Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] prend en charge plusieurs types de points de contrôle : automatique, indirect, manuel et interne. Le tableau suivant récapitule les types de **points de contrôle**.
  
|Nom|[!INCLUDE[tsql](../../includes/tsql-md.md)] .|Description|  
|----------|----------------------------------|-----------------|  
|Automatique|EXEC sp_configure **'** recovery interval **','** _seconds_ **'**|Émis automatiquement en arrière-plan pour respecter la limite de durée supérieure suggérée par l'option de configuration de serveur **recovery interval** . Les points de contrôle automatiques s'exécutent jusqu'à la fin.  Les points de contrôle automatiques sont accélérés en fonction du nombre d’écritures en attente et si le [!INCLUDE[ssDE](../../includes/ssde-md.md)] détecte une augmentation de latence d’écriture supérieure à 50 millisecondes.<br /><br /> Pour plus d'informations, consultez [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).|  
|Indirect|ALTER DATABASE ... SET TARGET_RECOVERY_TIME **=** _temps\_récupération\_cible_ { SECONDES &#124; MINUTES }|Émis en arrière-plan pour obtenir un temps de récupération cible spécifié par l'utilisateur pour une base de données. À partir de [!INCLUDE[ssSQL15_md](../../includes/sssql16-md.md)], la valeur par défaut est de 1 minute. La valeur par défaut est 0 pour les anciennes versions, ce qui indique que la base de données utilisera les points de contrôle automatiques, dont la fréquence dépend du paramètre d’intervalle de récupération de l’instance de serveur.<br /><br /> Pour plus d’informations, consultez [Modifier la durée de récupération cible d’une base de données &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).|  
|Manuel|CHECKPOINT [*durée_point_de_contrôle*]|Émis lorsque vous exécutez une commande CHECKPOINT [!INCLUDE[tsql](../../includes/tsql-md.md)] . Le point de contrôle manuel intervient dans la base de données active pour votre connexion. Par défaut, les points de contrôle manuels s'exécutent jusqu'à la fin. L'accélération fonctionne de la même manière que pour les points de contrôle automatiques.  Le paramètre *checkpoint_duration* peut aussi spécifier la durée demandée (en secondes) de l’exécution du point de contrôle.<br /><br /> Pour plus d'informations, consultez [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md).|  
|Interne|Aucun.|Émis par différentes opérations de serveur, telles que la création de sauvegarde et d'instantané de base de données pour garantir que les images de disque correspondent à l'état actuel du journal.|  
  
> [!NOTE]
> L’option d’installation avancée **-k** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permet à un administrateur de base de données de limiter le comportement d’E/S de point de contrôle en fonction du débit du sous-système d’E/S pour certains types de points de contrôle. L’option d’installation **-k** s’applique aux points de contrôle automatiques, ainsi qu’à tous les points de contrôle manuels et internes non accélérés.  
  
 Pour les points de contrôle automatiques, manuels et internes, seules les modifications apportées après le dernier point de contrôle doivent être restaurées par progression lors de la récupération de la base de données. Cela réduit le temps nécessaire pour récupérer une base de données.  
  
> [!IMPORTANT]
> Les transactions non validées longues augmentent le temps de récupération pour tous les types de points de contrôle.   
  
##  <a name="interaction-of-the-target_recovery_time-and-recovery-interval-options"></a><a name="InteractionBwnSettings"></a> Interaction des options TARGET_RECOVERY_TIME et « recovery interval »  
 Le tableau suivant résume l’interaction entre le paramètre **sp_configure '** recovery interval **'** à l’échelle du serveur et le paramètre `ALTER DATABASE ... TARGET_RECOVERY_TIME` spécifique à la base de données.  
  
|target_recovery_time|'recovery interval'|Type de point de contrôle utilisé|  
|----------------------------|-------------------------|-----------------------------|  
|0|0|Points de contrôle automatiques dont l'intervalle de récupération cible est de 1 minute.|  
|0|>0|Points de contrôle automatiques dont l’intervalle de récupération cible est spécifié par le paramètre défini par l’utilisateur de l’option **sp_configure 'recovery interval'** .|  
|>0|Non applicable.|Points de contrôle indirects dont le temps de récupération cible est déterminé par le paramètre TARGET_RECOVERY_TIME, exprimé en secondes.|  
  
##  <a name="automatic-checkpoints"></a><a name="AutomaticChkpt"></a> Points de contrôle automatiques  
Un point de contrôle automatique se produit chaque fois que le nombre d’enregistrements de journal atteint le nombre que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] estime pouvoir traiter pendant la durée spécifiée dans l’option de configuration de serveur **recovery interval** . Pour plus d'informations, consultez [Configure the recovery interval Server Configuration Option](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md).
 
Dans chaque base de données sans temps de récupération cible défini par l'utilisateur, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] génère des points de contrôle automatiques. La fréquence dépend de l’option de configuration de serveur avancée **recovery interval** , qui spécifie la durée maximale qu’une instance de serveur donnée doit utiliser pour récupérer une base de données lors d’un redémarrage du système. Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] estime le nombre maximal d'enregistrements de journal qu'il peut traiter dans l'intervalle de récupération. Lorsqu’une base de données qui utilise les points de contrôle automatiques atteint ce nombre maximal d’enregistrements du journal, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] émet un point de contrôle sur la base de données. 
 
L’intervalle de temps entre les points de contrôle automatiques peut varier **fortement** . Une base de données avec une charge de travail transactionnelle substantielle aura des points de contrôle plus fréquents qu’une base de données utilisée principalement pour des opérations en lecture seule. En mode de récupération simple, un point de contrôle automatique est également mis en file d’attente si le journal est rempli à 70 %.  
  
En mode de récupération simple, à moins qu'un facteur retarde la troncation du journal, un point de contrôle automatique tronque la section inutilisée du journal des transactions. À l’inverse, en mode de récupération utilisant les journaux de transactions, une fois qu’une chaîne de sauvegarde du journal a été établie, les points de contrôle automatiques ne provoquent pas la troncation du journal. Pour plus d'informations, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md).  
  
Après une panne système, le temps nécessaire pour récupérer une base de données dépend principalement de la quantité d’E/S aléatoire nécessaire aux pages de restauration par progression qui ont été modifiées au moment de l’incident. Cela signifie que le paramètre **recovery interval** n'est pas fiable. Il ne peut pas déterminer une durée précise de récupération. De plus, lorsqu'un point de contrôle automatique est en cours, l'activité d'E/S générale des données augmente considérablement et de manière imprévisible.  
   
###  <a name="impact-of-recovery-interval-on-recovery-performance"></a><a name="PerformanceImpact"></a> Impact de l’intervalle de récupération sur les performances de récupération  
Pour un système de traitement transactionnel en ligne (OLTP, online transaction processing), qui utilise de petites transactions, le paramètre **recovery interval** est le principal facteur déterminant le temps de récupération. Toutefois, l’option **recovery interval** n’affecte pas le temps nécessaire pour annuler une transaction longue. La récupération d’une base de données avec une transaction longue peut prendre beaucoup plus de temps que la valeur spécifiée dans le paramètre **recovery interval** . 
 
Par exemple, si une transaction d’exécution longue a mis deux heures pour effectuer des mises à jour avant la défaillance de l’instance de serveur, la récupération elle-même prendra beaucoup plus de temps pour restaurer la transaction longue que la valeur spécifiée pour l’option **recovery interval** . Pour plus d’informations sur l’impact d’une transaction longue sur la durée de récupération, consultez [Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md). Pour plus d’informations sur le processus de récupération, consultez [Vue d’ensemble de la restauration et de la récupération (SQL Server)](../../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#TlogAndRecovery).
  
En général, les valeurs par défaut fournissent les performances de récupération optimales. Toutefois, modifier l'intervalle de récupération peut améliorer les performances dans les circonstances suivantes :  
  
-   Si la récupération prend régulièrement beaucoup plus d'une minute lorsque les transactions longues ne sont pas restaurées.  
  
-   Si vous remarquez que les points de contrôle fréquents altèrent les performances sur une base de données.  
  
Si vous décidez d'augmenter le paramètre **recovery interval** , nous vous recommandons de l'augmenter progressivement par de petits incréments et d'évaluer l'effet de chaque augmentation incrémentielle sur les performances de récupération. Cette approche est importante, car à mesure que le paramètre **recovery interval** augmente, la récupération de la base de données prend plus de longtemps. Par exemple, si vous indiquez un **recovery interval** de 10 minutes, la récupération prend environ 10 fois plus de temps que si le paramètre **recovery interval** est défini sur 1 minute.  
  
##  <a name="indirect-checkpoints"></a><a name="IndirectChkpt"></a> Points de contrôle indirects
Les points de contrôle indirects, nouveauté de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], sont une alternative de base de données configurable aux points de contrôle automatiques. Ceci peut être configuré en spécifiant l’option de configuration de base de données **Temps de récupération cible**. Pour plus d’informations, consultez [Modifier la durée de récupération cible d’une base de données &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md).
En cas de panne système, les points de contrôle indirects fournissent un temps de récupération plus prédictible et plus rapide que les points de contrôle automatiques. Les points de contrôle indirects offrent les avantages suivants :  
  
-   Une charge de travail transactionnelle en ligne sur une base de données configurée pour les points de contrôle indirects peut rencontrer une dégradation des performances. Les points de contrôle indirects permettent de s’assurer que le nombre de pages de modifications est inférieur à un certain seuil afin que la récupération de la base de données se termine dans le temps de récupération cible. 

  L’option de configuration **recovery interval** utilise le nombre de transactions pour déterminer le temps de récupération, contrairement aux **points de contrôle indirects**, qui utilisent le nombre de pages de modifications. Quand des points de contrôle indirects sont activés sur une base de données recevant un grand nombre d’opérations DML, l’enregistreur en arrière-plan peut commencer à vider de manière intense les mémoires tampons modifiées sur le disque afin de s’assurer que le délai nécessaire à la récupération se situe dans le temps de récupération cible défini de la base de données. Cela peut entraîner une activité supplémentaire en termes d’E/S sur certains systèmes, ce qui peut contribuer à un goulot d’étranglement des performances si le sous-système du disque fonctionne au-delà du seuil d’E/S ou s’en rapproche.  
  
-   Les points de contrôle indirects vous permettent de contrôler le temps de récupération de base de données de manière fiable en factorisant le coût des E/S aléatoires lors des récupérations de type REDO. Cela permet à une instance de serveur de rester dans la limite supérieure de temps de récupération pour une base de données (sauf quand une transaction longue entraîne des temps excessifs de récupération de type UNDO).  
  
-   Les points de contrôle indirects réduisent les pics d'E/S associées au point de contrôle en entrant en continu des pages de modifications sur le disque en arrière-plan.  
  
Une charge de travail transactionnelle en ligne sur une base de données configurée pour les points de contrôle indirects peut rencontrer une dégradation des performances. Cela est dû au fait que l'enregistreur en arrière-plan utilisé par le point de contrôle indirect augmente parfois la charge d'écriture totale pour une instance de serveur.  
 
> [!IMPORTANT]
> Le point de contrôle indirect est le comportement par défaut pour les nouvelles bases de données créées dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], notamment les bases de données Model et TempDB.          
> Les bases de données qui ont été mises à niveau sur place ou restaurées à partir d’une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliseront le comportement de point de contrôle automatique précédent, sauf si elles sont modifiées explicitement pour utiliser le point de contrôle indirect.       

### <a name="improved-indirect-checkpoint-scalability"></a><a name="ctp23"></a> Scalabilité du point de contrôle indirect améliorée
Dans les versions antérieures à [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)], vous pouvez rencontrer des erreurs de planificateur improductives lorsqu’il existe une base de données qui génère un grand nombre de pages de modifications, comme `tempdb`. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduit une meilleure scalabilité pour le point de contrôle indirect, ce qui devrait permettre d’éviter ces erreurs sur les bases de données dont la charge de travail `UPDATE`/`INSERT` est importante.
  
##  <a name="internal-checkpoints"></a><a name="EventsCausingChkpt"></a> Points de contrôle internes  
Les points de contrôle internes sont générés par les divers composants serveur pour garantir que les images de disque correspondent à l'état actuel du journal. Les points de contrôle internes sont générés en réponse aux événements suivants :  
  
-   Des fichiers de base de données ont été ajoutés ou supprimés à l'aide de l'instruction ALTER DATABASE.  
  
-   Une sauvegarde de la base de données est effectuée.  
  
-   Une capture instantanée de base de données est créée, explicitement ou en interne pour DBCC CHECK.  
  
-   Une activité nécessitant l'arrêt de la base de données est effectuée. Par exemple, la valeur ON est attribuée à AUTO_CLOSE et la dernière connexion utilisateur à la base de données est fermée, ou une modification d'une option de base de données nécessitant un redémarrage de la base de données est effectuée.  
  
-   Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est arrêtée par l’arrêt du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER). Ces opérations provoquent la création d'un point de contrôle dans chaque base de données dans l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   La mise hors connexion d'une instance de cluster de basculement (FCI) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .      
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Related tasks  
 **Pour modifier l'intervalle de récupération sur une instance de serveur**  
  
-   [Configurer l’option de configuration du serveur recovery interval](../../database-engine/configure-windows/configure-the-recovery-interval-server-configuration-option.md)  
  
 **Pour configurer des points de contrôle indirects sur une base de données**  
  
-   [Modifier la durée de récupération cible d’une base de données &#40;SQL Server&#41;](../../relational-databases/logs/change-the-target-recovery-time-of-a-database-sql-server.md)  
  
 **Pour émettre un point de contrôle manuel sur une base de données**  
  
-   [CHECKPOINT &#40;Transact-SQL&#41;](../../t-sql/language-elements/checkpoint-transact-sql.md)  

## <a name="see-also"></a>Voir aussi  
[Journal des transactions &#40;SQL Server&#41;](../../relational-databases/logs/the-transaction-log-sql-server.md)            
[Guide d’architecture et gestion du journal des transactions SQL Server](../../relational-databases/sql-server-transaction-log-architecture-and-management-guide.md)      
 
