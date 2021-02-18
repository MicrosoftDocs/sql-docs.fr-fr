---
title: Aide de l’Assistant Installation | Microsoft Docs
description: Utilisez Configuration d’une instance de l’Assistant Installation de SQL Server pour spécifier s’il faut créer une instance par défaut ou une instance nommée de SQL Server.
ms.custom: ''
ms.date: 08/16/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
f1_keywords:
- instance configuration, Setup
helpviewer_keywords:
- Instance Name page [SQL Server Installation Wizard]
- SQL Server Installation Wizard, Instance Name page
ms.assetid: 5bf822fc-6dec-4806-a153-e200af28e9a5
author: cawrites
ms.author: chadam
robots: noindex,nofollow
ms.openlocfilehash: 7b678ac25174b75db198739e0e9bc191d29f18d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336170"
---
# <a name="installation-wizard-help"></a>Aide de l’assistant d’installation

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article décrit quelques-unes des pages de configuration de l’Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="instance-configuration-page"></a>Page Configuration de l’instance

Utilisez la page **Configuration d’une instance** de l’Assistant Installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour spécifier s’il faut créer une instance par défaut ou une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas déjà installée, une instance par défaut est créée, sauf si vous spécifiez une instance nommée.  
  
Chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consiste en un ensemble distinct de services avec des paramètres spécifiques pour les classements et les autres options. La structure des répertoires, la structure des registres et les noms de services reflètent tous le nom de l'instance et l'ID spécifique de l'instance créé durant l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Une instance est soit l'instance par défaut, soit une instance nommée. MSSQLSERVER est le nom de l'instance par défaut. Il n'est pas requis par le nom par défaut qu'un client spécifie le nom de l'instance pour établir une connexion. Une instance nommée est déterminée par l'utilisateur au cours de l'installation. Vous pouvez installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme instance nommée sans installer d'abord l'instance par défaut. Une seule installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], indépendamment de la version, peut être à un moment l'instance par défaut.  
  
> [!NOTE]  
> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep vous permet de spécifier le nom de l’instance quand vous finalisez une instance préparée dans la page **Configuration de l’instance**. Vous pouvez configurer l'instance préparée que vous finalisez comme instance par défaut s'il n'existe aucune instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur la machine.  
  
### <a name="multiple-instances"></a>Instances multiples
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prend en charge plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un seul serveur ou processeur, mais une seule instance peut être l'instance par défaut. Toutes les autres instances doivent être des instances nommées. Un ordinateur peut exécuter simultanément plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et chaque instance s'exécute indépendamment des autres.  
  
 Pour plus d’informations, consultez [Spécifications des capacités maximales pour SQL Server](../maximum-capacity-specifications-for-sql-server.md).  
  
### <a name="options"></a>Options

**Instances de cluster de basculement uniquement** : Spécifiez le nom réseau du cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ce nom identifie l'instance de cluster de basculement sur le réseau.  
  
**Choisir entre une valeur par défaut et une instance nommée** : Prenez les informations suivantes en compte lorsque vous décidez d’installer une instance par défaut ou une instance nommée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
* Si vous envisagez d'installer une seule instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur un serveur de bases de données, ce doit être une instance par défaut.  
  
* Utilisez une instance nommée lorsque vous envisagez plusieurs instances sur le même ordinateur. Un serveur ne peut héberger qu'une seule instance par défaut.  
* Toute application qui installe [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] doit l'installer en tant qu'instance nommée. Cette pratique réduit au minimum les conflits liés à l'installation de plusieurs applications sur le même ordinateur.
  
**Instance par défaut** : Choisissez cette option pour installer une instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un ordinateur ne peut héberger qu'une instance par défaut ; toutes les autres instances doivent être nommées. Toutefois, si vous avez une instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installée, vous pouvez ajouter une instance par défaut de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sur le même ordinateur.  
  
**Instance nommée** : Sélectionnez cette option pour créer une nouvelle instance nommée. Prenez en compte les informations suivantes lorsque vous nommez une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
* Les noms d'instance ne respectent pas la casse.  
  
* Les noms d'instance ne peuvent pas commencer ni se terminer par un trait de soulignement (_).  
  
* Les noms d'instance ne peuvent pas contenir le terme « Défaut » ou autres mots clés réservés. Si un mot clé réservé est utilisé dans un nom d'instance, une erreur d’installation se produit. Pour plus d’informations, consultez [Mots clés réservés &#40;Transact-SQL&#41;](../../t-sql/language-elements/reserved-keywords-transact-sql.md).  
  
* Si vous spécifiez MSSQLSERVER comme nom d'instance, une instance par défaut est créée.  
  
* Une installation de [!INCLUDE[ssGeminiLong](../../includes/ssgeminilong-md.md)] est toujours installée en tant qu’instance nommée de « [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] ». Vous ne pouvez pas spécifier de nom d'instance différent pour ce rôle de fonctionnalité.  
  
* Les noms d'instance sont limités à 16 caractères.  
  
* Le premier caractère d'un nom d'instance doit être une lettre. Les lettres acceptables sont celles définies par la norme Unicode 2.0. Elles incluent les caractères latins a-z, A-Z et les caractères littéraux d’autres langues.  
  
* Les autres caractères d'un nom d'instance peuvent être des lettres définies par le standard Unicode 2.0, des nombres décimaux provenant de scripts de latin de base ou nationaux, le symbole dollar ($) ou le trait de soulignement (_).  
  
* Les espaces incorporés ou autres caractères spéciaux ne sont pas autorisés dans les noms d'instance. La barre oblique inverse (\\), la virgule (,), les deux points (:), le point-virgule (;), le guillemet simple ('), l’esperluette (&), le tiret (-) et l’arobase (@) ne sont pas autorisés non plus.  
  
  Seuls les caractères valides dans la page de codes Windows actuelle peuvent être utilisés dans les noms d’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En cas d'utilisation d'un caractère Unicode non pris en charge, une erreur d’installation se produit.  
  
