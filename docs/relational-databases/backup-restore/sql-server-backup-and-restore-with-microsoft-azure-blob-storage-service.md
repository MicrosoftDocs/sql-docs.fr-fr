---
description: Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure
title: Sauvegarde et restauration avec le stockage Blob Azure
storage: Learn about SQL Server backup to and restore from Azure Blob storage, including the benefits of using Azure Blob storage to store SQL Server backups.
ms.custom: seo-lt-2019
ms.date: 12/17/2019
ms.prod: sql
ms.prod_service: backup-restore
ms.reviewer: ''
ms.technology: backup-restore
ms.topic: conceptual
ms.assetid: 6a0c9b6a-cf71-4311-82f2-12c445f63935
author: cawrites
ms.author: chadam
ms.openlocfilehash: cc9fa042411cdbd308ea40ae9584f039f155dc29
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236134"
---
# <a name="sql-server-backup-and-restore-with-microsoft-azure-blob-storage-service"></a>Sauvegarde et restauration SQL Server avec le service de stockage d’objets blob Microsoft Azure
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  ![Graphique de sauvegarde vers l’objet blob Azure](../../relational-databases/backup-restore/media/backup-to-azure-blob-graphic.png "Graphique de sauvegarde vers l’objet blob Azure")  
  
 Cette rubrique présente les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers le [service de stockage d’objets blob Microsoft Azure](https://www.windowsazure.com/develop/net/how-to-guides/blob-storage/)ainsi que la restauration à partir de ce service. Elle résume également les avantages liés à l’utilisation du service d’objets blob Microsoft Azure pour stocker des sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 SQL Server prend en charge le stockage des sauvegardes dans le service de stockage d’objets blob Microsoft Azure comme suit :  
  
-   **Gérer vos sauvegardes dans Microsoft Azure :** à l’aide des mêmes méthodes que celles utilisées pour sauvegarder sur disque (DISK) et sur bande (TAPE), réalisez maintenant vos sauvegardes dans le stockage Microsoft Azure en spécifiant une URL comme destination de sauvegarde. Utilisez cette fonctionnalité pour sauvegarder ou configurer manuellement votre propre stratégie de sauvegarde comme vous le feriez pour un stockage local ou d'autres options hors site. Cette fonctionnalité est également connue sous le nom **Sauvegarde SQL Server vers une URL**. Pour plus d’informations, consultez [SQL Server Backup to URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md). Cette fonctionnalité est disponible dans SQL Server 2012 SP1 CU2 ou version ultérieure. Cette fonctionnalité a été améliorée dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] pour fournir des performances et des fonctionnalités supplémentaires via l’utilisation d’objets blob de blocs, de signatures d’accès partagé et d’entrelacements.  
  
    > [!NOTE]  
    >  Pour les versions SQL Server antérieures à SQL Server 2012 SP1 CU2, utilisez le complément SQL Server Backup to Microsoft Azure Tool afin de créer rapidement et facilement des sauvegardes dans le stockage Microsoft Azure. Pour plus d'informations, consultez le [centre de téléchargement](https://go.microsoft.com/fwlink/?LinkID=324399).  
  
-   **Sauvegardes d’instantanés de fichiers pour les fichiers de base de données dans le stockage d’objets blob Azure** Les sauvegardes d’instantanés de fichiers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournissent, via l’utilisation d’instantanés Azure, des sauvegardes et des restaurations quasi instantanées des fichiers de base de données stockés à l’aide du service de stockage d’objets blob Azure. Cette fonctionnalité vous permet de simplifier vos stratégies de sauvegarde et de restauration et prend en charge la limite de restauration dans le temps. Pour plus d’informations, consultez [Sauvegarde d’instantanés de fichiers pour les fichiers de base de données dans Azure](../../relational-databases/backup-restore/file-snapshot-backups-for-database-files-in-azure.md). Cette fonctionnalité est disponible dans SQL Server 2016 ou version ultérieure.  
  
-   **Laisser SQL Server gérer les sauvegardes dans Microsoft Azure :** configurez SQL Server pour qu’il gère la stratégie de sauvegarde et planifie les sauvegardes d’une seule ou de plusieurs bases de données, ou définissez les valeurs par défaut au niveau de l’instance. Cette fonctionnalité s’appelle **[!INCLUDE[ss_smartbackup](../../includes/ss-smartbackup-md.md)]** . Pour plus d’informations, consultez [Sauvegarde managée SQL Server sur Microsoft Azure](../../relational-databases/backup-restore/sql-server-managed-backup-to-microsoft-azure.md). Cette fonctionnalité est disponible dans SQL Server 2014 ou version ultérieure.  
  
## <a name="benefits-of-using-the-microsoft-azure-blob-service-for-ssnoversion-backups"></a>Avantages de l’utilisation du service d’objets blob Microsoft Azure pour les sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  
  
-   Stockage hors site flexible, fiable et illimité : le stockage de vos sauvegardes dans le service d’objets blob Microsoft Azure peut représenter une option hors site pratique, flexible et facile d'accès. La création d'un stockage hors site pour vos sauvegardes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être aussi simple que la modification de vos scripts/tâches existants. En principe, le stockage hors site doit être suffisamment éloigné de l'emplacement de la base de données de production pour empêcher qu'un éventuel sinistre n'affecte tous les emplacements (bases de données hors site et de production). En choisissant de géorépliquer le stockage d'objets blob, vous disposez d'un niveau de protection supplémentaire au cas où un sinistre affecterait l'ensemble de la région. En outre, les sauvegardes restent accessibles à partir de n'importe quel endroit et à tout moment, et vous pouvez y accéder facilement pour les restaurations.  
  
    > [!IMPORTANT]  
    >  Grâce à l’utilisation d’objets blob de blocs dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], vous pouvez distribuer votre jeu de sauvegarde pour prendre en charge des tailles de fichiers de sauvegarde jusqu’à 12,8 To.  
  
