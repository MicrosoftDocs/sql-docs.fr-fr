---
title: Notes de publication de SQL Server 2012 Service Pack | Microsoft Docs
description: Cet article contient les notes de publication de synthèse des quatre Service Packs pour SQL Server 2012. Chaque Service Pack s’additionne aux Service Packs précédents.
ms.prod: sql
ms.technology: release-landing
ms.custom: ''
ms.date: 07/22/2020
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 67cb8b3e-3d82-47f4-840d-0f12a3bff565
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: 83a7828c93d6863e99d88d3cce45654e8f73abbd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341124"
---
# <a name="sql-server-2012-service-pack-release-notes"></a>Notes de publication de SQL Server 2012 Service Pack
[!INCLUDE [SQL Server Azure SQL Database ](../includes/applies-to-version/sqlserver.md)]
Cette rubrique contient les notes de publication de synthèse des quatre Service Packs pour SQL Server 2012. Chaque Service Pack s’additionne aux Service Packs précédents.

Les Service Packs sont disponibles en ligne uniquement et non sur le support d’installation et peuvent être téléchargés comme suit :
- [SQL Server 2012 SP4](https://go.microsoft.com/fwlink/?linkid=846937)
- [SQL Server 2012 SP3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information)
- [SQL Server 2012 SP2](https://support.microsoft.com/KB/2958429)
- [SQL Server 2012 SP1](https://go.microsoft.com/fwlink/p/?LinkID=268158)

## <a name="service-pack-4-release-notes"></a>Notes de publication de Service Pack 4

### <a name="download-pages"></a>Pages de téléchargement

- [SQL Server 2012 SP4 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56041)
- [Installation du correctif SQL Server 2012 SP4](https://www.microsoft.com/download/details.aspx?id=56040)
- [SQL Server 2012 SP4 Express](https://www.microsoft.com/download/details.aspx?id=56042)


### <a name="performance-and-scale-improvements"></a>Améliorations des performances et des capacités
- **Amélioration de la procédure de nettoyage de l’agent de distribution** : une base de données de distribution surdimensionnée provoquait une situation de blocage et d’interblocage. Cette amélioration vise à éviter certains scénarios de blocage ou d’interblocage. 
- **Mise à l’échelle dynamique des objets mémoire** : les objets mémoire sont partitionnés de façon dynamique en fonction du nombre de nœuds et de cœurs sur le matériel actuel. Le but de la promotion dynamique est d’empêcher les goulots d’étranglement potentiels et de partitionner automatiquement un objet mémoire thread-safe. Les objets mémoire non partitionnés peuvent être promus de façon dynamique pour être partitionnés par nœud. Le nombre de partitions est identique au nombre de nœuds NUMA. Les objets mémoire partitionnés par nœud peuvent ensuite être promus pour être partitionnés par UC, où le nombre de partitions est identique au nombre d’UC.
- **Autoriser un espace > 8 To pour le pool de mémoires tampons** : espace d’adressage virtuel de 128 To disponible pour le pool de mémoires tampons.
- **Nettoyage du suivi des modifications** : amélioration des performances et de l’efficacité du nettoyage du suivi des modifications pour les tables de suivi des modifications. 

### <a name="supportability-and-diagnostics-improvements"></a>Améliorations de la prise en charge et des diagnostics
- **Prise en charge des vidages complets pour les agents de réplication** : si les agents de réplication rencontrent une exception non gérée, le comportement par défaut actuel est de créer un vidage minimal des symptômes de l’exception. Le comportement par défaut nécessite une procédure de dépannage complexe pour les exceptions non gérées. SP4 introduit une nouvelle clé de Registre, qui prend en charge la création d’un vidage complet pour les agents de réplication.
- **Amélioration des diagnostics dans la sortie XML du plan d’exécution de requêtes** : la sortie XML du plan d’exécution de requêtes contient maintenant des informations sur les indicateurs de trace activés, les fractions de mémoire pour la jointure de boucles imbriquées optimisée, le temps processeur et le temps écoulé. 
- **Amélioration de la corrélation entre les diagnostics XE et DMV** : utilisation des champs query_hash et query_plan_hash pour identifier une requête de façon unique. DMV les définit comme des champs varbinary(8), tandis que XEvent les définit comme des champs UINT64. Étant donné que SQL Server n’utilise pas de valeurs bigint non signées, le cast ne fonctionne pas toujours. Cette amélioration introduit de nouvelles colonnes d’action et de filtre XEvent équivalentes aux champs query_hash et query_plan_hash. La différence est qu’elles sont définies comme des colonnes INT64, ce qui permet une meilleure corrélation des requêtes entre XE et DMV. 
- **Amélioration des diagnostics d’allocation et d’utilisation de la mémoire** : ajout du nouvel XEvent query_memory_grant_usage (transféré de SQL Server 2016 SP1).
- **Ajout du suivi de protocole dans les étapes de négociation SSL** : ajout d’informations de trace des bits pour la réussite ou l’échec de la négociation, telles que le protocole, etc. Ces informations peuvent aider à résoudre certains problèmes de connectivité, par exemple, dans des scénarios de déploiement de TLS 1.2
- **Définition du niveau de compatibilité correct pour la base de données de distribution** : après l’installation du Service Pack, le niveau de compatibilité de la base de données de distribution était changé à 90. Le changement de niveau était dû à un problème avec la procédure stockée sp_vupgrade_replication. Cette procédure stockée a été modifiée afin de définir le niveau de compatibilité correct pour la base de données de distribution. 
- **Nouvelle commande DBCC pour cloner une base de données** : la nouvelle commande DBCC de clonage d’une base de données permet aux utilisateurs avec pouvoir tels que les CSS de résoudre les problèmes rencontrés avec des bases de données de production existantes en clonant le schéma et les métadonnées, sans inclure les données. L’appel est effectué avec la commande DBCC clonedatabase ('nom_base_de_données_source', 'nom_base_de_données_clone'). Les bases de données clonées ne doivent pas être utilisées dans les environnements de production. Pour savoir si une base de données a été générée à partir d’un appel à une base de données clonée, utilisez la commande suivante, select DATABASEPROPERTYEX('clonedb', 'isClone'). La valeur renvoyée 1 est true et la valeur renvoyée 0 est false. 
- **Informations sur les fichiers TempDB et la taille des fichiers dans le journal des erreurs SQL Server** : si la taille et la croissance automatique sont différentes pour les fichiers de données TempDB au démarrage, le nombre de fichiers est indiqué et un avertissement est généré.
- **Messages de prise en charge d’IFI dans le journal des erreurs SQL Server** : dans le journal des erreurs, ils indiquent si l’initialisation instantanée des fichiers de base de données est activée ou désactivée.
- **Nouvelle DMF pour remplacer la commande DBCC INPUTBUFFER** : la nouvelle fonction de gestion dynamique, sys.dm_input_buffer, utilise le paramètre session_id et remplace la commande DBCC INPUTBUFFER
- **Amélioration des événements XEvent liés à un échec du routage en lecture seule pour un groupe de disponibilité** : actuellement, l’événement XEvent read_only_rout_fail est déclenché uniquement si aucun des serveurs de la liste de routage existante n’est disponible pour les connexions. Cette amélioration apporte des informations supplémentaires pour vous aider à résoudre ce type de problème. Elle étend également les points de code où un événement XEvent peut être déclenché. 
- **Amélioration de la gestion de Service Broker avec basculement du groupe de disponibilité** : actuellement, quand Service Broker est activé sur des bases de données du groupe de disponibilité et qu’il y a un basculement du groupe de disponibilité, toutes les connexions Service Broker créées à partir du réplica principal restent ouvertes. L’amélioration ferme toutes les connexions ouvertes pendant un basculement du groupe de disponibilité.
- **Partitionnement de NUMA logiciel automatique** : dans SQL Server 2014 SP2, le partitionnement de [NUMA logiciel](../database-engine/configure-windows/soft-numa-sql-server.md) automatique est utilisé quand l’indicateur de trace 8079 est activé au niveau du serveur. Quand l’indicateur de trace 8079 est activé au démarrage, SQL Server 2014 SP2 vérifie la disposition matérielle et configure automatiquement le NUMA logiciel sur les systèmes ayant 8 UC ou plus par nœud NUMA. Le comportement du NUMA logiciel automatique est compatible Hyperthread (processeur logique/HT). Le partitionnement et la création de nœuds supplémentaires permettent de dimensionner le traitement en arrière-plan en augmentant le nombre d’écouteurs, le nombre d’instances, ainsi que les capacités réseau et de chiffrement. Il est recommandé de tester les performances de la charge de travail avec le NUMA logiciel automatique avant de mettre en œuvre cette fonctionnalité dans un environnement de production.

## <a name="service-pack-3-release-notes"></a>Notes de publication de Service Pack 3

### <a name="download-pages"></a>Pages de téléchargement
- [SQL Server 2012 SP3 Feature Pack](https://go.microsoft.com/fwlink/?linkid=615935)
- [SQL Server 2012 SP3 Express](https://go.microsoft.com/fwlink/?linkid=692144)

Pour plus d’informations permettant d’identifier l’emplacement et le nom du fichier à télécharger en fonction de la version actuellement installée, consultez la section « Sélectionner le fichier approprié à télécharger » dans [Informations sur SQL Server 2012 Service Pack 3](https://support.microsoft.com/help/3072779/sql-server-2012-service-pack-3-release-information).

## <a name="service-pack-2-release-notes"></a>Notes de publication de Service Pack 2
  
### <a name="download-pages"></a>Pages de téléchargement 
- [SQL Server 2012 SP2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=43340)
- [SQL Server Express 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401007)

Utilisez le tableau suivant pour identifier l'emplacement et le nom du fichier à télécharger en fonction de la version installée actuellement. Les pages de téléchargement indiquent la configuration système requise et contiennent des instructions d'installation de base.  

|Si la version actuellement installée est...|Vous souhaitez…|Téléchargez et installez...|  
|---|---|---|   
|Installations 32 bits :|||  
|Une version 32 bits de n’importe quelle édition de SQL Server 2012|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 SP2|**SQLServer2012SP2-KB2958429-** <arch> **-** <lang id> **.exe** à partir de la [page de téléchargement de SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Une version 32 bits de SQL Server 2012 RTM Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 Express SP2|**SQLEXPR_** <arch>**8**<lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 32 bits de seulement le client et les outils de gestion pour SQL Server 2012 (y compris SQL Server 2012 Management Studio)|Effectuer la mise à niveau du client et des outils de gestion vers la version 32 bits de SQL Server 2012 SP2|**SQLEXPRWT_** <arch>**8**<lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 32 bits de SQL Server 2012 Management Studio Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 32 bits d'une édition quelconque de SQL Server 2012 et une version 32 bits du client et des outils de gestion (y compris SQL Server 2012 RTM Management Studio)|Effectuer la mise à niveau de tous les produits vers la version 32 bits de SQL Server 2012 SP2|**SQLEXPRADV_** <arch> **_** <lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express.](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 32 bits d'un ou plusieurs outils du [Feature Pack Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=56041) ou du [Feature Pack Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|Effectuer la mise à niveau des outils vers la version 32 bits du Feature Pack Microsoft SQL Server 2012 SP2|Un ou plusieurs outils à partir de la [page de téléchargement du Feature Pack Microsoft SQL Server 2012 SP2](https://www.microsoft.com/download/details.aspx?id=43340)|  
|Installations 64 bits :|||  
|Une version 64 bits d'une édition quelconque de SQL Server 2012|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP2|SQLServer2012SP2-KB2958429-<arch>-<langid>.exe à partir de la [page de téléchargement de SQL Server 2012 SP2](https://go.microsoft.com/fwlink/?LinkID=401006)|  
|Version 64 bits de SQL Server 2012 RTM Express|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP2|**SQLEXPR_** <arch>**8**<lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 64 bits uniquement du client et des outils de gestion de SQL Server 2012 (y compris SQL Server 2012 Management Studio)|Effectuer la mise à niveau du client et des outils de gestion vers la version 64 bits de SQL Server 2012 SP2|**SQLEXPRWT_** <arch>**8**<lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Version 64 bits de SQL Server 2012 Management Studio Express|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP2 Management Studio Express|**SQLManagementStudio_** <arch> **_** <lang> **.msi** à partir de la [page de téléchargement de SQL Server 2012 SP2 Express](https://go.microsoft.com/fwlink/?LinkID=401007)|  
|Une version 64 bits d'un ou plusieurs outils du [Feature Pack Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=56041) ou du [Feature Pack Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|Effectuer la mise à niveau des outils vers la version 64 bits du Feature Pack Microsoft SQL Server 2012 SP2|Un ou plusieurs outils à partir de la [page de téléchargement du Feature Pack Microsoft SQL Server 2012 SP2](https://www.microsoft.com/download/details.aspx?id=43340)|   


## <a name="service-pack-1-release-notes"></a>Notes de publication de Service Pack 1

### <a name="download-pages"></a>Pages de téléchargement  
- [SQL Server 2012 SP1 Feature Pack](https://go.microsoft.com/fwlink/p/?LinkID=268158)
- [SQL Server Express 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35579)


Utilisez le tableau suivant pour déterminer le fichier à télécharger et installer. Vérifiez que vous disposez de la configuration requise avant d'installer le Service Pack. La configuration requise est disponible sur les pages de téléchargement liées dans le tableau.  

|Si la version actuellement installée est...|Vous souhaitez…|Téléchargez et installez...|  
|---|---|---|  
|**Installations 32-bits :**|||  
|Une version 32 bits de n’importe quelle édition de SQL Server 2012|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe à partir d’ [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Une version 32 bits de SQL Server 2012 RTM Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 Express SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe à partir d’ [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Une version 32 bits de seulement le client et les outils de gestion pour SQL Server 2012 (y compris SQL Server 2012 Management Studio)|Effectuer la mise à niveau du client et des outils de gestion vers la version 32 bits de SQL Server 2012 SP1|SQLManagementStudio_x86_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Une version 32 bits de SQL Server 2012 Management Studio Express|Effectuer la mise à niveau vers la version 32 bits de SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x86_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Une version 32 bits d'une édition quelconque de SQL Server 2012 **et** une version 32 bits du client et des outils de gestion (y compris SQL Server 2012 RTM Management Studio)|Effectuer la mise à niveau de tous les produits vers la version 32 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x86-ENU.exe à partir d’ [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Une version 32 bits d'un ou de plusieurs outils du [Feature Pack Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=44272)|Effectuer la mise à niveau des outils vers la version 32 bits du Feature Pack Microsoft SQL Server 2012 SP1|Un ou plusieurs fichiers du [Feature Pack Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Pas d’installation 32 bits de SQL Server 2012|Installer SQL Server 2012 32 bits, y compris SP1 (nouvelle instance avec SP1 préinstallé)|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **et** SQLServer2012SP1-FullSlipstream-x86-ENU.box à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Pas d’installation 32 bits de SQL Server 2012 Management Studio|Installer la version 32 bits de SQL Server 2012 Management Studio, y compris SP1|SQLManagementStudio_x86_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Pas de version 32 bits de SQL Server 2012 RTM Express|Install la version 32 bits de SQL Server 2012 Express, y compris SP1|SQLEXPR32_x86_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkId=267905)|  
|Une installation 32 bits de **SQL Server 2008** ou de **SQL Server 2008 R2**|**Mise à niveau sur place** vers la version 32 bits de SQL Server 2012, y compris SP1|SQLServer2012SP1-FullSlipstream-x86-ENU.exe **et** SQLServer2012SP1-FullSlipstream-x86-ENU.box à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|**Installations 64-bits :**|||  
|Une version 64 bits d'une édition quelconque de SQL Server 2012|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Version 64 bits de SQL Server 2012 RTM Express|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Une version 64 bits uniquement du client et des outils de gestion de SQL Server 2012 (y compris SQL Server 2012 Management Studio)|Effectuer la mise à niveau du client et des outils de gestion vers la version 64 bits de SQL Server 2012 SP1|SQLManagementStudio_x64_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Version 64 bits de SQL Server 2012 Management Studio Express|Effectuer la mise à niveau vers la version 64 bits de SQL Server 2012 SP1 Management Studio Express|SQLManagementStudio_x64_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Une version 64 bits d'une édition quelconque de SQL Server 2012 **et** une version 64 bits du client et des outils de gestion (y compris SQL Server 2012 RTM Management Studio)|Effectuer la mise à niveau de tous les produits vers la version 64 bits de SQL Server 2012 SP1|SQLServer2012SP1-KB2674319-x64-ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Une version 64 bits d'un ou de plusieurs outils du [Feature Pack Microsoft SQL Server 2012 RTM](https://www.microsoft.com/download/details.aspx?id=44272)|Effectuer la mise à niveau des outils vers la version 64 bits du Feature Pack Microsoft SQL Server 2012 SP1|Un ou plusieurs fichiers du [Feature Pack Microsoft SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575)|  
|Aucune installation version 64 bits de SQL Server 2012|Installer une version 64 bits de Server 2012 notamment SP1 (nouvelle instance avec la version SP1 préinstallée)|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **et** SQLServer2012SP1-FullSlipstream-x64-ENU.box à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  
|Aucune installation version 64 bits de SQL Server 2012 Management Studio|Installer la version 64 bits de SQL Server 2012 Management Studio y compris le SP1|SQLManagementStudio_x64_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|No version 64 bits de SQL Server 2012 RTM Express|Installer la version 64 bits de SQL Server 2012 Express y compris le SP1|SQLEXPR_x64_ENU.exe à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=267905)|  
|Une installation 64 bits de **SQL Server 2008** ou de **SQL Server 2008 R2**|**Mise à niveau sur place** vers la version 64 bits de SQL Server 2012, y compris SP1|SQLServer2012SP1-FullSlipstream-x64-ENU.exe **et** SQLServer2012SP1-FullSlipstream-x64-ENU.box à partir d' [ici](https://go.microsoft.com/fwlink/p/?LinkID=268158)|  

### <a name="known-issues-fixed-in-this-service-pack"></a>Problèmes connus résolus dans ce Service Pack  
Pour obtenir la liste complète des bogues et problèmes connus corrigés dans ce Service Pack, consultez cet [article de la Base de connaissances](https://support.microsoft.com/kb/2674319).   

### <a name="reinstalling--instances-of-sql-server-failover-cluster-fails-if-you-use-the-same-ip-address"></a>Échec de la réinstallation des instances de cluster de basculement SQL Server si vous utilisez la même adresse IP  
**Problème :** si vous spécifiez une adresse IP incorrecte lors de l’installation d’une instance de cluster de basculement SQL Server, l’installation échoue. Après avoir désinstallé l'instance en échec, si vous tentez de réinstaller l'instance de cluster de basculement SQL Server avec le même nom d'instance et une adresse IP correcte, l'installation échoue. Cet échec est dû au groupe de ressources dupliqué conservé par l'installation précédente.  
  
**Solution de contournement :** pour résoudre ce problème, utilisez un autre nom d’instance durant la réinstallation, ou supprimez manuellement le groupe de ressources avant la réinstallation. Pour plus d'informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement SQL Server](failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md). 
  
### <a name="analysis-services-and-powerpivot"></a>Analysis Services et PowerPivot  
  
##### <a name="powerpivot-configuration-tool-does-not-create-the-powerpivot-gallery"></a>L’outil de configuration PowerPivot ne crée pas la Galerie PowerPivot  
**Problème :** l’outil de configuration PowerPivot configure un site d’équipe, et la Galerie PowerPivot n’est donc pas créée.  
  
**Solution de contournement :** créez une application (bibliothèque).  
  
1.  Vérifiez que la fonctionnalité de collection de site **Fonctionnalité d'intégration PowerPivot pour les collections de sites** est active.  
  
2.  Dans la page **Contenu du site** d'un site existant, cliquez sur **Ajouter une application**.  
  
3.  Cliquez sur **Galerie PowerPivot**.  
  
#### <a name="to-use-powerpivot-for-excel-with-excel-2013-you-must-use-the-add-in-that-is-installed-with-excel"></a>Pour utiliser PowerPivot pour Excel avec Excel 2013, vous devez utiliser le complément installé avec Excel  
**Problème :** avec Office 2010, PowerPivot pour Excel est un complément autonome qui peut être téléchargé à partir de [https://www.microsoft.com/bi/powerpivot.aspx](https://www.microsoft.com/bi/powerpivot.aspx). Sinon, vous pouvez également le télécharger depuis le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=29074). Notez qu’il existe deux versions du complément PowerPivot disponibles en téléchargement : une qui est fournie avec SQL Server 2008 R2, et une autre qui est fournie avec SQL Server 2012. Toutefois, pour Office 2013, PowerPivot pour Excel est fourni avec Office et s'installe en même temps qu'Excel. Bien que les versions SQL Server 2008 R2 et SQL Server 2012 de PowerPivot pour Excel 2010 ne soient pas compatibles avec Excel 2013, vous pouvez toujours installer PowerPivot pour Excel 2010 sur votre ordinateur client si vous souhaitez exécuter Excel 2010 en parallèle d'Excel 2013. En d'autres termes, les deux versions d'Excel peuvent coexister, de même que les compléments PowerPivot correspondants.  
  
**Solution de contournement :** pour utiliser PowerPivot pour Excel 2013, vous devez activer le complément COM. Dans Excel 2013, sélectionnez **Fichier** | **Options** | **Compléments**. Dans la liste déroulante **Gérer** , sélectionnez **Compléments COM** , puis cliquez sur **OK**. Dans **Compléments COM**, sélectionnez **Microsoft Office PowerPivot pour Excel 2013** , puis cliquez sur **OK**.  
  
### <a name="reporting-services"></a>Reporting Services  
  
#### <a name="install-and-configure-sharepoint-server-2013-prior-to-installing-reporting-services"></a>Installer et configurer SharePoint Server 2013 avant d’installer Reporting Services  
**Problème :** effectuez les opérations suivantes **avant** d’installer SQL Server Reporting Services (SSRS).  
  
1.  Exécutez l'Outil de préparation des produits SharePoint 2013.  
  
2.  Installez SharePoint Server 2013.  
  
3.  Exécutez l'Assistant Configuration de produit SharePoint 2013 ou terminez un ensemble équivalent d'étapes de configuration pour configurer la batterie SharePoint.  
  
**Solution de contournement :**  si vous avez installé le mode SharePoint de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] avant de configurer la batterie de serveurs SharePoint, la solution de contournement dépend des autres composants installés.  
  
#### <a name="power-view-in-sharepoint-server-2013-requires-microsoftanalysisservicesspclientdll"></a>Power View dans SharePoint Server 2013 nécessite Microsoft.AnalysisServices.SPClient.dll  
**Problème :** [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] n’installe pas un composant requis, **Microsoft.AnalysisServices.SPClient.dll**. Si vous installez SharePoint Server 2013 Preview et [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)][!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] en mode SharePoint, mais que vous ne téléchargez pas et n’installez pas le package de programme d’installation de PowerPivot pour SharePoint 2013, **spPowerPivot.msi** , Power View ne fonctionne pas et montre les symptômes suivants.  
  
**Symptômes :** Quand vous tentez de créer un rapport Power View, un message d’erreur semblable au suivant s’affiche :  
  
-   « Impossible de créer une connexion à la source de données... »  
  
Les détails de l'erreur interne contiennent un message similaire au message suivant :  
  
-   « La valeur Principal SharePoint n'est pas prise en charge pour la propriété de chaîne de connexion Identité de l'utilisateur.  
  
**Solution de contournement :** installez le package d’installation PowerPivot pour SharePoint 2013 (**spPowerPivot.msi**) sur SharePoint Server 2013. Le package d'installation est disponible dans le cadre du Feature Pack [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] . Le Feature Pack peut être téléchargé en accédant au centre de téléchargement [!INCLUDE[msCoName](../includes/msconame-md.md)] à l'adresse [SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
#### <a name="power-view-sheets-in-a-powerpivot-workbook-are-deleted-after-a-scheduled-data-refresh"></a>Les feuilles Power View dans un classeur PowerPivot sont supprimées après les actualisations des données planifiées  
**Problème** : dans le complément PowerPivot pour SharePoint, l’utilisation de l’**actualisation planifiée des données** sur un classeur avec Power View a pour effet de supprimer les feuilles Power View.  
  
**Solution de contournement** : pour utiliser l’**actualisation planifiée des données** avec des classeurs Power View, créez un classeur PowerPivot qui est simplement le modèle de données. Créez un classeur distinct avec les feuilles Excel et Power View qui se lie au classeur PowerPivot avec le modèle de données. Seul le classeur PowerPivot avec le modèle de données doit être planifiée pour l'actualisation des données.  
  
### <a name="data-quality-services"></a>Data Quality Services  
  
#### <a name="dqs-available-in-the-incorrect-edition-of-sql-server-2012"></a>DQS disponible dans l’édition incorrecte de SQL Server 2012  
**Problème :** dans la version commerciale de [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], la fonctionnalité Data Quality Services (DQS) est disponible dans les éditions de SQL Server autres que les éditions Enterprise, Business Intelligence et Developer. Après avoir installé SQL Server 2012 SP1, DQS ne sera disponible que dans les éditions Enterprise, Business Intelligence et Developer.  
  
**Solution de contournement** : si vous utilisez DQS dans une édition non prise en charge, effectuez une mise à niveau vers une édition prise en charge ou supprimez la dépendance à cette fonctionnalité dans vos applications.  
  
### <a name="sql-server-express"></a>SQL Server Express  
  
#### <a name="full-version-of-sql-server-management-studio-available-in-sql-server-2012-express-sp1"></a>Version complète de SQL Server Management Studio disponible dans SQL Server Express 2012 SP1  
La version SQL Server 2012 Express Service Pack 1 (SP1) inclut la version complète de SQL Server 2012 Management Studio (qui était précédemment disponible uniquement sur le DVD SQL Server 2012) au lieu de SQL Server 2012 Management Studio Express. Pour télécharger et installer SQL Server 2012 Express SP1, consultez [SQL Server 2012 Express Service Pack 1](https://go.microsoft.com/fwlink/p/?linkid=267905).  
  
### <a name="change-data-capture-service-and-designer-for-oracle-by-attunity"></a>Change Data Capture Service et concepteur pour Oracle d’Attunity  
  
#### <a name="upgrading-the-cdc-service-and-designer"></a>Mise à niveau de CDC Service et du concepteur  
**Problème :** Si Change Data Capture Designer pour Oracle et Change Data Capture Service pour Oracle par Attunity sont installés sur votre ordinateur quand vous installez SQL Server 2012 SP1, ces composants ne sont pas mis à niveau lors de l’installation du SP1.  
  
**Solution de contournement :** Pour mettre à niveau les composants CDC vers la version la plus récente :  
  
1.  Téléchargez les fichiers .msi pour le service de capture de données modifiées pour Oracle par Attunity à partir de la [page de téléchargement du Feature Pack SQL Server 2012 SP1](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Exécutez le fichier .msi.  
  
### <a name="sql-server-data-tier-application-framework-dacfx"></a>SQL Server Data-Tier Application Framework (DACFx)  
**Prise en charge de la mise à niveau sur place**  
  
Cette version de l'Infrastructure d'application de couche Données (DACFx) prend en charge la mise à niveau de versions précédentes, de sorte qu'il n'est pas nécessaire de supprimer les installations DACFx précédentes avant d'effectuer la mise à niveau de cette version. Vous pouvez trouver de versions à venir de DACFx [ici](/previous-versions/sql/sql-server-2016/dn702988(v=sql.130)).  
  
**Prise en charge des index XML sélectifs**  
  
SQL Server 2012 SP1 inclut la prise en charge des [Index XML sélectifs (SXI)](../relational-databases/xml/selective-xml-indexes-sxi.md), une nouvelle fonctionnalité SQL Server qui fournit une nouvelle façon d'indexer les données des colonnes XML avec des performances et une efficacité supplémentaires.  
  
DACFx prend maintenant en charge des index SXI dans tous les scénarios DAC et tous les outils clients. SXI n'est pris en charge que dans cette version du SSDT. Les versions RTM et de septembre 2012 de SSDT ne prennent pas en charge SXI.  
  
**Prise en charge du format natif de données BCP**  
  
Précédemment, le format de données utilisé pour stocker les données de table à l'intérieur des packages DACPAC et BACPAC était JSON. Avec cette mise à jour, le format natif de données BCP est maintenant le format de persistance des données. Cette modification permet d'améliorer la fidélité du type de données SQL Server à DACFx notamment la prise en charge des types SQL_Variant ainsi que les performances de déploiement des données pour les bases de données à grande échelle.  
  
**Conservation de l'état de la contrainte de validation dans la création et déploiement des packages**  
  
Précédemment, DACFx ne conservait pas l'état (WITH CHECK / NOCHECK) des contraintes de vérification définies sur les tables dans le schéma de la base de données et elle ne allouait pas les informations à l'intérieur des DACPAC. Ce comportement pourrait conduire à des problèmes potentiels sur le déploiement des packages quand il y a des données existantes de la table qui ne respectent pas les contraintes de vérification. Avec cette mise à jour, DACFx stocke désormais l'état actuel des contraintes de vérification au sein du DACPAC lors de son extraction d'une base de données et restaure de manière appropriée cet état lors le déploiement des packages.  
  
**Mises à jour de SqlPackage.exe (outil de ligne de commande DACFx)**  
  
-   Extraire DACPAC avec les données - Crée un fichier d’instantanés de base de données (.dacpac) à partir d’une base de données Azure SQL Database ou SQL Server active qui contient les données des tables d’utilisateur en plus du schéma de la base de données. Ces packages peuvent être publiés sur une base de données Azure SQL Database ou SQL Server nouvelle ou existante avec l’action de publication de SqlPackage.exe. Les données contenues dans le package remplacent les données existantes dans la base de données cible.  
  
-   Exporter BACPAC - Crée un fichier de sauvegarde logique (.bacpac) à partir d’une base de données Azure SQL Database ou SQL Server active qui contient le schéma de la base de données et les données utilisateur qui peuvent être utilisés pour migrer une base de données de la base de données SQL Server sur site à la base de données Azure SQL Database. Les bases de données compatibles avec Azure peuvent être exportées et importées entre les versions prises en charge de SQL Server.  
  
-   Importer BACPAC - Importe un fichier .bacpac afin de créer une nouvelle base de données Azure SQL Database ou SQL Server, ou d’en remplir une vide.  
  
Vous trouverez la documentation complète de SqlPackage.exe sur MSDN [ici](../tools/sqlpackage/sqlpackage.md).  
  
**Compatibilité du package**  
  
Cette version introduit plusieurs scénarios de compatibilité ascendante des packages DAC.  
  
-   Les packages DAC créés par cette version qui ne contiennent pas d'éléments SXI ou de données de tables peuvent être consommés par les versions précédentes de DACFx (SQL Server 2012 RTM, SQL Server 2012 CU1 et DACFx de septembre 2012).  
  
-   Tous les packages DAC créés par les versions précédentes de DACFx peuvent être consommés par cette version.  
  
## <a name="see-also"></a>Voir aussi
- [Installer des mises à jour de maintenance de SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Comment identifier la version et l'édition de votre SQL Server](https://support.microsoft.com/help/321185)
- [Installer des mises à jour de maintenance de SQL Server 2012](/previous-versions/sql/sql-server-2012/hh479746(v=sql.110))
- [Comment identifier la version et l'édition de votre SQL Server](https://support.microsoft.com/help/321185) 
- [Comment déterminer la version et l'édition de SQL Server](https://support.microsoft.com/kb/321185)  
- [Fonctionnalités prises en charge par les éditions de SQL Server 2014](./editions-and-components-of-sql-server-2016.md)  

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
