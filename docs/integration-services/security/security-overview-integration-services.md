---
description: Vue d'ensemble de la sécurité (Integration Services)
title: Vue d’ensemble de la sécurité (Integration Services) | Microsoft Docs
ms.custom: security
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- SSIS packages, security
- Integration Services, security
- security [Integration Services], about security
- passwords [Integration Services]
- packages [Integration Services], security
- SQL Server Integration Services, security
- SSIS, security
- Integration Services packages, security
- SQL Server Integration Services packages, security
ms.assetid: 01aa0b88-d477-4581-9a3b-2efc3de2b133
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6b757c7c0a60ab8cff5c496c2a0774646da4c7f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340774"
---
# <a name="security-overview-integration-services"></a>Vue d'ensemble de la sécurité (Integration Services)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La sécurité dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] est constituée de plusieurs couches qui fournissent un environnement de sécurité complet et souple. Ces couches de sécurité incluent l’utilisation de signatures numériques, de propriétés de package, de rôles de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et d’autorisations de système d’exploitation. La plupart de ces fonctionnalités de sécurité se répartissent dans les catégories suivantes : identité et contrôle d'accès.  

## <a name="threat-and-vulnerability-mitigation"></a>Limitation des menaces et des risques de vulnérabilité
  Bien que [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclue divers mécanismes de sécurité, les packages et les fichiers que les packages créent ou utilisent peuvent être exploités à des fins malveillantes.  
  
 Le tableau ci-dessous répertorie ces risques ainsi que les mesures proactives que vous pouvez prendre pour réduire ces risques.  
  
|Menace ou vulnérabilité|Définition|Limitation des risques|  
|-----------------------------|----------------|----------------|  
|Source du package|La source d'un package correspond à l'individu ou à l'organisation qui a créé le package. L'exécution d'un package à partir d'une source inconnue ou non approuvée peut être risquée.|Identifiez la source d'un package en utilisant une signature numérique et exécutez uniquement des packages provenant de sources connues et approuvées. Pour plus d’informations, consultez [Identifier la source de packages à l’aide de signatures numériques](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).|  
|Contenu d'un package|Le contenu d'un package inclut les éléments dans le package et leurs propriétés. Les propriétés peuvent contenir des données sensibles telles qu'un mot de passe ou une chaîne de connexion. Les éléments de package, tels qu'une instruction SQL, peuvent révéler la structure de votre base de données.|Contrôlez l'accès à un package et au contenu en procédant comme suit :<br /><br /> 1) Pour contrôler l’accès au package lui-même, appliquez les fonctionnalités de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux packages enregistrés dans la base de données **msdb** d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Aux packages enregistrés dans le système de fichiers, appliquez les fonctionnalités de sécurité du système de fichiers, telles que les listes de contrôle d'accès (ACL).<br /><br /> 2) Pour contrôler l’accès au contenu du package, définissez le niveau de protection de celui-ci.<br /><br /> Pour plus d’informations, consultez [Vue d’ensemble de la sécurité &#40;Integration Services&#41;](../../integration-services/security/security-overview-integration-services.md) et [Contrôle d’accès pour les données sensibles présentes dans les packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).|  
|Sortie d'un package|Lorsque vous configurez un package de façon à utiliser des configurations, des points de contrôle et une journalisation, le package stocke ces informations en dehors du package. Les informations stockées à l'extérieur du package peuvent contenir des données sensibles.|Pour protéger les configurations et les journaux que le package enregistre dans les tables de la base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , utilisez les fonctionnalités de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> Pour contrôler l'accès aux fichiers, utilisez les listes de contrôle d'accès (ACL) disponibles dans le système de fichiers.<br /><br /> Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](#files)|  
  
## <a name="identity-features"></a>Fonctionnalités d'identité  
 En implémentant des fonctionnalités d'identité dans vos packages, vous pouvez atteindre l'objectif suivant :  
  
 **S'assurer d'ouvrir et d'exécuter uniquement des packages provenant de sources approuvées**.  
  
 Pour vous assurer d'ouvrir et d'exécuter uniquement des packages provenant de sources approuvées, vous devez tout d'abord identifier leur source. Pour ce faire, signez les packages avec des certificats. Puis, lorsque vous ouvrez ou exécutez le package, vous pouvez vérifier la présence et la validité des signatures numériques à l'aide d' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Pour plus d’informations, consultez [Identifier la source de packages à l’aide de signatures numériques](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md).  
  
## <a name="access-control-features"></a>Fonctionnalités de contrôle d'accès  
 En implémentant des fonctionnalités d'identité dans vos packages, vous pouvez atteindre l'objectif suivant :  
  
 **S'assurer que seuls les utilisateurs autorisés ouvrent et exécutent des packages**.  
  
 Pour vous assurer que seuls les utilisateurs autorisés ouvrent et exécutent des packages, vous devez contrôler l'accès aux informations suivantes.  
  
-   Contrôler l'accès au contenu des packages, en particulier les données sensibles.  
  
-   Contrôler l'accès aux packages et configurations de package stockés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Contrôler l'accès aux packages et aux fichiers connexes tels que les configurations, journaux et fichiers de point de contrôle stockés dans le système de fichiers.  
  
-   Contrôler l'accès au service [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et aux informations sur les packages que le service affiche dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
### <a name="controlling-access-to-the-contents-of-packages"></a>Contrôle de l'accès au contenu des packages  
 Pour restreindre l’accès au contenu d’un package, vous pouvez chiffrer le package en définissant sa propriété ProtectionLevel. Vous pouvez définir cette propriété au niveau de protection requis par votre package. Par exemple, dans un environnement de développement en équipe, vous pouvez chiffrer un package à l'aide d'un mot de passe uniquement connu des membres de l'équipe qui travaillent sur celui-ci.  
  
 Lorsque vous définissez la propriété ProtectionLevel d’un package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] détecte automatiquement les propriétés sensibles et les gère en fonction du niveau de protection spécifié. Par exemple, vous définissez la propriété ProtectionLevel d’un package à un niveau qui chiffre les informations sensibles à l’aide d’un mot de passe. Pour ce package, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] chiffre automatiquement les valeurs de toutes les propriétés sensibles et n'affichera pas les données correspondantes sans que le mot de passe correct ne soit fourni.  
  
 En général, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] identifie des propriétés comme étant sensibles si elles contiennent des informations telles qu’un mot de passe ou une chaîne de connexion, ou si ces propriétés correspondent aux variables ou aux nœuds XML générés par la tâche. Le fait qu' [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] considère une propriété comme sensible repose sur le fait que le développeur du composant [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , tel qu'un gestionnaire de connexions ou une tâche, l'a désignée comme sensible. Les utilisateurs ne peuvent pas ajouter de propriétés à, ni en supprimer de, la liste des propriétés considérées sensibles. Si vous écrivez des tâches personnalisées, des gestionnaires de connexions ou des composants de flux de données, vous pouvez spécifier les propriétés [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] à considérer comme sensibles.  
  
 Pour plus d'informations, consultez [Access Control for Sensitive Data in Packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md).  
  
