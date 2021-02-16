---
title: Installer SQL Server à l’aide d’un fichier de configuration | Microsoft Docs
description: Vous pouvez utiliser le programme d’installation de SQL Server pour générer un fichier de configuration afin de déployer SQL Server dans votre organisation à l’aide d’une configuration uniforme.
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: a832153a-6775-4bed-83f0-55790766d885
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: b5b38250065d1c7474d5f500284a082b421332c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348341"
---
# <a name="install-sql-server-using-a-configuration-file"></a>Installer SQL Server à l’aide d’un fichier de configuration

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Le programme d’installation permet de générer un fichier de configuration basé sur les entrées système par défaut et celles effectuées au moment de l’exécution. Vous pouvez utiliser le fichier de configuration pour déployer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans la totalité de l'entreprise avec la même configuration. Vous pouvez également standardiser les installations manuelles dans l'ensemble de l'entreprise, en créant un fichier de commandes qui lance Setup.exe. 
 
Cet article est conservé pour SQL Server 2016 et les versions ultérieures. Si vous utilisez une version antérieure de SQL Server, consultez [Installer SQL Server 2014 à l’aide d’un fichier de configuration](/previous-versions/sql/2014/database-engine/install-windows/install-sql-server-using-a-configuration-file?view=sql-server-2014&preserve-view=true).
 
Le programme d'installation prend en charge l'utilisation du fichier de configuration uniquement via l'invite de commandes. L'ordre de traitement des paramètres lors de l'utilisation du fichier de configuration est décrit ci-dessous :  
  
-  Le fichier de configuration remplace les valeurs par défaut d'un package.  
  
-   Les valeurs de la ligne de commande remplacent les valeurs du fichier de configuration.  
  
 Le fichier de configuration permet d'assurer le suivi des paramètres et des valeurs de chaque installation. Cela le rend très utile pour vérifier et auditer des installations. 
  
## <a name="configuration-file-structure"></a>Structure du fichier de configuration  
 Le fichier ConfigurationFile.ini est un fichier texte avec des paramètres (paire nom/valeur) et des commentaires descriptifs. 
  
 Voici un exemple de fichier ConfigurationFile.ini :  
  
```  
; Microsoft SQL Server Configuration file  
[OPTIONS]  
; Specifies a Setup work flow, like INSTALL, UNINSTALL, or UPGRADE.  
; This is a required parameter.  
ACTION="Install"  
; Specifies features to install, uninstall, or upgrade.  
; The list of top-level features include SQL, AS, RS, IS, and Tools.  
; The SQL feature will install the database engine, replication, and full-text.  
; The Tools feature will install Management Tools, Books online,   
; SQL Server Data Tools, and other shared components.  
FEATURES=SQL,Tools  
```  
  
#### <a name="how-to-generate-a-configuration-file"></a>Comment générer un fichier de configuration  
  
1. Insérez le support d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans le dossier racine, double-cliquez sur Setup.exe. Pour effectuer l'installation à partir d'un partage réseau, recherchez le dossier racine sur le partage, puis double-cliquez sur Setup.exe. 
  
    > [!NOTE]  
    >  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Express Edition ne crée pas de fichier de configuration automatiquement. La commande suivante démarre l’installation et crée un fichier de configuration. 
    >   
    >  SETUP.exe /UIMODE=Normal /ACTION=INSTALL  
  
2. Suivez le déroulement des étapes de l'Assistant jusqu'à la page **Prêt pour l'installation** . Le chemin d'accès au fichier de configuration est spécifié dans la page **Prêt pour l'installation** , dans la section relative au chemin d'accès du fichier de configuration. Pour plus d’informations sur l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Installer SQL Server à partir de l’Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md). 
  
3. Annulez l'exécution du programme d'installation sans réellement terminer l'installation afin de générer le fichier INI. 
  
    > [!NOTE]  
    >  L'infrastructure d'installation écrit tous les paramètres appropriés pour les actions exécutées, à l'exception des informations sensibles comme les mots de passe. Le paramètre /IAcceptSQLServerLicenseTerms n'est pas écrit dans le fichier de configuration et requiert soit une modification du fichier de configuration, soit une valeur à fournir à l'invite de commandes. Pour plus d’informations, consultez [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md). De plus, une valeur est incluse pour les paramètres booléens pour lesquels une valeur n'est généralement pas fournie via l'invite de commandes. 
  
## <a name="using-the-configuration-file-to-install-ssnoversion"></a>Utilisation du fichier de configuration pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  

Vous ne pouvez utiliser le fichier de configuration que pour les installations en ligne de commande. 
  
> [!NOTE]  
> Si vous devez apporter des modifications au fichier de configuration, nous vous recommandons de faire une copie de ce dernier et de l''utiliser. 
  
### <a name="how-to-use-a-configuration-file-to-install-a-stand-alone-ssnoversion-instance"></a>Comment utiliser un fichier de configuration pour installer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autonome  
  
-   Exécutez l'installation à partir de l'invite de commandes et spécifiez le fichier ConfigurationFile.ini à l'aide du paramètre *ConfigurationFile* . 
  
