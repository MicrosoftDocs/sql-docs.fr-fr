---
title: Configurer les comptes de service Windows et les autorisations | Microsoft Docs
description: Familiarisez-vous avec les comptes de service qui sont utilisés pour démarrer et exécuter des services dans SQL Server. Découvrez comment les configurer et attribuer les autorisations appropriées.
ms.custom: contperf-fy20q4
ms.date: 03/17/2020
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: reference
helpviewer_keywords:
- startup service states [SQL Server]
- Setup [SQL Server], user accounts
- Windows permissions [SQL Server]
- modifying user accounts
- default accounts
- domains [SQL Server], user accounts
- startup accounts [SQL Server]
- system accounts [SQL Server]
- services [SQL Server], permissions
- ACL (access control list)
- local system accounts [SQL Server]
- instance-aware services [SQL Server]
- permissions [SQL Server], services
- SQL Server Agent service, user accounts
- Windows NT permissions [SQL Server]
- user accounts [SQL Server]
- identifying instance-unaware services [SQL Server]
- installing SQL Server, user accounts
- disabled startup state [SQL Server]
- user accounts [SQL Server], users
- Local Service account [SQL Server]
- SQL Server Installation Wizard
- instance-unaware services [SQL Server]
- services [SQL Server], configuring at installation
- Windows accounts [SQL Server]
- SQL Server services, user accounts
- user accounts [SQL Server], services
- MSSQLServer
- identifying instance-aware services [SQL Server]
- services [SQL Server], accounts
- access control lists
- optional accounts [SQL Server]
- service accounts [SQL Server]
- accounts [SQL Server], services
- built-in system accounts [SQL Server]
- automatic startup state
- domains [SQL Server]
- manual startup state [SQL Server]
- accounts [SQL Server], user
ms.assetid: 309b9dac-0b3a-4617-85ef-c4519ce9d014
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da2fc97ef8e5637dfff48d2070cb652b40ae4d28
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878913"
---
# <a name="configure-windows-service-accounts-and-permissions"></a>Configurer les comptes de service Windows et les autorisations

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Chaque service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] représente un processus ou un ensemble de processus permettant de gérer l'authentification des opérations de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec Windows. Cette rubrique décrit la configuration par défaut des services inclus dans cette version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ainsi que les options de configuration des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] susceptibles d'être définies durant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et par la suite. Cette rubrique permet aux utilisateurs expérimentés de comprendre les détails des comptes de service.

 La plupart des services et de leurs propriétés peuvent être configurés à l’aide du Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Voici les chemins d’accès aux quatre dernières versions lorsque Windows est installé sur le lecteur C.

|Version de SQL Server|Path|
|-|-|
|SQL Server 2019|C:\Windows\SysWOW64\SQLServerManager15.msc|
|SQL Server 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|

## <a name="services-installed-by-ssnoversion"></a><a name="Service_Details"></a> Services installés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

En fonction des composants que vous décidez d'installer, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installe les services suivants :