**Instances et fonctionnalités détectées** : Consultez la liste des instances et des composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installés sur l'ordinateur sur lequel le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est en cours d'exécution.  
  
**ID de l'instance** : Par défaut, le nom de l'instance est utilisé comme ID d'instance. Cet ID permet d'identifier les répertoires d'installation et les clés de Registre de votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le même comportement se produit pour les instances par défaut et les instances nommées. Pour une instance par défaut, le nom de l'instance et l'ID d'instance sont MSSQLSERVER. Pour utiliser un ID d’instance non défini par défaut, spécifiez-le dans le champ **ID d’instance**.  
  
> [!IMPORTANT]  
>  Avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep, l'ID d'instance affiché dans la page **Configuration d’instance** correspond à l'ID d'instance spécifié pendant l'étape de préparation d'image du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SysPrep. Vous ne serez pas en mesure de spécifier un ID d'instance différent pendant l'étape de finalisation d'image.

> [!NOTE]  
>  Les ID d'instance qui commencent par un trait de soulignement (_) ou qui contiennent le signe dièse (#) ou le symbole dollar ($) ne sont pas pris en charge.  
  
Pour plus d’informations sur les répertoires, les emplacements de fichiers et l’affectation de noms aux ID d’instance, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
Tous les composants d'une instance donnée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont gérés comme une unité. Tous les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Services Pack et mises à niveau s'appliquent à chaque composant d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Tous les composants de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui partagent le même nom d'instance doivent remplir les critères suivants :  
  
* Même version
* Même édition
* Mêmes paramètres de langue
* Même état cluster
* Même système d'exploitation  
  
> [!NOTE]  
> [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] n’est pas sensible aux clusters.  

## <a name="analysis-services-configuration---account-provisioning-page"></a>Page Configuration Analysis Services - Approvisionnement du compte
  