### <a name="controlling-access-to-packages"></a>Contrôle de l'accès aux packages  
 Vous pouvez enregistrer des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] dans la base de données d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]ou dans le système de fichiers sous la forme de fichiers XML avec l'extension .dtsx. Pour plus d’informations, consultez [Enregistrer des packages](../../integration-services/save-packages.md).  
  
#### <a name="saving-packages-to-the-msdb-database"></a>Enregistrement de packages dans la base de données msdb  
 L'enregistrement des packages dans la base de données msdb permet d'assurer la sécurité au niveau du serveur, de la base de données et de la table. Dans la base de données msdb, les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] sont stockés dans la table sysssispackages. Les packages étant enregistrés dans les tables sysssispackages et sysdtspackages de la base de données msdb, ils sont automatiquement sauvegardés lors de la sauvegarde de la base de données msdb.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Vous pouvez également protéger les packages stockés dans la base de données msdb en appliquant des rôles au niveau de la base de données [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] inclut trois rôles fixes au niveau de la base de données : db_ssisadmin, db_ssisltduser et db_ssisoperator pour le contrôle de l’accès aux packages. Un rôle de lecture et d'écriture peut être associé à chaque package. Vous pouvez également définir des rôles personnalisés au niveau de la base de données pour les utiliser dans les packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Les rôles ne peuvent être implémentés que sur les packages enregistrés dans la base de données msdb d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [Rôles Integration Services &#40;Service SSIS&#41;](../../integration-services/security/integration-services-roles-ssis-service.md).  
  