- **Services de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** : service pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)] relationnel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le fichier exécutable est \<MSSQLPATH>\MSSQL\Binn\sqlservr.exe.
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent** : exécute les travaux, surveille [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], déclenche les alertes et autorise l’automatisation de certaines tâches administratives. Le service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est présent mais désactivé sur les instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)]. Le fichier exécutable est \<MSSQLPATH>\MSSQL\Binn\sqlagent.exe.
- **[!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]**  : fournit des fonctionnalités OLAP et d’exploration de données pour les applications Business Intelligence. Le fichier exécutable est \<MSSQLPATH>\OLAP\Bin\msmdsrv.exe.
- **[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]**  : gère, exécute, crée, planifie et publie les rapports. Le fichier exécutable est \<MSSQLPATH>\Reporting Services\ReportServer\Bin\ReportingServicesService.exe.
- **[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]**  : assure la prise en charge de la gestion du stockage et de l’exécution des packages [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Le chemin du fichier exécutable est \<MSSQLPATH>\130\DTS\Binn\MsDtsSrvr.exe

   [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] peut inclure des services supplémentaires pour les déploiements avec scale-out. Pour plus d’informations, consultez [Procédure pas à pas : Configurez le scale-out d’Integration Services (SSIS)](../../integration-services/scale-out/walkthrough-set-up-integration-services-scale-out.md).

- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser** : service de résolution de noms qui fournit des informations de connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour les ordinateurs clients. Le chemin d’accès du fichier exécutable est c:\Program Files (x86)\Microsoft SQL Server\90\Shared\sqlbrowser.exe
- **Recherche en texte intégral** : crée rapidement des index de recherche en texte intégral sur le contenu et des propriétés de données structurées et semi-structurées pour fournir un filtrage de document et l’analyse lexicale pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- **SQL Writer** : permet aux applications de sauvegarde et restauration de fonctionner dans l’infrastructure du service de cliché instantané de volume (VSS).
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller** : fournit l’orchestration de relecture de trace pour plusieurs ordinateurs clients Distributed Replay.
- **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client** : un ou plusieurs ordinateurs clients Distributed Replay qui opèrent avec un contrôleur Distributed Replay pour simuler des charges de travail simultanées sur une instance du [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].
- **[!INCLUDE[rsql_launchpad](../../includes/rsql-launchpad-md.md)]**  : service approuvé hébergeant les exécutables externes fournis par Microsoft, tels que les runtimes R ou Python installés dans le cadre de R Services ou de Machine Learning Services. Le processus Launchpad peut lancer des processus satellites dont les ressources seront régies par la configuration de l’instance. Le service Launchpad s’exécute sous son propre compte d’utilisateur, et chaque processus satellite d’une exécution déclarée hérite du compte d’utilisateur de Launchpad. Les processus satellites sont créés et détruits à la demande pendant l’exécution.

  LaunchPad ne peut pas créer les comptes qu’elle utilise si vous installez SQL Server sur un ordinateur qui est également utilisé comme un contrôleur de domaine. Par conséquent, l’installation de R Services (en base de données) ou de Machine Learning Services (en base de données) échoue sur un contrôleur de domaine.

- **Moteur SQL Server PolyBase** : fournit des fonctions de requête distribuée aux sources de données externes.
- **Service de mouvement de données PolyBase SQL Server** : permet le déplacement de données entre SQL Server et les sources de données externes ainsi qu’entre les nœuds SQL dans les groupes de scale-out PolyBase.

## <a name="service-properties-and-configuration"></a><a name="Serv_Prop"></a> Propriétés et configuration des services

Les comptes de démarrage utilisés pour démarrer et exécuter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être des [comptes d’utilisateur de domaine](#Domain_User), des [comptes d’utilisateur local](#Local_User), des [comptes de service administrés](#MSA), des [comptes virtuels](#VA_Desc)ou des [comptes système intégrés](#Local_Service). Pour démarrer et s'exécuter, chaque service dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit posséder un compte de démarrage configuré lors de l'installation.

Cette section décrit les comptes qui peuvent être configurés pour démarrer des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les valeurs par défaut utilisées par le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le concept de SID par service, les options de démarrage et la configuration du pare-feu.

- [Comptes de service par défaut](#Default_Accts)
- [Démarrage automatique](#Auto_Start)
- [Configuration du type de démarrage du service](#Configure_services)
- [Port de pare-feu](#Firewall)

### <a name="default-service-accounts"></a><a name="Default_Accts"></a> Comptes de service par défaut

Le tableau suivant répertorie les comptes de service par défaut utilisés par le programme d'installation lors de l'installation de tous les composants. Les comptes par défaut répertoriés sont les comptes recommandés, sauf indication contraire.

**Serveur autonome ou contrôleur de domaine**

|Composant|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|Windows 7 et [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 et versions ultérieures|
|---------------|------------------------------------|----------------------------------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)*|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)\* \*\*|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)\*|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)\*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)\*|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)\*|
|Lanceur FD (recherche en texte intégral)|[SERVICE LOCAL](#Local_Service)|[Compte virtuel](#VA_Desc)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|[SERVICE LOCAL](#Local_Service)|[SERVICE LOCAL](#Local_Service)|
|Enregistreur VSS[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[SYSTÈME LOCAL](#Local_System)|[SYSTÈME LOCAL](#Local_System)|
|[!INCLUDE[rsql_extensions](../../includes/rsql-extensions-md.md)]|NTSERVICE\MSSQLLaunchpad|NTSERVICE\MSSQLLaunchpad|
|Moteur PolyBase |[SERVICE RÉSEAU](#Network_Service) |[SERVICE RÉSEAU](#Network_Service)|
|Service de déplacement de données PolyBase |[SERVICE RÉSEAU](#Network_Service) |[SERVICE RÉSEAU](#Network_Service)|

\*Quand des ressources externes à l’ordinateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont requises, [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande d’utiliser un compte de service administré (MSA), configuré avec le minimum de privilèges nécessaires.
\*\* Quand il est installé sur un contrôleur de domaine, un compte virtuel ne peut pas être utilisé comme compte de service.

**Instance de cluster de basculement SQL Server**

|Composant|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)]|[!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2|
|---------------|------------------------------------|---------------------------------------|
|[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Aucun. Fournit un compte d' [utilisateur de domaine](#Domain_User) .|Fournit un compte d' [utilisateur de domaine](#Domain_User) .|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent|Aucun. Fournit un compte d' [utilisateur de domaine](#Domain_User) .|Fournit un compte d' [utilisateur de domaine](#Domain_User) .|
|[!INCLUDE[ssAS](../../includes/ssas-md.md)]|Aucun. Fournit un compte d' [utilisateur de domaine](#Domain_User) .|Fournit un compte d' [utilisateur de domaine](#Domain_User) .|
|[!INCLUDE[ssIS](../../includes/ssis-md.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)|
|[!INCLUDE[ssRS](../../includes/ssrs.md)]|[SERVICE RÉSEAU](#Network_Service)|[Compte virtuel](#VA_Desc)|
|Lanceur FD (recherche en texte intégral)|[SERVICE LOCAL](#Local_Service)|[Compte virtuel](#VA_Desc)|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|[SERVICE LOCAL](#Local_Service)|[SERVICE LOCAL](#Local_Service)|
|Enregistreur VSS[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|[SYSTÈME LOCAL](#Local_System)|[SYSTÈME LOCAL](#Local_System)|

#### <a name="changing-account-properties"></a><a name="Changing_Accounts"></a> Modification des propriétés de compte

> [!IMPORTANT]
>
> - Utilisez toujours des outils [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tels que le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , pour modifier le compte utilisé par [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ou les services Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour changer le mot de passe du compte. En plus de modifier le nom du compte, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] effectue une configuration supplémentaire pour mettre à jour le magasin de sécurité local Windows qui protège la clé principale du service pour le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. D'autres outils tels que le Gestionnaire de contrôle de services Windows peuvent modifier le nom du compte mais pas tous les paramètres requis.
> - Pour les instances [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] que vous déployez dans une batterie de serveurs SharePoint, utilisez toujours l'Administration centrale de SharePoint pour modifier les comptes de serveur des applications de [!INCLUDE[ssGeminiMTS](../../includes/ssgeminimts-md.md)] et du [!INCLUDE[ssGeminiSrv](../../includes/ssgeminisrv-md.md)]. Les paramètres et autorisations associés sont mis à jour pour utiliser les nouvelles informations de compte lorsque vous utilisez l'Administration centrale.
> - Pour modifier des options [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , utilisez l'Outil de configuration de Reporting Services.

### <a name="managed-service-accounts-group-managed-service-accounts-and-virtual-accounts"></a><a name="New_Accounts"></a> Comptes de service administrés, comptes de service administrés de groupe et comptes virtuels

Les comptes de service administrés, les comptes de service administrés de groupe et les comptes virtuels sont conçus pour fournir des applications cruciales telles que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec l’isolation de leurs propres comptes, tout en éliminant le besoin d’un administrateur pour administrer manuellement le nom de principal du service et les informations d’identification de ces comptes. Cela simplifie beaucoup la gestion à long terme des utilisateurs des comptes de service, des mots de passe et des SPN.

- <a name="MSA"></a> **Comptes de service administrés**

  Un Compte de service administré (MSA) est un type de compte de domaine créé et a géré par le contrôleur de domaine. Il est affecté à un ordinateur membre unique pour exécuter un service. Le mot de passe est géré automatiquement par le contrôleur de domaine. Vous ne pouvez pas utiliser un MSA pour vous connecter à un ordinateur, mais un ordinateur peut utiliser un MSA pour démarrer un service Windows. Un MSA est vapable d’inscrire un nom de principal du service dans Active Directory quand il dispose d’autorisations de lecture et d’écriture servicePrincipalName. Un compte MSA est nommé avec un suffixe **$** , par exemple **DOMAIN\ACCOUNTNAME$** . Lorsque vous spécifiez un MSA, laissez le mot de passe vide. Comme un MSA est affecté à un ordinateur unique, il ne peut pas être utilisé sur différents nœuds d'un cluster Windows.

  > [!NOTE]
  > Le MSA doit être créé dans Active Directory par l'administrateur de domaine avant que le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse l'utiliser pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- <a name="GMSA"></a> **Comptes de service administrés de groupe**

  Un compte de service administré de groupe (gMSA) est un compte MSA pour plusieurs serveurs. Windows gère un compte de service pour les services en cours d’exécution sur un groupe de serveurs. Active Directory met automatiquement à jour le mot de passe du compte de service administré de groupe sans redémarrer les services. Vous pouvez configurer les services SQL Server pour utiliser un principal de compte de service administré de groupe. À compter de SQL Server 2014, SQL Server prend en charge les comptes de service administrés de groupe sur les instances autonomes et SQL Server 2016 et ultérieur pour les instances de cluster de basculement et les groupes de disponibilité.

  Pour utiliser un gMSA pour SQL Server 2014 ou version ultérieure, le système d’exploitation doit être Windows Server 2012 R2 ou version ultérieure. Les serveurs avec Windows Server 2012 R2 nécessitent l’application de l’article [2998082 de la Base de connaissances](https://support.microsoft.com/kb/2998082) afin que les services puissent se connecter sans interruption immédiatement après la modification du mot de passe.

  Pour plus d’informations, consultez [Comptes de service administrés de groupe](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831782(v=ws.11))

  > [!NOTE]
  > Le gMSA doit être créé dans Active Directory par l'administrateur de domaine avant que le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puisse l'utiliser pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- <a name="VA_Desc"></a>**Virtual Accounts**

  Les comptes virtuels (à partir de Windows Server 2008 R2 et Windows 7) sont des *comptes locaux gérés* qui fournissent les fonctionnalités suivantes pour simplifier l’administration du service. Le compte virtuel est géré automatiquement, et le compte virtuel peut accéder au réseau dans un environnement de domaine. Si la valeur par défaut est utilisée pour les comptes de service lors de l’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un compte virtuel qui utilise le nom de l’instance comme nom de service est utilisé, au format **NT SERVICE\\** _\<SERVICENAME>_ . Les services qui s’exécutent comme comptes virtuels accèdent aux ressources réseau à l’aide des informations d’identification du compte de l’ordinateur au format *<nom_domaine>* __\\__ *<nom_ordinateur>* __$__ . Lorsque vous spécifiez un compte virtuel pour démarrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], laissez le mot de passe vide. Si le compte virtuel n'inscrit pas le nom de principal du service, inscrivez-le manuellement. Pour plus d’informations sur l’enregistrement manuel d’un SPN, consultez [Enregistrement manuel d’un SPN](register-a-service-principal-name-for-kerberos-connections.md).

  > [!NOTE]
  > Les comptes virtuels ne peuvent pas être utilisés pour l'instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] parce que le compte virtuel n'aurait pas le même SID sur chaque nœud du cluster.

  Le tableau suivant répertorie des exemples de noms du compte virtuels.

  |Service|Nom de compte virtuel|
  |-------------|--------------------------|
  |Instance par défaut du service [!INCLUDE[ssDE](../../includes/ssde-md.md)]|**NT SERVICE\MSSQLSERVER**|
  |Instance nommée d'un service nommé [!INCLUDE[ssDE](../../includes/ssde-md.md)]**PAYROLL**|**NT SERVICE\MSSQL$PAYROLL**|
  |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur l’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|**NT SERVICE\SQLSERVERAGENT**|
  |[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent sur une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nommée **PAYROLL**|**NT SERVICE\SQLAGENT$PAYROLL**|

Pour plus d’informations sur les comptes de service administrés et les comptes virtuels, consultez la section **Concepts liés aux comptes de service administrés et aux comptes virtuels** du [Guide pas à pas des comptes de service](https://technet.microsoft.com/library/dd548356\(WS.10\).aspx) et du [Forum aux questions des comptes de service administrés](https://technet.microsoft.com/library/ff641729\(WS.10\).aspx).

**Note de sécurité :** [!INCLUDE[ssNoteLowRights](../../includes/ssnotelowrights-md.md)] Utilisez un [MSA](#MSA), un [gMSA](#GMSA) ou un [compte virtuel](#VA_Desc) quand c’est possible. Quand des comptes MSA, gMSA et virtuels ne sont pas possibles, utilisez un compte d’utilisateur ou un compte de domaine avec des privilèges faibles au lieu d’un compte partagé pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Utilisez des comptes distincts pour différents services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . N'accordez aucune autorisation supplémentaire au compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ni aux groupes de services. Les autorisations seront accordées par le biais de l'appartenance aux groupes ou accordées directement à un SID de service, lorsqu'un SID de service est pris en charge.

### <a name="automatic-startup"></a><a name="Auto_Start"></a> Démarrage automatique

Outre le fait que les services doivent posséder des comptes d'utilisateurs, ils ont chacun trois états de démarrage possibles, que les utilisateurs peuvent contrôler :

- **Désactivé** Le service est installé mais pas en cours d'exécution.
- **Manuel** Le service est installé, mais ne démarre que lorsqu'un autre service ou une autre application a besoin de ses fonctionnalités.
- **Automatique** Le service est démarré automatiquement par le système d'exploitation.

L'état de démarrage est sélectionné pendant l'installation. Lors de l'installation d'une instance nommée, le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser doit être configuré pour démarrer automatiquement.

### <a name="configuring-services-during-unattended-installation"></a><a name="Configure_services"></a> Configuration des services pendant l’installation sans assistance

Le tableau suivant répertorie les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] susceptibles d'être configurés durant l'installation. Pour les installations sans assistance, vous pouvez employer les commutateurs dans un fichier de configuration ou à l'invite de commandes.

|Nom du service SQL Server|Commutateurs pour des installations sans assistance\*|
|-----------------------------|---------------------------------------------|
|MSSQLSERVER|SQLSVCACCOUNT, SQLSVCPASSWORD, SQLSVCSTARTUPTYPE|
|SQLServerAgent\*\*|AGTSVCACCOUNT, AGTSVCPASSWORD, AGTSVCSTARTUPTYPE|
|MSSQLServerOLAPService|ASSVCACCOUNT, ASSVCPASSWORD, ASSVCSTARTUPTYPE|
|ReportServer|RSSVCACCOUNT, RSSVCPASSWORD, RSSVCSTARTUPTYPE|
|[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]|ISSVCACCOUNT, ISSVCPASSWORD, ISSVCSTARTUPTYPE|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller|DRU_CTLR, CTLRSVCACCOUNT,CTLRSVCPASSWORD, CTLRSTARTUPTYPE, CTLRUSERS|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|DRU_CLT, CLTSVCACCOUNT, CLTSVCPASSWORD, CLTSTARTUPTYPE, CLTCTLRNAME, CLTWORKINGDIR, CLTRESULTDIR|
|R Services ou Machine Learning Services|EXTSVCACCOUNT, EXTSVCPASSWORD, ADVANCEDANALYTICS\*\*\*|
|Moteur PolyBase| PBENGSVCACCOUNT, PBENGSVCPASSWORD, PBENGSVCSTARTUPTYPE, PBDMSSVCACCOUNT,PBDMSSVCPASSWORD, PBDMSSVCSTARTUPTYPE, PBSCALEOUT, PBPORTRANGE

\*Pour obtenir plus d’informations et un exemple de syntaxe pour des installations sans assistance, consultez [Installer SQL Server 2016 à partir de l’invite de commandes](../install-windows/install-sql-server-from-the-command-prompt.md).

\*\*Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est désactivé sur les instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] et [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] avec Advanced Services.

\*\*\*La définition du compte pour Launchpad au moyen des commutateurs seuls n’est pas prise en charge actuellement. Utilisez le Gestionnaire de configuration SQL Server pour modifier le compte et les autres paramètres de service.

### <a name="firewall-port"></a><a name="Firewall"></a> Port de pare-feu

Dans la plupart des cas, lors de l'installation initiale, le [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut être connecté par des outils tels que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] installés sur le même ordinateur que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’ouvre pas de ports dans le pare-feu Windows. Les connexions d'autres ordinateurs peuvent ne pas être possibles tant que le [!INCLUDE[ssDE](../../includes/ssde-md.md)] n'est pas configuré pour écouter sur un port TCP, et tant que le port approprié n'est pas ouvert aux connexions dans le pare-feu Windows. Pour plus d’informations, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).

## <a name="service-permissions"></a><a name="Serv_Perm"></a> Autorisations de service

Cette section décrit les autorisations configurées par le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le SID par service des services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- [Configuration du service et contrôle d’accès](#Serv_SID)
- [Privilèges et droits Windows](#Windows)
- [Autorisations de système de fichiers accordées aux SID par service SQL Server ou aux groupes Windows locaux SQL Server](#Reviewing_ACLs)
- [Autorisations de système de fichiers accordées aux autres comptes d’utilisateur ou groupes Windows](#File_System_Other)
- [Autorisations de système de fichiers pour des emplacements de disque exceptionnels](#Unusual_Locations)
- [Considérations supplémentaires](#Review_additional_considerations)
- [Autorisations de Registre](#Registry)
- [WMI](#WMI)
- [Canaux nommés](#Pipes)

### <a name="service-configuration-and-access-control"></a><a name="Serv_SID"></a> Configuration du service et contrôle d’accès

[!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] active un SID par service pour chacun de ses services pour fournir une isolation de service et une défense en profondeur. Le SID par service est dérivé du nom du service et est propre à ce service. Par exemple, un nom de SID de service pour une instance nommée du service [!INCLUDE[ssDE](../../includes/ssde-md.md)] peut être **NT Service\MSSQL$** _\<InstanceName>_ . L'isolation de service permet l'accès à des objets spécifiques sans devoir exécuter un compte à privilèges élevés ni affaiblir la protection de sécurité de l'objet. Un service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut limiter l'accès à ses ressources via une entrée de contrôle d'accès qui contient un SID de service.

> [!NOTE]
> Sur Windows 7 et [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 (et versions ultérieures) le SID par service peut être le compte virtuel utilisé par le service.

Pour la plupart des composants, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure directement l'ACL pour le compte par service, par conséquent, la modification du compte de service peut être effectuée sans devoir répéter le processus ACL de la ressource.

Lors de l'installation de [!INCLUDE[ssAS](../../includes/ssas-md.md)], un SID par service est créé pour le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Un groupe Windows local est créé, nommé au format **SQLServerMSASUser$** _nom_ordinateur_ **$** _nom_instance_. Le SID par service **NT SERVICE\MSSQLServerOLAPService** peut appartenir au groupe Windows local et le groupe Windows local reçoit les autorisations appropriées dans l’ACL. Si le compte utilisé pour démarrer le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] est modifié, le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit modifier les autorisations Windows (telles que le droit d'ouvrir une session en tant que service), mais les autorisations affectées au groupe Windows local seront toujours disponibles sans besoin de mise à jour, parce que le SID par service n'a pas changé. Cette méthode permet de renommer le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pendant des mises à niveau.

Pendant l'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée des groupes Windows locaux pour [!INCLUDE[ssAS](../../includes/ssas-md.md)] et le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser. Pour ces services, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configure l'ACL pour les groupes Windows locaux.

Selon la configuration du service, le compte de service pour un service ou un SID de service est ajouté en tant que membre du groupe de service lors de l'installation ou de la mise à niveau.

### <a name="windows-privileges-and-rights"></a><a name="Windows"></a> Privilèges et droits Windows

Le compte affecté pour démarrer un service a besoin d' **autorisations de démarrage, arrêt et pause** pour le service. Le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les affecte automatiquement. En premier lieu, installez les Outils d'administration de serveur distant. Consultez la rubrique [Outils d'administration de serveur distant pour Windows 10](https://www.microsoft.com/download/details.aspx?id=45520).

Le tableau suivant affiche les autorisations que le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] demande pour les SID par service ou les groupes Windows locaux utilisés par les composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Autorisations accordées par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|
|---------------------------------------|------------------------------------------------------------|
|**[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]:**<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut : **NT SERVICE\MSSQLSERVER**. Instance nommée : **NT Service\MSSQLServer$** _InstanceName_.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)<br /><br /> **Remplacer un jeton de niveau processus** (SeAssignPrimaryTokenPrivilege)<br /><br /> **Ignorer la vérification transversale** (SeChangeNotifyPrivilege)<br /><br /> **Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaPrivilege)<br /><br /> Autorisation de démarrer SQL Writer<br /><br /> Autorisation de lire le service Journal des événements<br /><br /> Autorisation de lire le service d'appel de procédure distante (RPC)|

|**Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :** \*<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut : **NT Service\SQLSERVERAGENT**. Instance nommée : **NT Service\SQLAGENT$** _InstanceName_.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)<br /><br /> **Remplacer un jeton de niveau processus** (SeAssignPrimaryTokenPrivilege)<br /><br /> **Ignorer la vérification transversale** (SeChangeNotifyPrivilege)<br /><br /> **Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaPrivilege)| | **[!INCLUDE[ssAS](../../includes/ssas-md.md)] :**<br /><br /> (Tous les droits sont accordés à un groupe Windows local. Instance par défaut : **SQLServerMSASUser$** _ComputerName_ **$MSSQLSERVER**. Instance nommée : **SQLServerMSASUser$** _ComputerName_ **$** _InstanceName_. Instance [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] : **SQLServerMSASUser$** _ComputerName_ **$** _PowerPivot_.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)<br /><br /> Tabulaire uniquement :<br /><br /> **Augmenter une plage de travail de processus** (SeIncreaseWorkingSetPrivilege)<br /><br /> **Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaPrivilege)<br /><br /> **Verrouiller les pages en mémoire** (SeLockMemoryPrivilege) : nécessaire uniquement quand la pagination est totalement désactivée.<br /><br /> Installations de cluster de basculement uniquement :<br /><br /> **Augmenter la priorité de planification** (SeIncreaseBasePriorityPrivilege)| | **[!INCLUDE[ssRS](../../includes/ssrs.md)] :**<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut : **NT SERVICE\ReportServer**. Instance nommée : **NT SERVICE\\ReportServer$** _InstanceName_.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)| | **[!INCLUDE[ssIS](../../includes/ssis-md.md)] :**<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut et instance nommée : **NT SERVICE\MsDtsServer130**. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] n’a pas de processus séparé pour une instance nommée.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)<br /><br /> Autorisation d'écrire dans le journal des événements d'application<br /><br /> **Ignorer la vérification transversale** (SeChangeNotifyPrivilege)<br /><br /> **Emprunter l’identité d’un client après l’authentification** (SeImpersonatePrivilege)| |**Recherche en texte intégral :**<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut : **NT Service\MSSQLFDLauncher**. Instance nommée : **NT Service\ MSSQLFDLauncher$** _InstanceName_.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)<br /><br /> **Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaPrivilege)<br /><br /> **Contourner la vérification de parcours** (SeChangeNotifyPrivilege)| |**Navigateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :**<br /><br /> (Tous les droits sont accordés à un groupe Windows local. Instance par défaut ou nommée : **SQLServer2005SQLBrowserUser** _$ComputerName_. Le navigateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’a pas de processus séparé pour une instance nommée.)|**Ouvrir une session en tant que service** (SeServiceLogonRight)| |**Enregistreur VSS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :**<br /><br /> (Tous les droits sont accordés au SID par service. Instance par défaut ou nommée : **NT Service\SQLWriter**. L’enregistreur VSS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’a pas de processus séparé pour une instance nommée.)|Le service SQLWriter s’exécute sous le compte LOCAL SYSTEM qui a toutes les autorisations requises. Le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne vérifie pas ni accorde d’autorisations pour ce service.| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contrôleur de Distributed Replay :** |**ouvrir une session en tant que service** (SeServiceLogonRight)| | **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller :** |**Ouvrir une session en tant que service** (SeServiceLogonRight)| |**Moteur PolyBase et DMS**| **Ouvrir une session en tant que service** (SeServiceLogonRight)| |**Launchpad :** |**Ouvrir une session en tant que service** (SeServiceLogonRight) <br /><br /> **Remplacer un jeton de niveau processus** (SeAssignPrimaryTokenPrivilege)<br /><br />**Ignorer la vérification transversale** (SeChangeNotifyPrivilege)<br /><br />**Ajuster les quotas de mémoire pour un processus** (SeIncreaseQuotaPrivilege)| |**R Services/Machine Learning Services :** **SQLRUserGroup** (SQL 2016 et 2017) |Ne dispose pas de l’autorisation **Permettre l’ouverture d’une session locale** par défaut | |**Machine Learning Services** « **Tous les packages d’application » [AppContainer]** (SQL 2019) |**Autorisations de lecture et d’exécution** sur les répertoires SQL Server « Binn », R_Services et PYTHON_Services |

 \*Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est désactivé sur les instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)].

### <a name="file-system-permissions-granted-to-sql-server-per-service-sids-or-local-windows-groups"></a><a name="Reviewing_ACLs"></a> Autorisations de système de fichiers accordées aux SID par service SQL Server ou aux groupes Windows locaux

Les comptes de service[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent avoir accès aux ressources. Les listes de contrôle d'accès sont définies pour le SID par service ou le groupe Windows local.

> [!IMPORTANT]
> Pour les installations de clusters de basculement, les ressources des disques partagés doivent être attribuées à la liste ACL d'un compte local.

Le tableau suivant répertorie les listes ACL définies par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|Compte de service pour|Fichiers et dossiers|Accès|
|-------------------------|-----------------------|------------|
|MSSQLServer|Instid\MSSQL\backup|Contrôle total|
||Instid\MSSQL\binn|Lecture, exécution|
||Instid\MSSQL\data|Contrôle total|
||Instid\MSSQL\FTData|Contrôle total|
||Instid\MSSQL\Install|Lecture, exécution|
||Instid\MSSQL\Log|Contrôle total|
||Instid\MSSQL\Repldata|Contrôle total|
||130\shared|Lecture, exécution|
||Données Instid\MSSQL\Template ([!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] uniquement)|Lire|
|SQLServerAgent\*|Instid\MSSQL\binn|Contrôle total|
||Instid\MSSQL\Log|Lecture, écriture, exécution, suppression (Read, Write, Execute, Delete)|
||130\com|Lecture, exécution|
||130\shared|Lecture, exécution|
||130\shared\Errordumps|Lire, Écrire|
||ServerName\EventLog|Contrôle total|
|FTS|Instid\MSSQL\FTData|Contrôle total|
||Instid\MSSQL\FTRef|Lecture, exécution|
||130\shared|Lecture, exécution|
||130\shared\Errordumps|Lire, Écrire|
||Instid\MSSQL\Install|Lecture, exécution|
||Instid\MSSQL\jobs|Lire, Écrire|
|MSSQLServerOLAPService|130\shared\ASConfig|Contrôle total|
||Instid\OLAP|Lecture, exécution|
||Instid\Olap\Data|Contrôle total|
||Instid\Olap\Log|Lire, Écrire|
||Instid\OLAP\Backup|Lire, Écrire|
||Instid\OLAP\Temp|Lire, Écrire|
||130\shared\Errordumps|Lire, Écrire|
|ReportServer|Instid\Reporting Services\Log Files|Lecture, écriture, suppression|
||Instid\Reporting Services\ReportServer|Lecture, exécution|
||Instid\Reporting Services\ReportServer\global.asax|Contrôle total|
||Instid\Reporting Services\ReportServer\rsreportserver.config|Lire|
||Instid\Reporting Services\RSTempfiles|Lecture, écriture, exécution, suppression|
||Instid\Reporting Services\RSWebApp|Lecture, exécution|
||130\shared|Lecture, exécution|
||130\shared\Errordumps|Lire, Écrire|
|MSDTSServer100|130\dts\binn\MsDtsSrvr.ini.xml|Lire|
||130\dts\binn|Lecture, exécution|
||130\shared|Lecture, exécution|
||130\shared\Errordumps|Lire, Écrire|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser|130\shared\ASConfig|Lire|
||130\shared|Lecture, exécution|
||130\shared\Errordumps|Lire, Écrire|
|SQLWriter|Non applicable (s'exécute en tant que système local)||
|Utilisateur|Instid\MSSQL\binn|Lecture, exécution|
||Instid\Reporting Services\ReportServer|Lecture, exécution, listage du contenu des dossiers|
||Instid\Reporting Services\ReportServer\global.asax|Lire|
||Instid\Reporting Services\RSWebApp|Lecture, exécution, listage du contenu des dossiers|
||130\dts|Lecture, exécution|
||130\tools|Lecture, exécution|
||100\tools|Lecture, exécution|
||90\tools|Lecture, exécution|
||80\tools|Lecture, exécution|
||130\sdk|Lire|
||Microsoft SQL Server\130\Setup Bootstrap|Lecture, exécution|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Controller|\<ToolsDir>\DReplayController\Log\ (répertoire vide)|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\DReplayController.exe|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\resources\|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\\{toutes les dll}|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\DReplayController.config|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\IRTemplate.tdf|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayController\IRDefinition.xml|Lecture, exécution, listage du contenu des dossiers|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay Client|\<ToolsDir>\DReplayClient\Log\|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\DReplayClient.exe|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\resources\|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\ (toutes les dll)|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\DReplayClient.config|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\IRTemplate.tdf|Lecture, exécution, listage du contenu des dossiers|
||\<ToolsDir>\DReplayClient\IRDefinition.xml|Lecture, exécution, listage du contenu des dossiers|
|Launchpad|%binn|Lecture, exécution|
||ExtensiblilityData|Contrôle total|
||Log\ExtensibilityLog|Contrôle total|

\*Le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent est désactivé sur les instances de [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] et [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] avec Advanced Services.

Lorsque des fichiers de base de données sont stockés à un emplacement défini par l'utilisateur, vous devez octroyer l'accès SID par service à cet emplacement. Pour plus d’informations sur l’octroi d’autorisations de système de fichiers à un SID par service, consultez [Configurer les autorisations du système de fichiers pour l’accès au moteur de base de données](../../database-engine/configure-windows/configure-file-system-permissions-for-database-engine-access.md).

### <a name="file-system-permissions-granted-to-other-windows-user-accounts-or-groups"></a><a name="File_System_Other"></a> Autorisations de système de fichiers accordées aux autres comptes d’utilisateur ou groupes Windows

Certaines autorisations de contrôle d'accès peuvent avoir été accordées à des comptes intégrés ou à d'autres comptes de services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le tableau suivant répertorie les listes ACL supplémentaires définies par le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

|Composant demandeur|Compte|Ressource|Autorisations|
|--------------------------|-------------|--------------|-----------------|
|MSSQLServer|Utilisateurs du journal des performances|Instid\MSSQL\binn|Lister le contenu des dossiers|
||Utilisateurs de l’Analyseur de performances|Instid\MSSQL\binn|Lister le contenu des dossiers|
||Utilisateurs du journal des performances, Utilisateurs de l'Analyseur de performances|\WINNT\system32\sqlctr130.dll|Lecture, exécution|
||Administrateur uniquement|\\\\.\root\Microsoft\SqlServer\ServerEvents\\<nom_instance_sql>\*|Contrôle total|
||Administrateurs, système|\tools\binn\schemas\sqlserver\2004\07\showplan|Contrôle total|
||Utilisateurs|\tools\binn\schemas\sqlserver\2004\07\showplan|Lecture, exécution|
|[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]|Compte Service Windows Report Server|*\<install>* \Reporting Services\LogFiles|Suppression<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||Compte Service Windows Report Server|*\<install>* \Reporting Services\ReportServer|Lire|
||Compte Service Windows Report Server|*\<install>* \Reporting Services\ReportServer\global.asax|Complète|
||Compte Service Windows Report Server|*\<install>* \Reporting Services\RSWebApp|Lecture, exécution|
||Tout le monde|*\<install>* \Reporting Services\ReportServer\global.asax|READ_CONTROL<br /><br /> FILE_READ_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_READ_ATTRIBUTES|
||Compte des services Windows ReportServer|*\<install>* \Reporting Services\ReportServer\rsreportserver.config|Suppression<br /><br /> READ_CONTROL<br /><br /> SYNCHRONIZE<br /><br /> FILE_GENERIC_READ<br /><br /> FILE_GENERIC_WRITE<br /><br /> FILE_READ_DATA<br /><br /> FILE_WRITE_DATA<br /><br /> FILE_APPEND_DATA<br /><br /> FILE_READ_EA<br /><br /> FILE_WRITE_EA<br /><br /> FILE_READ_ATTRIBUTES<br /><br /> FILE_WRITE_ATTRIBUTES|
||Tout le monde|Clés Report Server (ruche Instid)|Demander la valeur<br /><br /> Énumérer les sous-clés<br /><br /> Notifier<br /><br /> Contrôle en lecture|
||Utilisateur de Terminal Services|Clés Report Server (ruche Instid)|Demander la valeur<br /><br /> Définir la valeur<br /><br /> Créer une sous-clé<br /><br /> Énumérer les sous-clés<br /><br /> Notifier<br /><br /> DELETE<br /><br /> Contrôle en lecture|
||Utilisateurs avec pouvoir|Clés Report Server (ruche Instid)|Demander la valeur<br /><br /> Définir la valeur<br /><br /> Créer une sous-clé<br /><br /> Énumérer les sous-clés<br /><br /> Notifier<br /><br /> DELETE<br /><br /> Contrôle en lecture|

\*Ceci est l’espace de noms du fournisseur WMI.

### <a name="file-system-permissions-related-to-unusual-disk-locations"></a><a name="Unusual_Locations"></a> Autorisations de système de fichiers pour des emplacements de disque exceptionnels

L’emplacement d’installation par défaut est **systemdrive**, en général le lecteur C. Cette section donne des informations supplémentaires dans le cas où les bases de données tempdb ou utilisateur sont installées à des emplacements inhabituels.

**Lecteur non défini par défaut**

En cas d'installation sur un lecteur local autre que l'unité par défaut, le SID par service doit avoir accès à l'emplacement de fichier. Le programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurera l’accès requis.

**Partage réseau**

Quand des bases de données sont installées sur un partage réseau, le compte de service doit avoir accès à l’emplacement de fichier des bases de données tempdb et utilisateur. Le programme d'installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne peut pas configurer l'accès à un partage réseau. L’utilisateur doit configurer l’accès à un emplacement tempdb pour le compte de service avant d’exécuter le programme d’installation. L'utilisateur doit configurer l'accès à l'emplacement de la base de données d'utilisateur avant de créer la base de données.

> [!NOTE]
> Les comptes virtuels ne peuvent pas être authentifiés sur un emplacement distant. Tous les comptes virtuels utilisent l'autorisation de compte d'ordinateur. Configurez le compte d’ordinateur au format _<nom_domaine>_ **\\** _<nom_ordinateur>_ **$** .

### <a name="reviewing-additional-considerations"></a><a name="Review_additional_considerations"></a> Considérations supplémentaires

Le tableau suivant indique les autorisations nécessaires pour que les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournissent des fonctionnalités supplémentaires.

|Service/Application|Fonctionnalités|Autorisation requise|
|--------------------------|-------------------|-------------------------|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|Écrire sur MailSlot avec xp_sendmail.|Autorisations d'écriture réseau.|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (MSSQLSERVER)|Exécuter xp_cmdshell pour un utilisateur autre qu'un administrateur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|Fonctionne comme un composant du système d'exploitation et remplace le jeton de niveau processus.|
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent (MSSQLSERVER)|Utiliser la fonctionnalité de redémarrage automatique.|Doit être membre du groupe local Administrateurs.|
|Assistant Paramétrage du[!INCLUDE[ssDE](../../includes/ssde-md.md)]|Règle les bases de données pour des performances de requête optimales.|À la première utilisation, un utilisateur doté des informations d'identification d'administrateur système doit initialiser l'application. Après l’initialisation, les utilisateurs dbo peuvent utiliser l’Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour paramétrer uniquement les tables qui leur appartiennent. Pour plus d'informations, consultez « Initialisation de l'Assistant Paramétrage du [!INCLUDE[ssDE](../../includes/ssde-md.md)] » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|

> [!IMPORTANT]
> Avant de procéder à une mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], activez SQL Server Agent et vérifiez la configuration par défaut requise : le compte de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent doit être membre du rôle serveur fixe [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin**.

### <a name="registry-permissions"></a><a name="Registry"></a> Autorisations de Registre

La ruche du Registre est créée sous **HKLM\Software\Microsoft\Microsoft SQL Server\\** _<ID_Instance>_ pour les composants qui prennent les instances en charge. Par exemple

- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSASSQL13.MyInstance**
- **HKLM\Software\Microsoft\Microsoft SQL Server\MSSQL.130**

Le Registre maintient également le mappage d'un ID d'instance sur un nom d'instance. Le mappage de l'ID d'instance sur le nom d'instance se maintient comme suit :

- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\SQL] "InstanceName"="MSSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\OLAP] "InstanceName"="MSASSQL13"**
- **[HKEY_LOCAL_MACHINE\Software\Microsoft\Microsoft SQL Server\Instance Names\RS] "InstanceName"="MSRSSQL13"**

### <a name="wmi"></a><a name="WMI"></a> WMI

Windows Management Instrumentation (WMI) doit être en mesure de se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Pour cette prise en charge, le SID par service du fournisseur WMI Windows (**NT SERVICE\winmgmt**) est configuré dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)].

Le fournisseur WMI SQL nécessite les autorisations minimales suivantes :

- Appartenance aux rôles de base de données fixes **db_ddladmin** ou **db_owner** dans la base de données msdb.
- Autorisation **CREATE DDL EVENT NOTIFICATION** dans le serveur.
- Autorisation **CREATE TRACE EVENT NOTIFICATION** dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)].
- Autorisation de niveau serveur **VIEW ANY DATABASE** .

  Le programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crée un espace de noms WMI SQL et accorde l’autorisation de lecture au SID de service de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent.

### <a name="named-pipes"></a><a name="Pipes"></a> Canaux nommés

Pendant toute l'installation, le programme d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournit l'accès au [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] via le protocole de mémoire partagée, qui est un canal nommé local.

## <a name="provisioning"></a><a name="Provisioning"></a> Approvisionnement

Cette section décrit comment les comptes sont mis en service à l'intérieur des différents composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [Configuration du moteur de base de données](#DE_Prov)

  - [Principaux Windows](#Win_Principals)
  - [Compte sa](#sa)
  - [Connexion et privilèges SID par service SQL Server](#Logins)
  - [Connexion et privilèges de SQL Server Agent](#Agent)
  - [Instance de cluster de basculement et privilèges HADRON et SQL](#Hadron)
  - [Enregistreur et privilèges SQL](#Writer)
  - [WMI et privilèges SQL](#SQLWMI)
- [Approvisionnement SSAS](#SSAS)
- [Approvisionnement SSRS](#SSRS)

### <a name="database-engine-provisioning"></a><a name="DE_Prov"></a> Configuration du moteur de base de données

Les comptes suivants sont ajoutés comme connexions dans le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)].

#### <a name="windows-principals"></a><a name="Win_Principals"></a> Principaux Windows

Pendant l'installation, le programme d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert qu'au moins un compte d'utilisateur soit nommé comme membre du rôle serveur fixe **sysadmin** .

#### <a name="sa-account"></a><a name="sa"></a> Compte sa

Le compte **sa** est toujours présent sous la forme d'une connexion de [!INCLUDE[ssDE](../../includes/ssde-md.md)] et est un membre du rôle serveur fixe **sysadmin** . Quand le [!INCLUDE[ssDE](../../includes/ssde-md.md)] est installé en utilisant seulement l’authentification Windows (c’est-à-dire quand l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas activée), la connexion **sa** est toujours présente, mais elle est désactivée, et le mot de passe est complexe et aléatoire. Pour plus d’informations sur l’activation du compte **sa** , consultez [Modifier le mode d’authentification du serveur](../../database-engine/configure-windows/change-server-authentication-mode.md).

#### <a name="sql-server-per-service-sid-login-and-privileges"></a><a name="Logins"></a> Connexion et privilèges SID par service SQL Server

Le SID par service (parfois aussi appelé « principal de sécurité de service » (SID)) du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est provisionné en tant que connexion [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La connexion SID par service est membre du rôle serveur fixe **sysadmin** . Pour plus d’informations sur les SID par service, consultez [Utilisation des SID de service pour accorder des autorisations à des services dans SQL Server](../../relational-databases/security/using-service-sids-to-grant-permissions-to-services-in-sql-server.md).

#### <a name="sql-server-agent-login-and-privileges"></a><a name="Agent"></a> Connexion et privilèges de SQL Server Agent

Le SID par service du service Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est approvisionné comme une connexion du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . La connexion SID par service est membre du rôle serveur fixe **sysadmin** .

#### <a name="sshadrc-and-sql-failover-cluster-instance-and-privileges"></a><a name="Hadron"></a> [!INCLUDE[ssHADRc](../../includes/sshadrc-md.md)] et instance de cluster de basculement et privilèges SQL

Lors de l’installation du [!INCLUDE[ssDE](../../includes/ssde-md.md)] comme [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] ou instance de cluster de basculement SQL, **LOCAL SYSTEM** est configuré dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)]. La connexion **LOCAL SYSTEM** reçoit l’autorisation **ALTER ANY AVAILABILITY GROUP** (pour les [!INCLUDE[ssHADR](../../includes/sshadr-md.md)]) et l’autorisation **VIEW SERVER STATE** (pour l’instance de cluster de basculement SQL).

#### <a name="sql-writer-and-privileges"></a><a name="Writer"></a> Enregistreur et privilèges SQL

Le SID par service du service Enregistreur VSS [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est approvisionné comme une connexion du [!INCLUDE[ssDE](../../includes/ssde-md.md)] . La connexion SID par service est membre du rôle serveur fixe **sysadmin** .

#### <a name="sql-wmi-and-privileges"></a><a name="SQLWMI"></a> WMI et privilèges SQL

Le programme d’installation de[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] approvisionne le compte **NT SERVICE\Winmgmt** comme une connexion du [!INCLUDE[ssDE](../../includes/ssde-md.md)] et l’ajoute au rôle serveur fixe **sysadmin** .

#### <a name="ssrs-provisioning"></a>Approvisionnement SSRS

Le compte spécifié pendant l'installation est approvisionné comme un membre du rôle de base de données **RSExecRole** . Pour plus d’informations, consultez [Configurer le compte de service Report Server &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).

### <a name="ssas-provisioning"></a><a name="SSAS"></a> Approvisionnement SSAS

Les configurations requises pour le compte de service[!INCLUDE[ssAS](../../includes/ssas-md.md)] varient selon le déploiement du serveur. Si vous installez [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)], le programme d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiert que vous configuriez le service [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de façon à ce qu'il soit exécuté sous un compte de domaine. Les comptes de domaine sont obligatoires pour prendre en charge la fonctionnalité de compte géré intégrée à SharePoint. Pour cette raison, le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne fournit pas un compte de service par défaut, tel qu'un compte virtuel, pour une installation [!INCLUDE[ssGeminiShort](../../includes/ssgeminishort-md.md)] . Pour plus d’informations sur la configuration de [!INCLUDE[ssGemini](../../includes/ssgemini-md.md)] pour SharePoint, consultez [Configuration des comptes de service Power Pivot](/analysis-services/power-pivot-sharepoint/configure-power-pivot-service-accounts).

Pour toutes les autres installations [!INCLUDE[ssAS](../../includes/ssas-md.md)] autonomes, vous pouvez configurer le service pour qu'il s'exécute sous un compte de domaine, un compte système intégré, un compte géré ou un compte virtuel. Pour plus d’informations sur la configuration de compte, consultez [Configurer les comptes de service &#40;Analysis Services&#41;](/analysis-services/instances/configure-service-accounts-analysis-services).

Pour les installations en cluster, vous devez spécifier un compte de domaine ou un compte système intégré. Les comptes gérés ou les comptes virtuels ne sont pas pris en charge pour les clusters de basculement [!INCLUDE[ssAS](../../includes/ssas-md.md)] .

Toutes les installations [!INCLUDE[ssAS](../../includes/ssas-md.md)] requièrent que vous spécifiez un administrateur système de l'instance [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] . Les privilèges d'administrateur sont approvisionnés dans le rôle **serveur** d'Analysis Services.

### <a name="ssrs-provisioning"></a><a name="SSRS"></a> Approvisionnement SSRS

Le compte spécifié pendant l'installation est approvisionné dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] comme un membre du rôle de base de données **RSExecRole** . Pour plus d’informations, consultez [Configurer le compte de service Report Server &#40;Gestionnaire de configuration de SSRS&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).

## <a name="upgrading-from-previous-versions"></a><a name="Upgrade"></a> Mise à niveau depuis les versions précédentes

Cette section décrit les modifications effectuées pendant la mise à niveau d'une version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] nécessite [!INCLUDE[nextref_longhorn](../../includes/nextref-longhorn-md.md)] R2 SP1, Windows Server 2012, Windows 8.0, Windows Server 2012 R2, ou Windows 8.1. Toute version précédente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui exécute une version de système d'exploitation antérieure doit avoir le système d'exploitation mis à niveau avant la mise à niveau de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
- Pendant la mise à niveau de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] à [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], le programme d'installation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] configurera [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la façon suivante.

  - Le [!INCLUDE[ssDE](../../includes/ssde-md.md)] s'exécute avec le contexte de sécurité du SID par service. Le SID par service a accès aux dossiers de fichiers de l'instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (tels que DATA), et aux clés de Registre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  - Le SID par service du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est configuré dans le [!INCLUDE[ssDE](../../includes/ssde-md.md)] comme un membre du rôle serveur fixe **sysadmin** .
  - Les SID par service sont ajoutés aux groupes Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locaux, à moins que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne soit une instance de cluster de basculement.
  - Les ressources [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] restent approvisionnées sur les groupes Windows [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locaux.
  - Le nom du groupe Windows local pour les services **SQLServer2005MSSQLUser$** _<nom_ordinateur>_ **$** _<nom_instance>_ est remplacé par **SQLServerMSSQLUser$** _<nom_ordinateur>_ **$** _<nom_instance>_ . Les emplacements de fichiers pour les bases de données migrées auront des entrées de contrôle d'accès (ACE) pour les groupes Windows locaux. Les emplacements de fichiers pour les nouvelles bases de données auront des entrées ACE pour le SID par service.

- Pendant la mise à niveau de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conservera les ACE pour le SID par service [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].
- Pour une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , l'ACE du compte de domaine configuré pour le service sera conservé.

## <a name="appendix"></a><a name="Appendix"></a> Annexe

Cette section contient des informations supplémentaires sur les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

- [Description des comptes de service](#Serv_Accts)
- [Identification des services dépendant et ne dépendant pas des instances](#Identify_instance_aware_and_unaware)
- [Noms des services localisés](#Localized_service_names)

### <a name="description-of-service-accounts"></a><a name="Serv_Accts"></a> Description des comptes de service

Le compte de service est le compte utilisé pour démarrer un service Windows, comme le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]. Pour exécuter SQL Server, il n’est pas nécessaire d’ajouter le compte de service en tant que compte de connexion à SQL Server en plus du SID de service, qui est toujours présent et est membre du rôle de serveur fixe **sysadmin**.

#### <a name="accounts-available-with-any-operating-system"></a><a name="Any_OS"></a> Comptes disponibles avec tout système d’exploitation

En plus des nouveaux comptes[MSA](#MSA), [gMSA](#GMSA) et [virtuels](#VA_Desc) décrits précédemment, les comptes suivants peuvent être utilisés.

<a name="Domain_User"></a> **Compte d’utilisateur de domaine**

Si le service doit interagir avec des services réseau, des ressources de domaine telles que des partages de fichiers, ou s'il utilise des connexions de serveur liées à d'autres ordinateurs qui exécutent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez utiliser un compte de domaine doté de privilèges minimaux. De nombreuses activités de serveur à serveur ne peuvent être exécutées qu'avec un compte d'utilisateur de domaine. Ce compte doit être créé au préalable via l'administration de domaine dans votre environnement.

> [!NOTE]
> Si vous configurez le serveur SQL Server pour utiliser un compte de domaine, vous pouvez isoler les privilèges pour le service, mais vous devez gérer les mots de passe manuellement ou créer une solution personnalisée pour la gestion de ces mots de passe. De nombreuses applications serveur utilisent cette stratégie pour améliorer la sécurité, mais elle requiert une administration supplémentaire et est plus complexe. Dans ces déploiements, les administrateurs du service passent beaucoup de temps à exécuter des tâches de maintenance telles que la gestion des mots de passe du service et des noms de principal du service (SPN), qui sont obligatoires pour l'authentification Kerberos. De plus, ces tâches de maintenance peuvent interrompre le service.

<a name="Local_User"></a> **Comptes d’utilisateurs locaux**

Si l'ordinateur ne fait pas partie d'un domaine, un compte d'utilisateur local sans autorisations d'administrateur Windows est recommandé.

<a name="Local_Service"></a> **Compte de service local**

Le compte de service local est un compte intégré qui bénéficie du même niveau d'accès aux ressources et aux objets que les membres du groupe Utilisateurs. Cet accès limité constitue une mesure de sécurité pour le système, au cas où le fonctionnement de services ou de processus individuels serait compromis. Les services qui s'exécutent en tant que compte de service local accèdent aux ressources réseau dans le cadre d'une session Null, sans informations d'identification. 
> [!NOTE]
> Le compte de service local n’est pas pris en charge pour les services [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent. Le service local n'est pas pris en charge comme compte exécutant ces services, car il s'agit d'un service partagé et les autres services exécutés sous le service local disposent de droits d'accès d'administrateur système à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
Le nom réel du compte est **NT AUTHORITY\LOCAL SERVICE**.

<a name="Network_Service"></a> **Compte de service réseau**

Le compte de service réseau est un compte intégré qui bénéficie d'un niveau d'accès aux ressources et aux objets supérieur à celui des membres du groupe Utilisateurs. Les services qui s’exécutent en tant que compte de service réseau accèdent aux ressources réseau à l’aide des informations d’identification du compte d’ordinateur au format _<nom_domaine>_ **\\** _<nom_ordinateur>_ **$** . Le nom réel du compte est **NT AUTHORITY\NETWORK SERVICE**.

<a name="Local_System"></a> **Compte système local**

Le compte système local est un compte intégré doté de privilèges très élevés. Il dispose de privilèges étendus sur le système local et représente l'ordinateur sur le réseau. Le nom réel du compte est **NT AUTHORITY\SYSTEM**.

### <a name="identifying-instance-aware-and-instance-unaware-services"></a><a name="Identify_instance_aware_and_unaware"></a> Identification des services dépendant et ne dépendant pas des instances

Chaque service dépendant d'une instance est associé à une instance spécifique de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]et possède sa propre ruche de Registre. Vous pouvez installer plusieurs exemplaires de services dépendant d'une instance en exécutant le programme d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour chaque composant ou service. Les services ne dépendant pas d'une instance sont partagés entre toutes les instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] installées. Ils ne sont pas associés à une instance spécifique, ne sont installés qu'une seule fois et ne peuvent pas être installés côte à côte.

Les services dépendant d'une instance dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent les éléments suivants :

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent

   Sachez que le service de l'Agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est désactivé sur les instances [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] et [!INCLUDE[ssExpress](../../includes/ssexpress-md.md)] with Advanced Services.

- [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)]\*
- [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
- Recherche en texte intégral

  Les services ne dépendant pas d'une instance dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluent les éléments suivants :

- [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]
- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Browser
- Enregistreur SQL

 \*Analysis Services en mode intégré SharePoint s’exécute comme '[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]' en tant qu’instance nommée unique. Le nom de l'instance est fixe. Vous ne pouvez pas spécifier de nom différent. Vous ne pouvez installer qu’une instance d’Analysis Services s’exécutant comme '[!INCLUDE[ssGemini](../../includes/ssgemini-md.md)]' sur chaque serveur physique.

### <a name="localized-service-names"></a><a name="Localized_service_names"></a> Noms des services localisés

Le tableau ci-dessous indique les noms de services affichés dans les versions localisées de Windows.

|Langage|Nom du service local|Nom du service réseau|Nom du système local|Nom du groupe Admin|
|--------------|----------------------------|------------------------------|---------------------------|--------------------------|
|Anglais<br /><br /> Chinois simplifié<br /><br /> Chinois traditionnel<br /><br /> Coréen<br /><br /> Japonais|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|Allemand|NT-AUTORITÄT\LOKALER DIENST|NT-AUTORITÄT\NETZWERKDIENST|NT-AUTORITÄT\SYSTEM|VORDEFINIERT\Administratoren|
|Français|AUTORITE NT\SERVICE LOCAL|AUTORITE NT\SERVICE RÉSEAU|AUTORITE NT\SYSTEM|BUILTIN\Administrators|
|Italien|NT AUTHORITY\SERVIZIO LOCALE|NT AUTHORITY\SERVIZIO DI RETE|NT AUTHORITY\SYSTEM|BUILTIN\Administrators|
|Espagnol|NT AUTHORITY\SERVICIO LOC|NT AUTHORITY\SERVICIO DE RED|NT AUTHORITY\SYSTEM|BUILTIN\Administradores|
|Russe|NT AUTHORITY\LOCAL SERVICE|NT AUTHORITY\NETWORK SERVICE|NT AUTHORITY\СИСТЕМА|BUILTIN\Администраторы|

## <a name="related-content"></a>Contenu associé

[Considérations sur la sécurité pour une installation SQL Server](../../sql-server/install/security-considerations-for-a-sql-server-installation.md)

[Emplacements des fichiers pour les instances par défaut et les instances nommées de SQL Server](../../sql-server/install/file-locations-for-default-and-named-instances-of-sql-server.md)

[Installer Master Data Services](../../master-data-services/install-windows/install-master-data-services.md)