Utilisez cette page pour définir le mode serveur et pour octroyer des autorisations administratives aux utilisateurs ou services qui requièrent un accès non restreint à [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. L’installation n’ajoute pas automatiquement le groupe Windows local BUILTIN\Administrateurs au rôle des administrateurs de serveur [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de l’instance que vous installez. Si vous souhaitez ajouter le groupe Administrateurs local au rôle des administrateurs de serveur, vous devez spécifier explicitement ce groupe.  
  
Si vous installez [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)], vous devez octroyer des autorisations administratives aux administrateurs de batterie SharePoint ou aux administrateurs de service qui sont responsables d'un déploiement de serveurs [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] dans une batterie [!INCLUDE[SPS2010](../../includes/sps2010-md.md)].  
  
### <a name="options"></a>Options

**Mode serveur** : Le mode serveur spécifie le type de bases de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui peuvent être déployées sur le serveur. Les modes serveur sont déterminés pendant l’installation et ne peuvent pas être modifiés par la suite. Tous les modes s'excluent mutuellement, ce qui signifie que vous aurez besoin de deux instances de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], chacune configurée pour un mode différent, pour prendre en charge à la fois les solutions de traitement analytique en ligne (OLAP) classiques et de modèle tabulaire.  
  
**Spécifiez les administrateurs** : Vous devez spécifier au moins un administrateur du serveur pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les utilisateurs ou groupes que vous spécifiez deviendront membres du rôle Administrateurs du serveur de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que vous installez. Ces membres doivent avoir des comptes d'utilisateur de domaine Windows dans le même domaine que l'ordinateur sur lequel vous installez le logiciel.  
  
> [!NOTE]  
> Le contrôle de compte d'utilisateur (UAC) est une fonctionnalité de sécurité Windows qui requiert qu'un administrateur approuve de façon spécifique les actions ou applications administratives avant qu'elles soient autorisées à s'exécuter. Dans la mesure où le contrôle de compte d'utilisateur est activé par défaut, vous serez invité à autoriser des opérations spécifiques qui requièrent des privilèges élevés. Vous pouvez configurer le contrôle de compte d'utilisateur de manière à modifier son comportement par défaut ou à le personnaliser pour des programmes spécifiques. Pour plus d’informations sur la configuration des contrôles de compte d’utilisateur, consultez le [Guide pas à pas du contrôle de compte d’utilisateur](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc709691(v=ws.10)) et [Contrôle de compte d’utilisateur (Wikipédia)](https://go.microsoft.com/fwlink/?linkid=196351).  
  
### <a name="see-also"></a>Voir aussi
  
* [Configurer les comptes de service &#40;Analysis Services&#41;](/analysis-services/instances/configure-service-accounts-analysis-services)
* [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  

## <a name="analysis-services-configuration---data-directories-page"></a>Page Configuration de Analysis Services - Répertoires de données

Les répertoires par défaut indiqués dans le tableau suivant sont configurables par l'utilisateur pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’autorisation d’accéder à ces fichiers est octroyée aux administrateurs locaux et aux membres du groupe de sécurité SQLServerMSASUser$\<instance> créé et configuré lors de l’installation.  
  
### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur  
  
|Description|Répertoire par défaut|Recommandations|  
|-----------------|-----------------------|---------------------|  
|**Répertoire racine de données**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data\ |Vérifiez que le dossier \Program files\Microsoft SQL Server\ est protégé par des autorisations limitées. Le fonctionnement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dépend, dans de nombreuses configurations, de la performance de l'emplacement de stockage sur lequel le répertoire de données se trouve. Placez ce répertoire sur le stockage le plus performant joint au système. Pour les installations de cluster de basculement, vérifiez que les répertoires de données sont placés sur le disque partagé.|  
|**Répertoire de fichiers journaux**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log\ |Il s'agit du répertoire des fichiers journaux [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ; il inclut le journal FlightRecorder. Si vous augmentez la durée de la boîte noire, assurez-vous que le répertoire de journal dispose de l'espace adéquat.|  
|**Répertoire temporaire**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp\ |Placez le Répertoire temporaire sur le sous-système de stockage haute performance.|  
|**Répertoire de sauvegarde**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup\ |Il s'agit du répertoire pour les fichiers de sauvegarde par défaut [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour des installations de SharePoint, ce répertoire est également l’emplacement où le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] met en cache les fichiers de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].<br /><br /> Vérifier que des autorisations appropriées sont définies pour empêcher la perte de données et que le groupe d'utilisateurs pour [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dispose des autorisations adéquates pour écrire dans le répertoire de sauvegarde. L'utilisation d'un lecteur mappé pour les répertoires de sauvegarde n'est pas prise en charge.|  
  
### <a name="considerations"></a>Considérations  
  
* Lorsque les instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont déployées sur une batterie SharePoint, elles stockent des fichiers d'application, des fichiers de données et des propriétés dans les bases de données de contenu et les bases de données d'application de service.  
  
* Lors de l'ajout de fonctionnalités à une installation existante, vous ne pouvez ni modifier l'emplacement d'une fonctionnalité précédemment installée, ni spécifier l'emplacement d'une nouvelle fonctionnalité.  

* Vous pouvez être amené à configurer des logiciels d’analyse, tel que des applications antivirus et anti-espions, pour exclure les dossiers et les types de fichiers SQL Server. Pour plus d’informations, lisez cet article du support technique : [Logiciels antivirus sur les ordinateurs exécutant SQL Server](https://support.microsoft.com/kb/309422).
  
* Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ne partagez pas de répertoires dans cette boîte de dialogue avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Installez également les composants [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des répertoires distincts.  
  
* Les programmes et les fichiers de données ne peuvent pas être installés aux emplacements suivants :  
  
  * sur un lecteur de disque amovible ;  
  * dans un système de fichiers utilisant la compression ;  
  * dans un répertoire où figurent des fichiers système ;  
  
### <a name="see-also"></a>Voir aussi

Pour plus d’informations sur les répertoires, les emplacements de fichiers et l’affectation de noms aux ID d’instance, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md).  
  
### <a name="analysis-services-configuration---data-directories-page"></a>Page Configuration de Analysis Services - Répertoires de données

Les répertoires par défaut indiqués dans le tableau suivant sont configurables par l'utilisateur pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L’autorisation d’accéder à ces fichiers est octroyée aux administrateurs locaux et aux membres du groupe de sécurité SQLServerMSASUser$\<instance> créé et configuré lors de l’installation.  
  
#### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur
  
|Description|Répertoire par défaut|Recommandations|  
|-----------------|-----------------------|---------------------|  
|**Répertoire racine de données** |\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Data |Vérifiez que le dossier \Program files\Microsoft SQL Server\ est protégé par des autorisations limitées. Le fonctionnement de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] dépend, dans de nombreuses configurations, de la performance de l'emplacement de stockage sur lequel le répertoire de données se trouve. Placez ce répertoire sur le stockage le plus performant joint au système. Pour les installations de cluster de basculement, vérifiez que les répertoires de données sont placés sur le disque partagé.|  
|**Répertoire de fichiers journaux**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Log |Il s'agit du répertoire des fichiers journaux [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] ; il inclut le journal FlightRecorder. Si vous augmentez la durée de la boîte noire SQL, assurez-vous que le répertoire de journal dispose de l'espace adéquat.|  
|**Répertoire temporaire**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Temp |Placez le Répertoire temporaire sur le sous-système de stockage haute performance.|  
|**Répertoire de sauvegarde**|\<Drive:>\Program Files\Microsoft SQL Server\MSAS *nn*.\<InstanceID>\OLAP\Backup |Il s'agit du répertoire pour les fichiers de sauvegarde par défaut [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]. Pour [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour des installations de SharePoint, il s’agit également de l’emplacement où le service système [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] met en cache les fichiers de données [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)].<br /><br /> Vérifiez que des autorisations appropriées sont définies pour empêcher la perte de données et vérifiez que le groupe d'utilisateurs pour le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] a les autorisations adéquates pour écrire dans le répertoire de sauvegarde. L'utilisation d'un lecteur mappé pour les répertoires de sauvegarde n'est pas prise en charge.|  
  
#### <a name="considerations"></a>Considérations
  
* Lorsque les instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] sont déployées sur une batterie SharePoint, elles stockent des fichiers d'application, des fichiers de données et des propriétés dans les bases de données de contenu et les bases de données d'application de service.  
  
* Lors de l'ajout de fonctionnalités à une installation existante, vous ne pouvez ni modifier l'emplacement d'une fonctionnalité précédemment installée, ni spécifier l'emplacement d'une nouvelle fonctionnalité.  

* Vous pouvez être amené à configurer des logiciels d’analyse, tel que des applications antivirus et anti-espions, pour exclure les dossiers et les types de fichiers SQL Server. Pour plus d’informations, consultez [Logiciels antivirus sur des ordinateurs exécutant SQL Server](https://support.microsoft.com/kb/309422).
  
* Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ne partagez pas les répertoires dans cette boîte de dialogue avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Installez également les composants [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des répertoires distincts.  
  
* Les programmes et les fichiers de données ne peuvent pas être installés aux emplacements suivants :  
  
  * sur un lecteur de disque amovible ;  
  * dans un système de fichiers utilisant la compression ;  
  * dans un répertoire où figurent des fichiers système ;  
  
#### <a name="see-also"></a>Voir aussi

* Pour plus d’informations sur les répertoires, les emplacements de fichiers et l’affectation de noms aux ID d’instance, consultez [Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](file-locations-for-default-and-named-instances-of-sql-server.md)  
* [Autorisations de partage et NTFS sur un serveur de fichiers](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)

## <a name="database-engine-configuration---server-configuration-page"></a>Page <a name="serverconfig"></a> Configuration du moteur de base de données - Configuration du serveur

Utilisez cette page pour définir le mode de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et ajouter des utilisateurs ou des groupes Windows en tant qu’administrateurs de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].  
  
### <a name="considerations-for-running-sscurrent"></a>Éléments à prendre en considération pour l'exécution de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]

Dans les versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le groupe BUILTIN\Administrators était fourni en tant que connexion dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] et les membres du groupe Administrateurs local pouvaient se connecter en utilisant leurs informations d’identification d’administrateur. Cependant, l'utilisation d'autorisations élevées n'est pas une meilleure pratique.

Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] , le groupe BUILTIN\Administrators n’est pas fourni en tant que connexion. Assurez-vous de créer une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour chaque utilisateur administrateur et ajoutez cette connexion au rôle serveur fixe **sysadmin** lors de l'installation d'une nouvelle instance de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. Faites de même pour des comptes Windows qui exécutent des travaux de l’agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], y compris les travaux de l’agent de réplication.  
  
### <a name="options"></a>Options

**Mode de sécurité** : Sélectionnez **Authentification Windows** ou **Authentification en mode mixte** pour votre installation.  
  
**Approvisionnement principal Windows** : Dans les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le groupe local de Windows BUILTIN\Administrateurs a été placé dans le rôle serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin**, en octroyant aux administrateurs Windows l'accès à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], le groupe BUILTIN\Administrateurs n'est pas configuré dans le rôle serveur **sysadmin**. Au lieu de cela, vous devez configurer de manière explicite des administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les nouvelles installations lors de l'installation.  
  
> [!IMPORTANT]  
> Vous devez configurer de manière explicite des administrateurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les nouvelles installations lors de l'installation. L’installation ne vous permet pas de continuer tant que cette étape n'a pas été terminée.
  
**Spécifiez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Administrateurs** : Vous devez spécifier au moins un principal Windows pour l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour ajouter le compte sous lequel l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute, sélectionnez le bouton **Ajouter l'utilisateur actuel**. Pour ajouter ou supprimer des comptes dans la liste des administrateurs système, sélectionnez **Ajouter** ou **Supprimer**, puis modifiez la liste des utilisateurs, groupes ou ordinateurs qui disposent des privilèges d'administrateur pour l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
Lorsque vous avez terminé de modifier la liste, sélectionnez **OK**, puis vérifiez la liste des administrateurs dans la boîte de dialogue de configuration. Si la liste est complète, sélectionnez **Suivant**.  
  
Si vous sélectionnez **Authentification en mode mixte**, vous devez fournir les informations d'identification de connexion pour le compte de l'administrateur système [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intégré (**sa**).  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]  
  
**Mode d'authentification Windows** : Quand un utilisateur se connecte par le biais d'un compte d'utilisateur Windows, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] valide le nom et le mot de passe du compte à l'aide du jeton du principal Windows du système d'exploitation. L’authentification Windows est le mode d’authentification par défaut et offre une sécurité très supérieure à l’authentification en mode mixte. L'authentification Windows utilise le protocole de sécurité Kerberos, met en œuvre les stratégies de mot de passe en termes de validation de la complexité des mots de passe forts et prend en charge le verrouillage des comptes et l'expiration des mots de passe.  
  
> [!IMPORTANT]  
> [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  

> [!IMPORTANT]  
> [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)] Ne définissez jamais un mot de passe d'administrateur système **sa** vide ou faible.  
  
**Mode mixte (Authentification Windows ou authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)])**  : Permet aux utilisateurs de se connecter en utilisant l'authentification Windows ou l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les utilisateurs qui se connectent via un compte d'utilisateur Windows peuvent utiliser des connexions approuvées qui sont validées par Windows.  
  
