---
description: Mise à niveau de la fonction de recherche en texte intégral
title: Mise à niveau de la fonction de recherche en texte intégral | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], installing
- migrating full-text indexes [SQL Server]
- upgrading Full-Text Search
- installing Full-Text Search
- full-text search [SQL Server], upgrading
ms.assetid: 2fee4691-f2b5-472f-8ccc-fa625b654520
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4f000d04dee158502ee1623031c0a37936995862
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589259"
---
# <a name="upgrade-full-text-search"></a>Mise à niveau de la fonction de recherche en texte intégral

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

SQL Server met à niveau la recherche en texte intégral lors de l’installation ou quand vous joignez, restaurez ou copiez des fichiers de base de données et des catalogues de texte intégral à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

##  <a name="upgrade-a-server-instance"></a><a name="Upgrade_Server"></a> Mise à niveau d’une instance de serveur  

Pour une mise à niveau sur place, une instance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] est installée côte à côte avec l'ancienne version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], et les données sont migrées. Si l'ancienne version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégrait la recherche en texte intégral, une nouvelle version de cette fonctionnalité est installée automatiquement. Une installation côte à côte signifie que chacun des composants suivants existe au niveau de l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Analyseurs lexicaux, générateurs de formes dérivées et filtres  
 Chaque instance utilise désormais son propre ensemble d'analyseurs lexicaux, de générateur de formes dérivées et de filtres, au lieu de s'appuyer sur la version de système d'exploitation de ces composants. Ces composants sont également plus faciles à inscrire et à configurer au niveau de l'instance. Pour plus d’informations, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) et [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md).  
  
 Hôte de démon de filtre  
 Les démons de filtre de texte intégral sont des processus qui se chargent en toute sécurité ; par ailleurs, ils gèrent l'exécution des composants extensibles externes utilisés pour un index et une requête, par exemple les analyseurs lexicaux, les générateurs de formes dérivées et les filtres, sans altérer l'intégrité du moteur de texte intégral. Une instance de serveur utilise un processus multithread pour tous les filtres multithreads et un processus monothread pour tous les filtres monothreads.  
  
> [!NOTE]  
>  [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] a introduit un compte de service pour le service de lancement FDHOST (MSSQLFDLauncher). Ce service propage les informations sur le compte de services aux processus d'hôte de démon de filtre d'une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur la définition du compte de service, consultez [Définir le compte du service du Lanceur de démon de filtre de texte intégral](../../relational-databases/search/set-the-service-account-for-the-full-text-filter-daemon-launcher.md).  
  
 Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], chaque index de recherche en texte intégral réside dans un catalogue de texte intégral qui appartient à un groupe de fichiers, a un chemin d'accès physique et est traité en tant que fichier de base de données. Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et versions ultérieures, un catalogue de texte intégral est un objet logique ou virtuel qui contient un groupe d'index de recherche en texte intégral. Par conséquent, un nouveau catalogue de texte intégral n'est pas traité en tant que fichier de base de données avec un chemin d'accès physique. Toutefois, un nouveau groupe de fichiers est créé sur le même disque pendant la mise à niveau de tout catalogue de texte intégral qui contient des fichiers de données. Cela maintient le comportement d'E/S de l'ancien disque après la mise à niveau. Tout index de recherche en texte intégral de ce catalogue est placé dans le nouveau groupe de fichiers si le chemin d'accès racine existe. Si l'ancien chemin de catalogue de texte intégral est non valide, la mise à niveau conserve l'index de recherche en texte intégral dans le même groupe de fichiers comme table de base ou, pour une table partitionnée, dans le groupe de fichiers principal.  
  
## <a name="full-text-upgrade-options"></a><a name="FT_Upgrade_Options"></a> Options de mise à niveau de texte intégral  

Lors de la mise à niveau d’une instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], l’interface utilisateur vous permet de choisir l’une des options de mise à niveau de texte intégral suivantes.  
  
**Importer**  
 Les catalogues de texte intégral sont importés. En général, l'importation est considérablement plus rapide que lors d'une reconstruction (rebuild). Par exemple, lorsque vous utilisez un seul processeur, l'importation s'exécute approximativement 10 fois plus vite que lors de la reconstruction. Toutefois, un catalogue de texte intégral importé n'utilise pas les nouveaux analyseurs lexicaux installés avec la dernière version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour garantir la cohérence dans les résultats de la requête, les catalogues de texte intégral doivent être reconstruits.  
  
