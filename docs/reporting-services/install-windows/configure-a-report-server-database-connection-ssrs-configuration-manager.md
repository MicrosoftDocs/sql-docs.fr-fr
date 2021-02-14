---
title: Configurer une connexion à la base de données du serveur de rapports (Gestionnaire de configuration) | Microsoft Docs
description: Chaque instance de serveur de rapports nécessite une connexion à la base de données du serveur de rapports qui stocke les rapports, les sources de données partagées, les ressources et les métadonnées gérés par le serveur.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: seo-lt-2019, seo-mmd-2019
ms.date: 01/04/2020
ms.openlocfilehash: fdbf12310fb46add3ce9e3240d7c249028eda606
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057294"
---
# <a name="configure-a-report-server-database-connection-report-server-configuration-manager"></a>Configurer une connexion à la base de données du serveur de rapports (Gestionnaire de configuration du serveur de rapports)

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE [ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Chaque instance de serveur de rapports nécessite une connexion à la base de données du serveur de rapports qui stocke les rapports, les sources de données partagées, les ressources et les métadonnées gérés par le serveur. La connexion initiale peut être créée lors de l'installation d'un serveur de rapports si vous installez la configuration par défaut. Dans la plupart des cas, vous utilisez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour configurer la connexion une fois l'installation terminée. Vous pouvez modifier la connexion à tout moment afin de changer de type de compte ou de redéfinir les informations d'identification. Pour obtenir des instructions détaillées sur la création de la base de données et la configuration de la connexion, consultez [Créer une base de données du serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-native-mode-report-server-database.md).

Vous devez configurer une connexion à la base de données du serveur de rapports dans les circonstances suivantes :  

- Configuration d'un serveur de rapports pour une première utilisation  

- Configuration d'un serveur de rapports de manière à utiliser une base de données de serveur de rapports différente  

- Modification du compte ou du mot de passe de l'utilisateur permettant d'établir la connexion à la base de données. Vous devez uniquement mettre à jour la connexion de base de données lorsque les informations du compte sont stockées dans le fichier RSReportServer.config. Si vous utilisez le compte de service pour la connexion (qui utilise la sécurité intégrée de Windows comme type d'informations d'identification), le mot de passe n'est pas stocké, ce qui évite de mettre à jour les informations de connexion. Pour plus d’informations sur la modification des comptes, consultez [Configurer le compte de service Report Server &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

- Configuration d'un déploiement avec montée en puissance parallèle de serveurs de rapports La configuration d'un déploiement avec montée en puissance parallèle nécessite la création de plusieurs connexions à une base de données de serveur de rapports. Pour plus d’informations sur cette opération à plusieurs étapes, consultez [Configurer un déploiement par scale-out de serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="how-reporting-services-connects-to-the-database-engine"></a>Comment Reporting Services se connecte au moteur de base de données

L'accès du serveur de rapports à une base de données de serveur de rapports dépend des informations d'identification et de connexion, ainsi que de la validité des clés de chiffrement de l'instance de serveur de rapports qui utilise cette base de données. Il est nécessaire de recourir à des clés de chiffrement valides pour stocker et extraire des données sensibles. Des clés de chiffrement sont créées automatiquement lors de la première configuration de la base de données. Une fois les clés créées, vous devez les mettre à jour si vous changez l'identité du service Report Server. Pour plus d’informations sur l’utilisation des clés de chiffrement, consultez [Configurer et gérer des clés de chiffrement &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-encryption-keys-manage-encryption-keys.md).  

La base de données du serveur de rapports est un composant interne, accessible uniquement par le serveur de rapports. Les informations d'identification et de connexion que vous spécifiez pour la base de données du serveur de rapports sont utilisées exclusivement par le serveur de rapports. Les utilisateurs qui demandent des rapports n'ont pas besoin d'autorisations de base de données ou d'une connexion de base de données pour la base de données du serveur de rapports.  

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] utilise **System.Data.SqlClient** pour se connecter au [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui héberge la base de données de serveur de rapports. Si vous utilisez une instance locale du [!INCLUDE[ssDE](../../includes/ssde-md.md)], le serveur de rapports établit la connexion à l'aide de la mémoire partagée. Si vous utilisez un serveur de base de données distant pour la base de données du serveur de rapports, il se peut que vous ayez à activer les connexions distantes selon l'édition que vous utilisez. Si vous utilisez l’édition Enterprise, les connexions distantes sont activées par défaut pour TCP/IP.  

Pour vérifier que l’instance accepte les connexions à distance, cliquez successivement sur **Démarrer**, **Tous les programmes**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Outils de configuration**, **Gestionnaire de configuration SQL Server**, puis vérifiez que le protocole TCP/IP est activé pour chaque service.  

Lorsque vous activez les connexions à distance, les protocoles client et serveur sont également activés. Pour vérifier que les protocoles sont activés, cliquez successivement sur **Démarrer**, **Tous les programmes**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Outils de configuration**, **Gestionnaire de configuration SQL Server**, **Configuration du réseau SQL Server**, puis cliquez sur **Protocoles pour MSSQLSERVER**. Pour plus d’informations, consultez [Activer ou désactiver un protocole réseau de serveur](../../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="defining-a-report-server-database-connection"></a>Définition d'une connexion de base de données de serveur de rapports

Pour configurer la connexion, vous devez utiliser le Gestionnaire de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou l'utilitaire de ligne de commande **rsconfig** . Un serveur de rapports requiert les informations de connexion suivantes :  

- Nom de l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] hébergeant la base de données du serveur de rapports.  

- Nom de la base de données du serveur de rapports. Lorsque vous créez une connexion pour la première fois, vous pouvez créer une nouvelle base de données de serveur de rapports ou sélectionner une base de données existante. Pour plus d’informations, consultez [Créer une base de données du serveur de rapports &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md).  

- Type d'informations d'identification. Vous pouvez utiliser les comptes de service, un compte de domaine Windows ou une connexion de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

- Nom d'utilisateur et mot de passe (requis seulement si vous utilisez un compte de domaine Windows ou une connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ).

Les informations d'identification que vous fournissez doivent disposer de l'accès à la base de données du serveur de rapports. Si vous utilisez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , cette opération est effectuée automatiquement. Pour plus d'informations sur les autorisations requises pour accéder à la base de données, consultez la section « Autorisations de base de données » dans cette rubrique.  

### <a name="storing-database-connection-information"></a>Stockage des informations de connexion à la base de données

[!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] stocke et chiffre les informations de connexion dans les paramètres RSreportserver.config suivants. Vous devez recourir à l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] ou à l'utilitaire rsconfig.exe pour créer des valeurs chiffrées pour ces paramètres.  

Certaines valeurs ne sont pas définies pour tous les types de connexion. Si vous configurez la connexion à l’aide de valeurs par défaut (c’est-à-dire avec des comptes de service pour établir la connexion), \<**LogonUser**>, \<**LogonDomain**> et \<**LogonCred**> apparaîtront vides comme suit :  

```
<Dsn></Dsn>  
<ConnectionType></ConnectionType>  
<LogonUser></LogonUser>  
<LogonDomain></LogonDomain>  
<LogonCred></LogonCred>  
```

Si vous configurez la connexion de manière à utiliser un compte Windows ou une connexion de base de données spécifique, vous devez mettre à jour les valeurs qui sont stockées si vous changez par la suite le compte ou l'ouverture de session.  

### <a name="choosing-a-credential-type"></a>Choix d'un type d'informations d'identification

Il existe trois types d'informations d'identification utilisables dans une connexion à une base de données de serveur de rapports :  

- Sécurité intégrée de Windows avec utilisation du compte de service Report Server. Comme le serveur de rapports est implémenté comme service unique, seul le compte sous lequel le service s'exécute requiert l'accès à la base de données.  
  
- Un compte d'utilisateur Windows Si le serveur de rapports et la base de données du serveur de rapports sont installés sur le même ordinateur, vous pouvez utiliser un compte local. Sinon, vous devez utiliser un compte de domaine.  
  
- Connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
  
> [!NOTE]
> Une extension d'authentification personnalisée ne peut pas être utilisée pour établir une connexion à une base de données de serveur de rapports. Les extensions d'authentification personnalisées servent uniquement à authentifier un principal auprès d'un serveur de rapports. Elles n'affectent pas les connexions à la base de données du serveur de rapports ou aux sources de données externes qui fournissent un contenu aux rapports.
  
Si l'instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] est configurée pour l'authentification Windows et se trouve dans le même domaine ou un domaine approuvé avec le serveur de rapports, vous pouvez configurer la connexion pour utiliser le compte de service ou un compte d'utilisateur de domaine que vous gérez comme propriété de connexion à travers l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Si le serveur de base de données figure dans un domaine différent ou si vous utilisez la sécurité de groupe de travail, vous devez configurer la connexion pour utiliser une connexion à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Dans ce cas, veillez bien à chiffrer la connexion.  

::: moniker range=">=sql-server-ver15"

> [!NOTE]
> Quand vous utilisez Azure SQL Managed Instance pour héberger des bases de données du serveur de rapports, l’authentification SQL Server est le seul type d’informations d’identification pris en charge. De plus, notez que Managed Instance ne peut pas héberger l’instance du serveur de rapports.

::: moniker-end

#### <a name="using-service-accounts-and-integrated-security"></a>Utilisation de comptes de service et de la sécurité intégrée

Vous pouvez utiliser la sécurité intégrée de Windows pour vous connecter via le compte de service Report Server. Le compte bénéficie des droits de connexion à la base de données du serveur de rapports. Il s'agit du type d'informations d'identification par défaut que choisit le programme d'installation si vous installez [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] dans la configuration par défaut.  

Le compte de service est un compte approuvé qui implique une faible maintenance de la gestion d'une connexion de base de données de serveur de rapports. Comme le compte de service utilise la sécurité intégrée de Windows pour établir la connexion, il n'est pas nécessaire que les informations d'identification soient stockées. Toutefois, si vous modifiez par la suite l'identité ou le mot de passe du compte de service (par exemple, basculer d'un compte intégré vers un compte de domaine), veillez bien à utiliser l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour effectuer la modification. L'outil met automatiquement à jour les autorisations de base de données pour utiliser les informations modifiées sur le compte. Pour plus d’informations, consultez [Configurer le compte de service Report Server &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md).  

Si vous configurez la connexion de base de données pour qu'elle utilise le compte de service, le compte doit posséder les autorisations réseau si la base de données du serveur de rapports se trouve sur un ordinateur distant. N'utilisez pas le compte de service si la base de données du serveur de rapports se trouve dans un domaine différent, derrière un pare-feu ou si vous utilisez la sécurité des groupes de travail à la place de la sécurité des domaines. Utilisez un compte d'utilisateur de base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à la place.  

#### <a name="using-a-domain-user-account"></a>Utilisation d'un compte d'utilisateur de domaine

Vous pouvez spécifier un compte d'utilisateur Windows pour la connexion à la base de données du serveur de rapports. Si vous utilisez un compte local ou un compte de domaine, vous devez mettre à jour la connexion à la base de données du serveur de rapports chaque fois que vous changez le mot de passe ou le compte. Utilisez toujours l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour mettre à jour la connexion.  

#### <a name="using-a-sql-server-login"></a>Utilisation d'une connexion SQL Server

Vous pouvez spécifier une seule connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour établir la connexion à la base de données du serveur de rapports. Si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et que la base de données du serveur de rapports se trouve sur un ordinateur distant, utilisez IPSec pour sécuriser la transmission des données entre les serveurs. Si vous utilisez une connexion de base de données, vous devez mettre à jour la connexion à la base de données du serveur de rapports chaque fois que vous changez le mot de passe ou le compte.

### <a name="database-permissions"></a>Autorisations de base de données

Les rôles suivants sont attribués aux comptes utilisés pour la connexion à la base de données du serveur de rapports :  

- Rôles **public** et **RSExecRole** pour la base de données **ReportServer** .  

- Rôle **RSExecRole** pour les bases de données **master**, **msdb** et **ReportServerTempDB** .  

Lorsque vous utilisez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour créer ou modifier la connexion, ces autorisations sont automatiquement accordées. Si vous recourez à l'utilitaire rsconfig.exe et que vous spécifiez un autre compte pour la connexion, vous devez mettre à jour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour ce nouveau compte. Dans l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , vous pouvez créer les fichiers de script qui mettent à jour la connexion [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour le serveur de rapports.  

### <a name="verifying-the-database-name"></a>Vérification du nom de la base de données

Utilisez l'outil de configuration de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour déterminer quelle est la base de données de serveur de rapports qu'utilise une instance de serveur de rapports particulière. Pour rechercher le nom, connectez-vous à l'instance de serveur de rapports puis ouvrez la page Installation de la base de données.  

## <a name="using-a-different-report-server-database-or-moving-a-report-server-database"></a>Utilisation d'une base de données de serveur de rapports différente ou déplacement d'une base de données de serveur de rapports

Vous pouvez configurer une instance de serveur de rapports de manière à utiliser une autre base de données de serveur de rapports en modifiant les informations de connexion. Il est courant de changer de base de données lors du déploiement d'un serveur de rapports de production. C'est généralement au passage d'une base de données de serveur de rapports de test à une base de données de serveur de rapports de production que les serveurs de production sont transférés. Par ailleurs, vous pouvez déplacer une base de données de serveur de rapports vers un autre ordinateur. Pour plus d’informations, consultez [Mettre à niveau et migrer Reporting Services](../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="configuring-multiple-reports-servers-to-use-the-same-report-server-database"></a>Configuration de plusieurs serveurs de rapports de manière à utiliser la même base de données de serveur de rapports

Vous pouvez configurer plusieurs serveurs de rapports de manière à utiliser la même base de données de serveur de rapports. Cette configuration de déploiement est appelée déploiement avec montée en puissance parallèle. Cette configuration est une condition préalable requise si vous voulez exécuter plusieurs serveurs de rapports dans un cluster de serveurs. Toutefois, vous pouvez aussi utiliser cette configuration pour segmenter les applications de service ou pour tester l'installation et les paramètres d'une nouvelle instance de serveur de rapports afin de la comparer à l'installation existante d'un serveur de rapports. Pour plus d’informations, consultez [Configurer un déploiement par scale-out de serveur de rapports en mode natif &#40;Gestionnaire de configuration du serveur de rapports&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  

## <a name="next-steps"></a>Étapes suivantes

- [Créer une base de données du serveur de rapports](../../reporting-services/install-windows/ssrs-report-server-create-a-report-server-database.md)  
- [Gérer un serveur de rapports Reporting Services en mode natif](../../reporting-services/report-server/manage-a-reporting-services-native-mode-report-server.md)   
- [Configurer le compte de service Report Server](../../reporting-services/install-windows/configure-the-report-server-service-account-ssrs-configuration-manager.md)

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