Si vous devez choisir l'authentification en mode mixte et utiliser des connexions SQL pour vous adapter à des applications héritées, définissez des mots de passe forts pour tous les comptes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
> L'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est fournie uniquement dans un souci de compatibilité descendante. [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
**Entrez le mot de passe** : Entrez et confirmez la connexion d'administrateur système (**sa**). Les mots de passe constituant la première ligne de défense contre les intrus, le choix de mots de passe forts est essentiel à la sécurité de votre système. Ne définissez jamais un mot de passe d'administrateur système **sa** vide ou faible.  
  
> [!NOTE]  
> Les mots de passe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent contenir de 1 à 128 caractères, constitués d’une combinaison quelconque de lettres, de symboles et de chiffres. Si vous choisissez l'authentification en mode mixte, vous devez entrer un mot de passe **sa** fort avant de passer à la page suivante de l'Assistant Installation.  
  
#### <a name="strong-password-guidelines"></a>Instructions sur les mots de passe forts
  
Les mots de passe forts ne peuvent pas être aisément devinés par une personne et ils ne sont pas facilement piratés par un logiciel. Les mots de passe forts ne peuvent pas utiliser des conditions ou des termes interdits, notamment :  
  
* Une condition vide ou NULL
* « Mot de passe »
* "Admin"
* "Administrator"
* "sa"
* "sysadmin"

Un mot de passe fort ne peut pas se composer des termes suivants associés à l'ordinateur d'installation :  
  
* Le nom de l'utilisateur qui a ouvert la session sur la machine
* Nom de l’ordinateur  
  
Un mot de passe fort doit contenir plus de 8 caractères et satisfaire au moins trois des quatre critères suivants :  
  
* Il doit contenir des lettres majuscules.
* Il doit contenir des lettres minuscules.  
* Il doit contenir des chiffres.
* Il doit contenir des caractères non alphanumériques, comme #, % ou ^.  
  
Les mots de passe entrés sur cette page doivent répondre aux exigences des stratégies de mots de passe forts. Si vous avez une automation qui utilise l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], assurez-vous que le mot de passe répond aux exigences des stratégies de mots de passe forts.  
  
### <a name="see-also"></a>Voir aussi

Pour plus d'informations sur le choix de l'authentification Windows ou de l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez la rubrique [Choisir un mode d’authentification](../../relational-databases/security/choose-an-authentication-mode.md).  

Pour plus d’informations sur le choix d’un compte pour l’exécution de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)], consultez [Configurer les comptes et autorisations de service Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md).

