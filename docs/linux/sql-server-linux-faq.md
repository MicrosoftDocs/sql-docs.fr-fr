---
title: FAQ de SQL Server sur Linux
description: Cet article fournit des réponses aux questions fréquemment posées sur SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 10/23/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 68f135dfbc2b03cfb27c7ab8b14345f0ffcc32db
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348134"
---
# <a name="sql-server-on-linux-frequently-asked-questions-faq"></a>Forum aux questions pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les sections suivantes fournissent des questions et des réponses courantes sur l’exécution de SQL Server sur Linux.

## <a name="general-questions"></a>Questions générales

1. **Quelles sont les plateformes Linux prises en charge ?**

   SQL Server est actuellement pris en charge sur Red Hat Enterprise Server, SUSE Linux Enterprise Server et Ubuntu. Il prend également en charge l’exécution dans un conteneur avec Docker. Pour obtenir les informations les plus récentes sur les versions prises en charge, consultez [Plateformes prises en charge](sql-server-linux-setup.md#supportedplatforms).

1. **SQL Server sur Linux fonctionnera-t-il sur d’autres plateformes** ?

   SQL Server est testé et pris en charge sur Linux pour les distributions précédemment répertoriées. D’autres distributions Linux sont étroitement liées et pourraient être en mesure d’exécuter SQL Server (par exemple, CentOS est étroitement lié à Red Hat Enterprise Server). Toutefois, si vous choisissez d’installer SQL Server sur un système d’exploitation non pris en charge, consultez la section **Stratégie de support** de la [Stratégie de support technique pour Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) pour comprendre les implications de la prise en charge. Notez également que certaines distributions Linux gérées par la communauté ne disposent pas d’un moyen formel de recevoir un support si le système d’exploitation sous-jacent est le problème.

1. **Est-ce que SQL Server sur Linux est le même que sur Windows ?**

   Le moteur de base de données central pour SQL Server est le même sur Linux que sur Windows. Toutefois, certaines fonctionnalités ne sont actuellement pas prises en charge sur Linux. Pour obtenir la liste des fonctionnalités qui ne sont pas prises en charge sur Linux, consultez les [Fonctionnalités et services non pris en charge](sql-server-linux-editions-and-components-2019.md#Unsupported). Passez aussi en revue les [problèmes connus](sql-server-linux-release-notes.md#known-issues). Sauf indication contraire dans ces listes, les autres services et fonctionnalités de SQL Server sont pris en charge sur Linux.

1. **Quelle est la stratégie de prise en charge de SQL Server ?**

   Pour comprendre la stratégie de support, consultez la [Stratégie de support technique pour SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server).

1. **J’ai l’habitude de travailler avec SQL Server pour Windows. Existe-t-il des ressources pour m’aider à apprendre à utiliser SQL Server sur Linux ?**

   Les [démarrages rapides](sql-server-linux-setup.md#platforms) fournissent des instructions pas à pas sur l’installation de SQL Server sur Linux et l’exécution de requêtes Transact-SQL. D’autres didacticiels fournissent des instructions supplémentaires sur l’utilisation de SQL Server sur Linux. Pour obtenir une liste de conseils tiers, consultez la [liste MSSQLTIPS de conseils pour SQL Server sur Linux](https://www.mssqltips.com/sql-server-tip-category/226/sql-server-on-linux/).

## <a name="licensing"></a>Licence

1. **Comment fonctionne la gestion des licences sur Linux ?**

   SQL Server est concédé sous licence de la même façon pour Windows et Linux. En fait, vous obtenez une licence SQL Server, puis vous pouvez choisir d’utiliser cette licence sur la plateforme de votre choix. Pour plus d'informations, consultez [Comment installer SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

1. **Quelle édition de SQL Server dois-je choisir lorsque j’ai déjà acheté une licence ?**

   Lorsque vous exécutez mssql-conf, les options suivantes s’affichent :
   
   ```
   Choose an edition of SQL Server:
      1. Evaluation (free, no production use rights, 180-day limit)
      2. Developer (free, no production use rights)
      3. Express (free)
      4. Web (PAID)
      5. Standard (PAID)
      6. Enterprise (PAID)
      7. Enterprise Core (PAID)
      8. I bought a license through a retail sales channel and have a product key to enter.
   ```
     
   Si vous avez obtenu votre licence par le biais d’une licence en volume dans le cadre d’un Accord Entreprise ou par le biais de votre abonnement MSDN, vous devez sélectionner les options 4 à 7. Cette étape ne vous invite pas à entrer la licence, mais vous devez avoir déjà acheté la licence appropriée pour votre configuration. Si vous avez acheté l’édition Standard par le biais d’un canal de vente au détail, sélectionnez l’option 8. Cette option vous invite à entrer une clé. 

1. **Comment vérifier la version et l’édition de SQL Server sur Linux ?**

   Connectez-vous à l’instance SQL Server à l’aide d'un outil client tel que **sqlcmd**, **mssql-cli** ou Visual Studio Code. Exécutez ensuite la requête Transact-SQL suivante pour vérifier la version et l’édition de SQL Server que vous exécutez : 

   ```sql
   SELECT @@VERSION
   SELECT SERVERPROPERTY('Edition')
   ```

## <a name="installation"></a>Installation

1. **Comment installer SQL Server sur mes serveurs Linux ?**

   Microsoft gère des référentiels de packages pour l’installation de SQL Server et prend en charge l’installation par le biais de gestionnaires de package natifs tels que yum, zypper et apt. Pour installer rapidement, consultez un des [démarrages rapides](sql-server-linux-setup.md#platforms).

1. **Puis-je installer SQL Server sur le sous-système Linux pour Windows 10 ?**

   Non. Linux s’exécutant sur Windows 10 n’est actuellement pas une plateforme prise en charge pour SQL Server et les outils associés.

1. **Quels systèmes de fichiers Linux est-ce que SQL Server peut utiliser pour les fichiers de données ?**

   Actuellement, SQL Server sur Linux prend en charge ext4 et XFS. La prise en charge d’autres systèmes de fichiers sera ajoutée en fonction des besoins futurs.

1. **Puis-je télécharger les packages d’installation pour installer SQL Server hors connexion ?**

   Oui. Pour plus d’informations, consultez les liens de téléchargement de package dans les [notes de publication](sql-server-linux-release-notes.md). Passez également en revue les [instructions pour les installations hors connexion](sql-server-linux-setup.md#offline).

1. **Puis-je effectuer une installation sans assistance de SQL Server sur Linux ?**

   Oui. Pour plus d’informations sur l’installation sans assistance, consultez le [Guide d'installation de SQL Server sur Linux](sql-server-linux-setup.md#unattended). Consultez les exemples de scripts pour [RedHat](sample-unattended-install-redhat.md), [SUSE Linux Enterprise Server](sample-unattended-install-suse.md) et [Ubuntu](sample-unattended-install-ubuntu.md). Vous pouvez également consulter [cet exemple de script](/archive/blogs/sqlcat/unattended-install-and-configuration-for-sql-server-2017-on-linux) créé par l’équipe de consultants clients de SQL Server.

## <a name="tools"></a>Outils

1. **Puis-je utiliser le client SQL Server Management Studio sur Windows pour accéder à SQL Server sur Linux ?**

   Oui, vous pouvez utiliser tous vos outils existants qui s’exécutent sur Windows pour accéder à SQL Server sur Linux. Cela comprend notamment les outils Microsoft tels que SQL Server Management Studio (SSMS), SQL Server Data Tools (SSDT) et OSS, ainsi que les outils tiers.

1. **Existe-t-il un outil tel que SSMS qui s’exécute sur Linux ?**

   Le nouvel outil Azure Data Studio est un outil multiplateforme pour la gestion de SQL Server. Pour plus d’informations, consultez [Qu’est-ce qu’Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

1. **Les commandes telles que sqlcmd et bcp sont-elles disponibles sur Linux ?**

   Oui, [sqlcmd et bcp](sql-server-linux-setup-tools.md) sont disponibles en mode natif sur Linux, macOS et Windows. En outre, utilisez le nouvel outil de ligne de commande [mssql-scripter](https://github.com/Microsoft/mssql-scripter) sur Linux, macOS ou Windows pour générer des scripts T-SQL pour votre base de données SQL en cours d’exécution n’importe où. Consultez également la version préliminaire de [mssql-cli.](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/)

1. **Est-il possible d’afficher le moniteur d’activité lors d’une connexion via SSMS sur Windows pour une instance s’exécutant sur Linux ?**

   Oui, vous pouvez utiliser SSMS sur Windows pour vous connecter à distance et utiliser des outils/fonctionnalités tels que les commandes du moniteur d’activité sur une instance Linux.

1. **Quels outils sont disponibles pour surveiller les performances de SQL Server sur Linux ?**

   Vous pouvez utiliser les [vues de gestion dynamique (DMV) système](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) pour collecter divers types d’informations sur SQL Server, y compris des informations sur les processus Linux. Vous pouvez utiliser le [Magasin des requêtes](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md) pour améliorer les performances des requêtes. D’autres outils, tels que le [Tableau de bord de performances intégré](/archive/blogs/sql_server_team/new-in-ssms-performance-dashboard-built-in), fonctionnent à distance dans SQL Server Management Studio (SSMS) à partir de Windows.

   > [!TIP]
   > Une façon d’améliorer les performances est de configurer correctement votre système d’exploitation Linux et l’instance SQL Server. Pour plus d'informations, consultez [Meilleures pratiques relatives aux performances et lignes directrices de configuration pour SQL Server sur Linux](sql-server-linux-performance-best-practices.md).

## <a name="administration"></a>Administration

1. **Est-ce que Microsoft a créé une application comme le Gestionnaire de configuration SQL Server sur Linux ?**

   Oui, il existe un outil de configuration pour SQL Server sur Linux : [mssql-conf](sql-server-linux-configure-mssql-conf.md).

1. **SQL Server sur Linux prend-il en charge plusieurs instances sur le même hôte ?**

   Nous vous recommandons d’exécuter plusieurs conteneurs sur un hôte pour avoir plusieurs instances distinctes. Pour ce faire, il est facile d’utiliser Docker, mais chaque conteneur doit écouter sur un port différent. Pour plus d’informations, consultez [Exécuter plusieurs conteneurs SQL Server](./sql-server-linux-docker-container-deployment.md#multiple).

1. **L’authentification Active Directory est-elle prise en charge sur Linux ?**

   Oui. Pour plus d'informations, consultez [Authentification Active Directory avec SQL Server sur Linux](sql-server-linux-active-directory-authentication.md).

1. **Always On et le clustering sont-ils pris en charge sur Linux ?**

   Le clustering de basculement et la haute disponibilité sur Linux s’obtiennent avec Pacemaker sur Linux. Pour plus d'informations, consultez [Continuité d’activité et récupération de base de données - SQL Server sur Linux](sql-server-linux-business-continuity-dr.md).

1. **Est-il possible de configurer la réplication de Linux vers Windows et vice versa ?**

   Les réplicas d’échelle lecture peuvent être utilisés entre Windows et Linux pour la réplication unidirectionnelle des données.

1. **Est-il possible de migrer des bases de données existantes dans des versions antérieures de SQL Server de Windows vers Linux ?**

   Oui, il existe [plusieurs méthodes](sql-server-linux-migrate-overview.md) pour y parvenir.

1. **Puis-je migrer mes données à partir d’Oracle et d’autres moteurs de base de données vers SQL Server sur Linux ?**

   Oui. SSMA prend en charge la migration à partir de plusieurs types de moteurs de base de données : Microsoft Access, DB2, MySQL, Oracle et SAP ASE (anciennement SAP Sybase ASE). Pour obtenir un exemple d’utilisation de SSMA, consultez [Migrer un schéma Oracle vers SQL Server sur Linux avec l’assistant de migration SQL Server](../ssma/oracle/sql-server-linux-convert-from-oracle.md?toc=/sql/toc/toc.json).

1. **Quelles sont les autorisations requises pour les fichiers SQL Server ?**

   Tous les fichiers du dossier de fichiers `/var/opt/mssql` doivent être détenus par l’utilisateur **mssql** et appartenir au groupe **mssql**. L’utilisateur **mssql** et le groupe doivent avoir des autorisations en lecture-écriture pour tous les fichiers et répertoires. Notez les scénarios spéciaux suivants qui impliquent des autorisations de fichiers et de répertoires :

   * Les autorisations pour le propriétaire de mssql et le groupe sont requises pour les partages réseau montés utilisés pour stocker des fichiers SQL Server.
   * Si vous recherchez des fichiers de base de données ou des sauvegardes dans un répertoire autre que celui par défaut, vous devez également définir des autorisations pour ce répertoire.
   * Si vous modifiez l’umask racine par défaut à partir de 0022, la configuration de SQL Server échoue après l’installation. Vous devez ensuite appliquer manuellement les autorisations requises pour le compte de démarrage de SQL Server.

1. **Puis-je modifier la propriété des fichiers et répertoires SQL Server à partir du compte mssql et du groupe installés ?**

   Nous ne prenons pas en charge la modification de la propriété du répertoire et des fichiers de SQL Server à partir de l’installation par défaut. Le compte mssql et le groupe sont utilisés spécifiquement pour SQL Server et ne disposent d’aucun accès à une connexion interactive.
   
 1. **Les liens symboliques sont-ils pris en charge pour les répertoires de données et de journaux SQL Server ?** 
    
    Non, les liens symboliques ne sont pas pris en charge pour les répertoires de données et de journaux SQL Server. Pour modifier les répertoires de données et de journaux par défaut, consultez [Modification de l’emplacement des répertoires de données ou de journaux par défaut](sql-server-linux-configure-mssql-conf.md#datadir).
    
[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]