---
title: Options de fin du support
description: Découvrez les différentes options disponibles pour les produits SQL Server qui ont atteint la fin du support, par exemple SQL Server 2005, SQL Server 2008 et SQL Server 2008 R2.
ms.date: 08/12/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: c7560fbda04ef5a14c73ead8a7bbc8337e3e38ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352903"
---
# <a name="sql-server-end-of-support-options"></a>Options de fin du support SQL Server 
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Cet article explique les options d’adressage des produits SQL Server qui ont atteint la fin du support.

## <a name="understanding-the-sql-server-lifecycle"></a>Compréhension du cycle de vie de SQL Server

Chaque version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est prise en charge au minimum pendant 10 ans, soit cinq ans de support standard et cinq ans de support étendu :
-  **Le support standard** comprend des correctifs fonctionnels, de performances, d’évolutivité et de sécurité. 
-  **Le support étendue** comprend uniquement les correctifs de sécurité. 

**La fin du support** (parfois connu sous le nom de fin de vie) indique qu’un produit a atteint la fin de son cycle de vie et que la maintenance et le support ne sont plus disponibles pour le produit. Pour plus d’informations sur le cycle de vie de Microsoft, consultez [Stratégie du cycle de vie Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy).



## <a name="options"></a>Options

Une fois que votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a atteint la fin de la phase de support, vous pouvez choisir :
- de mettre à niveau vers une version actuelle de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- d’acheter un [Abonnement aux correctifs de sécurité étendus](https://www.microsoft.com/cloud-platform/extended-security-updates). 
- de migrer votre charge de travail vers une machine virtuelle Azure telle quelle pour des [Correctifs de sécurité étendus gratuits](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support).
- de migrer votre charge de travail vers un [service Azure SQL Database](/azure/sql-database/sql-database-paas-vs-sql-server-iaas). 

Pour plus d’informations et pour obtenir les documents et les outils nécessaires afin de planifier et d’automatiser la mise à niveau ou la migration, consultez [Fin du support de SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005) et [Fin du support de SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008).  

![Options de fin du support](media/sql-server-end-of-life-overview/sql-server-upgrade-paths.png)

Cet article décrit les avantages et les considérations à prendre en compte pour chaque approche, ainsi que des ressources supplémentaires qui peuvent vous guider dans le processus de prise de décision.

## <a name="upgrade-sql-server"></a>Mettre à niveau SQL Server

Une fois que votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a atteint la fin du support, vous pouvez choisir de mettre à niveau vers une version plus récente et prise en charge de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela vous donne une cohérence environnementale, vous permet d’utiliser le dernier ensemble de fonctionnalités et adopte le cycle de vie du support de la nouvelle version.

### <a name="benefits"></a>Avantages
- **Technologique les plus récentes** : Les nouvelles versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] introduisent des innovations qui incluent les fonctionnalités de performances, d’évolutivité et de haute disponibilité, ainsi qu’une sécurité améliorée. 
- **Contrôle** : Dans la mesure où vous gérez le matériel et les logiciels, vous bénéficiez d’un contrôle optimal sur les fonctionnalités et l’évolutivité.
- **Environnement familier** : Si vous procédez à la mise à niveau à partir d’une version antérieure de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il s’agit de l’environnement le plus similaire.
- **Large applicabilité** : S’applique aux applications de base de données, quel que soit leur type, y compris les systèmes OLTP et l’entreposage des données.
- **Risque faible pour les applications de base de données** : En conservant la compatibilité de la base de données au même niveau que le système hérité, les applications de base de données existantes sont protégées contre des changements fonctionnels et de performances qui peuvent avoir des effets néfastes. Une application doit être entièrement recertifiée lorsqu’elle a besoin d’utiliser des fonctionnalités qui sont contrôlées par un paramètre de compatibilité de base de données plus récent. Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considérations