## <a name="database-engine-configuration---data-directories-page"></a>Page <a name ="datadir"></a> Configuration du moteur de base de données - Répertoires de données

Cette page vous permet de spécifier l’emplacement de l’installation des programmes et fichiers de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Selon le type d'installation, le stockage pris en charge peut inclure un disque local, un stockage partagé ou un serveur de fichiers SMB.  
  
Pour spécifier un partage de fichiers SMB comme répertoire, vous devez taper manuellement le chemin d'accès UNC pris en charge. La navigation vers un partage de fichiers SMB n'est pas prise en charge. L’exemple suivant indique le format de chemin d’accès UNC pris en charge d’un partage de fichiers SMB :

`\\<ServerName>\<ShareName>\...`

### <a name="standalone-instance-of-ssnoversion"></a>Instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
Pour une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le tableau suivant répertorie les types de stockage et les répertoires par défaut pris en charge à configurer lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur
  
|Description|Types de stockage pris en charge|Répertoire par défaut|Recommandations|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Répertoire racine de données**|Disque local, serveur de fichiers SMB, stockage partagé* |\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\ |l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure les listes de contrôle d'accès (ACL) pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompt l'héritage dans le cadre de la configuration.|  
|**Répertoire de base de données utilisateur**|Disque local, serveur de fichiers SMB, stockage partagé*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data |Les meilleures pratiques recommandées pour les répertoires de données utilisateur dépendent de la charge de travail et des exigences en matière de performances.|  
|**Répertoire du journal de la base de données utilisateur**|Disque local, serveur de fichiers SMB, stockage partagé*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Assurez-vous que le répertoire du journal a un espace adéquat.|  
|**Répertoire de sauvegarde**|Disque local, serveur de fichiers SMB, stockage partagé*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup|Définissez les autorisations appropriées pour empêcher la perte de données et vérifiez que le compte d'utilisateur pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a les autorisations adéquates pour écrire dans le répertoire de sauvegarde. L'utilisation d'un lecteur mappé pour les répertoires de sauvegarde n'est pas prise en charge.|  
  
\* Bien que les disques partagés soient pris en charge, nous ne recommandons pas leur utilisation pour une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="failover-cluster-instance-of-ssnoversion"></a>Instance de cluster de basculement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Pour une instance de cluster de basculement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le tableau suivant répertorie les types de stockage et les répertoires par défaut pris en charge à configurer lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Description|Types de stockage pris en charge|Répertoire par défaut|Recommandations|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Répertoire racine de données**|Stockage partagé, serveur de fichiers SMB|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\\<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|L’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure les listes de contrôle d'accès (ACL) pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompt l'héritage dans le cadre de la configuration.|  
|**Répertoire de base de données utilisateur**|Stockage partagé, serveur de fichiers SMB|\<Drive:>Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|Les meilleures pratiques recommandées pour les répertoires de données utilisateur dépendent de la charge de travail et des exigences en matière de performances.|  
|**Répertoire du journal de la base de données utilisateur**|Stockage partagé, serveur de fichiers SMB|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|Assurez-vous que le répertoire du journal a un espace adéquat.|  
|**Répertoire de sauvegarde**|Disque local, stockage partagé, serveur de fichiers SMB|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Backup<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|Définissez les autorisations appropriées pour empêcher la perte de données et vérifiez que le compte d'utilisateur pour le service SQL Server a les autorisations adéquates pour écrire dans le répertoire de sauvegarde. L'utilisation d'un lecteur mappé pour les répertoires de sauvegarde n'est pas prise en charge.|  
  
### <a name="security-considerations"></a>Considérations relatives à la sécurité
  
Le programme d'installation configurera les listes de contrôle d'accès pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompra l'héritage dans le cadre de la configuration.  
  
Les suggestions suivantes s'appliquent aux serveurs de fichiers SMB :  
  
* Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un compte de domaine si un serveur de fichiers SMB est utilisé.  
  
* Le compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir des autorisations NTFS de contrôle complet sur le dossier de partage de fichiers SMB utilisé comme répertoire de données.  
  
* Des privilèges SeSecurityPrivilege sur le serveur de fichiers SMB doivent être accordés au compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour accorder ce privilège, utilisez la console de stratégie de sécurité locale sur le serveur de fichiers pour ajouter le compte utilisé pour l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la stratégie **Gérer le journal d'audit et de la sécurité** . Ce paramètre se trouve dans la section **Affectations des droits d’utilisateur** sous **Stratégies locales** dans la console Stratégie de sécurité locale.

### <a name="considerations"></a>Considérations
  
* Lors de l'ajout de fonctionnalités à une installation existante, vous ne pouvez ni modifier l'emplacement d'une fonctionnalité précédemment installée, ni spécifier l'emplacement d'une nouvelle fonctionnalité.  
  
* Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ne partagez pas les répertoires dans cette boîte de dialogue avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Installez également les composants [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans des répertoires distincts.  
  
* Les programmes et les fichiers de données ne peuvent pas être installés aux emplacements suivants :  
  
  * sur un lecteur de disque amovible ;
  * dans un système de fichiers utilisant la compression ;
  * dans un répertoire où figurent des fichiers système ;
  * sur un lecteur réseau mappé sur une instance de cluster de basculement.  
  
## <a name="a-nametempdb-database-engine-configuration---tempdb-page"></a>Page <a name="tempdb"><a/> Configuration du moteur de base de données - TempDB

Utilisez cette page pour spécifier l’emplacement, la taille, les paramètres de croissance du fichier journal et des données **tempdb** et le nombre de fichiers pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Selon le type d'installation, le stockage pris en charge peut inclure un disque local, un stockage partagé ou un serveur de fichiers SMB.  
  
Pour spécifier un partage de fichiers SMB comme répertoire, vous devez taper manuellement le chemin d'accès UNC pris en charge. La navigation vers un partage de fichiers SMB n'est pas prise en charge. L’exemple suivant indique le format de chemin d’accès UNC pris en charge d’un partage de fichiers SMB :

`\\<ServerName>\<ShareName>\....`
  
### <a name="data-and-log-directories-for-a-standalone-instance-of-ssnoversion"></a>Répertoires de données et des journaux pour une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Pour les instances autonomes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le tableau suivant répertorie les types de stockage pris en charge et les répertoires par défaut que vous pouvez configurer lors de l’installation.  
  
|Description|Type de stockage pris en charge|Répertoire par défaut|Recommandations|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**Répertoires de données**|Disque local, serveur de fichiers SMB, stockage partagé* |\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|L’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure les listes de contrôle d'accès (ACL) pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompt l'héritage dans le cadre de la configuration.<br /><br /> Les meilleures pratiques pour les répertoires **temdb** dépendent de la charge de travail et des exigences en matière de performances. Spécifiez plusieurs dossiers/lecteurs pour répartir les fichiers de données sur plusieurs volumes.|  
|**Répertoire du journal**|Disque local, serveur de fichiers SMB, stockage partagé*|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data|Assurez-vous que le répertoire du journal a un espace adéquat.|  
  
\* Bien que les disques partagés soient pris en charge, nous ne recommandons pas leur utilisation pour une instance autonome de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
### <a name="data-and-log-directories-for-a-failover-cluster-instance-of-ssnoversion"></a>Répertoires de données et des journaux pour une instance de cluster de basculement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Pour une instance de cluster de basculement de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le tableau suivant répertorie les types de stockage et les répertoires par défaut pris en charge à configurer lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Description|Type de stockage pris en charge|Répertoire par défaut|Recommandations|  
|-----------------|----------------------------|-----------------------|---------------------|  
|**répertoire de données tempdb**|Disque local, stockage partagé, serveur de fichiers SMB|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\Data<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|L’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure les listes de contrôle d'accès (ACL) pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompt l'héritage dans le cadre de la configuration.<br /><br /> Assurez-vous que le ou les répertoires spécifiés (si plusieurs fichiers sont spécifiés) sont valides pour tous les nœuds de cluster. Pendant le basculement, si les répertoires **tempdb** ne sont pas disponibles sur le nœud de basculement cible, la ressource SQL Server ne sera pas en ligne.|  
|**répertoire du journal tempdb**|Disque local, stockage partagé, serveur de fichiers SMB|\<Drive:>\Program Files\\[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]\MSSQL *nn*.\<InstanceID>\MSSQL\Data<br /><br /> **Conseil** : Si le **disque partagé** a été sélectionné dans la page **Sélection du disque du cluster**, la valeur par défaut est le premier disque partagé. La valeur par défaut de ce champ est vide si aucune sélection n'a été effectuée à la page **Sélection du disque du cluster**.|Les meilleures pratiques recommandées pour les répertoires de données utilisateur dépendent de la charge de travail et des exigences en matière de performances.<br /><br /> Assurez-vous que le répertoire spécifié est valide pour tous les nœuds du cluster. Pendant le basculement, si les répertoires **tempdb** ne sont pas disponibles sur le nœud de basculement cible, la ressource SQL Server ne sera pas en ligne.<br /><br /> Assurez-vous que le répertoire du journal a un espace adéquat.|  
  
### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur

Configurez les paramètres de **tempdb** en fonction de votre charge de travail et de la configuration requise. Les paramètres suivants s’appliquent aux fichiers de données **tempdb** :  
  
* **Nombre de fichiers** : nombre total de fichiers de données pour **tempdb**. La valeur par défaut est inférieure à 8 ou elle correspond au nombre de cœurs logiques détectés par l’installation. En règle générale, si le nombre de processeurs logiques est inférieur ou égal à 8, utilisez le même nombre de fichiers de données que de processeurs logiques. Si le nombre de processeurs logiques est supérieur à 8, utilisez 8 fichiers de données. Si une contention survient, augmentez le nombre de fichiers de données par des multiples de 4 (jusqu’au nombre de processeurs logiques) jusqu’à ce que la contention atteigne un niveau acceptable ou modifiez la charge de travail/le code.
  
* **Taille initiale (Mo)**  : la taille initiale en mégaoctets de chaque fichier de données **tempdb**. La valeur par défaut est de 8 Mo (ou de 4 Mo pour [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)] introduit une taille initiale maximale de fichier de 262 144 Mo (256 Go). [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] avait une taille initiale maximale de fichier de 1 024 Mo. Tous les fichiers de données **tempdb** ont la même taille initiale. Étant donné que **tempdb** est recréé à chaque fois que SQL Server démarre ou est basculé, spécifiez une taille proche de la taille requise par votre charge de travail pour une opération normale. Pour optimiser davantage la création de **tempdb** lors du démarrage, activez [initialisation instantanée des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).  
  
* **Taille initiale totale (Mo)** : la taille cumulée de tous les fichiers de données **tempdb**.  
  
* **Croissance automatique (Mo)** : la quantité d’espace en mégaoctets selon laquelle chaque fichier de données **tempdb** croît automatiquement en cas de manque d’espace. Dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et version ultérieure, tous les fichiers de données augmentent en taille en même temps en fonction de la quantité spécifiée dans ce paramètre.  
  
* **Croissance automatique totale (Mo)** : taille cumulée de chaque événement de croissance automatique.  
* **Répertoires de données** : affiche tous les répertoires qui contiennent des fichiers de données **tempdb**. Lorsqu’il existe plusieurs répertoires, les fichiers de données sont placés dans les répertoires de manière alternée. Par exemple, si vous créez 3 répertoires et spécifiez 8 fichiers de données, les fichiers de données 1, 4 et 7 seront créés dans le premier répertoire. Les fichiers de données 2, 5 et 8 seront créés dans le deuxième répertoire. Les fichiers de données 3 et 6 seront créés dans le troisième répertoire.  
  
* Pour ajouter des répertoires, sélectionnez **Ajouter...** .  
  
* Pour supprimer un répertoire, sélectionnez le répertoire, puis sélectionnez **Supprimer**.  
  
**Fichier journal TempDB** : le nom du fichier journal. Ce fichier est créé automatiquement. Les paramètres suivants s’appliquent uniquement aux fichiers journaux **tempdb** :  
  
* **Taille initiale (Mo)** : taille initiale du fichier journal **tempdb** . La valeur par défaut est de 8 Mo (ou de 4 Mo pour [!INCLUDE[ssexpress](../../includes/ssexpress_md.md)]). [!INCLUDE[sssql17](../../includes/sssql17-md.md)] introduit une taille initiale maximale de fichier de 262 144 Mo (256 Go). [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] avait une taille initiale maximale de fichier de 1 024 Mo. Étant donné que **tempdb** est recréé à chaque fois que SQL Server démarre ou est basculé, spécifiez une taille proche de la taille requise par votre charge de travail pour une opération normale. Pour optimiser davantage la création de **tempdb** lors du démarrage, activez [initialisation instantanée des fichiers de base de données](../../relational-databases/databases/database-instant-file-initialization.md).  
  
  > [!NOTE]
  > **tempdb** utilise une journalisation minimale. Le fichier journal **tempdb** ne peut pas être sauvegardé. Il est recréé à chaque fois que SQL Server démarre ou lorsqu’une instance de cluster bascule.

* **Croissance automatique (Mo)** : incrément de croissance du journal **tempdb** en mégaoctets.  La valeur par défaut de 64 Mo crée le nombre optimal de fichiers journaux virtuels pendant l’initialisation.  

  > [!NOTE]
  > Les fichiers journaux **tempdb** n’utilisent pas l’initialisation instantanée des fichiers de la base de données.
  
* **Répertoire du journal** : répertoire où les fichiers journaux **tempdb** sont créés. Il existe un seul répertoire du journal **tempdb**.  
  
### <a name="security-considerations"></a>Considérations relatives à la sécurité
  
L’installation configure les listes de contrôle d'accès (ACL) pour les répertoires [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et rompt l'héritage dans le cadre de la configuration.  

Les recommandations suivantes s'appliquent au serveur de fichiers SMB :  
  
* Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit être un compte de domaine si un serveur de fichiers SMB est utilisé.  
  
* Le compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit disposer d’autorisations NTFS de **contrôle total** sur le dossier de partage de fichiers SMB utilisé comme répertoire de données.  
  
* Des privilèges SeSecurityPrivilege sur le serveur de fichiers SMB doivent être accordés au compte utilisé pour installer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour accorder ce privilège, utilisez la console de stratégie de sécurité locale sur le serveur de fichiers pour ajouter le compte utilisé pour l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la stratégie **Gérer le journal d'audit et de la sécurité** . Ce paramètre se trouve dans la section **Affectations des droits d’utilisateur** sous **Stratégies locales** dans la console Stratégie de sécurité locale.  
  
> [!NOTE]
> Si vous spécifiez des répertoires d'installation autres que les répertoires par défaut, assurez-vous que les dossiers d'installation sont uniques pour cette instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aucun des répertoires dans cette boîte de dialogue ne doit être partagé avec des répertoires d'autres instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les composants [!INCLUDE[ssDE](../../includes/ssde-md.md)] et [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent également être installés dans des répertoires distincts.
  
### <a name="see-also"></a>Voir aussi

* [Configurer les comptes de service Windows et les autorisations](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)
* [Autorisations de partage et NTFS sur un serveur de fichiers](/iis/web-hosting/configuring-servers-in-the-windows-web-platform/configuring-share-and-ntfs-permissions)  

<!--
The MaxDOP setting applies only to SQL Server 2019 and later.
-->

::: moniker range=">=sql-server-ver15"

## <a name="a-namemaxdop-database-engine-configuration---maxdop-page"></a>Page de configuration du moteur de base de données <a name="maxdop"><a/> - MaxDOP

Le **degré maximal de parallélisme (MaxDOP)** détermine le nombre maximal de processeurs que peut utiliser une seule instruction. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduit la possibilité de configurer cette option lors de l’installation. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] détecte également automatiquement le paramètre MaxDOP recommandé pour le serveur en fonction du nombre de cœurs.  

