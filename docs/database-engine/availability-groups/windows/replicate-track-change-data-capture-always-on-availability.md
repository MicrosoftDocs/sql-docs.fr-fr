---
title: Réplication, suivi des modifications, capture des changements de données et groupes de disponibilité
description: Découvrez-en plus sur l’interopérabilité de la réplication, du suivi des modifications et de la capture des changements de données quand vous les utilisez avec des groupes de disponibilité Always On SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/21/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- change tracking [SQL Server], AlwaysOn Availability Groups
- change data capture [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: e17a9ca9-dd96-4f84-a85d-60f590da96ad
author: cawrites
ms.author: chadam
ms.openlocfilehash: f82db97e9b818ecca6682cf6778850dab8a238c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344551"
---
# <a name="replication-change-tracking--change-data-capture---always-on-availability-groups"></a>Réplication, suivi des modifications et capture de données modifiées - groupes de disponibilité Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La réplication, la capture de données modifiées (CDC) et le suivi des modifications (CT) sont pris en charge sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] aide à fournir des fonctionnalités haute disponibilité et de récupération de base de données supplémentaires.  
  
##  <a name="overview-of-replication-with-availability-groups"></a><a name="Overview"></a>Vue d’ensemble de la réplication avec des groupes de disponibilité  
  
###  <a name="publisher-redirection"></a><a name="PublisherRedirect"></a> Redirection de serveur de publication  
 Lorsqu'une base de données publiée repose sur la technologie [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], le serveur de distribution qui offre un accès d'agent à la base de données de publication est configuré avec des entrées redirected_publishers. Ces entrées redirigent la combinaison serveur de publication/base de données initialement configurée, en se servant d'un nom d'écouteur de groupe de disponibilité pour se connecter au serveur de publication et à la base de données de publication. Les connexions établies via le nom d'écouteur de groupe de disponibilité échouent lors d'un basculement. Lors du redémarrage de l'agent de réplication, à l'issue du basculement, la connexion est automatiquement redirigée vers le nouveau principal.  
  
 Dans un groupe de disponibilité, une base de données secondaire ne peut pas être un serveur de publication. La republication est prise en charge uniquement lorsque la réplication transactionnelle est associée à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Si une base de données publiée est membre d'un groupe de disponibilité et que le serveur de publication est redirigé, il doit être redirigé vers un nom d'écouteur de groupe de disponibilité associé au groupe de disponibilité. Il ne peut pas être redirigé vers un nœud explicite.  
  
> [!NOTE]  
>  Après un basculement vers un réplica secondaire, le Moniteur de réplication ne peut pas ajuster le nom de l'instance de publication de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et continue à afficher les informations de réplication sous le nom de l'instance principale [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]d'origine. Après le basculement, un jeton de suivi ne peut pas être écrit à l'aide du Moniteur de réplication, toutefois un jeton de suivi écrit sur le nouveau serveur de publication à l'aide de [!INCLUDE[tsql](../../../includes/tsql-md.md)]est visible dans le Moniteur de réplication.  
  
###  <a name="general-changes-to-replication-agents-to-support-availability-groups"></a><a name="Changes"></a> Modifications générales apportées aux agents de réplication pour prendre en charge les groupes de disponibilité  
 Trois agents de réplication ont été modifiés pour prendre en charge [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. L'agent de lecture du journal, ainsi que les agents d'instantané et de fusion ont été modifiés pour interroger la base de données de distribution pour le serveur de publication redirigé et pour utiliser le nom d'écouteur de groupe de disponibilité retourné, si un serveur de publication redirigé était déclaré, pour se connecter au serveur de publication de base de données.  
  
 Par défaut, lorsque les agents interrogent le serveur de distribution afin de déterminer si le serveur de publication d'origine a été redirigé, l'adéquation de la cible actuelle ou de la redirection est vérifiée avant de retourner l'hôte redirigé à l'agent. Il s'agit du comportement recommandé. Toutefois, si le démarrage de l’agent se produit très fréquemment, la surcharge liée à la procédure stockée de validation peut être considérée comme trop importante. Un nouveau commutateur de ligne de commande, *BypassPublisherValidation*, a été ajouté à l’Agent Lecture de journal, à l’Agent d’instantané et à l’Agent Fusion. Lorsque le commutateur est utilisé, le serveur de publication redirigé est retourné immédiatement à l'agent et l'exécution de la procédure stockée de validation est ignorée.  
  
 Les échecs retournés de la procédure stockée de validation sont consignés dans les journaux d'historique de l'agent. Les erreurs dont le niveau de gravité est supérieur ou égal à 16 entraîneront l'arrêt des agents. Certaines fonctions de reprise ont été intégrées aux agents afin de gérer la déconnexion attendue d'une base de données publiée en cas de basculement vers un nouveau principal.  
  
#### <a name="log-reader-agent-modifications"></a>Modifications apportées à l'agent de lecture du journal  
 L'agent de lecture du journal comporte les modifications suivantes.  
  
-   **Cohérence de bases de données répliquées**  
  
     Quand une base de données publiée est membre d’un groupe de disponibilité, par défaut, l’agent de lecture du journal ne traite pas les enregistrements de journal qui n’ont pas déjà été renforcés au niveau de tous les réplicas secondaires de groupe de disponibilité. Cela permet de s’assurer que lors du basculement, toutes les lignes répliquées sur un abonné sont également présentes dans le nouveau principal.  
  
     Quand le serveur de publication dispose de deux réplicas de disponibilité uniquement (un principal et un secondaire) et qu’un basculement a lieu, le réplica principal d’origine reste arrêté, car le lecteur de journal n’avance pas tant que toutes les bases de données secondaires ne sont pas remises en ligne ou tant que les réplicas secondaires défaillants n’ont pas été supprimés du groupe de disponibilité. Le lecteur de journal, qui s’exécute à présent sur la base de données secondaire, n’avance pas étant donné qu’Always On ne peut pas renforcer de modifications sur une base de données secondaire. Pour permettre au lecteur de journal de poursuivre et de continuer à disposer de la capacité de récupération d'urgence, supprimez le réplica principal d'origine du groupe de disponibilité à l'aide de ALTER AVAILABITY GROUP <nom_groupe> REMOVE REPLICA. Ajoutez ensuite un nouveau réplica secondaire au groupe de disponibilité.  
  
-   **Indicateur de trace 1448**  
  
     L'indicateur de trace 1448 permet à l'agent de lecture du journal de réplication d'avancer même si les réplicas secondaires asynchrones n'ont pas accusé réception d'une modification. Même si cet indicateur de trace est activé, l’Agent Lecture de journal attend toujours les réplicas secondaires synchrones. L'agent de lecture du journal n'ira pas au delà de l'accusé réception minimum des réplicas secondaires synchrones. Cet indicateur de trace s'applique à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], et pas simplement à un groupe de disponibilité, à une base de données de disponibilité ou à une instance de l'agent de lecture de journal. Cet indicateur de trace prend effet immédiatement, sans redémarrage. Il peut être activé d'avance ou lorsque le réplica secondaire asynchrone échoue.  
  
###  <a name="stored-procedures-supporting-availability-groups"></a><a name="StoredProcs"></a> Procédures stockées prenant en charge les groupes de disponibilité  
  
-   **sp_redirect_publisher**  
  
     La procédure stockée **sp_redirect_publisher** permet de spécifier un serveur de publication redirigé pour une combinaison existante serveur de publication/base de données. Si la base de données du serveur de publication appartient à un groupe de disponibilité, le serveur de publication redirigé correspond au nom d'écouteur de groupe de disponibilité.  
  
-   **Sp_get_redirected_publisher**  
  
     La procédure stockée **sp_get_redirected_publisher** permet aux agents de réplication d’interroger un serveur de distribution pour déterminer si une combinaison serveur de publication/base de données dispose d’un serveur de publication redirigé défini. Cette procédure stockée remplit deux objectifs. En premier lieu, elle permet à l'agent de déterminer si le serveur de publication d'origine a été redirigé. Ensuite, elle peut également initier une procédure stockée de validation exécutée sur le serveur de distribution (**sp_validate_redirected_publisher**) qui vérifie l’adéquation du nœud cible de la redirection pour servir de serveur de publication pour la base de données nommée.  
  
     Pour exécuter cette procédure stockée, l’appelant doit être membre du rôle serveur **sysadmin** , du rôle de base de données **db_owner** de la base de données de distribution, ou être membre d’une **liste d’accès à une publication** pour une publication définie associée à la base de données du serveur de publication.  
  
-   **sp_validate_redirected_publisher**  
  
     Cette procédure stockée tente de valider que le serveur de publication actuel est capable d'héberger la base de données publiée. Elle peut être appelée à tout moment pour vérifier que l'hôte actuel de la base de données publiée est en mesure de prendre en charge la réplication.  
  
-   **sp_validate_replicate_hosts_as_publishers**  
  
     S’il est utile que les agents vérifient que le serveur principal actuel peut fonctionner comme serveur de publication de réplication pour une base de données du serveur de publication, une fonctionnalité de validation plus générale est nécessaire pour établir la validité d’une topologie de réplication dans une base de données de disponibilité Always On. La procédure stockée **sp_validate_replica_hosts_as_publishers** est conçue pour satisfaire à ce besoin.  
  
     Cette procédure stockée est toujours exécutée manuellement. L'appelant doit être **sysadmin** sur le serveur de distribution, **dbowner** de la base de données de distribution ou un membre de la **liste d'accès à la publication** d'une publication de la base de données du serveur de publication. En outre, l'identifiant de connexion de l'appelant doit être un identifiant valide pour tous les hôtes de réplica de groupe de disponibilité, et disposer de privilèges de sélection sur la base de données de disponibilité associée à la base de données du serveur de publication.  
  
###  <a name="change-data-capture"></a><a name="CDC"></a> Capture de données modifiées  
 Les bases de données activées pour la capture de données modifiées (CDC) peuvent tirer parti de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] afin d’assurer que non seulement la base de données reste disponible en cas de défaillance, mais également que les modifications apportées aux tables de base de données continuent d’être surveillées et déposées dans les tables de modifications de capture de données modifiées. L'ordre dans lequel la capture de données modifiées et [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] sont configurés n'est pas important. Les bases de données activées pour la capture de données modifiées peuvent être ajoutées à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], et celles qui sont membres d’un groupe de disponibilité Always On peuvent être activées pour la capture de données modifiées. Dans les deux cas, toutefois, la configuration de la capture de données modifiées est toujours effectuée sur le réplica principal actuel ou prévu. CDC utilise l'agent de lecture du journal et est soumis aux mêmes limitations décrites dans la section **Modifications apportées à l'agent de lecture du journal** plus haut dans cette rubrique.  
  