- **Coût** : Cette approche nécessite le plus grand investissement initial et la gestion la plus courante. Vous devez acheter, maintenir et gérer votre matériel et vos logiciels.
- **Temps d’arrêt** : Il peut y avoir des temps d’arrêt en fonction de votre stratégie de mise à niveau. Il existe également un risque inhérent à l’exécution de problèmes lors d’un processus de mise à niveau sur place.
- **Complexité** : Si vous utilisez Windows Server 2008 ou [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)], vous devrez également mettre à niveau le système d’exploitation, car les versions les plus récentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent ne pas être prises en charge sur ces versions de Windows. Le processus de mise à niveau du système d’exploitation présente des risques supplémentaires. Par conséquent, une migration côte à côte peut être l’approche la plus prudente, mais elle est plus coûteuse. Les mises à niveau sur place du système d’exploitation ne sont pas prises en charge sur les instances de cluster de basculement pour Windows Server 2008 ou [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)]. 

  > [!NOTE]
  > Les mises à niveau propagées du système d’exploitation du cluster sont disponibles à partir de Windows Server 2016.

### <a name="resources"></a>Ressources

[Support d’installation](https://www.microsoft.com/evalcenter/evaluate-sql-server-2017-rtm)   
[Effectuer une mise à niveau de SQL Server à l’aide de l’Assistant Installation](../../database-engine/install-windows/upgrade-sql-server-using-the-installation-wizard-setup.md)

Nouveautés dans :
- [SQL Server 2016](../what-s-new-in-sql-server-2016.md)
- [SQL Server 2017](../what-s-new-in-sql-server-2017.md) 
- [SQL Server 2019](../what-s-new-in-sql-server-ver15.md)   

Exigences requises :
- [SQL Server 2017 et version antérieure](../install/hardware-and-software-requirements-for-installing-sql-server.md)  
- [SQL Server 2019](../install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)    

Mises à niveau de version et d’édition prises en charge :
- [SQL Server 2016](../../database-engine/install-windows/supported-version-and-edition-upgrades.md?view=sql-server-2016&preserve-view=true) 
- [SQL Server 2017](../../database-engine/install-windows/supported-version-and-edition-upgrades-2017.md)
- [SQL Server 2019](../../database-engine/install-windows/supported-version-and-edition-upgrades-version-15.md)

Outils :
-  [L’Assistant Expérimentation de base de données](../../dea/database-experimentation-assistant-overview.md) peut aider à évaluer la version cible de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour une charge de travail spécifique. 
-  [L’Assistant Migration de données](../../dma/dma-overview.md) peut aider à détecter les problèmes de compatibilité qui peuvent avoir un impact sur la fonctionnalité de base de données dans votre nouvelle version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
-  [L’Assistant Paramétrage des requêtes](../../relational-databases/performance/upgrade-dbcompat-using-qta.md) peut aider à paramétrer des charges de travail susceptibles d’avoir des effets négatifs lors de la mise à niveau de la compatibilité de la base de données.

L’image suivante fournit un exemple d’innovation sur les différentes versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au cours des années : 

![25 ans d’innovation SQL Server](media/sql-server-end-of-life-overview/sql-server-version-improvements.png)

## <a name="extend-support"></a>Étendre le support 

Si vous n’êtes pas prêt à effectuer la mise à niveau et que vous n’êtes pas prêt à migrer vers le cloud, vous avez la possibilité d’acheter un abonnement à des correctifs de sécurité étendus pour recevoir des correctifs de sécurité **Critiques** pendant trois ans au-delà de la date de la fin du support.  

### <a name="benefits"></a>Avantages 

- **Support des applications** : Il s’agit de la meilleure option si votre application nécessite une nouvelle certification sur une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cela est courant pour les applications qui n’utilisent pas de [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md). 
- **Infrastructure cohérente** : Vous n’êtes pas obligé de modifier votre infrastructure de quelque manière que ce soit. 
- **Support technique** : Si vous disposez de Software assurance ou d’un autre plan de support, vous pouvez continuer à recevoir le support technique de [!INCLUDE[msCoName](../../includes/msconame-md.md)] sur votre produit [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de fin du support. Il s’agit de la seule façon d’obtenir un support pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. 
- **Time** : Cette option est disponible pendant trois ans, ce qui vous donne un temps supplémentaire pour certifier vos applications. 

### <a name="considerations"></a>Considérations 

- **Disponibilité limitée** : Cette option est disponible uniquement pour les clients disposant de la Software Assurance ou de licences d’abonnement. 
- **Coût** : Cette option peut s’avérer couteuse, étant donné que les correctifs de sécurité étendus sont environ 75 % des coûts annuels de la licence locale.
- **Délai d’exécution limité** : Cette option n’est disponible que pendant trois ans. Vous devez donc toujours effectuer la mise à niveau ou la migration à la fin de la période de trois ans si vous souhaitez garantir votre sécurité et votre conformité.
- **Aucun correctifs de bogues** : Si vous rencontrez un bogue non lié à la sécurité avec le produit, [!INCLUDE[msCoName](../../includes/msconame-md.md)] ne publie pas de correctif pour celui-ci. 
- **Support limitée** : Les correctifs de sécurité étendus n’incluent pas de nouvelles fonctions, de nouvelles améliorations fonctionnelles ni de nouveaux correctifs logiciels requis par le client. Les correctifs de sécurité sont limités à ceux qui sont classés comme Critiques par le [Centre de réponse aux problèmes de sécurité Microsoft (MSRC)](https://portal.msrc.microsoft.com/).

### <a name="resources"></a>Ressources

[Vue d’ensemble des correctifs de sécurité étendus (ESU)](sql-server-extended-security-updates.md)       
[Forum aux questions ESU détaillé](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Étendre le support gratuitement tel quel vers une machine virtuelle Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default)       

## <a name="azure-virtual-machine"></a>Machine virtuelle Azure

Une autre option consiste à migrer votre charge de travail vers une [machine virtuelle Azure exécutant SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview). Vous pouvez migrer votre système tel quel et conserver votre fin du support [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou vous pouvez effectuer une mise à niveau vers une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ceci est idéal pour les migrations et applications nécessitant un accès au niveau du système d’exploitation. Les machines virtuelles [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont prêtes pour la technologie lift-and-shift pour des applications existantes qui requièrent une migration rapide vers le cloud avec un minimum de modifications ou sans aucune modification. 

### <a name="benefits"></a>Avantages

- **Correctifs de sécurité étendus gratuits** : Si vous choisissez de conserver votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tel quel en utilisant [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], vous pouvez obtenir des mises à jour de sécurité étendues gratuites pendant trois ans après la fin de la date de prise en charge, même sans avoir Software Assurance. 
- **Économique** : Vous économisez le coût du logiciel matériel et serveur, en payant uniquement pour une utilisation horaire. 
- **Lift-and-shift** : Vous pouvez lift-and-shift votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et votre infrastructure d’applications dans le cloud avec un minimum de modifications ou sans aucune modification. 
- **Environnement hébergé** : Vous bénéficierez des avantages d’un environnement hébergé, tels que le matériel de déchargement et la maintenance logicielle. 
- **Automatisation** : Si vous êtes sur [!INCLUDE[winserver2008r2-md](../../includes/winserver2008r2-md.md)] et supérieur, vous bénéficiez des avantages de la mise à jour corrective automatisée et des sauvegardes automatisées. 
- **Contrôle du système d’exploitation** : Vous contrôlez l’environnement du système d’exploitation, mais avec l’ensemble des fonctionnalités familières de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Déploiement rapide** : Vous pouvez effectuer rapidement le déploiement à partir d’une bibliothèque d’images de machine virtuelle. 
- **Mobilité de licence** : Vous pouvez apporter votre licence, ce qui vous permet de réduire les coûts d’exploitation. 
- **Haute disponibilité** : Non seulement vous tirez parti de la disponibilité des machines virtuelles intégrée par l’infrastructure Azure qui offre une disponibilité de 99,99 %, mais vous pouvez également tirer parti des options de haute disponibilité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], telles que les instances de cluster de basculement et les groupes de disponibilité Always On. 
- **Risque faible pour les applications de base de données** : En conservant la compatibilité de la base de données au même niveau que les bases de données héritées, les applications de base de données existantes sont protégées contre des changements fonctionnels et de performances qui peuvent avoir des effets néfastes. Une application doit être entièrement recertifiée lorsqu’elle a besoin d’utiliser des fonctionnalités qui sont contrôlées par un paramètre de compatibilité de base de données plus récent. Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considérations

- **Facilité de gestion** : Vous devez gérer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le logiciel du système d’exploitation. 
- **Réseau** : Vous devez configurer l’ordinateur virtuel pour qu’il s’intègre à votre infrastructure de réseau et d’Active Directory, qui est une couche de complexité supplémentaire. 
- **ICF de stockage partagée** : Les machines virtuelles Azure prennent uniquement en charge les instances de cluster de basculement à l’aide d’espaces de stockage direct ou de partages de fichiers premium et ne prennent pas en charge une instance de cluster de basculement utilisant un stockage partagé. Par conséquent, les machines virtuelles Azure prennent uniquement en charge les instances de cluster de basculement lors de l’utilisation de Windows Server 2012 ou version ultérieure.
- **Temps d’arrêt d’extensibilité** : Vous avez un temps d’arrêt pendant le changement des ressources de processeur et stockage. 
- **Limitation de taille** : Bien que l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse prendre en charge autant de bases de données que nécessaire, le total cumulé de toutes les bases de données pour une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est de 256 To, par opposition à 524 Po pour une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. 

### <a name="resources"></a>Ressources

[Vue d’ensemble des machines virtuelles SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)       
[Sélection d’une option SQL Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Migrer SQL Server vers une machine virtuelle Azure](/azure/virtual-machines/windows/sql/virtual-machines-windows-migrate-sql)       
[Correctifs de sécurité étendus gratuits (ESU) pour la migration vers Azure tel quel](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)       
[Vue d’ensemble des correctifs de sécurité étendus (ESU)](sql-server-extended-security-updates.md)       
[Forum aux questions ESU détaillé](https://www.microsoft.com/cloud-platform/extended-security-updates)       
[Mise à jour corrective automatisée des machines virtuelles SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)       
[Copie de sauvegarde automatisée des machines virtuelles SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-backup-v2)       
[Haute disponibilité des machines virtuelles SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr)       
[Forum aux questions sur les machines virtuelles SQL](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-faq)       

## <a name="azure-sql-database-single-database"></a>Base de données unique Azure SQL Database

Si vous souhaitez décharger la maintenance, réduire les coûts et éliminer le besoin de mise à niveau à l’avenir, vous pouvez déplacer votre charge de travail vers la [base de données unique Azure SQL Database](/azure/sql-database/sql-database-single-database). Cette option est idéale pour les applications cloud modernes qui veulent utiliser les dernières fonctionnalités stables de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et ont des contraintes de délais en matière de développement et de marketing. 

### <a name="benefits"></a>Avantages

- **Coût** : Une base de données unique peut être rentable, étant donné que le matériel, les logiciels et la maintenance sont déchargés et que vous pouvez payer l’utilisation par la seconde ou l’heure. 
- **Flexibilité** : La base de données unique est particulièrement adaptée aux applications conçues pour le cloud si la productivité des développeurs et une commercialisation rapide pour les solutions sont des critères essentiels ou si un accès externe est requis.  
- **Fonctionnalités communes** : Les fonctionnalités [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] les plus couramment utilisées sont disponibles, mais pas autant que pour SQL Managed Instance.  
- **Déploiement rapide** : Vous pouvez rapidement déployer une base de données unique. 
- **Scalabilité** : Vous pouvez rapidement et facilement monter et descendre en puissance en fonction des besoins de votre entreprise, ce qui offre des avantages supplémentaires en matière de coûts. 
- **Disponibilité** : Le coût du service comprend le stockage et la haute disponibilité, avec une disponibilité de 99,995 % garantie.  
- **Automatisation** : Les mises à jour correctives et les sauvegardes se produisent automatiquement, ce qui vous permet de gagner des heures de maintenance.  
- **Intelligent Insights** : Obtenez des informations sur les performances de votre base de données avec l’analyse décisionnelle intégrée.  
- **Sans version** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] est sans version, ce qui signifie que vous êtes toujours sur la version la plus récente et que vous n’avez jamais à vous soucier de la mise à niveau ou du temps d’arrêt. En outre, vous travaillez toujours sur la version la plus récente et la plus importante, avec nos dernières fonctionnalités stables publiées dans le cloud en premier.
- **Risque faible pour les applications de base de données** : En conservant la compatibilité de la base de données au même niveau que la base de données locale, les applications existantes sont protégées contre des changements fonctionnels et de performances qui peuvent avoir des effets néfastes. Une application doit être entièrement recertifiée lorsqu’elle a besoin d’utiliser des fonctionnalités qui sont contrôlées par un paramètre de compatibilité de base de données plus récent. Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considérations

- **Options de migration limitées** :  Vous ne pouvez migrer qu’une seule base de données à la fois, au lieu d’une instance entière.   
- **Limitation des fonctionnalités** :  Bien que les fonctionnalités Azure SQL Database les plus couramment utilisées soient disponibles, l’ensemble de fonctionnalités pour une seule base de données n’est pas aussi complet que pour Azure SQL Managed Instance ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Différences Transact-SQL** :  Il existe des différences [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) entre une base de données unique et une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale. 
- **Limitations de taille** :  Une base de données unique a une taille de base de données maximale de 100 To, par rapport à une taille de 524 Po pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
- **Durée de maintenance** : Il n’existe aucune garantie quant à la durée de la maintenance exacte, bien qu’elle soit presque transparente. 

### <a name="resources"></a>Ressources

[Vue d’ensemble d’Azure SQL Database](/azure/sql-database/sql-database-technical-overview)       
[Sélection d’une option SQL Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Comparaison des fonctionnalités SQL Database](/azure/sql-database/sql-database-features)       
[Migrer SQL Server vers une base de données unique](/azure/sql-database/sql-database-single-database-migrate)       
[Processus de migration plus large](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       
[Différences T-SQL de base de données unique](/azure/sql-database/sql-database-transact-sql-information)       
Limite des ressources [vCore](/azure/sql-database/sql-database-vcore-resource-limits-single-databases) et [DTU](/azure/sql-database/sql-database-dtu-resource-limits-single-databases)       
[Intelligent Insights](/azure/sql-database/sql-database-intelligent-insights)       

Outils :
- [Assistant de migration des données](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="sql-managed-instance"></a>Instance managée SQL

Si vous souhaitez tirer parti du déchargement de la maintenance et des coûts, mais que vous recherchez l’ensemble de fonctionnalités d’une base de données unique [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] trop restrictive, vous pouvez passer à [SQL Managed Instance](/azure/sql-database/sql-database-managed-instance). Une instance gérée ressemble beaucoup à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale, sans avoir à se préoccuper des défaillances matérielles ni des mises à jour correctives. Une instance gérée est une collection de bases de données système et utilisateur avec un ensemble partagé de ressources prêt pour la technologie « lift-and-shift » pouvant être utilisé pour la plupart des migrations vers le cloud. Cette option est idéale pour de nouvelles applications ou des applications locales existantes qui souhaitent utiliser les dernières fonctionnalités stables [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et sont migrées vers le cloud avec des modifications minimales. 

### <a name="benefits"></a>Avantages

- **Coût** : Vous pouvez réduire les coûts en déchargeant la maintenance logicielle et matérielle.  
- **Lift and shift** : Vous pouvez effectuer une migration lift and shift de votre instance locale [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégrale vers une instance gérée, y compris toutes les bases de données avec un minimum de modifications de base de données ou pas du tout. 
- **Fonctionnalités** : L’ensemble de fonctionnalités d’une instance gérée correspond étroitement à celui d’une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], comme les requêtes de bases de données croisées, la publication et la distribution de réplication transactionnelle, la planification des travaux SQL et la prise en charge du CLR. 
- **Scalabilité** : Toutes les bases de données d’une instance gérée partagent des ressources et il est possible de les monter ou de les descendre en puissance à tout moment sans temps d’arrêt.   
- **Automatisation** : Les mises à jour correctives et les sauvegardes se produisent automatiquement, ce qui vous permet de gagner des heures de maintenance.  
- **Disponibilité** : Le coût du service comprend le stockage et la haute disponibilité, avec une disponibilité de 99,99 % garantie.  
- **Intelligent Insights** : Obtenez des informations sur les performances de vos bases de données avec l’analyse décisionnelle intégrée.  
- **Sans version** : [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] est sans version, ce qui signifie que vous êtes toujours sur la version la plus récente et que vous n’avez jamais à vous soucier de la mise à niveau ou du temps d’arrêt. En outre, vous travaillez toujours sur la version la plus récente et la plus importante, avec nos dernières fonctionnalités stables publiées dans le cloud en premier.
- **Risque faible pour les applications de base de données** : En conservant la compatibilité de la base de données au même niveau que les bases de données locales, les applications de base de données existantes sont protégées contre des changements fonctionnels et de performances qui peuvent avoir des effets néfastes. Une application doit être entièrement recertifiée lorsqu’elle a besoin d’utiliser des fonctionnalités qui sont contrôlées par un paramètre de compatibilité de base de données plus récent. Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

### <a name="considerations"></a>Considérations

- **Coût** : L’option d’instance gérée peut être plus coûteuse que l’option de base de données unique.  
- **Différences Transact-SQL** : Il existe des différences [!INCLUDE[tsql](../../includes/tsql-md.md)] (T-SQL) entre une base de données unique et une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locale.  
- **Déploiement** :  Le déploiement d’une instance gérée peut prendre plus de temps qu’une base de données unique.  
- **Limitation des fonctionnalités** : Bien qu’une instance gérée partage la plupart des fonctionnalités avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], certaines fonctionnalités ne sont pas prises en charge. 
- **Limitation de taille** : La taille de stockage combinée pour toutes les bases de données au sein d’une instance gérée est limitée à 8 To, par opposition à 524 Po pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] localement.  
- **Réseau** : Les exigences réseau pour une instance gérée ajoute une couche de complexité supplémentaire à votre infrastructure et requiert une passerelle Azure ExpressRoute ou VPN.
- **Durée de maintenance** : Il n’existe aucune garantie quant à la durée de la maintenance exacte, bien qu’elle soit presque transparente. 

### <a name="resources"></a>Ressources

[Vue d’ensemble des instances gérées SQL](/azure/sql-database/sql-database-managed-instance)       
[Sélection d’une option SQL Azure](/azure/sql-database/sql-database-paas-vs-sql-server-iaas)       
[Comparaison des fonctionnalités SQL Database](/azure/sql-database/sql-database-features)       
[Migration de SQL Server vers Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance-migrate)       
[Processus de migration plus large](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)       

Outils :
- [Assistant de migration des données](../../dma/dma-overview.md)
- [Database Migration Service](/azure/dms/dms-overview)

## <a name="non-sql-options"></a>Options non-SQL

Pour certains types d’applications, vous pouvez également envisager d’utiliser une solution non relationnelle ou NoSQL, telle que Azure Cosmos DB ou le Stockage Table Azure.

### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Envisagez Azure Cosmos DB pour des applications web modernes, évolutives et mobiles qui utilisent des données JSON et qui nécessitent à la fois des capacités robustes d’exécution de requêtes et de traitement de données transactionnelles. Pour plus d’informations, consultez [Cosmos DB](https://azure.microsoft.com/services/cosmos-db/). Pour plus d’informations sur l’importation de données, consultez [Importer des données dans Cosmos DB](/azure/cosmos-db/import-data/).

Azure Cosmos DB présente les avantages suivants :
- Vos documents sont indexés, et vous pouvez exécuter des requêtes sur ceux-ci en utilisant la syntaxe SQL que vous connaissez.
- La base de données ne contient pas de schéma.
- Vous pouvez ajouter des propriétés à des documents sans avoir à reconstruire les index.
- Le moteur de base de données prend directement en charge JSON et JavaScript.
- Vous bénéficiez de la prise en charge native des données géospatiales et de l’intégration à d’autres services Azure, comme Azure Search, HDInsight et Data Factory.
- Vous obtenez un stockage offrant des performances élevées et une latence faible avec des niveaux de débit réservés.

### <a name="azure-table-storage"></a>Stockage de table Azure

Envisagez le Stockage Table Azure pour stocker plusieurs pétaoctets de données semi-structurées dans une solution rentable. Pour plus d’informations, consultez [Stockage de table](https://azure.microsoft.com/services/storage/tables/).

Le Stockage Table Azure présente les avantages suivants :
- Vous pouvez faire évoluer vos applications et votre schéma de table sans mettre les données hors connexion.
- Vous pouvez procéder à une montée en puissance sans partitionner votre jeu de données.
- Vous obtenez un stockage géo-redondant qui réplique les données dans plusieurs régions.

## <a name="lifecycle-dates"></a>Dates du cycle de vie

Le tableau suivant fournit une approximation des dates du cycle de vie des produits [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus de détails et de précision, consultez la page [Stratégie de cycle de vie Microsoft](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy). 

| **Version**     | **Année de mise en production** | **Support standard de fin d’année** | **Support étendu de fin d’année** |
| :---------------| :--------------- | :------------------------------ | :---------------------------- |
| [SQL Server 2019](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202019) | 2019 | 2025 | 2030 |
| [SQL Server 2017](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202017) | 2017 | 2022 | 2027 |
| [SQL Server 2016](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202016) | 2016 | 2021 | 2026 |
| [SQL Server 2014](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202014) | 2014 | 2019 | 2024 |
| [SQL Server 2012](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202012) | 2012 | 2017 | 2022 |
| [SQL Server 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008%20R2) | 2010 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2008](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202008) | 2008 | 2012 | [2019](https://www.microsoft.com/sql-server/sql-server-2008) |
| [SQL Server 2005](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202005) | 2006 | 2011 | [2016](https://www.microsoft.com/sql-server/sql-server-2005) |
| [SQL Server 2000](https://support.microsoft.com/lifecycle/search?alpha=SQL%20Server%202000) | 2000 | 2005 | [2013](/archive/blogs/cdnitmanagers/sql-server-2000-end-of-support-april-2013) |

> [!IMPORTANT]
> S’il existe une différence entre cette table et la page du Cycle de vie [!INCLUDE[msCoName](../../includes/msconame-md.md)], le Cycle de vie [!INCLUDE[msCoName](../../includes/msconame-md.md)] remplace cette table, car cette table est destinée à être utilisée comme référence approximative.  

## <a name="next-steps"></a>Étapes suivantes  
[SQL Server 2019](https://www.microsoft.com/sql-server/sql-server-2019)   
[Fin du support de SQL Server 2005](https://www.microsoft.com/sql-server/sql-server-2005)   
[Fin du support de SQL Server 2008](https://www.microsoft.com/cloud-platform/windows-sql-server-2008)   
[Vue d’ensemble des correctifs de sécurité étendus (ESU)](sql-server-extended-security-updates.md)   
[Correctifs de sécurité étendus gratuits (ESU) pour la migration vers Azure tel quel](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-2008-eos-extend-support)   
[Vue d’ensemble des machines virtuelles SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-iaas-overview)   
[Vue d’ensemble d’Azure SQL Database](/azure/sql-database/sql-database-technical-overview)