Si cette page est ignorée lors de l’installation, la valeur par défaut MaxDOP est la valeur recommandée affichée dans cette page au lieu de la valeur [!INCLUDE[ssde_md](../../includes/ssde_md.md)] par défaut pour les versions précédentes (0). Vous pouvez également configurer manuellement ce paramètre sur cette page et le modifier après l’installation. 

### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur

* Le **degré maximal de parallélisme (MaxDOP)** est la valeur du nombre maximal de processeurs à utiliser lors de l’exécution parallèle d’une seule instruction. La valeur par défaut s’aligne aux lignes directrices du degré maximal de parallélisme dans [Configurer l’option de configuration du serveur de degré maximal de parallélisme](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).

## <a name="a-namememory-database-engine-configuration---memory-page"></a>Page de configuration du moteur de base de données <a name="memory"><a/> - Mémoire

**min server memory** détermine la limite de mémoire inférieure que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilise pour le pool de mémoires tampons et pour d’autres caches. La valeur par défaut est 0 et la valeur recommandée est également 0. Pour plus d’informations, sur les effets de **min server memory**, consultez le [Guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

**max server memory** détermine la limite de mémoire supérieure que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] utilise pour le pool de mémoires tampons et pour d’autres caches. La valeur par défaut est de 2 147 483 647 mégaoctets (Mo) et les valeurs calculées recommandées sont alignées avec les lignes directrices de configuration de la mémoire dans les [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md#manually) pour une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autonome, en fonction de la mémoire système existante. Pour plus d’informations, sur les effets de **max server memory**, consultez le [Guide d’architecture de gestion de la mémoire](../../relational-databases/memory-management-architecture-guide.md#effects-of-min-and-max-server-memory).

Si cette page est ignorée lors de l’installation, la valeur **max server memory** utilisée par défaut est la valeur [!INCLUDE[ssde_md](../../includes/ssde_md.md)] (2 147 483 647 mégaoctets). Vous pouvez configurer manuellement ces paramètres sur cette page une fois que vous avez choisi la case d’option **Recommandé**, et vous pouvez modifier ces paramètres après l’installation. Pour en savoir plus, consultez les [Options de configuration de la mémoire du serveur](../../database-engine/configure-windows/server-memory-server-configuration-options.md).

### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur
  
**Par défaut** : Cette case d’option est sélectionnée par défaut et définit les paramètres **min server memory** et **max server memory** des valeurs par défaut [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. 

**Recommandé** : Cette case d’option doit être sélectionnée pour accepter les valeurs calculées recommandées ou pour modifier les valeurs calculées en valeurs configurées par l’utilisateur.  
  
**Min Server Memory (Mo)** : Si vous passez de la valeur calculée recommandée à une valeur configurée par l’utilisateur, entrez la valeur **min server memory**.  
  
**Max Server Memory (Mo)** : Si vous passez de la valeur calculée recommandée à une valeur configurée par l’utilisateur, entrez la valeur **max server memory**.  

**Cliquez ici pour accepter les configurations de mémoire recommandées pour le moteur de base de données de SQL Server** : Sélectionnez cette case à cocher pour accepter les configurations de mémoire calculées recommandées sur ce serveur. Si la case d’option **Recommandé** est cochée, le programme d’installation ne peut pas continuer tant que cette case à cocher n’est pas sélectionnée.

::: moniker-end

## <a name="database-engine-configuration---filestream-page"></a>Page Configuration du moteur de base de données - FILESTREAM

Utilisez cette page pour activer FILESTREAM pour cette installation de [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)]. FILESTREAM intègre le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] avec un système de fichiers NTFS en stockant les données d’objet BLOB **varbinary(max)** en tant que fichiers dans le système de fichiers. Les instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] peuvent insérer, mettre à jour, interroger, rechercher et sauvegarder des données FILESTREAM. Les interfaces de système de fichiers Microsoft Win32 fournissent l'accès de diffusion en continu aux données. 
  
