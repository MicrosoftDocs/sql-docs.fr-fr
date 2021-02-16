---
title: Prise en charge de Scale Out pour la haute disponibilité | Microsoft Docs
description: Découvrez comment configurer SQL Server Integration Services (SSIS) Scale Out Master pour la haute disponibilité.
ms.custom: performance
ms.date: 05/23/2018
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: dd53bf6f045687f7d79f6c0e836490adb60cda44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348865"
---
# <a name="scale-out-support-for-high-availability"></a>Prise en charge de Scale Out pour la haute disponibilité

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Dans SSIS Scale Out, la haute disponibilité côté Scale Out Worker est fournie par l’exécution de packages avec plusieurs Scale Out Workers.

La haute disponibilité côté Scale Out Master est obtenue avec la fonctionnalité [Always On pour le catalogue SSIS](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb) et le cluster de basculement Windows. Dans cette solution, plusieurs instances de Scale Out Master sont hébergées dans un cluster de basculement Windows. Quand le service Scale Out Master ou SSISDB est arrêté sur le nœud principal, le service ou SSISDB sur le nœud secondaire continue d’accepter les demandes d’utilisateur et de communiquer avec les Scale Out Workers.

La haute disponibilité côté Scale Out Master peut aussi être obtenue au moyen d’une instance de cluster de basculement SQL Server. Consultez [Prise en charge de Scale Out pour la haute disponibilité au moyen d’une instance de cluster de basculement SQL Server](scale-out-failover-cluster-instance.md).

Pour configurer la haute disponibilité côté Scale Out Master à l’aide de la fonctionnalité Always On pour le catalogue SSIS, suivez les étapes ci-dessous :

