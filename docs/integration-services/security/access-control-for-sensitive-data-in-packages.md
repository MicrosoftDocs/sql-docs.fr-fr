---
description: Contrôle d'accès pour les données sensibles présentes dans les packages
title: Contrôle d’accès pour les données sensibles présentes dans les packages | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.packageprotectionlevel.f1
- sql13.ssis.bids.projectprotectionlevel.f1
- sql13.dts.designer.packagepassword.f1
- sql13.ssis.bids.projectpassword.f1
helpviewer_keywords:
- passwords [Integration Services]
- packages [Integration Services], security
- protection levels for packages [Integration Services]
- configuration files [Integration Services]
- encryption [Integration Services]
- cryptography [Integration Services]
- security [Integration Services], protection levels
ms.assetid: d4b073c4-4238-41fc-a258-4e114216e185
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b85dc4949e363a8e62ca3d992a04365f7ce414f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353013"
---
# <a name="access-control-for-sensitive-data-in-packages"></a>Contrôle d'accès pour les données sensibles présentes dans les packages

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Pour protéger les données d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , vous pouvez définir un niveau de protection qui permet de protéger uniquement les données sensibles ou toutes les données du package. En outre, vous pouvez chiffrer ces données avec un mot de passe ou une clé utilisateur ou vous fier à la base de données pour chiffrer les données. De plus, le niveau de protection que vous utilisez pour un package n'est pas nécessairement statique, mais change tout au long du cycle de vie du package. On définit souvent un niveau de protection pendant le développement et un autre dès le déploiement du package.  
  
> [!NOTE]  
>  Outre les niveaux de protection décrits dans cette rubrique, vous pouvez utiliser des rôles fixes au niveau de la base de données pour protéger les packages enregistrés sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .  
  
## <a name="definition-of-sensitive-information"></a>Définition des informations sensibles  
 Dans un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , les informations suivantes sont définies comme *sensibles*:  
  
-   La partie mot de passe d'une chaîne de connexion. Cependant, si vous sélectionnez une option qui chiffre toutes les données, la chaîne de connexion toute entière sera considérée comme sensible.  
  
-   Les nœuds XML générés par la tâche qui sont marqués comme sensibles. Le marquage des nœuds XML est contrôlé par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et ne peut pas être modifié par les utilisateurs.  
  
-   Toute variable marquée comme sensible. Le marquage des variables est contrôlé par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
 Le fait qu' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] considère une propriété comme sensible repose sur le fait que le développeur du composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , tel qu'un gestionnaire de connexions ou une tâche, l'a désignée comme sensible. Les utilisateurs ne peuvent pas ajouter de propriétés à la liste des propriétés considérées sensibles, ni en supprimer.  
  
## <a name="encryption"></a>Chiffrement  
 Le chiffrement, tel qu’il est utilisé par les niveaux de protection des packages, est effectué au moyen de l’API de protection des données (DPAPI) [!INCLUDE[msCoName](../../includes/msconame-md.md)] , qui fait partie de l’API de chiffrement (CryptoAPI).  
  
 Les niveaux de protection de package qui chiffrent les packages à l'aide de mots de passe nécessitent que vous fournissiez également un mot de passe. Si vous changez de niveau de protection en passant d'un niveau qui n'utilise pas de mot de passe à un niveau qui en utilise, un mot de passe vous sera demandé.  
  
 En outre, pour les niveaux de protection utilisant un mot de passe, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] utilise l’algorithme de chiffrement Triple DES avec une longueur de clé de 192 bits, disponible dans la bibliothèque de classes [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] (FCL).  
  