#### <a name="saving-packages-to-the-file-system"></a>Enregistrement de packages dans le système de fichiers  
 Si vous stockez des packages dans le système de fichiers plutôt que dans la base de données msdb, veillez à sécuriser les fichiers de package et les dossiers qui les contiennent.  
  
### <a name="controlling-access-to-files-used-by-packages"></a>Contrôle de l'accès aux fichiers utilisés par des packages  
 Les packages configurés pour utiliser des configurations, des points de contrôle et une journalisation génèrent des informations stockées en dehors du package. Ces informations peuvent être sensibles et doivent être protégées. Les fichiers de point de contrôle ne peuvent être enregistrés que dans le système de fichiers, mais les configurations et les journaux peuvent l'être dans le système de fichiers ou dans les tables d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les configurations et les journaux enregistrés dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bénéficient de la sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , mais les informations écrites dans le système de fichiers requièrent une sécurité supplémentaire.  
  
 Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](#files).  
  
#### <a name="storing-package-configurations-securely"></a>Stockage sécurisé des configurations de package  
 Les configurations de package peuvent être enregistrées dans une table d'une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le système de fichiers.  
  
 Les configurations peuvent être enregistrées dans n'importe qu'elle base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et non pas uniquement dans la base de données msdb. Vous pouvez donc spécifier la base de données qui sert de référentiel de configurations de package. Vous pouvez également spécifier le nom de la table qui contiendra les configurations : [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] crée alors automatiquement la table avec la structure correcte. L'enregistrement des configurations dans une table permet d'assurer la sécurité au niveau du serveur, de la base de données et de la table. En outre, les configurations enregistrées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont automatiquement sauvegardées lors de la sauvegarde de la base de données.  
  
 Si vous stockez des configurations dans le système de fichiers plutôt que dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], veillez à sécuriser les dossiers qui contiennent les fichiers de configuration de package.  
  
 Pour plus d'informations sur les configurations, consultez [Package Configurations](../packages/legacy-package-deployment-ssis.md).  
  
### <a name="controlling-access-to-the-integration-services-service"></a>Contrôle de l'accès au service Integration Services  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilise le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir la liste des packages stockés. Pour empêcher tout utilisateur non autorisé de consulter des informations sur les packages stockées sur des ordinateurs locaux et distants, et par conséquent d'accéder à des informations privées, restreignez l'accès aux ordinateurs qui exécutent le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Pour plus d’informations, consultez [Accéder au service Integration Services](#service).  

## <a name="access-to-files-used-by-packages"></a><a name="files"></a> Accéder aux fichiers utilisés par des packages
  Le niveau de protection de package ne protège pas les fichiers stockés en dehors du package. Il s'agit des fichiers suivants :  
  
-   Fichiers de configuration  
  
-   fichiers de point de contrôle  
  
-   Fichiers journaux  
  
 Ces fichiers doivent être protégés séparément, notamment s'ils contiennent des informations sensibles.  
  