> [!NOTE]  
>  Le processus de reconstruction peut s'exécuter en mode multithread, et si plus de 10 processeurs sont disponibles, la reconstruction peut s'effectuer plus vite que l'importation si vous la laissez utiliser tous les processeurs.  
  
 Si aucun catalogue de texte intégral n'est disponible, les index de recherche en texte intégral associés sont reconstruits. Cette option est disponible uniquement pour les bases de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 Pour plus d'informations sur l'impact de l'importation de l'index de recherche en texte intégral, consultez « Considérations relatives au choix d'une option de mise à niveau », plus loin dans cette rubrique.  
  
 **Recréation**  
 Les catalogues de texte intégral sont reconstruits à l'aide des analyseurs lexicaux nouveaux et améliorés. La reconstruction des index peut prendre du temps, et une quantité importante de ressources en termes d'UC et de mémoire peut être requise après la mise à niveau.  
  
 **Réinitialiser**  
 Les catalogues de texte intégral sont réinitialisés. Lors de la mise à niveau à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les catalogues de texte intégral sont supprimés, mais les métadonnées pour les catalogues de texte intégral et les index de recherche en texte intégral sont conservées. Après leur mise à niveau, tous les index de recherche en texte intégral ont le suivi des modifications désactivé et aucune analyse n'est démarrée automatiquement. Le catalogue reste vide tant que vous n'avez pas procédé manuellement à une alimentation complète, au terme de la mise à niveau.  
  
##  <a name="considerations-for-choosing-a-full-text-upgrade-option"></a><a name="Choosing_Upgade_Option"></a> Considérations relatives au choix d’une option de mise à niveau de texte intégral  

Au moment de choisir l'option de mise à niveau pour votre mise à niveau, tenez compte des éléments suivants :  
  
- Avez-vous besoin de cohérence dans les résultats de la requête ?  
  
   [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] installe de nouveaux analyseurs lexicaux pour la recherche en texte intégral et sémantique. Les analyseurs lexicaux sont utilisés au moment de l'indexation et au moment de la requête. Si vous ne reconstruisez pas les catalogues de texte intégral, vos résultats de recherche peuvent être incohérents. Si vous exécutez une requête de texte intégral qui recherche une expression qui est divisée différemment par l'analyseur lexical dans une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l'analyseur lexical actuel, une ligne ou un document contenant l'expression peut ne pas être extrait. Cela est dû au fait que les expressions indexées ont été divisées à l'aide d'une logique différente de celle de la requête utilise. La solution consiste à réalimenter (reconstruire) les catalogues de texte intégral avec les nouveaux analyseurs lexicaux afin que le temps d'indexation et le comportement de cette requête soient identiques. Vous pouvez choisir l'option Reconstruire pour y parvenir, ou vous pouvez reconstruire manuellement après le choix de l'option Importer.  
  
- Certains index de recherche en texte intégral ont-ils été construits sur la base de colonnes clés de texte intégral de type Integer ?  
  
   La reconstruction effectue, dans quelques cas, des optimisations internes qui améliorent le performances des requêtes de l'index de recherche en texte intégral mis à niveau. Spécifiquement, si vous avez des catalogues de texte intégral qui contiennent des index de recherche en texte intégral dont la colonne clé de texte intégral de la table de base correspond à un type de données Integer, la reconstruction permet d'obtenir une performance idéale des requêtes de texte intégral après la mise à niveau. Nous recommandons vivement que vous utilisiez l'option **Reconstruire** dans ce cas.  
  
   > [!NOTE]  
   > Pour les index de texte intégral, nous recommandons que la colonne servant de clé de texte intégral corresponde à un type de données Integer. Pour plus d’informations, consultez [Améliorer les performances des index de recherche en texte intégral](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md).  
  
- Quelle est la priorité pour obtenir votre instance de serveur en ligne ?  
  
   L'importation ou la reconstruction pendant la mise à niveau mobilise beaucoup de ressources processeur, ce qui retarde la mise à niveau et en ligne du reste de l'instance serveur. Si le fait d'avoir l'instance de serveur en ligne dès que possible est important et si vous êtes disposé à exécuter une alimentation manuelle après la mise à niveau, la **réinitialisation** est appropriée.  
  
