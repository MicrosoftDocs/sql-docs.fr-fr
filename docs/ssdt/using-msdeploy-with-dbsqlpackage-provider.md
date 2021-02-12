---
title: Utilisation de MSDeploy avec le fournisseur dbSqlPackage
description: En savoir plus sur le fournisseur MSDeploy obsolète DbSqlPackage. Affichez des paramètres, des exemples et d’autres outils de publication de SQL Server et Azure SQL Database.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 213b91ab-03e9-431a-80f0-17eed8335abe
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 04/26/2017
ms.openlocfilehash: e9ceb447b078f7219d2fadec2dee4274a74190e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066824"
---
# <a name="using-msdeploy-with-dbsqlpackage-provider"></a>Utilisation de MSDeploy avec le fournisseur dbSqlPackage

**DbSqlPackage** est un fournisseur **MSDeploy** qui vous permet d'interagir avec des bases de données SQL Server/Azure SQL Database. **DbSqlPackage** prend en charge les actions suivantes :  
  
-   **Extraire** : crée un fichier de capture instantanée de base de données (.dacpac) à partir d’une base de données SQL Server ou Azure SQL Database active.  
  
-   **Publier** : met à jour de manière incrémentielle un schéma de base de données pour qu’il corresponde au schéma d’un fichier .dacpac source.  
  
-   **DeployReport** (déployer un rapport) : crée un rapport XML sur les modifications devant être apportées par une action de publication.  
  
-   **Script** : crée un script Transact\-SQL équivalent au script exécuté par l’action Publier.  
  
