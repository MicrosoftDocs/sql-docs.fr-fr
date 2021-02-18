---
title: 'Installer les outils clients : Cluster de basculement'
description: Découvrez comment installer les outils clients, tels que SQL Server Management Studio, sur une instance de basculement SQL Server.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.reviewer: ''
ms.prod: sql
ms.technology: failover-cluster-instance
ms.topic: how-to
ms.assetid: 3c82d510-9798-46be-bebb-cac9bef56936
author: cawrites
ms.author: chadam
ms.openlocfilehash: b5147a57dd5915ef850b1a05cd6e21a78a2932f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351729"
---
# <a name="install-client-tools-on-a-sql-server-failover-cluster"></a>Installer les outils clients sur un cluster de basculement SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Les outils clients tels que [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] sont des fonctionnalités partagées communes à toutes les instances sur le même ordinateur. Elles sont à compatibilité descendante, et les versions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] prises en charge peuvent être installées côte à côte. Seule une version de l'outil client existe sur un nœud à la fois.  
  
 Si les outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont installés au cours de l'installation sur le premier nœud du cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , ils sont automatiquement ajoutés aux nœuds qui peuvent être ajoutés plus tard à l'instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide de la fonctionnalité d'ajout de nœud.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La documentation en ligne n’est pas automatiquement ajoutée aux nœuds supplémentaires ajoutés au cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de la fonctionnalité d’ajout de nœud. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] La documentation en ligne peut être installée manuellement sur les nœuds sur lesquels vous voulez posséder une copie locale de la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Si vous n'installez pas les outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] au cours de l'installation initiale du cluster [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous pouvez les installer plus tard comme décrit dans la procédure suivante.  
  
## <a name="installation-procedures"></a>Procédures d'installation  
  
#### <a name="installing-ssnoversion-client-tools-using-the-setup-user-interface"></a>Installation des outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'aide de l'interface utilisateur du programme d'installation  
  
1.  Insérez le support d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Dans le dossier d'installation racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir du partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe.  
  
2.  Dans la page **Installation**, cliquez sur **Nouvelle installation autonome [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou ajout de fonctionnalités à une installation existante**. Ne cliquez pas sur **Installation d’un nouveau cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]** .  
  
3.  L'outil d'analyse de configuration système vérifie l'état système de votre ordinateur avant que le programme d'installation ne se poursuive.  
  
4.  Dans la page **Type d’installation**, cliquez sur **Effectuer une nouvelle installation de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]** .  
  
5.  Dans la page **Sélection de composant** , sélectionnez les outils à installer et suivez le reste des étapes du processus d’installation.  
  
#### <a name="installing-ssnoversion-client-tools-at-the-command-prompt"></a>Installation des outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l'invite de commandes  
  
1.  Pour installer les outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et la documentation en ligne de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], exécutez la commande suivante : Setup.exe/q/Action=Install /Features=Tools  
  
2.  Pour installer uniquement les outils de gestion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de base, exécutez la commande suivante : Setup.exe/q/Action=Install Features=SSMS. Celle-ci installe la prise en charge de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] pour le [!INCLUDE[ssDEnoversion](../../../includes/ssdenoversion-md.md)], [!INCLUDE[ssExpress](../../../includes/ssexpress-md.md)], l’utilitaire sqlcmd et le fournisseur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell.  
  
3.  Pour installer l’ensemble des outils de gestion [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], exécutez la commande suivante : Setup.exe/q/Action=Install /Features=ADV_SSMS. Pour plus d’informations sur les valeurs de paramètre pour les fonctionnalités, consultez [Installer SQL Server 2016 à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="uninstalling-ssnoversion-client-tools"></a>Désinstallation des outils clients [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
 Ils sont répertoriés avec l’intitulé **[!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]** dans le Panneau de configuration, sous Ajout/Suppression de programmes. Vous pouvez les supprimer à partir de cet emplacement. Lorsque vous utilisez la fonctionnalité Supprimer un nœud pour désinstaller une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] du cluster de basculement, les composants clients ne sont pas désinstallés en même temps.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire les fichiers journaux d'installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
