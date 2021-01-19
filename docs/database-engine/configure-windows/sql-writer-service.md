---
title: Service SQL Writer | Microsoft Docs
description: Apprenez-en plus sur le Service SQL Writer. Découvrez comment il fournit une fonctionnalité de sauvegarde et de restauration dans SQL Server par le biais du service VSS.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- VDI [SQL Server]
- restoring [SQL Server], SQL Writer Service
- backups [SQL Server], while SQL Server running
- Volume Shadow Copy Service
- volume backups while running [SQL Server]
- Virtual Backup Device Interface [SQL Server]
- SQL Writer Service
- services [SQL Server], SQL Writer
- MSDE Writer
- VSS
ms.assetid: 0f299867-f499-4c2a-ad6f-b2ef1869381d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8bd9e1cacb36ce4906e30c2b8030b5e6b0b09787
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170911"
---
# <a name="sql-writer-service"></a>Service SQL Writer
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Le service SQL Writer complète les fonctionnalités de sauvegarde et de restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] par le biais du service VSS.  
  
 Le service SQL Writer est installé automatiquement. Il doit être en cours d'exécution lorsque l'application VSS (Volume Shadow Copy Service) demande une sauvegarde ou une restauration. Pour configurer le service, utilisez l'applet des services [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Le service SQL Writer s'installe sur tous les systèmes d'exploitation.  
  
## <a name="purpose"></a>Objectif  
 Lors de son exécution, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] verrouille les fichiers de données pour être seul à pouvoir y accéder. Lorsque le service SQL Writer n'est pas exécuté, les programmes de sauvegarde exécutés dans Windows n'ont pas accès à ces fichiers de données, et les sauvegardes doivent s'effectuer au moyen de la sauvegarde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Utilisez le service SQL Writer pour permettre aux programmes de sauvegarde de Windows de copier les fichiers de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] même lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
## <a name="volume-shadow-copy-service"></a>Service VSS  
 Le service VSS est un ensemble d'API COM qui forment un cadre permettant la sauvegarde de volumes même lorsque des opérations d'écriture sont en cours. Il offre une interface cohérente qui permet la coordination entre les différentes applications utilisateur qui mettent à jour les données sur le disque (les writers) et celles qui assurent la sauvegarde des applications (les demandeurs).  
  
 Ce service capture et copie des images stables pour la sauvegarde sur des systèmes en cours d'utilisation, en particulier des serveurs, sans dégradation superflue des performances et de la stabilité des services qu'ils assurent. Pour plus d'informations sur le service VSS, consultez votre documentation Windows.  

> [!NOTE]
> Lorsque VSS est utilisé pour sauvegarder une machine virtuelle qui héberge un groupe de disponibilité de base et des bases de données dans un état secondaire, ces bases de données *ne seront pas* sauvegardées avec la machine virtuelle à partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU2 et [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU9.  En effet, les groupes de disponibilité de base ne prennent pas en charge la sauvegarde des bases de données sur le réplica secondaire.  Avant ces versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la sauvegarde échoue avec une erreur.
  
## <a name="virtual-backup-device-interface-vdi"></a>Interface d'unité de sauvegarde virtuelle  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit une API appelée « Interface d’unité de sauvegarde virtuelle » qui permet aux éditeurs de logiciels indépendants d’intégrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans leurs produits pour la prise en charge des opérations de sauvegarde et de restauration. Conçues pour fournir une fiabilité et des performances optimales, ces API prennent en charge l'éventail complet de fonctions de sauvegarde et de restauration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , y compris la gamme totale des sauvegardes à chaud et instantanées. Si une application de fournisseur tiers demande une sauvegarde d’instantané (VSS), le service SQL Writer appelle les fonctions de l’API VDI afin d’effectuer les sauvegardes réelles. Notez que l’API VDI est indépendante de VSS et qu’elle est souvent utilisée dans les solutions logicielles qui n’utilisent pas les API VSS.
  
## <a name="permissions"></a>Autorisations  
 Le service SQL Writer doit s'exécuter sous le compte **système local** . Le service SQL Writer utilise la connexion **NT Service\SQLWriter** pour la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fait d’utiliser la connexion **NT Service\SQLWriter** permet au processus SQL Writer de s’exécuter à un niveau de droits inférieur dans un compte indiqué comme étant **sans connexion**, ce qui limite la vulnérabilité. Si le service SQL Writer est désactivé, les utilitaires qui s'appuient sur les instantanés VSS, tels que System Center Data Protection Manager, ainsi que certains autres produits tiers, seront rompus ou pire, risquent d'effectuer des sauvegardes de bases de données qui ne sont pas cohérentes. Si ni [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le système sur lequel il s'exécute, ni le système hôte (dans le cas d'une machine virtuelle), ne doit utiliser un élément autre que la sauvegarde [!INCLUDE[tsql](../../includes/tsql-md.md)] , le service SQL Writer peut être désactivé en toute sécurité et la connexion supprimée.  Notez que le service SQL Writer peut être appelé par une sauvegarde au niveau du système ou du volume, que la sauvegarde repose directement sur des instantanés ou non. Certains logiciels de sauvegarde système utilisent VSS pour éviter d’être bloqués par des fichiers ouverts ou verrouillés. Le service SQL Writer nécessite des autorisations élevées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En effet, au cours de ses activités, il fige brièvement toutes les E/S pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="features"></a>Fonctionnalités  
 SQL Writer prend en charge les possibilités suivantes :  
  
-   Sauvegarde et restauration complètes de bases de données, y compris des catalogues de texte intégral  
  
-   Sauvegarde et restauration différentielle  
  
-   Restauration avec déplacement  
  
-   Modification du nom d'une base de données  
  
-   Sauvegarde de copie seule  
  
-   Récupération automatique d'un instantané de base de données  
  
 SQL Writer ne prend pas en charge les fonctions suivantes :  
  
-   Sauvegarde de journaux  
  
-   Sauvegarde de fichiers et de groupes de fichiers  
  
-   Restauration de pages  
  
## <a name="remarks"></a>Notes
Le service SQL Writer est distinct du moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et partagé entre différentes versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et différentes instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le même serveur.  Le fichier du service SQL Writer est inclus dans le package d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], avec le même numéro de version que le moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qu’il accompagne.  Lorsqu’une nouvelle instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est installée sur un serveur ou qu’une instance existante est mise à niveau, si le numéro de version de l’instance concernée est supérieur à celui du service SQL Writer qui se trouve actuellement sur le serveur, ce fichier est remplacé par celui du package d’installation.  Notez que, si le service SQL Writer a été mis à jour par un Service Pack ou une mise à jour cumulative et qu’une version finale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d’installation, il est possible de remplacer une version récente du service SQL Writer par une ancienne version, à condition que l’installation ait un numéro de version majeure supérieur.  Par exemple, le service SQL Writer a été mis à jour dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] SP2 CU2.  Si cette instance est mise à niveau vers la version finale [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], le service SQL Writer mis à jour est remplacé par une version antérieure.  Dans ce cas, vous devrez appliquer la dernière version CU à la nouvelle instance afin d’obtenir la dernière version du service SQL Writer.

