---
title: Paramètres du projet de base de données
description: En savoir plus sur les paramètres du projet de base de données. Découvrez comment utiliser des paramètres pour contrôler les aspects de votre base de données, le débogage et les configurations de build.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.DebugProperties
- sql.data.tools.dacsettings.dialog
- sql.data.tools.dbsqlclrlanguagevb
- SQL.DATA.TOOLS.DBADVANCEDBUILDSETTINGSCS
- sql.data.tools.dbadvancedbuildsettingscs
- ql.data.tools.criticalerror.dialog
- sql.data.tools.dbassemblysigningchangekeypassword
- sql.data.tools.advanceddeploymentsystemdefined.dialog
- sql.data.tools.advanceddatabasesettings.dialog
- sql.data.tools.extendedpropertiesvalueeditor.dialog
- sql.data.tools.dbsqlclrlanguagecs
- ql.data.tools.dbassemblysigningpasswordneeded
- sql.data.tools.vbadvancedsettings.dialog
- sql.data.tools.dbsqlclr
- sql.data.tools.SqlCmdVariablesProperties
- sql.data.tools.dbassemblysigning
- sql.data.tools.advanceddeploymentsettings.dialog
- sql.data.tools.advancedpublishsettings.dialog
- sql.data.tools.BuildActionProperties
- sql.data.tools.serverselectionpolicy.dialog
- sql.data.tools.dbadvancedbuildsettingsvb
- sql.data.tools.dbprojectwizard.buildeventcommandline
- sql.data.tools.dbreferencepaths
- sql.data.tools.GeneralProperties
- sql.data.tools.BuildProperties
- sql.data.tools.DeployProperties
- sql.data.tools.csadvancedsettings.dialog
- sql.data.tools.dbassemblyinfo
- sql.data.tools.extendedpropertieseditor.dialog
ms.assetid: 34418730-1aaa-4948-aee2-8f1e62cda85c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 99552883fb06c01a862ddc973ff3747d11a89ee8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079550"
---
# <a name="database-project-settings"></a>Paramètres du projet de base de données

Vous utilisez des paramètres de projet de base de données pour contrôler des aspects de vos configurations de build, de débogage et de bases de données. Ces paramètres peuvent être répartis dans les catégories suivantes :  
  