## <a name="1-prerequisites"></a>1. Prérequis
Configurez un cluster de basculement Windows. Pour obtenir des instructions, consultez le billet de blog [Installation de la fonctionnalité de cluster de basculement et des outils pour Windows Server 2012](https://techcommunity.microsoft.com/t5/failover-clustering/installing-the-failover-cluster-feature-and-tools-in-windows/ba-p/371733). Installez la fonctionnalité et les outils sur tous les nœuds de cluster.

## <a name="2-install-scale-out-master-on-the-primary-node"></a>2. Installer Scale Out Master sur le nœud principal
Installez les services SQL Server Moteur de base de données, Integration Services et Scale Out Master sur le nœud principal pour Scale Out Master. 

Pendant l’installation, effectuez les étapes suivantes :

### <a name="21-set-the-account-running-scale-out-master-service-to-a-domain-account"></a>2.1 Définir le compte exécutant le service Scale Out Master sur un compte de domaine
Ce compte doit pouvoir accéder ultérieurement à SSISDB sur le nœud secondaire dans le cluster de basculement Windows. Étant donné que le service Scale Out Master et SSISDB peuvent basculer séparément, ils peuvent se trouver sur des nœuds différents après le basculement.

![Configuration du serveur à haute disponibilité](media/ha-server-config.PNG)

### <a name="22-include-the-dns-host-name-for-the-scale-out-master-service-in-the-cns-of-the-scale-out-master-certificate"></a>2.2 Inclure le nom d’hôte DNS du service Scale Out Master dans les noms communs (CN) du certificat Scale Out Master

Ce nom d’hôte est le point de terminaison de Scale Out Master, qui est créé en tant que service générique en cluster dans le cluster de basculement (voir l’étape 7).   (Veillez à fournir un nom d’hôte DNS, et non un nom de serveur.)

![Configuration de Master à haute disponibilité](media/ha-master-config.PNG)

## <a name="3-install-scale-out-master-on-the-secondary-node"></a>3. Installer Scale Out Master sur le nœud secondaire
Installez les services SQL Server Moteur de base de données, Integration Services et Scale Out Master sur le nœud secondaire pour Scale Out Master. 

Utilisez le même certificat Scale Out Master que celui utilisé sur le nœud principal. Exportez le certificat TLS/SSL Scale Out Master sur le nœud principal avec une clé privée, puis installez-le dans le magasin de certificats racine de l’ordinateur local sur le nœud secondaire. Sélectionnez ce certificat quand vous installez Scale Out Master sur le nœud secondaire.

![Configuration de Master à haute disponibilité 2](media/ha-master-config2.PNG)

> [!NOTE]
> Vous pouvez configurer plusieurs Scale Out Masters de sauvegarde en répétant ces opérations pour le Scale Out Master sur d’autres nœuds secondaires.

## <a name="4-set-up-and-configure-ssisdb-support-for-always-on"></a>4. Configurer la prise en charge de SSISDB pour Always On

Pour configurer la prise en charge de SSISDB pour Always On, suivez les instructions dans [Always On pour le catalogue SSIS (SSISDB)](../catalog/ssis-catalog.md#always-on-for-ssis-catalog-ssisdb).

Vous devez également créer un écouteur de groupe de disponibilité pour le groupe de disponibilité auquel vous ajoutez SSISDB. Consultez [Créer ou configurer un écouteur de groupe de disponibilité](../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).

## <a name="5-update-the-scale-out-master-service-configuration-file"></a>5. Mettre à jour le fichier de configuration du service Scale Out Master
Mettez à jour le fichier de configuration du service Scale Out Master (`\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`) sur le nœud principal et chaque nœud secondaire. Définissez **SqlServerName** sur *nom DNS de l’écouteur de groupe de disponibilité],[port]* .

## <a name="6-enable-package-execution-logging"></a>6. Activer la journalisation de l’exécution des packages

La journalisation dans SSISDB est effectuée par la connexion **##MS_SSISLogDBWorkerAgentLogin##** . Le mot de passe est généré automatiquement pour cette connexion. Pour effectuer la journalisation de tous les réplicas de SSISDB, effectuez les opérations suivantes

### <a name="61-change-the-password-of-ms_ssislogdbworkeragentlogin-on-the-primary-sql-server"></a>6.1 Changer le mot de passe de **##MS_SSISLogDBWorkerAgentLogin##** sur l’instance SQL Server principale

### <a name="62-add-the-login-to-the-secondary-sql-server"></a>6.2 Ajouter la connexion au nœud SQL Server secondaire

### <a name="63-update-the-connection-string-used-for-logging"></a>6.3 Mettre à jour la chaîne de connexion utilisée pour la journalisation
Appelez la procédure stockée `[catalog].[update_logdb_info]` avec les valeurs de paramètre suivantes :

-   `@server_name = '[Availability Group Listener DNS name],[Port]'`

-   `@connection_string = 'Data Source=[Availability Group Listener DNS name],[Port];Initial Catalog=SSISDB;User Id=##MS_SSISLogDBWorkerAgentLogin##;Password=[Password]];'`

## <a name="7-configure-the-scale-out-master-service-role-of-the-windows-server-failover-cluster"></a>7. Configurer le rôle du service Scale Out Master du cluster de basculement Windows Server

1.  Dans le Gestionnaire du cluster de basculement, connectez-vous au cluster pour Scale-out. Sélectionnez le cluster. Sélectionnez **Action** dans le menu, puis sélectionnez **Configurer un rôle**.

2.  Dans la boîte de dialogue **Assistant Haute disponibilité**, sélectionnez **Service générique** dans la page **Sélectionner un rôle**. Sélectionnez SQL Server Integration Services Scale Out Master 14.0 dans la page **Sélectionner un service**.

3.  Dans la page **Point d’accès client**, entrez le nom d’hôte DNS du service Scale Out Master.

    ![Assistant Haute disponibilité 1](media/ha-wizard1.PNG)

4.  Terminez l’Assistant.

Sur les machines virtuelles Azure, cette opération de configuration nécessite des étapes supplémentaires. L’explication complète de ces concepts et de ces étapes dépasse le cadre de cet article.

1.  Vous devez configurer un domaine Azure. Le clustering de basculement Windows Server nécessite que tous les ordinateurs du cluster soient membres du même domaine. Pour plus d’informations, consultez [Activer Azure Active Directory Domain Services à l’aide du portail Azure](/azure/active-directory-domain-services/create-instance).

2. Vous devez configurer un équilibreur de charge Azure. Cette opération est obligatoire pour l’écouteur du groupe de disponibilité. Pour plus d’informations, consultez [Tutoriel : équilibrer la charge du trafic interne vers les machines virtuelles avec un équilibreur de charge de base à l’aide du portail Azure](/azure/load-balancer/tutorial-load-balancer-basic-internal-portal).

## <a name="8-update-the-scale-out-master-address-in-ssisdb"></a>8. Mettre à jour l’adresse Scale Out Master dans SSISDB

Sur l’instance SQL Server principale, exécutez la procédure stockée `[catalog].[update_master_address]` avec la valeur de paramètre `@MasterAddress = N'https://[Scale Out Master service DNS host name]:[Master Port]'`. 

## <a name="9-add-the-scale-out-workers"></a>9. Ajouter les Scale Out Workers

Maintenant, vous pouvez ajouter des Scale Out Workers avec [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md). Entrez `[SQL Server Availability Group Listener DNS name],[Port]` dans la page de connexion.

## <a name="upgrade-scale-out-in-high-availability-environment"></a>Mettre à niveau Scale Out dans un environnement à haute disponibilité
Pour mettre à niveau Scale Out dans un environnement à haute disponibilité, suivez les [étapes de mise à niveau d’Always On pour le catalogue SSIS](../catalog/ssis-catalog.md#Upgrade), mettez à niveau Scale Out Master et Scale Out Worker sur chaque ordinateur, puis recréez le rôle du cluster de basculement Windows Server à l’étape 7 ci-dessus avec la nouvelle version du service Scale Out Master.

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants :
-   [SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)