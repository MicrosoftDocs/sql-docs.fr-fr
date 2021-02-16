---
title: Mise en miroir de bases de données et réplication (SQL Server) | Microsoft Docs
description: Découvrez comment utiliser la mise en miroir des bases de données parallèlement à la réplication afin d’améliorer la disponibilité de la base de données de publication dans SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- replication [SQL Server], database mirroring and
ms.assetid: 82796217-02e2-4bc5-9ab5-218bae11a2d6
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 16746834acfe8a53511166a35667f136b561c28a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353185"
---
# <a name="database-mirroring-and-replication-sql-server"></a>Mise en miroir de bases de données et réplication (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  La mise en miroir des bases de données peut avoir lieu en parallèle à la réplication afin d'améliorer la disponibilité de la base de données de publication. La mise en miroir des bases de données consiste à avoir deux exemplaires d'une même base de données qui résident généralement sur des ordinateurs différents. À un moment donné précis, les clients ne peuvent accéder qu'à un seul exemplaire de la base de données. Cet exemplaire s'appelle la base de données principale. Les mises à jour apportées par les clients sur la base de données principale sont appliquées à l'autre exemplaire de la base de données, appelé base de données miroir. La mise en miroir consiste à répercuter dans la base de données miroir chaque insertion, mise à jour ou suppression apportée à la base de données principale.  
  
 Le basculement de réplication vers un miroir est entièrement pris en charge pour les bases de données de publication, avec la prise en charge limitée des bases de données d'abonnement. La mise en miroir de bases de données n'est pas prise en charge pour la base de données de distribution. Pour plus d’informations sur la façon de récupérer une base de données de distribution ou d’abonnement sans avoir à reconfigurer la réplication, consultez [Sauvegarder et restaurer des bases de données répliquées](../../relational-databases/replication/administration/back-up-and-restore-replicated-databases.md).   
  
> [!NOTE]  
>  Après un basculement, le miroir devient la base de données principale. Dans cette rubrique, les termes « principale » et « miroir » renvoient toujours aux bases de données principale et miroir d'origine.  
  
## <a name="requirements-and-considerations-for-using-replication-with-database-mirroring"></a>Conditions requises et éléments à prendre en compte pour procéder à une réplication avec mise en miroir des bases de données  
 Tenez compte des exigences et des points suivants lors d'une réplication avec mise en miroir des bases de données :  
  
-   Les bases de données principale et miroir doivent partager un serveur de distribution. Nous vous conseillons de choisir un serveur de distribution à distance qui offre une plus grande tolérance de panne en cas de basculement inopiné du serveur de publication.  
  
-   La réplication prend en charge la mise en miroir des bases de données de publication pour les réplications de fusion et les réplications transactionnelles mettant en jeu des Abonnés en lecture seule ou des Abonnés avec mise à jour en file d'attente. Les Abonnés avec mise à jour immédiate, les serveurs de publication Oracle et les serveurs de publication d'une topologie d'égal à égal ne sont pas pris en charge.  
  
-   Les métadonnées et les objets qui existent en dehors de la base de données, c'est-à-dire connexions, travaux, serveurs liés, etc. ne sont pas copiés vers le miroir. Si vous voulez faire figurer les métadonnées et les objets dans le miroir, vous devez les copier manuellement. Pour plus d’informations, consultez [Gestion des connexions et des travaux après un basculement de rôle &#40;SQL Server&#41;](../../sql-server/failover-clusters/management-of-logins-and-jobs-after-role-switching-sql-server.md).  
  
## <a name="configuring-replication-with-database-mirroring"></a>Configuration de la réplication avec mise en miroir des bases de données  
 La configuration de la réplication et de la mise en miroir des bases de données se fait en cinq étapes. Chaque étape est décrite plus en détail dans les sections qui suivent.  
  
1.  Configurez le serveur de publication.  
  
2.  Configurez la mise en miroir des bases de données.  
  
3.  Configurez la base de données miroir de façon à ce qu'elle utilise le même serveur de distribution que la base de données principale.  
  