### <a name="ui-element-list"></a>Liste d’éléments d’interface utilisateur
  
**Activer FILESTREAM pour l'accès Transact-SQL** : Sélectionnez cette option pour activer FILESTREAM pour l'accès [!INCLUDE[tsql](../../includes/tsql-md.md)] . Cette case doit être cochée avant que les autres options soient disponibles.  
  
**Activer FILESTREAM pour l'accès à la diffusion en continu des E/S de fichier** : Sélectionnez cette option pour activer l'accès en continu Win32 pour FILESTREAM.  
  
**Nom du partage Windows** : Entrez le nom du partage Windows dans lequel les données FILESTREAM doivent être stockées.  
  
**Permettre aux clients distants d'avoir un accès à la diffusion en continu aux données FILESTREAM** : Cochez cette case pour permettre aux clients distants d'accéder aux données FILESTREAM sur ce serveur.  
  
### <a name="see-also"></a>Voir aussi

* [Activer et configurer FILESTREAM](../../relational-databases/blob/enable-and-configure-filestream.md)
* [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  

## <a name="database-engine-configuration---user-instance-page"></a>Page Configuration du moteur de base de données - Instance utilisateur

Utilisez la page **Instance utilisateur** pour :

* Générez une instance distincte de [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour les utilisateurs sans autorisations d’administrateur.
* Ajouter des utilisateurs au rôle Administrateur.  
  
### <a name="options"></a>Options
  
**Activer les instances utilisateur** : La valeur par défaut est activée. Pour désactiver la possibilité d’activer les instances utilisateur, décochez la case.  
  
L'instance utilisateur, également désignée comme instance enfant ou client, est une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est générée par l'instance parent (l'instance principale exécutée en tant que service, telle que [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]) pour le compte d'un utilisateur. L'instance utilisateur s'exécute en tant que processus utilisateur dans le contexte de sécurité de cet utilisateur. L'instance utilisateur est isolée de l'instance parent et de toutes les autres instances utilisateur exécutées sur l'ordinateur. La fonctionnalité d’instance utilisateur est également appelée RANU « Run As Normal User » (exécuté avec des droits d'utilisateur normal).  
  
> [!NOTE]  
> Les connexions configurées en tant que membres du rôle serveur fixe **sysadmin** durant l’installation sont configurées en tant qu’administrateurs dans l’exemple de base de données. Si elles ne sont pas supprimées, elles sont membres du rôle serveur fixe **sysadmin** sur l’instance utilisateur.  
  
**Ajouter l'utilisateur au [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]rôle Administrateur** :  La valeur par défaut est désactivée. Pour ajouter l'utilisateur de l'installation en cours au rôle Administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], cochez la case.  
  
Les utilisateurs [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] qui sont membres de BUILTIN\Administrators ne sont pas ajoutés automatiquement au rôle de serveur fixe **sysadmin** quand ils se connectent à [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Seuls les utilisateurs [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] qui ont été ajoutés explicitement à un rôle d'administrateur de niveau serveur peuvent administrer [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Les membres du groupe Built-In\Users peuvent se connecter à l'instance de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)], mais ils auront des autorisations limitées pour effectuer les tâches de base de données. Pour cette raison, les utilisateurs dont les privilèges [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] sont hérités de BUILTIN\Administrators et de Built-In\Utilisateurs dans les versions précédentes de Windows doivent bénéficier explicitement des privilèges d’administrateur dans les instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] qui s’exécutent sur [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)].  
  
Pour apporter des modifications aux rôles d’utilisateur une fois que le programme d’installation se termine, utilisez [SQL Server Management Studio](../../relational-databases/security/authentication-access/join-a-role.md) ou [Transact-SQL](../../t-sql/statements/alter-role-transact-sql.md).