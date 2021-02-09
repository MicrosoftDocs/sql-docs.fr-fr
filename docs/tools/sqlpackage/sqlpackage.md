---
title: SqlPackage.exe
description: Découvrez comment automatiser des tâches de développement de bases de données avec SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 11/4/2020
ms.openlocfilehash: ef49071f97d255d98f8086b9ff329c77d7b4afad
ms.sourcegitcommit: 5ceafd29b8f22edb800cec150f0ccddea43313e7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/28/2021
ms.locfileid: "98983662"
---
# <a name="sqlpackageexe"></a>SqlPackage.exe

**SqlPackage.exe** est un utilitaire en ligne de commande qui automatise les tâches de développement de base de données suivantes :  
  
- [Version](#version) : retourne le numéro de build de l’application SqlPackage.  Ajouté dans la version 18.6.

- [Extraire](sqlpackage-extract.md) : Crée un fichier d’application de la couche Données (.dacpac) contenant le schéma, ou le schéma et les données utilisateur à partir d’une base de données SQL connectée.  
  
- [Publier](sqlpackage-publish.md) : met à jour de manière incrémentielle un schéma de base de données pour qu’il corresponde au schéma d’un fichier .dacpac source. Si la base de données n'existe pas sur le serveur, elle est créée par l'opération de publication. Dans le cas contraire, une base de données existante est mise à jour.  
  
- [Exporter](sqlpackage-export.md) : Exporte une base de données SQL connectée, y compris le schéma de base de données et les données utilisateur, dans un fichier BACPAC (.bacpac).  
  
- [Importer](sqlpackage-import.md) : Importe le schéma et les données des tables à partir d’un fichier BACPAC dans une nouvelle base de données utilisateur.  
  
- [DeployReport](sqlpackage-deploy-drift-report.md) (déployer un rapport) : crée un rapport XML sur les modifications devant être apportées par une action de publication.  
  
- [DriftReport](sqlpackage-deploy-drift-report.md) (dériver un rapport) : crée un rapport XML sur les modifications apportées à une base de données inscrite depuis sa dernière inscription.  
  
- [Script](sqlpackage-script.md) : crée un script de mise à jour incrémentielle Transact-SQL qui met à jour le schéma d'une cible afin qu'il corresponde au schéma d'une source.  
  
La ligne de commande de **SqlPackage.exe** vous permet de spécifier ces actions à l’aide de paramètres et de propriétés spécifiques aux actions.  

**[Téléchargez la version la plus récente](sqlpackage-download.md)** . Pour plus d’informations sur la dernière version, consultez les [notes de publication](release-notes-sqlpackage.md).
  
## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

### <a name="usage-examples"></a>Exemples d'utilisation

**Générer une comparaison de bases de données à l’aide de fichiers .dacpac avec une sortie de script SQL**

Commencez par créer un fichier .dacpac de vos dernières modifications de base de données :

```
sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_current_version.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```
 
Créez un fichier .dacpac de votre cible de base de données (sans modification) :

 ```
 sqlpackage.exe /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /Action:Extract /SourceServerName:"." /SourceDatabaseName:"Contoso.Database"
 ```

Créez un script SQL qui génère les différences entre les deux fichiers .dacpac :

```
sqlpackage.exe /Action:Script /SourceFile:"C:\sqlpackageoutput\output_current_version.dacpac" /TargetFile:"C:\sqlpackageoutput\output_target.dacpac" /TargetDatabaseName:"Contoso.Database" /OutputPath:"C:\sqlpackageoutput\output.sql"
 ```

 ## <a name="version"></a>Version

Affiche la version sqlpackage comme numéro de build.  Peut être utilisé dans les invites interactives et dans les [pipelines automatisés](sqlpackage-pipelines.md).

```
sqlpackage.exe /Version
 ```


## <a name="exit-codes"></a>Codes de sortie

Commandes qui retournent les codes de sortie suivants :

- 0 = réussite
- non-zéro = échec


## <a name="parameters"></a>Paramètres
Certains paramètres sont partagés entre les actions SqlPackage. Voici un tableau résumant les paramètres. pour plus d’informations, cliquez sur les pages d’actions spécifiques.

| Paramètre | Forme abrégée | [Extraction](sqlpackage-extract.md#parameters-for-the-extract-action) | [Publier](sqlpackage-publish.md#parameters-for-the-publish-action) | [Export](sqlpackage-export.md#parameters-for-the-export-action) | [Importer](sqlpackage-import.md#parameters-for-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-parameters) | [DriftReport](sqlpackage-deploy-drift-report.md#driftreport-action-parameters) | [Script](sqlpackage-script.md#parameters-for-the-script-action) |
|---|---|---|---|---|---|---|---|---|
|**/AccessToken:**|**/at**| x | x | x | x | x | x | x |
|**/ClientId:**|**/cid**| | x | | | | | |
|**/DeployScriptPath:**|**/dsp**| | x | | | | | x |
|**/DeployReportPath:**|**/drp**| | x | | | | | x |
|**/Diagnostics:**|**/d**| x | x | x | x | x | x | x |
|**/DiagnosticsFile :**|**/df**| x | x | x | x | x | x | x |
|**/MaxParallelism:**|**/mp**| x | x | x | x | x | x | x |
|**/OutputPath:**|**/op**|  |  |  | | x | x | x |
|**/OverwriteFiles:**|**/of**| x | x | x | | x | x | x |
|**/Profile:**|**/pr**| | x | | | x | | x |
|**/Properties:**|**/p**| x | x | x | x | x | | x |
|**/Quiet:**|**/q**| x | x | x | x | x | x | x |
|**/Secret:**|**/secr**| | x | | | | | |
|**/SourceConnectionString:**|**/scs**| x | x | x | | x | | x | x |
|**/SourceDatabaseName:**|**/sdn**| x | x | x | | x | | x |
|**/SourceEncryptConnection:**|**/sec**| x | x | x | | x | | x |
|**/SourceFile:**|**/sf**| | x | | x | x | | x |
|**/SourcePassword:**|**/sp**| x | x | x | | x | | x |
|**/SourceServerName:**|**/ssn**| x | x | x | | x | | x |
|**/SourceTimeout :**|**/st**| x | x | x | | x | | x |
|**/SourceTrustServerCertificate:**|**/stsc**| x | x | x | | x | | x |
|**/SourceUser:**|**/su**| x | x | x | | x | | x |
|**/TargetConnectionString:**|**/tcs**| | | | x | x | x | x |
|**/TargetDatabaseName:**|**/tdn**| | x | | x | x | x | x |
|**/TargetEncryptConnection:**|**/tec**| | x | | x | x | x | x |
|**/TargetFile :**|**/tf**| x | | x | | x | | x |
|**/TargetPassword:**|**/tp**| | x | | x | x | x | x |
|**/TargetServerName:**|**/tsn**| | x | | x | x | x | x |
|**/TargetTimeout:**|**/tt**| | x | | x | x | x | x |
|**/TargetTrustServerCertificate:**|**/ttsc**| | x | | x | x | x | x |
|**/TargetUser:**|**/tu**| | x | | x | x | x | x |
|**/TenantId:**|**/tid**| x | x | x | x | x | x | x |
|**/UniversalAuthentication:**|**/ua**| x | x | x | x | x | x | x |
|**/Variables:**|**/v**| | | | | x | | x |

## <a name="properties"></a>Propriétés
Certaines propriétés sont partagées entre les actions SqlPackage.  Voici un tableau résumant les propriétés. pour plus d’informations, cliquez sur les pages d’actions spécifiques.

| Propriété | [Extraction](sqlpackage-extract.md#properties-specific-to-the-extract-action) | [Publier](sqlpackage-publish.md#properties-specific-to-the-publish-action) | [Export](sqlpackage-export.md#properties-specific-to-the-export-action) | [Importer](sqlpackage-import.md#properties-specific-to-the-import-action) | [DeployReport](sqlpackage-deploy-drift-report.md#deployreport-action-properties) | [Script](sqlpackage-script.md#properties-specific-to-the-script-action) |
|---|---|---|---|---|---|---|
|AdditionalDeploymentContributorArguments=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributors=(STRING)| | x | | | x | x |
|AdditionalDeploymentContributorPaths=(STRING)| | x | | | x | x |
|AllowDropBlockingAssemblies=(BOOLEAN)| | x | | | x | x |
|AllowIncompatiblePlatform=(BOOLEAN)| | x | | | x | x |
|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)| | x | | | x | x |
|BackupDatabaseBeforeChanges=(BOOLEAN)| | x | | | x | x |
|BlockOnPossibleDataLoss=(BOOLEAN 'True')| | x | | | x | x |
|BlockWhenDriftDetected=(BOOLEAN 'True')| | x | | | x | x |
|CommandTimeout=(INT32 '60')| x | x | x | x | x | x |
|CommentOutSetVarDeclarations=(BOOLEAN)| | x | | | x | x |
|CompareUsingTargetCollation=(BOOLEAN)| | x | | | x | x |
|CreateNewDatabase=(BOOLEAN)| | x | | | x | x |
|DacApplicationDescription=(STRING)| x | | | | | |
|DacApplicationName=(STRING)| x | | | | | |
|DacMajorVersion=(INT32 '1')| x | | | | | |
|DacMinorVersion=(INT32 '0')| x | | | | | |
|DatabaseEdition=(ENUM 'Default')| | x | | x | x | x |
|DatabaseLockTimeout=(INT32 '60')| x | x | x | | x | x |
|DatabaseMaximumSize=(INT32)| | x | | x | x | x |
|DatabaseServiceObjective=(STRING)| | x | | x | x | x |
|DeployDatabaseInSingleUserMode=(BOOLEAN)| | x | | | x | x |
|DisableAndReenableDdlTriggers=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotAlterReplicatedObjects=(BOOLEAN 'True')| | x | | | x | x |
|DoNotDropObjectType=(STRING)| | x | | | x | x |
|DoNotDropObjectTypes=(STRING)| | x | | | x | x |
|DropConstraintsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropDmlTriggersNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropIndexesNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|DropObjectsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropPermissionsNotInSource=(BOOLEAN)| | x | | | x | x |
|DropRoleMembersNotInSource=(BOOLEAN)| | x | | | x | x |
|DropStatisticsNotInSource=(BOOLEAN 'True')| | x | | | x | x |
|ExcludeObjectType=(STRING)| | x | | | x | x |
|ExcludeObjectTypes=(STRING)| | x | | | x | x |
|ExtractAllTableData=(BOOLEAN)| x | | | | | |
|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')| x | | | | | |
|ExtractReferencedServerScopedElements=(BOOLEAN 'True')| x | | | | | |
|ExtractUsageProperties=(BOOLEAN)| x | | | | | |
|GenerateSmartDefaults=(BOOLEAN)| | x | | | x | x |
|IgnoreAnsiNulls=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreAuthorizer=(BOOLEAN)| | x | | | x | x |
|IgnoreColumnCollation=(BOOLEAN)| | | | | x | x |
|IgnoreColumnOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreComments=(BOOLEAN)| | x | | | x | x |
|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreDdlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDdlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreDefaultSchema=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerOrder=(BOOLEAN)| | x | | | x | x |
|IgnoreDmlTriggerState=(BOOLEAN)| | x | | | x | x |
|IgnoreExtendedProperties=(BOOLEAN)| x | x | | | x | x |
|IgnoreFileAndLogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFilegroupPlacement=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFileSize=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFillFactor=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreIdentitySeed=(BOOLEAN)| | x | | | x | x |
|IgnoreIncrement=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreIndexPadding=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreKeywordCasing=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreLockHintsOnIndexes=(BOOLEAN)| | x | | | x | x |
|IgnoreLoginSids=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreNotForReplication=(BOOLEAN)| | x | | | x | x |
|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')| | x | | | x | x |
|IgnorePartitionSchemes=(BOOLEAN)| | x | | | x | x |
|IgnorePermissions=(BOOLEAN 'True')| x | x | | | x | x |
|IgnoreQuotedIdentifiers=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreRoleMembership=(BOOLEAN)| | x | | | x | x |
|IgnoreRouteLifetime=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreTableOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreTablePartitionOptions=(BOOLEAN)| | x | | | x | x |
|IgnoreUserLoginMappings=(BOOLEAN)| x | | | | | |
|IgnoreUserSettingsObjects=(BOOLEAN)| | x | | | x | x |
|IgnoreWhitespace=(BOOLEAN 'True')| | x | | | x | x |
|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)| | x | | | x | |
|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)| | x | | | x | |
|ImportContributorArguments=(STRING)| | | | x | | |
|ImportContributors=(STRING)| | | | x | | |
|ImportContributorPaths=(STRING)| | | | x | | |
|IncludeCompositeObjects=(BOOLEAN)| | x | | | x | x |
|IncludeTransactionalScripts=(BOOLEAN)| | x | | | x | x |
|LongRunningCommandTimeout=(INT32)| x | x | x | x | x | x |
|NoAlterStatementsToChangeClrTypes=(BOOLEAN)| | x | | | x | x |
|PopulateFilesOnFileGroups=(BOOLEAN 'True')| | x | | | x | x |
|RegisterDataTierApplication=(BOOLEAN)| | x | | | x | x |
|RunDeploymentPlanExecutors=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCollation=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseCompatibility=(BOOLEAN)| | x | | | x | x |
|ScriptDatabaseOptions=(BOOLEAN 'True')| | x | | | x | x |
|ScriptDeployStateChecks=(BOOLEAN)| | x | | | x | x |
|ScriptFileSize=(BOOLEAN)| | x | | | x | x |
|ScriptNewConstraintValidation=(BOOLEAN 'True')| | x | | | x | x |
|ScriptRefreshModule=(BOOLEAN 'True')| | x | | | x | x |
|Storage=({File&#124;Memory} 'File')| x | x | x | x | x | x |
|TableData=(STRING)| x | | x | | | |
|TargetEngineVersion=(ENUM 'Latest')| | | x | | | |
|TempDirectoryForTableData=(STRING)| x | | x | | | |
|TreatVerificationErrorsAsWarnings=(BOOLEAN)| | x | | | x | x |
|UnmodifiableObjectWarnings=(BOOLEAN 'True')| | x | | | x | x |
|VerifyCollationCompatibility=(BOOLEAN 'True')| | x | | | x | x |
|VerifyDeployment=(BOOLEAN 'True')| | x | | | x | x |
|VerifyExtraction=(BOOLEAN)| x | | | | | |
|VerifyFullTextDocumentTypesSupported=(BOOLEAN)| | | x | | | |


## <a name="next-steps"></a>Étapes suivantes

- Découvrir plus en détail l’action [Extract de SqlPackage](sqlpackage-extract.md)
- Découvrir plus en détail l’action [Publish de SqlPackage](sqlpackage-publish.md)
- Découvrir plus en détail l’action [Export de SqlPackage](sqlpackage-export.md)
- Découvrir plus en détail l’action [Import de SqlPackage](sqlpackage-import.md)