4.  Configurez les agents de réplication pour le basculement.  
  
5.  Ajoutez les bases de données principale et miroir dans le Moniteur de réplication.  
  
 Les étapes 1 et 2 peuvent être inversées.  
  
#### <a name="to-configure-database-mirroring-for-a-publication-database"></a>Pour configurer la mise en miroir d'une base de données de publication  
  
1.  Configurez le serveur de publication :  
  
    1.  Nous vous conseillons d'utiliser un serveur de distribution distant. Pour plus d’informations sur la configuration de la distribution, consultez [Configuration de la distribution](../../relational-databases/replication/configure-distribution.md).  
  
    2.  Vous pouvez activer une base de données en vue de publications d'instantanés, de publications transactionnelles et/ou de publications de fusion. Pour les bases de données mises en miroir contenant plusieurs types de publications, vous devez activer la base de données pour les différents types au niveau du même nœud à l’aide de [sp_replicationdboption](../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md). Vous pourriez, par exemple, appeler la procédure stockée suivante sur le principal :  
  
        ```  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='publish', @value=true;  
        exec sp_replicationdboption @dbname='<PublicationDatabase>', @optname='mergepublish', @value=true;  
        ```  
  
         Pour plus d’informations sur la création de publications, consultez [Publier des données et des objets de base de données](../../relational-databases/replication/publish/publish-data-and-database-objects.md).  
  
2.  Configurez la mise en miroir des bases de données. Pour plus d’informations, consultez [Établir une session de mise en miroir de bases de données au moyen de l’authentification Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md) et [Configuration de la mise en miroir d’une base de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/setting-up-database-mirroring-sql-server.md).  
  
3.  Configurez la distribution pour le miroir. Spécifiez le nom du miroir comme serveur de publication, puis spécifiez le serveur de publication et le dossier d'instantanés que le principal utilise. Par exemple, si vous configurez la réplication par le biais de procédures stockées, exécutez [sp_adddistpublisher](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md) sur le serveur de distribution, puis [sp_adddistributor](../../relational-databases/system-stored-procedures/sp-adddistributor-transact-sql.md) sur le miroir. Pour **sp_adddistpublisher**:  
  
    -   Donnez au paramètre **\@publisher** le nom de réseau du miroir.  
  
    -   Donnez au paramètre **\@working_directory** le nom du dossier d’instantanés utilisé par le principal.  
  