### <a name="configuration-files"></a>Fichiers de configuration  
 Si une configuration contient des informations sensibles, telles que des informations de connexion et de mot de passe, vous devez penser à enregistrer la configuration dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou à utiliser une liste de contrôle d’accès pour limiter l’accès à l’emplacement ou au dossier de stockage des fichiers et pour autoriser l’accès uniquement à certains comptes. En règle générale, vous accordez l'accès aux comptes que vous autorisez à exécuter des packages et à ceux qui gèrent et résolvent les problèmes des packages, ce qui peut comprendre l'inspection du contenu des fichiers de configuration, des fichiers de point de contrôle et des fichiers journaux. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit le stockage le plus sécurisé, car il offre une protection aux niveaux du serveur et des bases de données. Pour enregistrer des configurations dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous utilisez le type de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour enregistrer dans le système de fichiers, vous utilisez le type de configuration XML.  
  
 Pour plus d’informations, consultez [Configurations de package](../packages/legacy-package-deployment-ssis.md), [Créer des configurations de package](../packages/legacy-package-deployment-ssis.md)et [Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md).  
  
### <a name="checkpoint-files"></a>fichiers de point de contrôle  
 De même, si le fichier de point de contrôle utilisé par le package contient des informations sensibles, vous devez utiliser une liste de contrôle d'accès pour sécuriser l'emplacement ou le dossier de stockage du fichier. Les fichiers de points de contrôle contiennent des informations d'état relatives à la progression du package, ainsi que les valeurs actuelles de certaines variables. Par exemple, le package peut inclure une variable personnalisée qui contient un numéro de téléphone. Pour plus d'informations, consultez [Redémarrer des packages à l'aide de points de contrôle](../../integration-services/packages/restart-packages-by-using-checkpoints.md).  
  
### <a name="log-files"></a>Fichiers journaux  
 Les entrées de journaux qui sont écrites dans le système de fichiers doivent également être sécurisées au moyen d'une liste de contrôle d'accès. Elles peuvent aussi être stockées dans des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et protégées par la sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les entrées de journaux peuvent inclure des informations sensibles. Par exemple, si le package contient une tâche d'exécution SQL qui construit une instruction SQL faisant référence à un numéro de téléphone, l'entrée de journal de l'instruction SQL inclut le numéro de téléphone. L'instruction SQL peut également révéler des informations confidentielles relatives à des noms de tables et de colonnes dans des bases de données. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  

## <a name="access-to-the-integration-services-service"></a><a name="service"></a> Accéder au service Integration Services
  Les niveaux de protection des packages peuvent définir des restrictions quant aux utilisateurs autorisés à modifier et exécuter un package. Une protection supplémentaire est nécessaire pour limiter les utilisateurs autorisés à afficher la liste des packages actuellement en cours d’exécution sur un serveur et ceux qui peuvent arrêter les packages en cours d’exécution dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] utilise le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir la liste des packages en cours d’exécution. Les membres du groupe Administrateurs de Windows peuvent afficher et arrêter tous les packages en cours d'exécution. Les utilisateurs n'appartenant pas au groupe Administrateurs de Windows ne peuvent afficher et arrêter que les packages qu'ils ont démarrés.  
  
 Il est important de restreindre l'accès aux ordinateurs qui exécutent un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , notamment un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui peut énumérer des dossiers distants. Tout utilisateur authentifié peut demander l'énumération des packages. Même si le service ne trouve pas le service, il énumère les dossiers. Ces noms de dossiers peuvent être utilisés par un utilisateur malveillant. Si un administrateur a configuré le service pour qu'il énumère les dossiers sur un ordinateur distant, les utilisateurs peuvent également voir des noms de dossiers qu'ils ne pourraient pas voir en temps normal.  

## <a name="related-tasks"></a>Tâches associées  
 La liste suivante contient des liens vers les rubriques qui indiquent comment effectuer une tâche particulière relative à la sécurité.  
  
-   [Créer un rôle défini par l'utilisateur](../../integration-services/security/integration-services-roles-ssis-service.md#create)  
  
-   [Affecter un rôle de lecture et d'écriture à un package](../../integration-services/security/integration-services-roles-ssis-service.md#assign)  
  
-   [Implémenter une stratégie de signature en définissant une valeur du Registre](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#registry)  
  
-   [Signer un package à l'aide d'un certificat numérique](../../integration-services/security/identify-the-source-of-packages-with-digital-signatures.md#cert)  
  
-   [Définir ou modifier le niveau de protection des packages](../../integration-services/security/access-control-for-sensitive-data-in-packages.md#set_protection)