## <a name="ensure-consistent-query-results-after-importing-a-full-text-index"></a>Garantie de résultats de requête cohérents après l’importation d’un index de recherche en texte intégral  

Si un catalogue de texte intégral a été importé lors de la mise à niveau d’une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], des discordances entre la requête et le contenu de l’index de recherche en texte intégral peuvent se produire en raison de différences dans le comportement des analyseurs lexicaux anciens et nouveaux. Dans ce cas, pour garantir une correspondance totale entre requêtes et contenu d'index de recherche en texte intégral, choisissez l'une des options suivantes :  
  
- reconstruisez le catalogue de texte intégral contenant l’index de recherche en texte intégral ([ALTER FULLTEXT CATALOG](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)*nom_catalogue* REBUILD) ;  
  
- publiez un FULL POPULATION sur l’index de recherche en texte intégral ([ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md) ON *nom_table* START FULL POPULATION).  
  
 Pour plus d’informations sur les analyseurs lexicaux, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
## <a name="upgrade-noise-word-files-to-stoplists"></a>Mise à niveau des fichiers de mots parasites vers des listes de mots vides

Lorsqu'une base de données est mise à niveau à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], les fichiers de mots parasites ne sont plus utilisés. Toutefois, les anciens fichiers de mots parasites sont stockés dans le dossier `FTDATA\ FTNoiseThesaurusBak` et vous pouvez les utiliser ultérieurement lors de la mise à jour ou de la génération des listes de mots vides correspondantes [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
Après avoir effectué une mise à niveau de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]:  
  
- Si vous n'avez jamais ajouté, modifié ou supprimé des fichiers de mots parasites de votre installation de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], la liste de mots vides système doit correspondre à vos besoins.  
  
- Si vos fichiers de mots parasites ont été modifiés dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], ces changements sont perdus pendant la mise à niveau. Pour recréer ces mises à jour, vous devez recréer manuellement ces changements dans la liste de mots vides correspondante. Pour plus d’informations, consultez [ALTER FULLTEXT STOPLIST &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-stoplist-transact-sql.md).  
  
- Si vous ne souhaitez pas appliquer de mots vides à vos index de recherche en texte intégral (par exemple, si vous avez supprimé ou effacé vos fichiers de mots parasites dans votre installation [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]), vous devez désactiver la liste de mots vides pour chaque index de recherche en texte intégral mis à niveau. Exécutez l’instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante (en remplaçant *database* par le nom de la base de données mise à niveau et *table* par le nom de la *table*) :  
  
    ```  
    Use database;   
    ALTER FULLTEXT INDEX ON table  
       SET STOPLIST OFF;  
    GO  
    ```  
  
     La clause STOPLIST OFF supprime le filtrage par mot vide et déclenche une alimentation de la table, sans filtrer les mots considérés comme des mots parasites.  
  
## <a name="backup-and-imported-full-text-catalogs"></a>Sauvegarde et catalogues de texte intégral importés

