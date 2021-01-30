---
description: sp_changemergepublication (Transact-SQL)
title: sp_changemergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_changemergepublication_TSQL
- sp_changemergepublication
helpviewer_keywords:
- sp_changemergepublication
ms.assetid: 81fe1994-7678-4852-980b-e02fedf1e796
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e7807a445de3ddf919679b09002646c87b313c5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159854"
---
# <a name="sp_changemergepublication-transact-sql"></a>sp_changemergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés d'une publication de fusion. Cette procédure stockée est exécutée sur le serveur de publication dans la base de données de publication.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_changemergepublication [ @publication= ] 'publication'  
    [ , [ @property= ] 'property' ]  
    [ , [ @value= ] 'value' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @publication = ] 'publication'` Nom de la publication. *publication* est de **type sysname**, sans valeur par défaut.  
  
`[ @property = ] 'property'` Propriété à modifier pour la publication donnée. *Property* est de **type sysname** et peut prendre l’une des valeurs indiquées dans le tableau qui suit.  
  
`[ @value = ] 'value'` Nouvelle valeur de la propriété spécifiée. la *valeur* est de type **nvarchar (255)** et peut prendre l’une des valeurs indiquées dans le tableau suivant.  
  
 Le tableau ci-dessous décrit les propriétés modifiables de la publication ainsi que les limites liées aux valeurs de ces propriétés.  
  
|Propriété|Valeur|Description|  
|--------------|-----------|-----------------|  
|**allow_anonymous**|**true**|Les abonnements anonymes sont autorisés.|  
||**false**|Les abonnements anonymes ne sont pas autorisés.|  
|**allow_partition_realignment**|**true**|Les opérations de suppression sont envoyées à l'Abonné pour refléter les résultats d'une modification de partition en supprimant des données qui ne font plus partie de la partition de l'Abonné. Il s'agit du comportement par défaut.|  
||**false**|Les données d'une ancienne partition demeurent sur l'Abonné : les modifications apportées à ces données sur le serveur de publication ne sont pas répliquées sur l'Abonné. À la place, les modifications apportées à l'Abonné sont répliquées vers le serveur de publication. Cela permet de conserver dans un abonnement des données issues d'une ancienne partition afin qu'elles soient accessibles à des fins d'historique.|  
|**allow_pull**|**true**|Les abonnements par extraction de données (pull) sont autorisés pour la publication concernée.|  
||**false**|Les abonnements par extraction de données (pull) sont pas autorisés pour la publication concernée.|  
|**allow_push**|**true**|Les abonnements par envoi de données (push) sont autorisés pour la publication concernée.|  
||**false**|Les abonnements par envoi de données (push) ne sont pas autorisés pour la publication concernée.|  
|**allow_subscriber_initiated_snapshot**|**true**|L'Abonné peut initier le processus d'instantané.|  
||**false**|L'Abonné ne peut pas initier le processus d'instantané.|  
|**allow_subscription_copy**|**true**|Vous pouvez copier les bases de données d'abonnement qui sont abonnées à la publication.|  
||**false**|Vous ne pouvez pas copier les bases de données d'abonnement qui sont abonnées à la publication.|  
|**allow_synctoalternate**|**true**|Permet à un serveur partenaire de synchronisation différent de se synchroniser avec le serveur de publication.|  
||**false**|Ne permet pas à un serveur partenaire de synchronisation différent de se synchroniser avec le serveur de publication.|  
|**allow_web_synchronization**|**true**|Les abonnements peuvent être synchronisés via HTTPS.|  
||**false**|Les abonnements ne peuvent pas être synchronisés via HTTPS.|  
|**alt_snapshot_folder**||Spécifie l'emplacement de l'autre dossier de l'instantané.|  
|**automatic_reinitialization_policy**|**1**|Les modifications sont téléchargées depuis l'Abonné avant que l'abonnement ne soit réinitialisé.|  
||**0**|L'abonnement est réinitialisé sans téléchargement préalable des modifications.|  
|**centralized_conflicts**|**true**|Tous les enregistrements en conflit sont stockés sur le serveur de publication. Vous devez réinitialiser les abonnés existants si vous modifiez cette propriété.|  
||**false**|Les enregistrements en conflit sont stockés sur le serveur qui est sorti perdant de la résolution du conflit. Vous devez réinitialiser les abonnés existants si vous modifiez cette propriété.|  
|**compress_snapshot**|**true**|L'instantané se trouvant dans un dossier d'instantané de remplacement est compressé au format CAB. L'instantané se trouvant dans le dossier d'instantané par défaut ne peut pas être compressé. La modification de cette propriété requiert un nouvel instantané.|  
||**false**|L'instantané n'est pas compressé par défaut. La modification de cette propriété requiert un nouvel instantané.|  
|**conflict_logging**|**publisher**|Les enregistrements en conflit sont stockés sur le serveur de publication.|  
||**côté**|Les enregistrements en conflit sont stockés dans l'Abonné à l'origine du conflit. Non pris en charge pour les [!INCLUDE[ssEW](../../includes/ssew-md.md)] abonnés *.*|  
||**versions**|Les enregistrements en conflit sont stockés dans le serveur de publication et l'Abonné.|  
|**conflict_retention**||**Entier** qui spécifie la période de rétention, en jours, pendant laquelle les conflits sont conservés. La définition de *conflict_retention* sur **0** signifie qu’aucun nettoyage de conflit n’est nécessaire.|  
|**description**||Description de la publication.|  
|**dynamic_filters**|**true**|La publication est filtrée sur une clause dynamique.|  
||**false**|La publication n'est pas filtrée dynamiquement.|  
|**enabled_for_internet**|**true**|La publication est activée pour Internet. Le protocole FTP (File Transfer Protocol) peut être utilisé pour le transfert des fichiers d'instantané vers un Abonné. Les fichiers de synchronisation de la publication sont placés dans le répertoire C:\Program Files\Microsoft SQL Server\MSSQL\Repldata\ftp.|  
||**false**|La publication n'est pas activée pour Internet.|  
|**ftp_address**||Adresse réseau du service FTP du serveur de distribution. Indique l'emplacement de stockage des fichiers d'instantané de la publication.|  
|**ftp_login**||Nom d’utilisateur utilisé pour se connecter au service FTP.|  
|**ftp_password**||Mot de passe utilisateur utilisé pour la connexion au service FTP.|  
|**ftp_port**||Numéro de port du service FTP du serveur de distribution. Indique le numéro de port TCP du site FTP où sont stockés les fichiers d'instantané de la publication.|  
|**ftp_subdirectory**||Indique l'emplacement où sont créés les fichiers d'instantanés si la publication prend en charge la propagation d'instantanés par FTP.|  
|**generation_leveling_threshold**|**int**|Spécifie le nombre de modifications contenues dans une génération. Une génération est une collection de modifications remises à un serveur de publication ou à un Abonné.|  
|**keep_partition_changes**|**true**|La synchronisation est optimisée et seuls les abonnés qui détiennent des lignes dans les partitions modifiées sont concernés. La modification de cette propriété requiert un nouvel instantané.|  
||**false**|La synchronisation n'est pas optimisée et les partitions envoyées à tous les abonnés seront vérifiées lors de la modification de leurs données. La modification de cette propriété requiert un nouvel instantané.|  
|**max_concurrent_merge**||Il s’agit d’un **entier** qui représente le nombre maximal de processus de fusion simultanés qui peuvent être exécutés sur une publication. Si ce nombre est 0, il n'y a pas de limite. Si ce nombre est supérieur au nombre planifié de processus de fusion à exécuter simultanément, les travaux en trop sont placés dans une file d'attente jusqu'à ce qu'un processus de fusion en cours d'exécution s'achève.|  
|**max_concurrent_dynamic_snapshots**||Il s’agit d’un **entier** qui représente le nombre maximal de sessions d’instantané pour générer un instantané de données filtrées qui peut s’exécuter simultanément sur une publication de fusion qui utilise des filtres de lignes paramétrables. Si la **valeur est 0**, il n’y a aucune limite. Si ce nombre est supérieur au nombre planifié de processus d'instantané à exécuter simultanément, les travaux en trop sont placés dans une file d'attente jusqu'à ce qu'un processus de fusion en cours d'exécution s'achève.|  
|**post_snapshot_script**||Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de distribution ou de fusion exécute le script de post-instantané après que tous les autres scripts d'objets et données répliqués ont été appliqués lors d'une synchronisation initiale. La modification de cette propriété requiert un nouvel instantané.|  
|**pre_snapshot_script**||Spécifie un pointeur vers un emplacement de fichier **. SQL** . L'Agent de fusion exécute le script de pré-instantané avant tous les scripts d'objets répliqués, lors de l'application d'un instantané chez un abonné. La modification de cette propriété requiert un nouvel instantané.|  
|**publication_compatibility_level**|**100RTM**|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|  
||**90RTM**|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|  
|**publish_to_activedirectory**|**true**|Ce paramètre est déconseillé et il n'est pris en charge que pour la compatibilité descendante des scripts. Vous ne pouvez plus ajouter d'informations de publication dans Active Directory.|  
||**false**|Supprime les informations de publication d'Active Directory.|  
|**replicate_ddl**|**1**|Les instructions DDL (Data Definition Language) exécutées sur le serveur de publication sont répliquées.|  
||**0**|Les instructions DDL ne sont pas répliquées.|  
|**fixation**||Il s’agit d’un **entier** qui représente le nombre d’unités de *retention_period_unit* pour lesquelles enregistrer les modifications pour la publication donnée. L'abonnement expire et doit être réinitialisé s'il n'est pas synchronisé pendant la période de rétention et que les modifications en attente qu'il aurait dû recevoir ont été supprimées par une opération de nettoyage sur le serveur de distribution. La période de rétention maximale autorisée correspond au nombre de jours entre la date actuelle et le 31 décembre 9999.<br /><br /> Remarque : la période de rétention pour les publications de fusion dispose d’une période de grâce de 24 heures pour gérer les abonnés dans différents fuseaux horaires.|  
|**retention_period_unit**|**day**|La période de rétention est spécifiée en jours.|  
||**week**|La période de rétention est spécifiée en semaines.|  
||**month**|La période de rétention est spécifiée en mois.|  
||**year**|La période de rétention est spécifiée en années.|  
|**snapshot_in_defaultfolder**|**true**|Les fichiers d'instantané sont stockés dans le dossier d'instantané par défaut.|  
||**false**|Les fichiers d’instantanés sont stockés dans l’autre emplacement spécifié par *alt_snapshot_folder*. Cette combinaison indique que les fichiers d'instantané sont stockés dans les emplacements par défaut et de remplacement.|  
|**snapshot_ready**|**true**|L'instantané de la publication est disponible.|  
||**false**|L'instantané de la publication n'est pas disponible.|  
|**statut**|**active**|La publication est dans un état actif.|  
||**inactive**|La publication est dans un état inactif.|  
|**sync_mode**|**natif** ou<br /><br /> **BCP natif**|La sortie programme de la copie en bloc en mode natif de toutes les tables est utilisée pour l'instantané initial.|  
||**character**<br /><br /> ou **caractère BCP**|La sortie programme de la copie en bloc en mode caractère de toutes les tables est utilisée pour l'instantané initial, ce qui est requis pour tous les Abonnés non [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**use_partition_groups**<br /><br /> Remarque : après avoir utilisé partition_groups, si vous souhaitez revenir à l’utilisation de **« SetupBelongs »** et que vous définissez **use_partition_groups = false** dans **changemergearticle**, cela peut ne pas être reflété correctement après la prise d’un instantané. Les déclencheurs générés par l'instantané sont conformes avec les groupes de partition.<br /><br /> La solution de contournement de ce scénario consiste à définir l’État sur inactif, à modifier le **use_partition_groups**, puis à définir l’État sur actif.|**true**|La publication utilise des partitions précalculées.|  
||**false**|La publication n'utilise pas de partitions précalculées.|  
|**validate_subscriber_info**||Répertorie les fonctions utilisées pour extraire des informations d'Abonné. Puis, valide les critères de filtrage dynamiques utilisés pour l'Abonné pour vérifier que les informations sont partitionnées régulièrement.|  
|**web_synchronization_url**||Valeur par défaut de l'URL Internet utilisée pour la synchronisation Web.|  
|NULL (par défaut)||Retourne la liste des valeurs prises en charge pour la *propriété*.|  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirme que l’action entreprise par cette procédure stockée peut invalider un instantané existant. *force_invalidate_snapshot* est un **bit**, avec **0** comme valeur par défaut.  
  
 **0** indique que la modification de la publication n’invalide pas l’instantané. Si la procédure stockée détecte que la modification requiert un nouvel instantané, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que la modification de la publication peut entraîner l’instantané. S’il existe des abonnements qui nécessitent un nouvel instantané, accorde l’autorisation de marquer l’instantané existant comme obsolète et de générer un nouvel instantané.  
  
 Consultez la section Notes pour connaître les propriétés qui, une fois modifiées, nécessitent la génération d’un nouvel instantané.  
  
`[ @force_reinit_subscription = ] force_reinit_subscription` Confirme que l’action entreprise par cette procédure stockée peut nécessiter la réinitialisation des abonnements existants. *force_reinit_subscription* est un **bit** avec **0** comme valeur par défaut.  
  
 **0** indique que la modification de la publication ne nécessite pas la réinitialisation des abonnements. Si la procédure stockée détecte que la modification nécessite la réinitialisation des abonnements existants, une erreur se produit et aucune modification n'est effectuée.  
  
 **1** indique que la modification de la publication entraîne la réinitialisation des abonnements existants et autorise la réinitialisation de l’abonnement.  
  
 Voir la section Remarques pour connaître les propriétés dont la modification requiert la réinitialisation de tous les abonnements existants.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_changemergepublication** est utilisé dans la réplication de fusion.  
  
 La modification des propriétés suivantes requiert qu'un nouvel instantané soit généré. Vous devez spécifier la valeur **1** pour le paramètre *force_invalidate_snapshot* .  
  
-   **alt_snapshot_folder**  
  
-   **compress_snapshot**  
  
-   **dynamic_filters**  
  
-   **ftp_address**  
  
-   **ftp_login**  
  
-   **ftp_password**  
  
-   **ftp_port**  
  
-   **ftp_subdirectory**  
  
-   **post_snapshot_script**  
  
-   **publication_compatibility_level** ( **80SP3** uniquement)  
  
-   **pre_snapshot_script**  
  
-   **snapshot_in_defaultfolder**  
  
-   **sync_mode**  
  
-   **use_partition_groups**  
  
 Vous devez réinitialiser les abonnements existants pour modifier les propriétés suivantes. Vous devez spécifier la valeur **1** pour le paramètre *force_reinit_subscription* .  
  
-   **dynamic_filters**  
  
-   **validate_subscriber_info**  
  
 Pour répertorier les objets de publication à Active Directory à l’aide de l' *publish_to_Active_Directory*, l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objet doit déjà être créé dans Active Directory.  
  
## <a name="example"></a>Exemple  
 [!code-sql[HowTo#sp_changemergepublication](../../relational-databases/replication/codesnippet/tsql/sp-changemergepublicatio_1.sql)]  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** ou du rôle de base de données fixe **db_owner** peuvent exécuter **sp_changemergepublication**.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et modifier les propriétés d’une publication](../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Changer les propriétés des publications et des articles](../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [sp_addmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_dropmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procédures stockées de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