4.  Spécifiez le nom du miroir pour le paramètre d’agent **-PublisherFailoverPartner**. Agent Ce paramètre est obligatoire ; il permet aux agents suivants d'identifier le miroir après le basculement :  
  
    -   Agent d'instantané (pour toutes les publications)  
  
    -   Agent de lecture du journal (pour toutes les publications transactionnelles)  
  
    -   Agent de lecture de la file d'attente (pour les publications transactionnelles prenant en charge les abonnements avec mise à jour en file d'attente)  
  
    -   Agent de fusion (pour les abonnements de fusion)  
  
    -   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Écouteur de réplication (replisapi.dll : pour les abonnements de fusion synchronisés via la synchronisation web)  
  
    -   Contrôle ActiveX SQL Merge (pour les abonnements de fusion synchronisés à l'aide du contrôle)  
  
     L'agent de distribution et le contrôle ActiveX SQL Distribution n'ont pas ce paramètre puisqu'ils ne se connectent pas au serveur de publication.  
  
     Les modifications apportées aux paramètres prennent effet au prochain démarrage de l'Agent. Si l'Agent fonctionne en continu, vous devez l'arrêter, puis le redémarrer. Les paramètres peuvent être définis dans les profils d'agent et à l'invite de commandes. Pour plus d'informations, consultez les pages suivantes :  
  
    -   [Afficher et modifier des paramètres d’invite de commandes d’un Agent de réplication &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/view-and-modify-replication-agent-command-prompt-parameters.md)  
  
    -   [Concepts des exécutables de l'agent de réplication](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  
     Nous vous conseillons d’ajouter le paramètre **-PublisherFailoverPartner** à un profil d’agent, puis de spécifier le nom du miroir dans le profil. Par exemple, si vous configurez la réplication à l'aide de procédures stockées :  
  
    ```  
    -- Execute sp_help_agent_profile in the context of the distribution database to get the list of profiles.  
    -- Select the profile id of the profile that needs to be updated from the result set.  
    -- In the agent_type column returned by sp_help_agent_profile:   
    -- 1 = Snapshot Agent; 2 = Log Reader Agent; 3 = Distribution Agent; 4 = Merge Agent; 9 = Queue Reader Agent.  
  
    exec sp_help_agent_profile;  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Snapshot Agent profile (profile 1).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 1, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
  
    -- Setting the -PublisherFailoverPartner parameter in the default Merge Agent profile (profile 6).  
    -- Execute sp_add_agent_parameter in the context of the distribution database.  
    exec sp_add_agent_parameter @profile_id = 6, @parameter_name = N'-PublisherFailoverPartner', @parameter_value = N'<Failover Partner Name>';  
    ```  
  
5.  Ajoutez les bases de données principale et miroir dans le Moniteur de réplication. Pour plus d’informations, consultez [Ajouter et supprimer des serveurs de publication à partir du moniteur de réplication](../../relational-databases/replication/monitor/add-and-remove-publishers-from-replication-monitor.md).  
  
## <a name="maintaining-a-mirrored-publication-database"></a>Gestion d'une base de données de publication mise en miroir  
 La gestion d'une base de données de publication reste quasiment identique qu'elle soit mise en miroir ou non, mais quelques précautions sont toutefois à prendre :  
  
-   L'administration et la surveillance doivent se faire au niveau du serveur actif. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], les publications apparaissent sous le dossier **Publications locales** uniquement pour le serveur actif. En cas de basculement sur le miroir, par exemple, les publications s'affichent sur le miroir et plus sur le principal. Si la base de données bascule sur le miroir, vous aurez à actualiser manuellement [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et le moniteur de réplication pour répercuter les modifications.  
  
-   Le moniteur de réplication affiche les nœuds de serveur de publication dans l'arborescence des objets de la base de données principale et de la base de données miroir. Si la base de données principale est le serveur actif, les informations de publication sont affichées uniquement sous le nœud de base de données principale dans le moniteur de réplication.  
  
     Si la base de données miroir est le serveur actif :  
  
    -   Si un agent possède une erreur, celle-ci est indiquée sur le nœud de base de données principale, pas sur le nœud de base de données miroir.  
  
    -   Si la base de données principale n'est pas disponible, les nœuds de base de données principale et miroir affichent des listes identiques de publications. La surveillance doit être réalisée sur les publications sous le nœud de base de données miroir.  
  
-   Si vous faites appel aux procédures stockées ou aux Replication Management Objects pour gérer la réplication sur le miroir, vous devez spécifier comme serveur de publication le nom de l'instance où la base de données est activée pour la réplication. Pour déterminer le nom approprié, utilisez la fonction [publishingservername](../../t-sql/functions/replication-functions-publishingservername.md).  
  
     En cas de mise en miroir d'une base de données de publication, les métadonnées de réplication stockées dans la base de données miroir sont identiques à celles de la base de données principale. Par conséquent, en cas de bases de données de publication activées pour la réplication sur le principal, le nom d'instance du serveur de publication stocké dans les tables système du miroir est celui du principal, et non du miroir. Cela affecte la configuration et l'administration de la réplication si la base de données de publication bascule sur le miroir. Supposons que vous configuriez la réplication à l’aide de procédures stockées sur le miroir, une fois le basculement réalisé, et que vous vouliez ajouter un abonnement par extraction de données à une base de données de publication activée au niveau du principal, vous devez alors utiliser le nom principal et non le nom miroir dans le paramètre **\@publisher** de **sp_addpullsubscription** ou **sp_addmergepullsubscription**.  
  
     Si vous activez une base de données de publication au niveau du miroir, une fois le basculement vers le miroir réalisé, le nom d’instance du serveur de publication stocké dans les tables système est le nom du miroir. Dans ce cas, vous devez utiliser le nom du miroir dans le paramètre **\@publisher**.  
  
    > [!NOTE]  
    >  Dans certains cas, comme **sp_addpublication**, le paramètre **\@publisher** n’est pris en charge que pour les serveurs de publication non-[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Il ne sert alors à rien en cas de mise en miroir des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Pour synchroniser un abonnement dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] après un basculement : synchronisez les abonnements par extraction à partir de l'Abonné, puis synchronisez les abonnements par émission de données à partir du serveur de publication actif.  
  
### <a name="replication-behavior-if-mirroring-is-removed"></a>Comportement de la réplication en cas de suppression de la mise en miroir  
 Gardez les points suivants à l'esprit si la mise en miroir des bases de données est retirée d'une base de données publiée :  
  
-   Si la base de données de publication du principal n'est plus mise en miroir, la réplication se poursuit comme si de rien n'était vis-à-vis du principal d'origine.  
  
-   Si la base de données de publication bascule du principal vers le miroir et que la relation de mise en miroir est par la suite désactivée ou supprimée, les agents de réplication ne pourront pas travailler avec le miroir. Si la base de données principale est définitivement perdue, désactivez, puis reconfigurez la réplication avec le miroir choisi comme serveur de publication.  
  
-   Si la mise en miroir des bases de données est totalement supprimée, la base de données miroir passe en état de récupération et doit être restaurée pour devenir fonctionnelle. Le comportement de la base de données récupérée à l'égard de la réplication varie selon que l'option KEEP_REPLICATION est ou non activée. Cette option oblige l'opération de restauration à conserver les paramètres de réplication lors de la restauration d'une base de données publiée sur un serveur autre que celui sur lequel elle a été créée. N'utilisez l'option KEEP_REPLICATION que si l'autre base de données de publication n'est pas disponible. L'option n'est pas prise en charge si l'autre base de données de publication est intacte et en cours de réplication. Pour plus d’informations sur KEEP_REPLICATION, consultez [RESTORE &#40;Transact-SQL&#41;](../../t-sql/statements/restore-statements-transact-sql.md).  
  
## <a name="log-reader-agent-behavior"></a>Comportement de l'Agent de lecture du journal  
 Le tableau suivant décrit la façon dont se comporte l'Agent de lecture du journal dans les différents modes d'opération de la mise en miroir des bases de données.  
  
|Mode d'opération|Comportement de l'Agent de lecture du journal en cas d'indisponibilité du miroir|  
|--------------------|------------------------------------------------------------|  
|Mode haute sécurité avec basculement automatique|Si le miroir n'est pas disponible, l'Agent de lecture du journal propage les commandes vers la base de données de distribution. Le principal ne peut pas basculer sur le miroir tant que ce dernier n'est pas connecté et que toutes les transactions du principal n'y figurent pas.|  
|Mode hautes performances|Si le miroir n'est pas disponible, la base de données principale s'exécute sans filet (elle n'est pas mise en miroir). Toutefois, l'Agent de lecture du journal réplique les transactions renforcées sur le miroir. Si le service est forcé et que le serveur miroir joue le rôle de principal, l'Agent de lecture du journal travaille en fonction du miroir et commence à collecter les nouvelles transactions.<br /><br /> Sachez que la durée de latence de la réplication augmente si le miroir se trouve derrière le principal.|  
|Mode Haute sécurité sans basculement automatique|Toutes les transactions validées sont renforcées sur le disque dur du miroir. L'Agent de lecture du journal ne réplique que les transactions renforcées du miroir. Si le miroir n'est pas disponible, le principal empêche toute autre activité dans la base de données. L'Agent de lecture du journal n'a plus aucune transaction à répliquer.|  
  
## <a name="see-also"></a>Voir aussi  
 [Réplication SQL Server](../../relational-databases/replication/sql-server-replication.md)   
 [Copie des journaux de transaction et réplication &#40;SQL Server&#41;](../../database-engine/log-shipping/log-shipping-and-replication-sql-server.md)  
  
  