-   **Récolte des modifications pour la capture de données modifiées sans réplication**  
  
     Si la capture de données modifiées est activée pour une base de données, mais que la réplication ne l'est pas, le processus de capture utilisé pour récolter les modifications à partir du journal et les déposer dans les tables de modifications de capture de données modifiées s'exécute au niveau de l'hôte de capture de données modifiées comme son propre travail de l'Agent SQL.  
  
     Pour reprendre la récolte des modifications après un basculement, la procédure stockée **sp_cdc_add_job** doit être exécutée au niveau du nouveau principal pour créer le travail de capture local.  
  
     L'exemple suivant crée le travail de capture.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'capture';  
    ```  
  
-   **Récolte des modifications pour la capture de données modifiées avec réplication**  
  
     Si la capture de données modifiées et la réplication sont activées pour une base de données, le lecteur de journal gère le flux des tables de modifications de capture de données modifiées. Dans ce cas, les techniques utilisées par la réplication pour tirer parti de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] s’assurent que les modifications continuent d’être récoltées à partir du journal et à être déposées dans les tables de modifications de capture de données modifiées après le basculement. Aucune opération supplémentaire ne doit être effectuée pour la capture de données modifiées dans cette configuration pour que les tables de modifications soient remplies.  
  
-   **Nettoyage de la capture de données modifiées**  
  
     Pour s’assurer que le nettoyage approprié se produit au niveau de la nouvelle base de données principale, un travail local de nettoyage doit toujours être créé. L'exemple suivant crée le travail de nettoyage.  
  
    ```sql  
    EXEC sys.sp_cdc_add_job @job_type = 'cleanup';  
    ```  
  
    > [!NOTE]  
    >  Vous devez créer les travaux au niveau de toutes les cibles possibles de basculement avant le basculement, et les marquer comme étant désactivés jusqu'à ce que le réplica de disponibilité sur un hôte devienne le nouveau réplica principal. Les travaux de capture de données modifiées s'exécutant au niveau de l'ancienne base de données principale doivent également être désactivés lorsque la base de données locale devient une base de données secondaire. Pour désactiver et activer des travaux, utilisez l’option *\@enabled* de [sp_update_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md). Pour plus d’informations sur la création de travaux de capture de données modifiées, consultez [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md).  
  
-   **Ajout de rôles de capture de données modifiées à un réplica de base de données principal Always On**  
  
     Lorsqu'une table est activée pour la capture de données modifiées, il est possible d'associer un rôle de base de données à l'instance de capture. Si un rôle est spécifié, l'utilisateur souhaitant utiliser les fonctions table de capture de données modifiées pour accéder à des modifications pour la table doit non seulement avoir un accès choisi aux colonnes de table faisant l'objet d'un suivi, mais doit également être membre du rôle nommé. Si le rôle spécifié n'existe pas encore, il sera créé. Quand des rôles de base de données sont ajoutés automatiquement à une base de données principale Always On, les rôles sont également propagés aux bases de données secondaires du groupe de disponibilité.  
  
-   **Applications clientes qui accèdent aux données de modifications de capture de données modifiées et Always On**  
  
     Les applications clientes qui utilisent les fonctions table (TVF) ou des serveurs liés pour accéder aux données de la table de modifications ont également besoin de pouvoir localiser un hôte de capture de données modifiées approprié après le basculement. Le nom d'écouteur de groupe de disponibilité correspond au mécanisme fourni par [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] pour permettre à une connexion, en toute transparence, d'être redirigée vers un hôte différent. Une fois qu'un nom d'écouteur de groupe de disponibilité est associé à un groupe de disponibilité, il peut être utilisé dans les chaînes de connexion TCP. Deux scénarios de connexion différents sont pris en charge par le biais du nom d'écouteur de groupe de disponibilité.  
  
    -   L’un garantit que les demandes de connexion sont toujours dirigées vers le réplica principal actuel.  
  
    -   L’autre garantit que les demandes de connexion sont dirigées vers un réplica secondaire en lecture seule.  
  
     Utilisée pour rechercher un réplica secondaire en lecture seule, une liste de routage en lecture seule doit également être définie pour le groupe de disponibilité. Pour plus d’informations sur l’accès de routage aux serveurs secondaires accessibles en lecture, consultez [Pour configurer des réplicas de disponibilité pour le routage en lecture seule](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).  
  
    > [!NOTE]  
    >  Il existe un certain délai de propagation associé à la création d'un nom d'écouteur de groupe de disponibilité et son utilisation par les applications clientes afin d'accéder à un réplica de base de données du groupe de disponibilité.  
  
     Utilisez la requête suivante pour déterminer si un nom d'écouteur de groupe de disponibilité a été défini pour le groupe de disponibilité qui héberge une base de données CDC. La requête retourne le nom d'écouteur du groupe de disponibilité, s'il a été créé.  
  
    ```sql  
    SELECT dns_name   
    FROM sys.availability_group_listeners AS l  
    INNER JOIN sys.availability_databases_cluster AS d  
        ON l.group_id = d.group_id  
    WHERE d.database_name = N'MyCDCDB';  
    ```  
  
-   **Redirection de la charge de requête vers un réplica secondaire accessible en lecture**  
  
     Même si, dans de nombreux cas, une application cliente souhaite toujours se connecter au réplica principal actuel, il ne s’agit pas de la seule façon de tirer parti de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Si un groupe de disponibilité est configuré pour prendre en charge des réplicas secondaires accessibles en lecture, les données modifiées peuvent également être collectées à partir des nœuds secondaires.  
  
     Lorsqu'un groupe de disponibilité est configuré, l'attribut ALLOW_CONNECTIONS associé à SECONDARY_ROLE est utilisé pour spécifier le type d'accès secondaire pris en charge. Dans le cas d’une configuration de type ALL, toutes les connexions au réplica secondaire sont autorisées, mais seules celles qui exigent un accès en lecture seule aboutissent. Dans le cas d'une configuration de type READ_ONLY, il est nécessaire de spécifier l'intention en lecture seule lors de l'établissement de la connexion à la base de données secondaire pour que la connexion réussisse. Pour plus d’informations, consultez [Configurer l’accès en lecture seule sur un réplica de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md).  
  
     La requête suivante peut être utilisée pour déterminer si l'intention en lecture seule est nécessaire pour se connecter à un réplica secondaire accessible en lecture.  
  
    ```sql  
    SELECT g.name AS AG, replica_server_name, secondary_role_allow_connections_desc  
    FROM sys.availability_replicas AS r  
    JOIN sys.availability_groups AS g  
        ON r.group_id = g.group_id  
    WHERE g.name = N'MY_AG_NAME';  
    ```  
  
     Le nom d'écouteur de groupe de disponibilité ou le nom du nœud explicite peut être utilisé pour rechercher le réplica secondaire. Si le nom d'écouteur du groupe de disponibilité est utilisé, l'accès est dirigé vers un réplica secondaire approprié.  
  
     Quand **sp_addlinkedserver** est utilisé pour créer un serveur lié afin d’accéder au serveur secondaire, le paramètre *\@datasrc* est utilisé pour le nom d’écouteur de groupe de disponibilité ou le nom du serveur explicite, et le paramètre *\@provstr* permet de spécifier l’intention de lecture seule.  
  
    ```sql  
    EXEC sp_addlinkedserver   
    @server = N'linked_svr',   
    @srvproduct=N'SqlServer',  
    @provider=N'SQLNCLI11',   
    @datasrc=N'AG_Listener_Name',   
    @provstr=N'ApplicationIntent=ReadOnly',   
    @catalog=N'MY_DB_NAME';  
    ```  
  
-   **Accès client aux données de modifications de capture de données modifiées et aux connexions de domaine**  
  
     En général, vous devez utiliser les connexions de domaine pour l’accès client aux données modifiées résidant dans les bases de données qui sont membres des groupes de disponibilité Always On. Pour garantir l’accès continu aux données modifiées après le basculement, l’utilisateur de domaine aura besoin des privilèges d’accès sur tous les hôtes prenant en charge les réplicas de groupe de disponibilité. Si un utilisateur de base de données est ajouté à une base de données dans un réplica principal et que l'utilisateur est associé à une connexion de domaine, l'utilisateur de base de données est propagé aux bases de données secondaires et continue d'être associé à la connexion de domaine spécifiée. Si le nouvel utilisateur de base de données est associé à une connexion d'authentification SQL Server, l'utilisateur au niveau des bases de données secondaires sera propagé sans connexion. Lorsque la connexion d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associée peut être utilisée pour accéder aux données modifiées au niveau du serveur principal où l'utilisateur de la base de données a été défini à l'origine, ce nœud est le seul où l'accès est possible. La connexion d'authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas accéder aux données d'une base de données secondaire, ni à celles d'une nouvelle base de données primaire autre que la base de données d'origine où l'utilisateur de la base de données a été défini.  
     
-   **Désactivation de la capture des données modifiées**  
Si la capture des données modifiées doit être désactivé sur une base de données qui fait partie d’un groupe de disponibilité Always On, des étapes supplémentaires sont nécessaires pour vérifier que la troncation du journal n’est pas affectée. Vous devez implémenter l’une des étapes suivantes pour empêcher la capture des données modifiées de bloquer la troncation du journal après avoir désactivé la capture de données modifiées :
    - Redémarrer le service SQL Server sur chaque instance de réplica secondaire
    - OU supprimer la base de données de toutes les instances de réplica secondaire du groupe de disponibilité et l’ajouter aux instances de réplica du groupe de disponibilité par amorçage automatique ou manuel
  
###  <a name="change-tracking"></a><a name="CT"></a> Suivi des modifications  
 Une base de données activée pour le suivi des modifications peut faire partie d’un groupe de disponibilité Always On. Aucune configuration supplémentaire n’est nécessaire. Les applications clientes de suivi des modifications qui utilisent les fonctions table (TVF) de capture de données modifiées pour accéder aux données modifiées ont besoin de pouvoir localiser le réplica principal après le basculement. Si l'application cliente se connecte via le nom d'écouteur de groupe de disponibilité, les demandes de connexion seront toujours dirigées correctement vers le réplica principal actuel.  
  
> [!NOTE]  
>  Les données de suivi des modifications doivent toujours être obtenues à partir du réplica principal. Toute tentative d'accès aux données de modification depuis un réplica secondaire provoque l'erreur suivante :  
>   
>  Msg 22117, Niveau 16, État 1, Ligne 1  
>   
>  Pour les bases de données qui sont membres d'un réplica secondaire (pour les bases de données secondaires), le suivi des modifications n'est pas pris en charge. Exécutez les requêtes de suivi des modifications sur les bases de données du réplica principal.  
  
##  <a name="prerequisites-restrictions-and-considerations-for-using-replication"></a><a name="Prereqs"></a> Conditions préalables requises, restrictions et considérations relatives à l'utilisation de la réplication  
 Cette section décrit les considérations à prendre en compte pour déployer la réplication avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], y compris les conditions préalables requises, les restrictions et les recommandations.  
  
### <a name="prerequisites"></a>Prérequis  
  
-   Lors de l'utilisation de la réplication transactionnelle, si la base de données de publication se trouve dans un groupe de disponibilité, le serveur de publication et le serveur de distribution doivent exécuter au moins [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. L'abonné peut utiliser un niveau inférieur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Lors de l'utilisation de la réplication de fusion, si la base de données de publication se trouve dans un groupe de disponibilité :  
  
    -   Abonnement par émission de données : le serveur de publication et le serveur de distribution doivent exécuter au moins [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
    -   Abonnement par extraction : le serveur de publication, le serveur de distribution et les bases de données de l’abonné doivent s’exécuter sur au moins [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Cela est dû au fait que l'Agent de fusion sur l'abonné doit comprendre la façon dont un groupe de disponibilité peut basculer sur son serveur secondaire.  
  
-   Les instances de serveur de publication satisfont toutes les conditions préalables pour faire partie d’un groupe de disponibilité Always On. Pour plus d’informations, consultez [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="restrictions"></a>Restrictions  
 Combinaisons de réplication prises en charge sur [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
|Réplication|Serveur de publication|Serveur de distribution|Abonné|  
|-|-|-|-|  
|**Transactionnelle**|Oui<br /><br /> Remarque : N’inclut pas la prise en charge de la réplication transactionnelle bidirectionnelle et réciproque.|Oui|Oui| 
|**P2P**|Non|Non|Non|  
|**Fusionner**|Oui|Non|Non|  
|**Instantané**|Oui|Non|Oui|
  
 **La base de données du serveur de distribution n’est pas prise pour une utilisation avec la mise en miroir de base de données.  
  
### <a name="considerations"></a>Considérations  
  
-   La base de données de distribution n’est pas prise en charge pour une utilisation avec la mise en miroir de bases de données, mais elle est prise en charge avec [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] soumise à certaines limitations, consultez [Configurer le groupe de disponibilité de distribution](../../../relational-databases/replication/configure-distribution-availability-group.md#limitations-or-exclusions). La configuration de la réplication est couplée à l'instance de SQL Server sur laquelle le serveur de distribution est configuré ; par conséquent la base de données de distribution ne peut pas être mise en miroir ni répliquée. Pour fournir une haute disponibilité au serveur de distribution, utilisez un cluster de basculement SQL Server. Pour plus d'informations, consultez [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
-   Le basculement d’abonné vers une base de données secondaire, s’il est pris en charge, est une procédure manuelle pour les abonnés de réplication de fusion. La procédure est, pour l'essentiel, identique à la méthode utilisée pour le basculement vers une base de données de l'abonné en miroir. Les abonnés de réplication transactionnelle ne nécessitent pas de traitement spécial lorsqu’ils participent à [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Les abonnés doivent exécuter [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ou une version ultérieure pour pouvoir participer à un groupe de disponibilité.  Pour plus d’informations, consultez [Abonnés de réplication et groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)
  
-   Les métadonnées et les objets qui existent en dehors de la base de données, c'est-à-dire les connexions, les travaux et les serveurs liés, ne sont pas propagés vers les réplicas secondaires. Si vous voulez faire figurer les métadonnées et les objets dans la nouvelle base de données principale après le basculement, vous devez les copier manuellement. Pour plus d’informations, consultez [Gestion des connexions et des travaux pour les bases de données d’un groupe de disponibilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md).  

### <a name="distributed-availability-groups"></a>Groupes de disponibilité distribués

Le serveur de publication ou la base de données de distribution d’un groupe de disponibilité ne peuvent pas être configurés dans le cadre d’un groupe de disponibilité distribué. La base de données du serveur de publication dans un groupe de disponibilité et la base de données de distribution dans un groupe de disponibilité nécessitent toutes deux un point de terminaison d’écouteur pour une configuration et une utilisation appropriées. Toutefois, il n’est pas possible de configurer un point de terminaison d’écouteur pour un groupe de disponibilité distribué.
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Réplication**  
  
-   [Configurer la réplication pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-replication-for-always-on-availability-groups-sql-server.md)  
  
-   [Gestion d’une base de données de publication Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/maintaining-an-always-on-publication-database-sql-server.md)  
  
-   [FAQ sur l’administration de la réplication](../../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)  
  
 **Modifier la capture de données**  
  
-   [Activer et désactiver la capture de données modifiées &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)  
  
-   [Administrer et surveiller la capture de données modifiées &#40;SQL Server&#41;](../../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
-   [Utiliser les données modifiées &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
  
 **Suivi des modifications**  
  
-   [Activer et désactiver le suivi des modifications &#40;SQL Server&#41;](../../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)  
  
-   [Gérer le suivi des modifications &#40;SQL Server&#41;](../../../relational-databases/track-changes/manage-change-tracking-sql-server.md)  
  
-   [Utiliser le suivi des modifications &#40;SQL Server&#41;](../../../relational-databases/track-changes/work-with-change-tracking-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Abonnés de réplication et groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/replication-subscribers-and-always-on-availability-groups-sql-server.md)   
 [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Vue d’ensemble des groupes de disponibilité Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Groupes de disponibilité Always On : Interopérabilité &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-interoperability-sql-server.md)   
 [Instances de cluster de basculement Always On &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [À propos du suivi des modifications &#40;SQL Server&#41;](../../../relational-databases/track-changes/about-change-tracking-sql-server.md)   
 [Réplication SQL Server](../../../relational-databases/replication/sql-server-replication.md)   
 [Suivi des modifications de données &#40;SQL Server&#41;](../../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)  
  
  