-   Archive de sauvegarde : le service de stockage d’objets blob Microsoft Azure offre une meilleure alternative à l’option de stockage sur bande souvent utilisée pour archiver les sauvegardes. Le stockage sur bande peut nécessiter le transport physique vers un emplacement hors site, ainsi que des mesures de protection des supports. En stockant vos sauvegardes dans le service de Stockage Blob Microsoft Azure, vous disposez d’une option d’archivage instantanée, hautement disponible et durable.  
  
-   Aucune surcharge de gestion du matériel avec les services Microsoft Azure. Les services Microsoft Azure gèrent le matériel et assurent la géoréplication pour la redondance et la protection contre les défaillances matérielles.  
  
-   Actuellement, pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutant sur un ordinateur virtuel Microsoft Azure, la sauvegarde dans les services de stockage d’objets blob Microsoft Azure peut être réalisée en créant des disques attachés. Toutefois, le nombre de disques que vous pouvez attacher à un ordinateur virtuel Microsoft Azure est limité. Cette limite est de 16 disques pour une instance extra-large et inférieure pour les instances plus petites. En activant une sauvegarde directe dans le stockage d’objets blob Microsoft Azure, vous pouvez contourner la limite de 16 disques.  
  
     En outre, le fichier de sauvegarde qui est maintenant stocké dans le service de stockage d’objets blob Microsoft Azure est directement accessible pour une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutant sur un ordinateur virtuel Microsoft Azure, sans avoir besoin d’attacher/détacher la base de données ou de télécharger et attacher le disque dur virtuel.  
  
-   Avantages en termes de coûts : payez uniquement pour le service utilisé. Peut être économique comme option de sauvegarde et d'archivage hors site. Pour plus d’informations et de liens, consultez la section [Considérations sur la facturation Microsoft Azure](#Billing) .  
  
##  <a name="microsoft-azure-billing-considerations"></a><a name="Billing"></a> Considérations sur la facturation Microsoft Azure :  
 En comprenant les coûts de stockage Microsoft Azure, vous pouvez prévoir le coût de création et de stockage des sauvegardes dans Microsoft Azure.  
  
 La [Calculatrice Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=277060) peut vous aider à évaluer les coûts.  
  
 **Stockage :** les tarifs sont basés sur l'espace utilisé et sont calculés sur une échelle graduée en fonction du niveau de redondance. Pour plus d’informations, consultez la section **Gestion des données** de l’article [Détails de la tarification](https://go.microsoft.com/fwlink/?LinkId=277059) .  
  
 **Transferts de données :** les transferts de données entrants vers Microsoft Azure sont gratuits. Les transferts sortants sont facturés en fonction de l'utilisation de la bande passante et calculés selon une échelle graduée spécifique à la région. Pour plus d'informations, consultez la section [Transferts de données](https://go.microsoft.com/fwlink/?LinkId=277061) de l'article Détails de la tarification.  
  
## <a name="see-also"></a>Voir aussi  

[Bonnes pratiques et résolution des problèmes liés à la sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting.md)   

[Sauvegarde et restauration des bases de données système &#40;SQL Server&#41;](../../relational-databases/backup-restore/back-up-and-restore-of-system-databases-sql-server.md)   

[Tutoriel : Utilisation du service Stockage Blob Microsoft Azure avec des bases de données SQL Server 2016](../tutorial-use-azure-blob-storage-service-with-sql-server-2016.md)

[Sauvegarde SQL Server vers une URL](../../relational-databases/backup-restore/sql-server-backup-to-url.md)  
  
  