## <a name="protection-levels"></a>Niveaux de protection  
 Le tableau suivant décrit les niveaux de protection fournis par [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les valeurs entre parenthèses sont des valeurs de l’énumération <xref:Microsoft.SqlServer.Dts.Runtime.DTSProtectionLevel> . Ces valeurs apparaissent dans la fenêtre Propriétés que vous utilisez pour configurer les propriétés du package lorsque vous travaillez avec des packages dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
|Niveau de protection|Description|  
|----------------------|-----------------|  
|Ne pas enregistrer les informations sensibles (**DontSaveSensitive**)|Supprime les valeurs des propriétés sensibles dans le package lors de son enregistrement. Ce niveau de protection n'effectue pas de chiffrement, mais empêche les propriétés marquées comme sensibles d'être enregistrées avec le package, rendant de ce fait les données sensibles inaccessibles aux autres utilisateurs. Si un utilisateur différent ouvre le package, les informations sensibles sont remplacées par des espaces et l'utilisateur doit fournir les informations sensibles.<br /><br /> Utilisé avec l’utilitaire **dtutil** (dtutil.exe), ce niveau de protection correspond à la valeur 0.|  
|Chiffrer avec mot de passe (**EncryptAllWithPassword**)|Utilise un mot de passe pour chiffrer l'ensemble du package. Le package est chiffré à l'aide d'un mot de passe fourni par l'utilisateur lorsque le package est créé ou exporté. Pour ouvrir le package dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou exécuter le package à l’aide de l’utilitaire d’invite de commandes **dtexec** , l’utilisateur doit fournir le mot de passe du package. Sans le mot de passe, l'utilisateur ne peut pas accéder au package ni l'exécuter.<br /><br /> En cas d’utilisation avec l’utilitaire **dtutil** , ce niveau de protection correspond à la valeur 3.|  
|Chiffrer tout avec la clé utilisateur (**EncryptAllWithUserKey**)|Utilise une clé basée sur le profil utilisateur actuel pour chiffrer l'ensemble du package. Seul l’utilisateur qui a créé ou exporté le package peut ouvrir ce dernier dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou l’exécuter à l’aide de l’utilitaire d’invite de commandes **dtexec** .<br /><br /> Avec l’utilitaire **dtutil** , ce niveau de protection correspond à la valeur 4.<br /><br /> Remarque : pour les niveaux de protection s’appuyant sur une clé utilisateur, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se base sur les normes DPAPI. Pour plus d’informations sur DPAPI, consultez la bibliothèque MSDN à l’adresse [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Chiffrer les données sensibles avec un mot de passe (**EncryptSensitiveWithPassword**)|Utilise un mot de passe pour chiffrer uniquement les valeurs des propriétés sensibles dans le package. DPAPI est utilisé pour ce chiffrement. Les données sensibles sont enregistrées en tant que partie du package, mais ces données sont chiffrées à l'aide d'un mot de passe fourni par l'utilisateur actuel lorsque le package est créé ou exporté. Pour ouvrir le package dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , l'utilisateur doit fournir le mot de passe du package. Si le mot de passe n'est pas fourni, le package est ouvert sans les données sensibles et l'utilisateur actuel doit fournir de nouvelles valeurs pour les données sensibles. Si l'utilisateur tente d'exécuter le package sans fournir de mot de passe, l'exécution du package échoue. Pour plus d'informations sur les mots de passe et l'exécution de lignes de commande, consultez [dtexec Utility](../../integration-services/packages/dtexec-utility.md).<br /><br /> Avec l’utilitaire **dtutil** , ce niveau de protection correspond à la valeur 2.|  
|Chiffrer les données sensibles avec une clé utilisateur (**EncryptSensitiveWithUserKey**)|Utilise une clé basée sur le profil utilisateur actuel pour chiffrer uniquement les valeurs des propriétés sensibles dans le package. Seul le même utilisateur qui utilise le même profil peut charger le package. Si un utilisateur différent ouvre le package, les informations sensibles sont remplacées par des espaces et l'utilisateur actuel doit fournir de nouvelles valeurs pour les informations sensibles. Si l'utilisateur tente d'exécuter le package, l'exécution du package échoue. DPAPI est utilisé pour ce chiffrement.<br /><br /> Avec l’utilitaire **dtutil** , ce niveau de protection correspond à la valeur 1.<br /><br /> Remarque : pour les niveaux de protection s’appuyant sur une clé utilisateur, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] se base sur les normes DPAPI. Pour plus d’informations sur DPAPI, consultez la bibliothèque MSDN à l’adresse [https://msdn.microsoft.com/library](/previous-versions/ms995355(v=msdn.10)).|  
|Se fier au stockage du serveur pour le chiffrement (**ServerStorage**)|Protège le package entier à l'aide des rôles de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette option est prise en charge quand un package est enregistré dans la base de données msdb [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . En outre, le catalogue SSISDB utilise le niveau de protection **ServerStorage**<br /><br /> Cette option n'est pas prise en charge lorsqu'un package est enregistré dans le système de fichiers à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].|  
  
## <a name="protection-level-setting-and-the-ssisdb-catalog"></a>Paramètre de niveau de protection et catalogue SSISDB  
 Le catalogue SSISDB utilise le niveau de protection **ServerStorage** . Lorsque vous déployez un projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sur le serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , le catalogue chiffre automatiquement les données du package et les valeurs sensibles. Le catalogue déchiffre automatiquement les données lorsque vous les récupérez.  
  
 Si vous exportez le projet (.ispac) du serveur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] vers le système de fichiers, le système change automatiquement le niveau de protection en **EncryptSensitiveWithUserKey**. Si vous importez un projet à l'aide de l' **Assistant Importation de projet Integration Services** dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la propriété **ProtectionLevel** dans la fenêtre **Propriétés** indique une valeur de **EncryptSensitiveWithUserKey**.  
  