-   [Paramètres du projet](#bkmk_proj_settings)  
  
-   [Vérification Transact-SQL étendue](#bkmk_evf)  
  
-   [SQLCLR](#bkmk_sqlclr)  
  
-   [SQLCLR et Build SQLCLR](#bkmk_sqlclr_sqlclrbuild)  
  
-   [Créer](#bkmk_build)  
  
-   [Variables SQLCMD](#bkmk_sqlcmd_variables)  
  
-   [Événements de build](#bkmk_build_events)  
  
-   [Déboguer](#bkmk_debug)  
  
-   [Chemins d'accès des références](#bkmk_ref_paths)  
  
-   [Analyse du code](#bkmk_code_analysis)  
  
### <a name="to-configure-properties-for-your-database-project"></a>Pour configurer les propriétés de votre projet de base de données  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de base de données pour lequel vous souhaitez configurer les propriétés, puis sélectionnez **Propriétés**.  
  
    Vous pouvez aussi double-cliquer sur le nœud **Propriétés** du projet dans l' **Explorateur de solutions**.  
  
2.  La feuille de propriétés de votre projet de base de données s'affiche.  
  
3.  Cliquez sur l'onglet **Paramètres du projet** . Vous pouvez à présent configurer les propriétés générales de votre projet de base de données. Notez la disponibilité des différents onglets (représentant différentes catégories) dans le volet gauche.  
  
## <a name="project-settings"></a><a name="bkmk_proj_settings"></a>Paramètres du projet  
Les paramètres du tableau suivant s'appliquent à toutes les configurations de ce projet de base de données.  
  
|Champ|Valeur par défaut|Description|  
|---------|-----------------|---------------|  
|Plateforme cible|Microsoft SQL Server 2012|Spécifie la version de SQL Server que vous ciblez avec ce projet de base de données.|  
|Activer la vérification Transact\-SQL étendue pour les objets communs.|Cette option n'est pas activée lorsque vous créez un projet.<br /><br />Cette option est activée lorsque vous créez un projet à partir de l’Explorateur d’objets SQL Server connecté à SQL Azure, importez une instance Azure SQL Database dans le projet ou modifiez la plateforme cible d’un projet afin qu’elle corresponde à SQL Azure.|Lorsque cette option est activée, les erreurs détectées dans le projet, pour lequel la vérification SQL Server Compiler a échoué, sont signalées. Si vous modifiez votre plateforme cible pour qu'elle corresponde à SQL Azure, la vérification étendue est activée. L'option n'est pas désactivée si vous modifiez la plateforme cible.<br /><br />Vous pouvez activer cette option pour d'autres versions de SQL Server, mais la validation est limitée aux bases de données partiellement autonomes Microsoft SQL Server 2012 et à SQL Azure. Les versions de SQL Server ne prennent pas toutes en charge la syntaxe Transact\-SQL.<br /><br />Pour plus d’informations, consultez [Vérification Transact-SQL étendue](#bkmk_evf) plus loin dans cette rubrique|  
|Types de sortie|||  
|Application de la couche Données (fichier .dacpac)|Activé et verrouillé. La sortie de la génération d'un projet de base de données génère toujours un package .dacpac lorsque le projet est généré.|Si vous utilisez la version de SQL Server Data Tools (SSDT) qui comporte l'option « Créer un fichier .dacpac de bas niveau supplémentaire (v2.0) », vérifiez si vous souhaitez que le package soit compatible avec SQL Server Management Studio ou avec le portail de gestion SQL Azure. Vous pouvez déployer un package .dacpac directement à partir de SSDT, mais vous pouvez déployer uniquement un fichier .dacpac version 2.0 via SQL Server Management Studio lors de la publication de SQL Server Data Tools.|  
|Créer un script (fichier .sql)||Spécifie si un script SQL CREATE complet est généré pour tous les objets du projet et placé dans le dossier bin/debug lorsque le projet est généré. Vous pouvez créer un script de mise à jour incrémentielle à l'aide de la commande **Publier le projet** ou de l'utilitaire SQL Compare.|  
|Générique|||  
|Schéma par défaut|dbo|Spécifie le schéma par défaut dans lequel les objets SQLCLR et Transact\-SQL sont créés. Vous pouvez ignorer ce paramètre en spécifiant le schéma directement sur les objets.|  
|Inclure le nom du schéma dans le nom de fichier|non|Spécifie si les noms de fichiers incluent le schéma comme un préfixe (par exemple, dbo.Products.table.sql). Si cette case à cocher est désactivée, les noms de fichiers pour les objets prennent la forme ObjectName.ObjectType.sql (par exemple, Products.table.sql).|  
|Valider la casse des identificateurs|Oui|Spécifie si la casse des identificateurs dans les objets SQL du projet est validée lors de la génération du projet. Cette option s'applique aux projets de base de données qui spécifient un classement respectant la casse pour la base de données.|  
|Paramètres de base de données|Paramètres par défaut basés sur les paramètres de configuration standard d'une base de données.|Le mode de classement et les paramètres au niveau de la base de données pour une base de données SQL Server sont des exemples de paramètres que vous pouvez spécifier.|  
  
## <a name="extended-transact-sql-verification"></a><a name="bkmk_evf"></a>Vérification Transact-SQL étendue  
  
> [!IMPORTANT]  
> La fonctionnalité Vérification Transact-SQL étendue sera supprimée de la prochaine version de fonctionnalité de SQL Server Data Tools et de la prochaine version majeure de Visual Studio.  
  
La vérification Transact-SQL étendue est une fonctionnalité du système de projet de base de données qui permet aux développeurs de soumettre leur projet de base de données à Transact-SQL Compiler Service, au moment de la génération, afin de valider le code par rapport à l’interpréteur et à l’analyseur du moteur SQL Server.  
  
### <a name="transact-sql-compiler-service"></a>Transact-SQL Compiler Service  
Transact-SQL Compiler Service est un composant basé sur le moteur de base de données Microsoft SQL Server 2012. Ce service permet de valider la syntaxe et la sémantique des instructions DDL avec la même fidélité qu'un moteur de base de données Microsoft SQL Server 2012. Cela signifie, de manière intrinsèque, que le Compiler Service ne prend pas en charge la syntaxe ou les fonctionnalités qui ont été déconseillées dans Microsoft SQL Server 2012. Pour plus d’informations sur les fonctionnalités dépréciées, consultez [Fonctionnalités du moteur de base de données supprimées dans SQL Server 2012](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).  
  
Pour les besoins de la validation du projet de base de données, le Compiler Service crée une base de données partiellement autonome et simule l'exécution des instructions DDL sur cette base de données. Pour plus d'informations, consultez [Bases de données partiellement autonomes](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
Le Compiler Service distingue deux catégories de limitations.  
  
Fonctionnalités qui reposent sur une configuration de base de données ou d'instance, notamment :  
  
-   Références d'objet en 3 ou 4 parties  
  
-   FileTables  
  
-   Suivi des modifications  
  
-   Fonctions d'ensemble de lignes - OPENROWSET, OPENQUERY, OPENDATASOURCE  
  
-   Recherche sémantique en texte intégral  
  
Fonctionnalités qui ne sont pas prises en charge actuellement pour la validation, notamment :  
  
-   Service Broker  
  
-   Schémas partitionnés avec groupes de fichiers définis par l'utilisateur  
  
-   Classement de métadonnées SQL Azure (Compiler Service utilise le classement de métadonnées de base de données partiellement autonome SQL Server 2012 - Latin1_General_100_CI_AS_KS_WS_SC)  
  
### <a name="enablingdisabling-extended-verification"></a>Activation/Désactivation de la vérification étendue  
La vérification Transact-SQL étendue est activée par défaut dans un projet de base de données créé directement à partir d'une instance Azure SQL Database ou d'un projet dont la plateforme cible est définie sur SQL Azure. Il est recommandé d'utiliser la vérification étendue lors du développement pour une base de données SQL Azure ou à portée d'application ciblant SQL Server 2012. Pour plus d'informations sur les bases de données à portée d'application, consultez [Bases de données partiellement autonomes](/previous-versions/sql/sql-server-2012/ff929071(v=sql.110)).  
  
La fonctionnalité de vérification étendue peut également être utilisée lors du développement d'une base de données à portée d'application pour SQL Server 2008/R2 afin de parvenir à la compatibilité avec Microsoft SQL Server 2012 et SQL Azure.  
  
##### <a name="to-enable-or-disable-extended-verification-at-the-project-level"></a>Pour activer ou désactiver la vérification étendue au niveau du projet  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le fichier de projet, puis sélectionnez **Propriétés**.  
  
2.  Dans **Paramètres du projet**, sous **Plateforme cible**, activez ou désactivez la case **Activer la vérification Transact-SQL étendue pour les objets communs**.  
  
##### <a name="to-disable-extended-verification-at-the-file-level"></a>Pour désactiver la vérification étendue au niveau du fichier  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur un fichier .sql.  
  
    > [!NOTE]  
    > Pour désactiver la fonctionnalité de vérification Transact\-SQL étendue au niveau du fichier, vous devez définir la propriété **Action de génération** du fichier sur **Générer**.  
  
2.  Dans **Propriétés**, affectez à la propriété **Vérification Transact-SQL étendue** la valeur **False**.  
  
    ![Propriétés du fichier](../ssdt/media/ssdt-evf.gif "Propriétés du fichier")  
  
### <a name="special-considerations-for-collations"></a>Considérations spéciales relatives aux classements  
Pour plus d'informations concernant les classements dans les bases de données partiellement autonomes, consultez [Classements de base de données autonome](/previous-versions/sql/sql-server-2012/ff929080(v=sql.110)).  
  
## <a name="sqlclr"></a><a name="bkmk_sqlclr"></a>SQLCLR  
Pour plus d'informations sur les options d'assembly, consultez [Boîte de dialogue Informations de l'assembly](/visualstudio/ide/reference/assembly-information-dialog-box?queryresult=true).  
  
Pour plus d'informations sur la signature, consultez la section **Signature de l'assembly** de la rubrique [Page Signature, Concepteur de projets](/visualstudio/ide/reference/signing-page-project-designer?queryresult=true) .  
  
## <a name="sqlclr-and-sqlclr-build"></a><a name="bkmk_sqlclr_sqlclrbuild"></a>SQLCLR et Build SQLCLR  
Les pages de propriétés **SQLCLR** et **Build SQLCLR** contiennent de nombreux paramètres d'utilisation des objets CLR SQL dans votre projet. Plus particulièrement, la page de propriétés **SQLCLR** dispose d'un paramètre de niveau d'autorisation pour définir les autorisations sur l'assembly SQLCLR. Elle comprend aussi le paramètre Générer le DDL pour contrôler si le DDL des objets SQLCLR ajoutés au projet doit être généré. La page de propriétés **Build** SQLCLR contient toutes les options du compilateur que vous pouvez définir pour configurer la compilation du code SQLCLR dans le projet.  
  
La page de propriétés **Build SQLCLR** contient des paramètres de build avancés pour la génération de vos objets CLR SQL. Différentes options sont fournies en fonction du langage (VB ou C#)) utilisé pour coder les objets SQL CLR.  
  
1.  Si l'objet est écrit en C#, accédez aux options en cliquant sur le bouton **Avancé** dans la page de propriétés **Build SQLCLR**. La description des options C# est disponible dans l'article [Boîte de dialogue Paramètres de build avancés (C#)](/visualstudio/ide/reference/advanced-build-settings-dialog-box-csharp).  
  
2.  Si l'objet est écrit en VB, sélectionnez d'abord VB dans la liste déroulante **Langage** , puis cliquez sur le bouton **Avancé** . La description des options VB est disponible dans l'article [Boîte de dialogue Paramètres avancés du compilateur (Visual Basic)](/visualstudio/ide/reference/advanced-compiler-settings-dialog-box-visual-basic)  
  

## <a name="build"></a><a name="bkmk_build"></a>Build  
Vous pouvez choisir une configuration de build pour chaque projet de base de données dans votre solution. Par défaut il n'existe qu'une seule configuration, mais vous pouvez ajouter des configurations personnalisées. Vous pouvez décider de procéder ainsi, par exemple, si vous voulez une configuration personnalisée dans laquelle vous supprimez toujours la base de données, puis la recréez. Dans les solutions qui contiennent différents types de projets, vous pouvez créer une configuration de solution personnalisée contenant une configuration de build spécifique pour chaque projet.  
  
#### <a name="to-specify-a-build-configuration-for-a-solution"></a>Pour spécifier une configuration de build pour une solution  
  
1.  Dans l' **Explorateur de solutions**, cliquez sur le nœud de solution pour lequel vous voulez spécifier une configuration de build.  
  
2.  Dans le menu **Générer**, cliquez sur **Gestionnaire de configuration**. La boîte de dialogue **Gestionnaire de configurations** s’affiche.  
  
    Indiquez les paramètres de configuration à utiliser pour chaque projet de votre solution.  
  
#### <a name="to-specify-a-build-configuration-for-a-database-project"></a>Pour spécifier une configuration de build pour un projet de base de données  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de base de données pour lequel vous souhaitez spécifier une configuration de build, puis sélectionnez **Propriétés**.  
  
2.  Sous l'onglet **Build** , utilisez la liste déroulante **Configuration** pour spécifier les paramètres de configuration à utiliser pour ce projet.  
  
Les paramètres du tableau suivant s'appliquent aux configurations de build de ce projet de base de données.  
  
|Champ|Valeur par défaut|Description|  
|---------|-----------------|---------------|  
|Chemin de sortie de la génération|bin\Debug\|Spécifie où la sortie de la génération est générée lorsque vous générez ou déployez le projet de base de données. Si vous spécifiez un chemin d'accès relatif, vous devez le faire par rapport au chemin d'accès de projet de base de données. Si le chemin d'accès n'existe pas, il est créé.|  
|Nom du fichier de sortie de la génération|*DatabaseProjectName*|Spécifie le nom que vous souhaitez donner à la sortie qui est créée lorsque vous générez le projet de base de données.|  
|Traiter les avertissements Transact\-SQL comme des erreurs|Non|Spécifie si un avertissement Transact\-SQL doit provoquer l'annulation du processus de génération et de déploiement. Si cette case est décochée, des avertissements apparaissent mais le processus de génération et de déploiement continue. Ce paramètre est spécifique au projet, et non à l'utilisateur, et il est stocké dans le fichier .sqlproj.|  
|Supprimer les avertissements Transact\-SQL|Vide|Spécifie une liste de numéros d'avertissement, délimitée par des virgules ou des points-virgules, qui identifient des avertissements supprimés.<br /><br />Les avertissements supprimés n'apparaissent pas dans la fenêtre **Liste d'erreurs** et n'affectent pas le succès de la génération, même si vous activez la case à cocher **Traiter les avertissements Transact\-SQL comme des erreurs**.|  
  
## <a name="sqlcmd-variables"></a><a name="bkmk_sqlcmd_variables"></a>Variables SQLCMD  
Dans les projets de base de données SQL Server, utilisez les variables SQLCMD pour fournir une substitution dynamique à utiliser en vue du débogage ou de la publication. Entrez le nom de la variable et les valeurs, et pendant la génération les valeurs seront remplacées. S'il n'y a aucune valeur locale, la valeur par défaut est utilisée. En entrant ces variables dans les propriétés du projet, elles seront automatiquement proposées dans la publication et sont stockées dans les profils de publication. Vous pouvez extraire les valeurs des variables du projet dans la publication à l'aide du bouton Charger des valeurs.  
  
Assurez-vous que les variables appropriées sont entrées dans les propriétés du projet, car ces variables ne sont pas validées par rapport à un script dans le projet et ne sont pas utilisées dans un script renseigné automatiquement.  
  
En outre, la publication des lignes de commande vous permet de remplacer ces valeurs de la ligne de commande ou d'utiliser un profil.  
  
## <a name="build-events"></a><a name="bkmk_build_events"></a>Événements de build  
Vous pouvez utiliser ces paramètres pour spécifier une ligne de commande à exécuter avant le démarrage de l'opération de génération et une ligne de commande à exécuter au terme de l'opération de génération.  
  
|Champ|Valeur par défaut|Description|  
|---------|-----------------|---------------|  
|Ligne de commande de l'événement avant génération|None|Spécifie la ligne de commande à exécuter avant la génération du projet. Cliquez sur **Modifier pré-build** pour modifier la ligne de commande.|  
|Ligne de commande d'événement après génération|None|Spécifie la ligne de commande à exécuter après la génération du projet. Cliquez sur **Modifier post-build** pour modifier la ligne de commande.|  
|Exécuter l'événement post-build|En cas de build réussie|Spécifie si la ligne de commande post-build doit être exécutée toujours, uniquement en cas de génération réussie ou uniquement lorsque la génération a mis à jour la sortie de projet (le script de compilation).|  
  
## <a name="debug"></a><a name="bkmk_debug"></a>Déboguer  
Vous pouvez utiliser ces paramètres pour contrôler le débogage de votre projet de base de données.  
  
|Champ|Valeur par défaut|Description|  
|---------|-----------------|---------------|  
|Action de démarrage|None|Spécifie un script ou un programme externe à exécuter lorsque vous déboguez votre projet.|  
|Chaîne de connexion cible|Data Source=(localdb)\\*SolutionName*;Initial Catalog=*DatabaseProjectName*;Integrated Security=True;Pooling=False;Connect Timeout=30|Spécifie les informations de connexion du serveur de base de données que vous souhaitez cibler pour la configuration de build spécifiée. La chaîne de connexion par défaut est spécifiée par rapport à une instance LocalDB SQL Server et une base de données.|  
|Déployer les propriétés de la base de données|Oui|Spécifie si les paramètres DatabaseProperties.DatabaseProperties sont déployés ou mis à jour quand vous déployez le projet de base de données.|  
|Toujours recréer la base de données|Non|Spécifie si la base de données doit être supprimée et recréée au lieu d'effectuer une mise à niveau incrémentielle. Vous pouvez cocher cette case par exemple si vous souhaitez exécuter des tests unitaires de bases de données par rapport à un déploiement propre de la base de données. Si cette case à cocher est désactivée, la base de données existante sera mise à jour au lieu d'être supprimée et recréée.|  
|Bloquer le déploiement incrémentiel si une perte de données peut se produire|Oui|Spécifie si le déploiement s'arrête si une mise à jour entraîne la perte de données. Si cette case à cocher est activée, les modifications susceptibles de créer une perte de données entraînent l'arrêt du déploiement avec une erreur, ce qui empêche toute perte des données. Par exemple, le déploiement s'arrête si une colonne `varchar(50)` a été remplacée par une colonne `varchar(30)`.<br /><br />**REMARQUE :** le déploiement est bloqué uniquement si les tables dans lesquelles une perte de données peut se produire contiennent des données. Le déploiement se poursuit si aucune donnée n'a été perdue.|  
|Objets DROP dans la cible mais pas dans le projet|Non|Spécifie si les objets qui sont dans la base de données cible, mais pas dans le projet de base de données, doivent être supprimés dans le cadre du script de déploiement. Vous pouvez exclure des fichiers de votre projet pour les supprimer temporairement de votre script de compilation. Toutefois, il est possible que vous souhaitiez laisser les versions existantes de ces objets dans la base de données cible. Cette case à cocher n'a aucun effet si la case **Toujours recréer la base de données** est cochée, car la base de données sera supprimée.|  
|Ne pas utiliser d'instructions ALTER ASSEMBLY pour mettre à jour les types CLR|Non|Spécifie si les instructions ALTER ASSEMBLY permettent de mettre à jour des types CLR (Common Language Run-time) ou si l'objet qui instancie le type CLR doit plutôt être supprimé et recréé lors du déploiement de modifications.|  
|Avancé...|Non|Bouton de commande qui vous permet de spécifier les options qui contrôlent les événements et le comportement du déploiement.|  
  
## <a name="reference-paths"></a><a name="bkmk_ref_paths"></a>Chemins d'accès des références  
Vous pouvez utiliser cette page pour définir les variables de serveur et de base de données associées à une référence entre bases de données. En outre, vous pouvez spécifier les valeurs de ces variables. Pour plus d'informations, consultez [Utilisation de références dans les projets de base de données](/previous-versions/visualstudio/visual-studio-2010/bb386242(v=vs.100)).  
  
## <a name="code-analysis"></a><a name="bkmk_code_analysis"></a>Analyse du code  
Vous pouvez utiliser l'analyse du code pour découvrir d'éventuels problèmes dans vos scripts, tels que les problèmes de conception, d'attribution de nom et de performances. Les règles pour les projets de base de données sont organisées en ensembles de règles prédéfinis qui ciblent des zones spécifiques, et vous pouvez activer ou désactiver une règle dans l'onglet **Analyse du code** de la page de propriétés **Propriétés du projet** . Dans le même onglet, vous pouvez spécifier que l'analyse du code soit exécutée automatiquement chaque fois qu'un projet est généré, ou si les avertissements doivent être traités comme des erreurs.  
  
Pour utiliser l’analyse du code manuellement, cliquez avec le bouton droit sur votre projet dans **l’Explorateur de solutions** et sélectionnez **Exécuter l’analyse du code**. Les avertissements d'analyse du code s'affichent dans la fenêtre **Liste d'erreurs** . Vous pouvez double-cliquer sur un avertissement pour accéder au code source comportant le problème. Pour afficher des informations supplémentaires et les corrections possibles d’un avertissement, utilisez le menu contextuel **Afficher de l’aide sur l’erreur**. Pour plus d’informations sur l’analyse du code, consultez [Analyse du code de la base de données pour améliorer la qualité du code](/previous-versions/visualstudio/visual-studio-2010/dd172133(v=vs.100)).  