Pour les catalogues de texte intégral qui sont reconstruits ou réinitialisés pendant la mise à niveau (et pour les nouveaux catalogues de texte intégral), le catalogue de texte intégral est un concept logique et ne réside pas dans un groupe de fichiers. Par conséquent, pour sauvegarder un catalogue de texte intégral, vous devez identifier tous les groupes de fichiers contenant un index de recherche en texte intégral du catalogue et les sauvegarder un par un. Pour plus d’informations, consultez [Sauvegarder et restaurer des catalogues et des index de recherche en texte intégral](../../relational-databases/search/back-up-and-restore-full-text-catalogs-and-indexes.md).  
  
 Pour les catalogues de texte intégral importés à partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], le catalogue de texte intégral est encore un fichier de base de données dans son propre groupe de fichiers. Le processus de sauvegarde [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] pour les catalogues de texte intégral s'applique encore mais le service MSFTESQL n'existe pas dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur le processus [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , consultez [Sauvegarde et restauration d’un catalogue de texte intégral](/previous-versions/sql/sql-server-2005/ms142511(v=sql.90)) dans la documentation en ligne de SQL Server 2005.  
  
##  <a name="migrating-full-text-indexes-when-upgrading-a-database"></a><a name="Upgrade_Db"></a> Migration d’index de recherche en texte intégral lors de la mise à niveau d’une base de données

 Les fichiers de base de données et les catalogues de texte intégral d’une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être mis à niveau vers une instance existante en joignant ou restaurant les données ou bien en utilisant l’Assistant Copie de base de données. [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Les index de recherche en texte intégral sont, le cas échéant, importés, réinitialisés ou reconstruits. La propriété de serveur **upgrade_option** détermine l’option de mise à niveau de texte intégral que l’instance de serveur utilise pendant ces mises à niveau de base de données.  
  
 Après avoir joint, restauré ou copié une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] dans une instance plus récente, la base de données est immédiatement disponible et elle est ensuite automatiquement mise à niveau. Selon le volume de données indexé, l'importation peut prendre plusieurs heures et la reconstruction jusqu'à dix fois plus longtemps. Notez également que lorsque l'option de mise à niveau est Importer, si le catalogue de texte intégral n'est pas disponible, les index de recherche en texte intégral associés sont reconstruits.  
  
 **Pour modifier le comportement de mise à niveau de texte intégral sur une instance de serveur**
  
- [!INCLUDE[tsql](../../includes/tsql-md.md)]: Utilisez l’action **upgrade\_option** de [sp\_fulltext\_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
  
- [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] **:** utilisez **l’option de mise à niveau de texte intégral** de la boîte de dialogue **Propriétés du serveur** . Pour plus d’informations, consultez [Gérer et surveiller la recherche en texte intégral pour une instance de serveur](../../relational-databases/search/manage-and-monitor-full-text-search-for-a-server-instance.md).  
  
##  <a name="considerations-for-restoring-a-ssversion2005-full-text-catalog"></a><a name="Considerations_for_Restore"></a> Considérations relatives à la restauration d’un catalogue de texte intégral [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]
  
Une méthode de mise à niveau de données de texte intégral d’une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] est de restaurer une sauvegarde de la base de données complète sur une instance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] plus récente.  
  
 En important un catalogue [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , vous pouvez sauvegarder et restaurer la base de données et le fichier catalogue. Le comportement est identique à celui de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]:  
  
- La sauvegarde de base de données complète inclut alors le catalogue de texte intégral. Pour faire référence au catalogue de texte intégral, utilisez son nom de fichier [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , sysft_+*nom-catalogue*.  
  
- Si le catalogue de texte intégral est hors connexion, la sauvegarde échouera.  
  
 Pour plus d’informations sur la sauvegarde et la restauration de catalogues de texte intégral [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , consultez [Sauvegarde et restauration de catalogues de texte intégral](./back-up-and-restore-full-text-catalogs-and-indexes.md) et [Restauration et sauvegarde de fichiers et catalogues de texte intégral](/previous-versions/sql/sql-server-2008-r2/ms190643(v=sql.105))dans la documentation en ligne de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] .  
  
 Quand la base de données est restaurée sur une instance de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] plus récente, un nouveau fichier de base de données est créé pour le catalogue de texte intégral. Le nom par défaut de ce fichier est ftrow_ *nom-catalogue*.ndf. Par exemple, si *nom-catalogue* est `cat1`, le nom par défaut du fichier de base de données [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] serait `ftrow_cat1.ndf`. En revanche, si le nom par défaut est déjà utilisé dans le répertoire cible, le nouveau fichier de base de données serait nommé `ftrow_`*nom-catalogue*`{`*GUID*`}.ndf`, où *GUID* est l’identificateur global unique du nouveau fichier.  
  
 Après avoir importé les catalogues, les fichiers **sys.database_files** et **sys.master_file** sont mis à jour pour supprimer les entrées de catalogue et la colonne de **chemin d’accès** dans **sys.fulltext_catalogs** a la valeur Null.  
  
 **Pour sauvegarder une base de données**  
  