## <a name="protection-level-setting-based-on-package-life-cycle"></a>Définition du niveau de protection en fonction du cycle de vie du package  
 Vous définissez le niveau de protection d’un package [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] lorsque vous commencez son développement dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Plus tard, lorsque le package est déployé, importé ou exporté à partir de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], ou copié à partir de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le magasin de packages [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou le système de fichiers, vous pouvez mettre à jour le niveau de protection du package. Par exemple, si vous créez et enregistrez des packages sur votre ordinateur avec une des options de niveau de protection à clé utilisateur, vous souhaiterez sans doute modifier le niveau de protection lorsque vous donnerez le package à d'autres utilisateurs, pour qu'ils puissent l'ouvrir.  
  
 En général, vous modifiez le niveau de protection selon les étapes suivantes :  
  
1.  Pendant le développement, laissez le niveau de protection de packages défini à la valeur par défaut, **EncryptSensitiveWithUserKey**. Ce paramètre aide à s'assurer que seul le développeur peut afficher les valeurs sensibles dans le package. Ou vous pouvez envisager d'utiliser **EncryptAllWithUserKey** ou **DontSaveSensitive**.  
  
2.  Lorsqu'il est temps de déployer les packages, vous devez affecter un niveau de protection qui ne dépend pas de la clé utilisateur du développeur. Par conséquent, vous devez sélectionner en général **EncryptSensitiveWithPassword** ou **EncryptAllWithPassword**. Chiffrez les packages en assignant un mot de passe fort temporaire également connu de l'équipe d'exploitation dans l'environnement de production.  
  
3.  Une fois que les packages ont été déployés dans l'environnement de production, l'équipe d'exploitation peut rechiffrer les packages déployés en assignant un mot de passe fort connu uniquement d'eux. Ils peuvent également chiffrer les packages déployés en sélectionnant **EncryptSensitiveWithUserKey** ou **EncryptAllWithUserKey**, et en utilisant les informations d'identification locales du compte qui exécutera les packages.  

## <a name="set-or-change-the-protection-level-of-packages"></a><a name="set_protection"></a> Définir ou modifier le niveau de protection des packages
  Pour contrôler l’accès au contenu des packages et aux valeurs sensibles qu’ils contiennent, telles que les mots de passe, définissez la valeur de la propriété **ProtectionLevel** . Les packages contenus dans un projet doivent avoir le même niveau de protection que le projet pour permettre sa génération. Si vous modifiez le paramètre de propriété **ProtectionLevel** du projet, vous devez mettre à jour manuellement le paramètre de propriété des packages.  
  
 Pour obtenir une vue d’ensemble des fonctionnalités de sécurité dans [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], consultez [Vue d’ensemble de la sécurité &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
 Les procédures dans cette rubrique décrivent comment utiliser [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] ou l’utilitaire en ligne de commande dtutil pour modifier la propriété **ProtectionLevel** .  
  
> [!NOTE]  
>  En plus des procédures dans cette rubrique, vous pouvez en général définir ou modifier la propriété **ProtectionLevel** d’un package quand vous importez ou exportez le package. Vous pouvez également modifier la propriété **ProtectionLevel** d’un package quand vous utilisez l’Assistant Importation et exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour enregistrer un package.  
  
### <a name="to-set-or-change-the-protection-level-of-a-package-in-sql-server-data-tools"></a>Pour définir ou modifier le niveau de protection d'un package dans les outils de données SQL Server  
  
1.  Examinez les valeurs disponibles pour la propriété **ProtectionLevel** dans la section [Niveaux de protection](#protection-levels), puis déterminez la valeur appropriée pour votre package.  
  
2.  Dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], ouvrez le projet [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] qui contient le package.  
  
3.  Ouvrez le package dans le Concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
4.  Si la fenêtre Propriétés n'affiche pas les propriétés du package, cliquez sur l'aire de conception.  
  
5.  Dans la fenêtre Propriétés, dans le groupe **Sécurité** , sélectionnez la valeur appropriée pour la propriété **ProtectionLevel** .  
  
     Si vous sélectionnez un niveau de protection qui requiert un mot de passe, entrez le mot de passe comme valeur de la propriété **PackagePassword** .  
  
6.  Dans le menu **Fichier** , sélectionnez **Enregistrer les éléments sélectionnés** pour enregistrer le package modifié.  
  
### <a name="to-set-or-change-the-protection-level-of-packages-at-the-command-prompt"></a>Pour définir ou modifier le niveau de protection de package à l'invite de commandes  
  
1.  Examinez les valeurs disponibles pour la propriété **ProtectionLevel** dans la section [Niveaux de protection](#protection-levels), puis déterminez la valeur appropriée pour votre package.  
  
2.  Examinez les mappages pour l’option **Encrypt** dans la rubrique [Utilitaire dtutil](../../integration-services/dtutil-utility.md)et déterminez l’entier approprié à utiliser comme valeur de la propriété **ProtectionLevel** sélectionnée.  
  
3.  Ouvrez une fenêtre d’invite de commandes.  
  
4.  À l’invite de commandes, naviguez jusqu’au dossier qui contient le ou les packages pour lesquels vous souhaitez définir la propriété **ProtectionLevel** .  
  
     Les exemples de syntaxe affichés à l'étape suivante supposent que ce dossier est le dossier actif.  
  
5.  Définissez ou modifiez le niveau de protection des packages en utilisant une commande semblable à celle des exemples suivants :  
  
    -   La commande suivante définit la propriété **ProtectionLevel** d’un package dans le système de fichiers au niveau 2, « Chiffrer les données sensibles avec un mot de passe », avec le mot de passe « strongpassword » :  
  
         `dtutil.exe /file "C:\Package.dtsx" /encrypt file;"C:\Package.dtsx";2;strongpassword`  
  
    -   La commande suivante définit la propriété **ProtectionLevel** de tous les packages dans un dossier spécifique dans le système de fichiers au niveau 2, « Chiffrer les données sensibles avec un mot de passe », avec le mot de passe « strongpassword » :  
  
         `for %f in (*.dtsx) do dtutil.exe /file %f /encrypt file;%f;2;strongpassword`  
  
         Si vous utilisez une commande semblable dans un fichier de commandes, entrez l'espace réservé de fichier, « % f », comme « %% f » dans le fichier de commandes.  

## <a name="package-project-protection-level-dialog-box"></a><a name="protection_dialog"></a> Boîte de dialogue Niveau de protection du package
  Utilisez la boîte de dialogue **Niveau de protection du package** pour mettre à jour le niveau de protection d'un package. Le niveau de protection détermine la méthode de protection (par mot de passe ou par clé utilisateur) et l'étendue de la protection du package. La protection peut inclure toutes les données ou uniquement les données sensibles.  
  
 Pour comprendre les exigences et les options de la sécurité du package, il peut être utile de consulter [Vue d’ensemble de la sécurité &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md).  
  
### <a name="options"></a>Options  
 **Niveau de protection du package**  
 Sélectionnez un niveau de protection dans la liste.  
  
 **Mot de passe**  
 Si vous utilisez le niveau de protection **Chiffrer les données sensibles avec un mot de passe** ou **Chiffrer toutes les données avec un mot de passe** , tapez un mot de passe.  
  
 **Retapez le mot de passe**  
 Entrez à nouveau le mot de passe.  

## <a name="package-password-dialog-box"></a><a name="password_dialog"></a> Boîte de dialogue Mot de passe du package
  Utilisez la boîte de dialogue **Mot de passe du package** pour fournir le mot de passe d'un package qui est chiffré par un mot de passe. Vous devez fournir un mot de passe si le package utilise le niveau de protection **Chiffrer les données sensibles avec un mot de passe** ou **Chiffrer toutes les données avec un mot de passe** .  
  
### <a name="options"></a>Options  
 **Mot de passe**  
 Entrez le mot de passe.  
  
## <a name="see-also"></a>Voir aussi  
 [Packages Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-packages.md)   
 [Vue d’ensemble de la sécurité &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md)  
 [Utilitaire dtutil](../../integration-services/dtutil-utility.md)  