### <a name="how-to-use-a-configuration-file-to-prepare-and-complete-an-image-of-a-stand-alone-ssnoversion-instance-sysprep"></a>Procédure d'utilisation d'un fichier de configuration afin de préparer et finaliser une image d'une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autonome (SysPrep)  
  
1. Pour préparer une ou plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les configurer sur le même ordinateur. 
  
    - Exécutez **Préparation de l’image d’une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page **Avancé** du Centre d’installation et capturez le fichier de configuration de préparation d’image. 
  
    - Utilisez le même fichier de configuration de préparation d'image comme modèle pour préparer d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 
  
    - Exécutez **Finalisation d’image d’une instance autonome préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page **Avancé** du Centre d’installation pour configurer une instance préparée sur l’ordinateur. 
  
2. Pour préparer une image du système d'exploitation, notamment une instance préparée non configurée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], à l'aide de l'outil SysPrep de Windows. 
  
    -   Exécutez **Préparation de l’image d’une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page Avancé du Centre d’installation et capturez le fichier de configuration de préparation d’image. 
  
    -   Exécutez **Finalisation d’image d’une instance autonome préparée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** dans la page **Avancé** du Centre d’installation, mais annulez cette opération dans la page **Prêt à finaliser l’image** après avoir capturé le fichier de configuration complet. 
  
    -   Le fichier de configuration de finalisation d'image peut être stocké avec l'image Windows pour l'automatisation de la configuration des instances préparées. 
  
### <a name="how-to-install-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Procédure d'installation d'un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du fichier de configuration  
  
1. Option d'installation intégrée (créez un cluster de basculement à nœud unique sur un nœud et exécutez AddNode sur les nœuds supplémentaires) :  
  
    -   Effectuez l'installation d'un cluster de basculement et capturez le fichier de configuration qui répertorie tous les paramètres d'installation. 
  
    -   Effectuez l’installation du cluster de basculement à partir de la ligne de commande en spécifiant le paramètre *ConfigurationFile* . 
  
    -   Sur un nœud supplémentaire destiné à être ajouté, exécutez AddNode afin de capturer le fichier ConfigurationFile.ini applicable au cluster de basculement existant. 
  
    -   Exécutez AddNode à partir de la ligne de commande sur tous les nœuds supplémentaires destinés à joindre le cluster de basculement, en spécifiant le même fichier de configuration à l'aide du paramètre ConfigurationFile. 
  
2. Option d'installation avancée (préparez le cluster de basculement sur tous les nœuds de cluster de basculement, puis, après avoir préparé tous les nœuds, exécutez l'opération de création sur le nœud propriétaire du disque partagé) :  
  
    -   Exécutez **Prepare** sur l'un des nœuds et capturez le fichier ConfigurationFile.ini. 
  
    -   Indiquez le même fichier ConfigurationFile.ini au programme d'installation sur tous les nœuds à préparer pour le cluster de basculement. 
  
    -   Après avoir préparé tous les nœuds, exécutez une opération de création du cluster de basculement sur le nœud propriétaire du disque partagé et capturez le fichier ConfigurationFile.ini. 
  
    -   Vous pouvez ensuite spécifier ce fichier ConfigurationFile.ini pour créer le cluster de basculement. 
  
### <a name="how-to-add-or-remove-a-node-to-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Comment ajouter ou supprimer un nœud dans un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du fichier de configuration  
  
-   Si vous disposez d'un fichier de configuration qui a été précédemment utilisé pour ajouter ou supprimer un nœud dans un cluster de basculement, vous pouvez réutiliser ce même fichier pour ajouter ou supprimer des nœuds supplémentaires. 
  
### <a name="how-to-upgrade-a-ssnoversion-failover-cluster-using-the-configuration-file"></a>Comment mettre à niveau un cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide du fichier de configuration  
  
1. Exécutez la mise à niveau sur le nœud passif et capturez le fichier ConfigurationFile.ini. Pour ce faire, vous pouvez effectuer la mise à niveau réelle ou sortir à la fin du processus sans effectuer la mise à niveau réelle. 
  
2. Sur tous les nœuds supplémentaires à mettre à niveau, spécifiez le fichier ConfigurationFile.ini pour exécuter le processus. 
  
## <a name="sample-syntax"></a>Exemple de syntaxe  
 Voici quelques exemples qui illustrent l'utilisation du fichier de configuration :  
  
-   Pour spécifier le fichier de configuration à l'invite de commandes :  
  
```  
Setup.exe /ConfigurationFile=MyConfigurationFile.INI  
```  
  
-   Pour spécifier des mots de passe à l'invite de commandes plutôt que dans le fichier de configuration :  
  
```  
Setup.exe /SQLSVCPASSWORD="************" /AGTSVCPASSWORD="************" /ASSVCPASSWORD="************" /ISSVCPASSWORD="************" /RSSVCPASSWORD="************" /ConfigurationFile=MyConfigurationFile.INI  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Installer SQL Server à partir de l’invite de commandes](./install-sql-server-from-the-command-prompt.md)   
 [Installation d’un cluster de basculement SQL Server](../../sql-server/failover-clusters/install/sql-server-failover-cluster-installation.md)   
 [Mettre à niveau une instance de cluster de basculement SQL Server](../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)  
  