- [Sauvegardes complètes de bases de données &#40;SQL Server&#41;](../../relational-databases/backup-restore/full-database-backups-sql-server.md)  
  
- [Sauvegardes des journaux de transactions &#40;SQL Server&#41;](../../relational-databases/backup-restore/transaction-log-backups-sql-server.md) (mode de récupération complète uniquement)  
  
 **Pour restaurer une sauvegarde de la base de données**  
  
- [Restaurations complètes de bases de données &#40;mode de récupération simple&#41;](../../relational-databases/backup-restore/complete-database-restores-simple-recovery-model.md)  
  
- [Restaurations complètes de bases de données &#40;mode de récupération complète&#41;](../../relational-databases/backup-restore/complete-database-restores-full-recovery-model.md)  
  
### <a name="example"></a> Exemple

 L'exemple suivant utilise la clause MOVE dans l'instruction [RESTORE](../../t-sql/statements/restore-statements-transact-sql.md) , afin de restaurer une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] nommée `ftdb1`. Les fichiers catalogue, le journal et la base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] sont déplacés vers les nouveaux emplacements sur l'instance de serveur [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] , comme suit :  
  
- Le fichier de base de données, `ftdb1.mdf`, est déplacé vers `C:\Program Files\Microsoft SQL Server\MSSQL.1MSSQL13.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf`.  
  
- Le fichier journal, `ftdb1_log.ldf`, est déplacé vers un répertoire de journal sur votre lecteur de disque journal, *lecteur_journal*`:\`*répertoire_journal*`\ftdb1_log.ldf`.  
  
- Les fichiers catalogue qui correspondent au catalogue `sysft_cat90` sont déplacés vers `C:\temp`. Après avoir importé les index de recherche en texte intégral, ceux-ci sont automatiquement placés dans un fichier de base de données, C:\ftrow_sysft_cat90.ndf, et C:\temp est supprimé.  
  
```  
RESTORE DATABASE [ftdb1] FROM  DISK = N'C:\temp\ftdb1.bak' WITH  FILE = 1,  
   MOVE N'ftdb1' TO N'C:\Program Files\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQL\DATA\ftdb1.mdf',  
    MOVE N'ftdb1_log' TO N'log_drive:\log_directory\ftdb1_log.ldf',  
    MOVE N'sysft_cat90' TO N'C:\temp';  
```  
  
##  <a name="attaching-a-sql-server-2005-database"></a><a name="Attaching_2005_ft_catalogs"></a> Joindre une base de données SQL Server 2005

Dans [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les versions ultérieures, un catalogue de texte intégral est un concept logique qui renvoie à un groupe d'index de recherche en texte intégral. Le catalogue de texte intégral est un objet virtuel qui n'appartient à aucun groupe de fichiers. Toutefois, quand vous joignez une base de données [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] qui contient des fichiers catalogue de texte intégral à une instance de serveur [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] plus récente, les fichiers catalogue sont joints à partir de leur emplacement précédent avec les autres fichiers de base de données, qui sont les mêmes que dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
L'état de chaque catalogue de texte intégral attaché sur [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] est le même que lors du détachement de la base de données de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Si le remplissage de l'index de recherche en texte intégral est interrompu par l'opération de détachement, le remplissage reprend sur [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)], et l'index de recherche en texte intégral devient disponible pour la recherche en texte intégral.  
  
 Si [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ne parvient pas à trouver un fichier catalogue de texte intégral ou si ce dernier a été déplacé durant l'opération d'attachement sans spécification du nouvel emplacement, le comportement dépend de l'option de mise à niveau de texte intégral sélectionnée. Si l’option de mise à niveau de texte intégral a la valeur **Importer** ou **Reconstruire**, le catalogue de texte intégral attaché est reconstruit. Si l’option de mise à niveau de texte intégral a la valeur **Réinitialiser**, le catalogue de texte intégral attaché est réinitialisé.  
  
 Pour plus d’informations sur le détachement et l’attachement d’une base de données, consultez [Attacher et détacher une base de données &#40;SQL Server&#41;](../../relational-databases/databases/database-detach-and-attach-sql-server.md), [CREATE DATABASE &#40;SQL Server Transact-SQL&#41;](../../t-sql/statements/create-database-transact-sql.md), [sp_attach_db](../../relational-databases/system-stored-procedures/sp-attach-db-transact-sql.md) et [sp_detach_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-detach-db-transact-sql.md).  
  
## <a name="see-also"></a>Voir aussi

 [Commencer à utiliser la recherche en texte intégral](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurer et gérer des filtres pour la recherche](../../relational-databases/search/configure-and-manage-filters-for-search.md)  