Pour plus d’informations concernant DACFx, consultez la documentation sur l’API gérée par DACFx sous [https://msdn.microsoft.com/library/microsoft.sqlserver.dac.aspx](/dotnet/api/microsoft.sqlserver.dac) ou [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md) (outil en ligne de commande DACFx).  
  
> [!IMPORTANT]  
> La fonctionnalité de fournisseur dbSqlPackage sera supprimée dans la prochaine version majeure de Visual Studio. Pour plus d'informations sur la façon de publier une base de données avec Web Deploy, consultez [Fournisseur dbDacFx pour la publication de base de données incrémentielle](https://www.iis.net/learn/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing).  
  
## <a name="command-line-syntax"></a>Syntaxe de la ligne de commande  
**MSDeploy** avec le fournisseur **dbSqlPackage** utilise une ligne de commande au format suivant :  
  
```  
  
MSDeploy -verb: MSDeploy-verb -source:dbSqlPackage="Input"[,dbSqlPackage-source-parameters] -dest:dpSqlPackage="Input"[,dbSqlPackage-target-parameters]  
```  
  
## <a name="ms-deploy-verbs"></a>Verbes MS-Deploy  
Vous devez spécifier les verbes MS-Deploy à l’aide du commutateur **-verb** à partir de la ligne de commande MS-Deploy. Le fournisseur **dbSqlPackage** prend en charge les verbes **MSDeploy** suivants :  
  
|Verbe|Description|  
|--------|---------------|  
|dump|Fournit les informations (y compris le nom, le numéro de version et la description) relatives à une base de données source contenue dans un fichier .dacpac. Spécifiez la base de données source depuis la ligne de commande en utilisant le format suivant :<br /><br />**msdeploy -verb:dump -source:dbSqlPackage="** _.dacpac-file-path_ **"**|  
|synchronisation|Spécifie les actions dbSqlPackage depuis la ligne de commande en utilisant le format suivant :<br /><br />**msdeploy -verb:sync -source:dbSqlPackage**="input" _[,DbSqlPackage-source-parameters] -_ **dest:dbSqlPackage**="input" *[,DbSqlPackage-destination-parameters]*<br /><br />Pour obtenir les paramètres source et de destination valide pour le verbe sync, consultez les sections ci-dessous.|  
  
## <a name="dbsqlpackage-source"></a>Source dbSqlPackage  
Le fournisseur **dbSqlPackage** accepte une entrée qui correspond à une chaîne de connexion SQL Server/SQL Azure valide ou à un chemin d'accès à un fichier .dacpac présent sur le disque.  La syntaxe de spécification de la source d'entrée pour le fournisseur est la suivante :  
  
|Entrée|Default|Description|  
|---------|-----------|---------------|  
|**-source:dbSqlPackage=** {*input*}|**N/A**|*entrée* correspond à une chaîne de connexion SQL Server ou SQL Azure valide ou à un chemin d'accès à un fichier .dacpac présent sur le disque.<br /><br />**REMARQUE :** les seules propriétés de chaîne de connexion prises en charge lors de l’utilisation d’une chaîne de connexion comme source d'entrée sont *InitialCatalog, DataSource, UserID, Password, IntegratedSecurity, Encrypt, TrustServerCertificate* et *ConnectionTimeout*.|  
  
Si votre source d'entrée correspond à une chaîne de connexion à une base de données SQL Server/Azure SQL Database, **dbSqlPackage** extrait un instantané de base de données au format de fichier .dacpac à partir d'une base de données SQL Server/Azure SQL Database.  
  
Les paramètres **sources** sont :  
  
|Paramètre|Default|Description|  
|-------------|-----------|---------------|  
|**Profile** :{ *string*}|N/A|Spécifie le chemin d'accès à un profil de publication DAC. Le profil définit une collection de propriétés et de variables à utiliser lors de la génération du fichier .dacpac. Le profil de publication est transmis à la destination et utilisé en tant qu'options par défaut lors de l'exécution d'une action **Publier**, **Script** ou **DeployReport**.|  
|**DacApplicationName**={ *string* }|Nom de la base de données|Définit le nom de l'application à stocker dans les métadonnées DACPAC. La chaîne par défaut est le nom de la base de données.|  
|**DacMajorVersion** ={*integer*}|**1**|Définit la version principale à stocker dans les métadonnées DACPAC.|  
|**DacMajorVersion** ={*integer*}|**0**|Définit la version secondaire à stocker dans les métadonnées DACPAC.|  
|**DacApplicationDescription**={ *string* }|N/A|Définit la description de l'application à stocker dans les métadonnées DACPAC.|  
|**ExtractApplicationScopedObjectsOnly={True &#124; False}**|**True**|Si la valeur est **True**, extrait uniquement les objets à portée d'application depuis la source. Si la valeur est **False**, extrait à la fois les objets à portée et hors de portée des applications.|  
|**ExtractReferencedServerScopedElements={True &#124; False}**|**True**|Si la valeur est **True**, extrait les objets de connexion, d’audit de serveur et d’informations d’identification référencés par les objets de base de données source.|  
|**ExtractIgnorePermissions={True &#124; False}**|**False**|Si la valeur est **True**, ignore les autorisations d'extraction des objets extraits. Si la valeur est **False**, aucune extraction n'est effectuée.|  
|**ExtractStorage={File&#124;Memory}**|**File**|Spécifie le type de stockage de sauvegarde pour le modèle de schéma utilisé lors de l'extraction.|  
|**ExtractIgnoreExtendedProperties={True&#124;False}**|**False**|Spécifie si les propriétés étendues doivent être ignorées.|  
|**VerifyExtraction = {True&#124;False}**|**False**|Spécifie si le fichier dacpac extrait doit être vérifié.|  
  
## <a name="dbsqlpackage-destination"></a>Destination dbSqlPackage  
Le fournisseur **dbSqlPackage** accepte une entrée qui correspond à une chaîne de connexion SQL Server/SQL Azure valide ou à un chemin d'accès à un fichier .dacpac présent sur le disque en tant qu'entrée de destination.  La syntaxe de spécification de la destination pour le fournisseur est la suivante :  
  
|Entrée|Default|Description|  
|---------|-----------|---------------|  
|-**dest:dbSqlPackage**={*input*}|N/A|*entrée* correspond à une chaîne de connexion SQL Server ou SQL Azure valide ou à un chemin d'accès complet ou partiel à un fichier .dacpac présent sur le disque. Si *entrée* est un chemin d'accès de fichier, aucun autre paramètre ne peut être spécifié.|  
  
Les paramètres **Destination** suivants sont disponibles pour toutes les opérations **dbSqlPackage** :  
  
|Propriété|Default|Description|  
|------------|-----------|---------------|  
|**Action={Publish&#124;DeployReport&#124;Script}**|N/A|Paramètre facultatif indiquant l'action à effectuer sur la **Destination**.|  
|**AllowDropBlockingAssemblies ={True &#124; False}**|**False**|Spécifie si la publication **SqlClr** doit supprimer les assemblys bloquants dans le cadre d'un plan de déploiement. Par défaut, les assemblys bloquants ou de référence bloquent la mise à jour d'assembly si l'assembly de référence doit être supprimé.|  
|**AllowIncompatiblePlatform={True &#124; False}**|**False**|Spécifie si l'action de publication doit être effectuée malgré la possibilité d'une incompatibilité avec les plateformes SQL Server.|  
|**BackupDatabaseBeforeChanges={True &#124; False}**|**False**|Sauvegarde la base de données avant le déploiement des modifications.|  
|**BlockOnPossibleDataLoss={ True &#124; False}**|**True**|Spécifie si l'épisode de publication doit prendre fin si l'opération de publication est susceptible d'entraîner une perte de données.|  
|**BlockWhenDriftDetected={ True &#124; False}**|**True**|Spécifie s'il faut bloquer la mise à jour d'une base de données dont le schéma ne correspond plus à son inscription ou qui est désinscrite.|  
|**CommentOutSetVarDeclarations= {True &#124; False}**|**False**|Spécifie si les déclarations de variable **SETVAR** doivent être commentées dans le script de publication généré. Cela peut vous être utile si vous prévoyez d'utiliser un outil tel que **SQLCMD.EXE** pour spécifier les valeurs de la ligne de commande au moment de la publication.|  
|**CompareUsingTargetCollation={ True &#124; False}**|**False**|Ce paramètre détermine la façon dont le classement de la base de données est géré durant le déploiement ; par défaut, le classement de la base de données cible sera mis à jour s'il ne correspond pas à celui spécifié par la source.  Lorsque cette option est définie, le classement de la base de données (ou du serveur) cible doit être utilisé.|  
|**CreateNewDatabase={ True &#124; False}**|**False**|Spécifie si la base de données cible doit être mise à jour ou bien supprimée, puis recréée lors de la publication vers une base de données.|  
|**DeployDatabaseInSingleUserMode={ True &#124; False}**|**False**|Si la valeur est **True**, la base de données est définie en mode mono-utilisateur avant le déploiement.|  
|**DisableAndReenableDdlTriggers={True &#124; False}**|**True**|Spécifie si les déclencheurs DDL (Data Definition Language) doivent être désactivés au début du processus de publication, puis réactivés à la fin de ce dernier.|  
|**DoNotAlterChangeDataCaptureObjects={ True &#124; False}**|**True**|Si la valeur est **True**, les objets de capture de données modifiées ne sont pas altérés.|  
|**DoNotAlterReplicatedObjects=( True &#124; False}**|**True**|Spécifie si les objets répliqués sont identifiés lors de la vérification.|  
|**DropConstraintsNotInSource= {True &#124; False}**|**True**|Spécifie si l'action de publication doit supprimer de la base de données cible les contraintes absentes de l'instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**DropDmlTriggersNotInSource= {True &#124; False}**|**True**|Spécifie si l'action de publication doit supprimer de la base de données cible les déclencheurs DML (Data Manipulation Language) absents de l'instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**DropExtendedPropertiesNotInSource= {True &#124; False}**|**True**|Spécifie si l’action de publication doit supprimer de la base de données cible les propriétés étendues absentes de l’instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**DropIndexesNotInSource= {True &#124; False}**|**True**|Spécifie si l'action de publication doit supprimer de la base de données cible les index absents de l'instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**DropObjectsNotInSource= {True &#124; False}**|**False**|Spécifie si les objets qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) sont supprimés de la base de données cible au moment de la publication dans une base de données.|  
|**DropPermissionsNotInSource= {True &#124; False}**|**False**|Spécifie si l'action de publication doit supprimer de la base de données cible les autorisations absentes de l'instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**DropRoleMembersNotInSource= {True &#124; False}**|**False**|Spécifie si l’action de publication doit supprimer de la base de données cible les membres de rôle absents de l’instantané de base de données (.dacpac) au moment de la publication vers une base de données.|  
|**GenerateSmartDefaults={True &#124; False}**|**False**|Spécifie si **SqlPackage.exe** doit fournir automatiquement une valeur par défaut lorsqu'il met à jour une table contenant des données et une colonne n'acceptant pas les valeurs Null.|  
|**IgnoreAnsiNulls= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau du paramètre **ANSI NULLS** doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreAuthorizer= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'agent d'autorisation doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreColumnCollation= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau du classement des colonnes doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreComments= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'ordre des commentaires doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreCryptographicProviderFile= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau du chemin d'accès au fichier d'un fournisseur de services de chiffrement doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreDdlTriggerOrder= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'ordre des déclencheurs DDL (Data Definition Language) doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreDdlTriggerState={True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'état d'activation des déclencheurs DDL (Data Definition Language) doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreDefaultSchema={True &#124; False}**|**False**|Spécifie si les différences situées au niveau du schéma par défaut doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreDmlTriggerOrder= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l’ordre des déclencheurs DML doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreDmlTriggerState= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'état d'activation des déclencheurs DML (Data Definition Language) doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreExtendedProperties= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des propriétés étendues doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreFileAndLogFilePath={True &#124; False}**|**True**|Spécifie si les différences situées au niveau des chemins d'accès aux fichiers et fichiers journaux doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreFilegroupPlacement= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau du positionnement des **FILEGROUP** doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreFileSize= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau de la taille des fichiers doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreFillFactor= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau des facteurs de remplissage doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
  
|Propriété|Default|Description|  
|------------|-----------|---------------|  
|**IgnoreFullTextCatalogFilePath= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau du chemin d'accès aux fichiers d'index de recherche en texte intégral doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreIdentitySeed= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de la valeur initiale d'une colonne d'identité doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreIncrement= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau de l'incrément d'une colonne d'identité doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreIndexOptions ={True &#124; False}**|**False**|Spécifie si les différences situées au niveau des options d'index doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreIndexPadding= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau du remplissage d'index doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreKeywordCasing= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau de la casse des mots clés doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreLockHintsOnIndexes= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des indicateurs de verrou des index doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreLoginSids= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau de l'identificateur de sécurité (SID) doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreNotForReplication= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau du paramètre Pas pour la réplication doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreObjectPlacementOnPartitionScheme= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau du positionnement d’un objet au sein du schéma de partition doivent être ignorées ou mises à jour au moment de la publication dans une base de données.|  
|**IgnorePartitionSchemes= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des schémas et des fonctions de partition doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnorePermissions= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des autorisations doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreQuotedIdentifiers= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des paramètres des identificateurs entre guillemets doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreRoleMembership={True &#124; False}**|**False**|Spécifie si les différences situées au niveau du membre de rôle des informations de connexion doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreRouteLifetime= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau des membres de rôle des informations de connexion doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreSemicolonBetweenState= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau des points-virgules placés entre les instructions Transact-SQL doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreTableOptions= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des options de table doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreUserSettingsObjects= {True &#124; False}**|**False**|Spécifie si les différences situées au niveau des options des paramètres utilisateur doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreWhitespace= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau des espaces blancs doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreWithNocheckOnCheckConstraints={True &#124; False}**|**False**|Spécifie si les différences situées au niveau de la valeur de la clause **WITH NOCHECK** pour les contraintes de validation doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IgnoreWithNocheckOnForeignKeys={True &#124; False}**|**False**|Spécifie si les différences situées au niveau de la valeur de la clause **WITH NOCHECK** pour les clés étrangères doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**IncludeCompositeObjects= {True &#124; False}**|**False**|Spécifie s'il faut inclure tous les éléments composites dans le cadre d'une opération de publication unique.|  
|**IncludeTransactionalScripts={True &#124; False}**|**False**|Spécifie si les instructions transactionnelles doivent être utilisées si possible au moment de la publication vers une base de données.|  
|**NoAlterStatementsToChangeClrTypes={True &#124; False}**|**False**|Spécifie que la publication doit toujours supprimer, puis recréer un assembly en cas de différence, au lieu d'insérer une instruction ALTER ASSEMBLY.|  
|**PopulateFilesOnFilegroups= {True &#124; False}**|**True**|Spécifie si un nouveau fichier doit également être créé au moment de la création d'un nouveau **FileGroup** dans la base de données cible.|  
|**RegisterDataTierApplication={True &#124; False}**|**False**|Spécifie si le schéma est inscrit avec le serveur de la base de données.|  
|**ScriptDatabaseCollation {True &#124; False}**|**False**|Spécifie si les différences situées au niveau du classement de la base de données doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**ScriptDatabaseCompatibility= {True &#124; False}**|**True**|Spécifie si les différences situées au niveau de la compatibilité de la base de données doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|  
|**ScriptDatabaseOptions= {True &#124; False}**|**True**|Spécifie si les propriétés de la base de données cible doivent être définies ou mises à jour au moment de la publication vers une base de données.|  
|**ScriptFileSize={True &#124; False}**|**False**|Contrôle si la taille est spécifiée lors de l'ajout d'un fichier à un groupe de fichiers.|  
|**ScriptNewConstraintValidation= {True &#124; False}**|**True**|Spécifie si toutes les contraintes doivent être validées dans leur ensemble à la fin du processus de publication, évitant ainsi les erreurs relatives aux données provoquées par une contrainte de validation ou de clé étrangère rencontrée durant l'action de publication. Si cette option a la valeur **False**, les contraintes seront publiées sans que les données correspondantes ne soient validées.|  
|**ScriptDeployStateChecks={True &#124; False}**|**False**|Spécifie si des instructions doivent être générées dans le script de publication dans le but de vérifier que le nom du serveur et de la base de données correspondent à ceux spécifiés dans le projet de base de données.|  
|**ScriptRefreshModule= {True &#124; False}**|**True**|Spécifie s'il faut inclure les instructions d'actualisation à la fin du script de publication.|  
|**Storage={File&#124;Memory}**|**Mémoire**|Spécifie comment les éléments sont stockés lors de l'élaboration du modèle de base de données. Pour des raisons de performance, la valeur par défaut est **Memory**. Pour des bases de données très volumineuses, le stockage Fichier sauvegardé est requis.|  
|**TreatVerificationErrorsAsWarnings= {True &#124; False}**|**False**|Spécifie si les erreurs rencontrées lors de la validation de la publication doivent être considérées comme des avertissements. Cette vérification est effectuée conformément au plan de déploiement généré avant l'exécution de ce dernier dans la base de données cible. La vérification du plan permet de détecter les problèmes, comme la perte d'objets cibles (tels que les index), qui doivent être supprimés pour que la modification soit effectuée. La vérification permet également de détecter les situations dans lesquelles les dépendances (tables ou vues par exemple) existent en raison d'une référence à un projet composite, mais n'existent pas dans la base de données cible. Vous pouvez choisir de considérer les erreurs de vérification comme des avertissements afin d'obtenir une liste complète de tous les problèmes, au lieu de permettre à l'action de publication d'être interrompue à la première erreur.|  
|**UnmodifiableObjectWarnings= {True &#124; False}**|**True**|Spécifie si des avertissements doivent être générés lorsque des différences sont trouvées au niveau d'objets ne pouvant pas être modifiés (par exemple, au niveau de la taille du fichier ou de son chemin d'accès ).|  
|**VerifyCollationCompatibility={True &#124; False}**|**True**|Spécifie si la compatibilité du classement est vérifiée.|  
|**VerifyDeployment={True &#124; False}**|**True**|Spécifie si des vérifications doivent être effectuées avant la publication dans le but de rechercher les problèmes susceptibles d'empêcher une publication correcte. Par exemple, l'action de publication peut être interrompue si des erreurs sont rencontrées au cours de la publication, en raison de la présence de clés étrangères dans la base de données cible qui n'existent pas dans le projet de base de données.|  
  
> [!NOTE]  
> Les paramètres de destination spécifiés remplacent ceux qui sont indiqués dans le profil publication source.  
  
> [!NOTE]  
> Les variables et valeurs **SQLCMD** doivent être spécifiées dans le paramètre source du profil de publication, car elles ne peuvent pas être spécifiées en tant que paramètre de destination.  
  
Les paramètres **Destination** suivants sont disponibles uniquement pour les opérations **DeployReport** et **Script** :  
  
|Paramètre|Default|Description|  
|-------------|-----------|---------------|  
|**OutputPath**={ *string* }|N/A|Paramètre facultatif qui indique à **dbSqlPackage** de créer le fichier de sortie XML DeployReport ou Script SQL à l'emplacement de disque spécifié par *chaîne*. Cette action remplace tous les scripts présents à l'emplacement fourni par chaîne.|  
  
> [!NOTE]  
> Si le paramètre **OutputPath** n'est pas spécifié pour une action **DeployReport** ou **Script**, un message est retourné.  
  
## <a name="examples"></a>Exemples  
Ce qui suit est un exemple de syntaxe pour une opération **Extraire** utilisant **dbSqlPackage** :  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source connection string>",<source parameter> -dest:dbSqlPackage="<target dacpac file path>"  
```  
  
Ce qui suit est un exemple de syntaxe pour une opération **Publier** utilisant **dbSqlPackage** :  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Publish,<destination parameters>  
```  
  
Ce qui suit est un exemple de syntaxe pour une opération **DeployReport** utilisant **dbSqlPackage** :  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=DeployReport,OutputPath="<path to output XML file>",<destination parameters>  
```  
  
Ce qui suit est un exemple de syntaxe pour une opération **Script** utilisant **dbSqlPackage** :  
  
```  
MSDeploy.exe -verb:sync -source:dbSqlPackage="<source dacpac file path>" -dest:dbSqlPackage="<target SQL Server/SQL Azure connection string>",Action=Script,OutputPath="<path to output sql script>",<destination parameters>  
```  
