---
title: SqlPackage - Publier
description: Découvrez comment automatiser les tâches de développement de bases de données avec l’opération Publish de SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 84fdd99b00de38b88b1a21963c4565e5c3b27ea7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081450"
---
# <a name="sqlpackage-publish-parameters-properties-and-sqlcmd-variables"></a>Paramètres, propriétés et variables SQLCMD de l’action Publish de SqlPackage
L’opération Publish de SqlPackage.exe procède à une mise à jour incrémentielle du schéma d’une base de données cible afin qu’il corresponde à la structure d’une base de données source. La publication d’un package de déploiement qui contient des données utilisateur pour toutes les tables ou pour un sous-ensemble de tables met à jour les données des tables en plus du schéma. Le déploiement de données remplace le schéma et les données des tables existantes de la base de données cible. Le déploiement de données ne modifie pas le schéma existant ni les données de la base de données cible pour les tables qui ne sont pas comprises dans le package de déploiement.  

## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Quand une base de données assortie d’informations d’identification utilisateur de l’authentification SQL est extraite, le mot de passe est remplacé par un mot de passe différent d’une complexité appropriée. Après la publication du package d’application de la couche Données (dacpac), le mot de passe utilisateur est considéré comme modifié.


## <a name="parameters-for-the-publish-action"></a>Paramètres de l’action Publish

|Paramètre|Forme abrégée|Valeur|Description|
|---|---|---|---|
|**/Action:**|**/a**|Publish|Indique l'action à effectuer. |
|**/AccessToken:**|**/at**|{string}| Dans le cadre de l'authentification par jeton, spécifie le jeton d'accès à utiliser pour se connecter à la base de données cible. |
|**/AzureKeyVaultAuthMethod:**|**/akv**|{Interactive&#124;ClientIdSecret}|Spécifie la méthode d’authentification utilisée pour accéder à Azure KeyVault si une opération de publication comprend les modifications apportées à une table/colonne chiffrée. |
|**/ClientId:**|**/cid**|{string}|Spécifie l'ID du client à utiliser dans l'authentification auprès d'Azure Key Vault quand c'est nécessaire. |
|**/DeployScriptPath:**|**/dsp**|{string}|Spécifie un chemin d’accès de fichier facultatif pour la sortie du script de déploiement. Dans les déploiements Azure, s’il existe des commandes TSQL permettant de créer et de modifier la base de données MASTER, un script est écrit dans le même chemin d’accès, mais avec le nom de fichier de sortie « NomFichier_Master.sql ». |
|**/DeployReportPath:**|**/drp**|{string}|Spécifie un chemin d’accès de fichier facultatif pour la sortie du fichier XML du rapport de déploiement. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Spécifie si la journalisation des diagnostics est affichée dans la console. Valeur par défaut False. |
|**/DiagnosticsFile :**|**/df**|{string}|Spécifie un fichier où stocker les journaux de diagnostic. |
|**/MaxParallelism:**|**/mp**|{int}| Spécifie le degré de parallélisme d'opérations simultanées sur une base de données. La valeur par défaut est 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Indique si sqlpackage.exe doit remplacer les fichiers existants. Si vous choisissez False, sqlpackage.exe abandonne l'action si un fichier existant est rencontré. La valeur par défaut est True. |
|**/Profile:**|**/pr**|{string}|Spécifie le chemin d'accès à un profil de publication DAC. Le profil définit une collection de propriétés et de variables à utiliser lors de la génération de sorties.|
|**/Properties:**|**/p**|{PropertyName}={Value}|Spécifie une paire nom-valeur pour une propriété spécifique à l’action ; {PropertyName}={Value}. Reportez-vous à l'aide d'une action spécifique pour afficher le nom des propriétés relatives à cette action. Exemple : sqlpackage.exe /Action:Publish /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Spécifie si les commentaires détaillés sont supprimés. Valeur par défaut False.|
|**/Secret:**|**/secr**|{string}|Spécifie le secret du client à utiliser dans l'authentification auprès d'Azure Key Vault quand c'est nécessaire. |
|**/SourceConnectionString:**|**/scs**|{string}|Spécifie une chaîne de connexion SQL Server/Azure valide à la base de données source. Si ce paramètre est spécifié, la chaîne de connexion doit être utilisée exclusivement par tous les autres paramètres sources. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Définit le nom la base de données source. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Spécifie si le chiffrement SQL doit être utilisé pour la connexion à la base de données source. |
|**/SourceFile:**|**/sf**|{string}|Spécifie un fichier source à utiliser comme source d'action plutôt qu’une base de données. Si ce paramètre est utilisé, aucun autre paramètre source ne doit être valide. |
|**/SourcePassword:**|**/sp**|{string}|Pour les scénarios d’authentification SQL Server, définit le mot de passe à utiliser pour accéder à la base de données source. |
|**/SourceServerName:**|**/ssn**|{string}|Définit le nom du serveur hébergeant la base de données source. |
|**/SourceTimeout :**|**/st**|{int}|Spécifie le délai d'attente (en secondes) pour l'établissement d'une connexion à la base de données source. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Spécifie s'il faut utiliser TLS pour chiffrer la connexion de la base de données source et ignorer la vérification de la chaîne de certificats pour valider la chaîne d'approbation. |
|**/SourceUser:**|**/su**|{string}|Pour les scénarios d’authentification SQL Server, définit l’utilisateur SQL Server à utiliser pour accéder à la base de données source. |
|**/TargetConnectionString:**|**/tcs**|{string}|Spécifie une chaîne de connexion SQL Server/Azure valide à la base de données cible. Si ce paramètre est spécifié, la chaîne de connexion doit être utilisée exclusivement par tous les autres paramètres cibles. |
|**/TargetDatabaseName:**|**/tdn**|{string}|Spécifie une substitution pour le nom de la base de données cible de l'action sqlpackage.exe. |
|**/TargetEncryptConnection:**|**/tec**|{True&#124;False}|Spécifie si le chiffrement SQL doit être utilisé pour la connexion de la base de données cible. |
|**/TargetPassword:**|**/tp**|{string}|Pour les scénarios d’authentification SQL Server, définit le mot de passe à utiliser pour accéder à la base de données cible. |
|**/TargetServerName:**|**/tsn**|{string}|Définit le nom du serveur hébergeant la base de données cible. |
|**/TargetTimeout:**|**/tt**|{int}|Spécifie le délai d'attente (en secondes) pour l'établissement d'une connexion à la base de données cible. Pour Azure AD, il est recommandé que cette valeur soit supérieure ou égale à 30 secondes.|
|**/TargetTrustServerCertificate:**|**/ttsc**|{True&#124;False}|Spécifie s'il faut utiliser TLS pour chiffrer la connexion de la base de données cible et ignorer la vérification de la chaîne de certificats pour valider la chaîne d'approbation. |
|**/TargetUser:**|**/tu**|{string}|Pour les scénarios d’authentification SQL Server, définit l’utilisateur SQL Server à utiliser pour accéder à la base de données cible. |
|**/TenantId:**|**/tid**|{string}|Représente l’ID de locataire ou le nom de domaine Azure AD. Cette option est requise pour prendre en charge les utilisateurs invités ou importés Azure AD, ainsi que les comptes Microsoft tels que outlook.com, hotmail.com ou live.com. Si ce paramètre est omis, l’ID de locataire par défaut pour Azure AD sera utilisé, en supposant que l’utilisateur authentifié est un utilisateur natif pour cette instance AD. Toutefois, dans ce cas, les utilisateurs invités ou importés et/ou les comptes Microsoft hébergés dans cette instance Azure AD ne sont pas pris en charge et l’opération échoue. <br/> Pour plus d’informations sur l’authentification universelle avec Active Directory, consultez [Authentification universelle avec SQL Database et Azure Synapse Analytics (prise en charge de SSMS pour MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Spécifie si l’authentification universelle doit être utilisée. Quand la valeur est true, le protocole d’authentification interactive prend en charge l’authentification multifacteur. Cette option peut également être utilisée pour l’authentification Azure AD sans authentification multifacteur, à l’aide d’un protocole interactif qui oblige l’utilisateur à entrer son nom d’utilisateur et son mot de passe ou de l’authentification intégrée (informations d’identification Windows). Lorsque/UniversalAuthentication est défini sur true, aucune authentification Azure AD ne peut être spécifiée dans SourceConnectionString (/SCS). Quand/UniversalAuthentication est défini sur false, l’authentification Azure AD doit être spécifiée dans SourceConnectionString (/SCS). <br/> Pour plus d’informations sur l’authentification universelle avec Active Directory, consultez [Authentification universelle avec SQL Database et Azure Synapse Analytics (prise en charge de SSMS pour MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/Variables:**|**/v**|{PropertyName}={Value}|Spécifie une paire nom-valeur pour une variable spécifique à l’action ; {VariableName}={Value}. Le fichier DACPAC contient la liste des variables SQLCMD valides. Si une valeur n’est pas fournie pour chaque variable, une erreur est générée. |

## <a name="properties-specific-to-the-publish-action"></a>Propriétés spécifiques à l’action Publish

|Propriété|Valeur|Description|
|---|---|---|
|**/p:**|AdditionalDeploymentContributorArguments=(STRING)|Spécifie des arguments de collaborateur du déploiement supplémentaires pour les collaborateurs du déploiement. Il doit s'agir d'une liste de valeurs délimitée par des points-virgules.|
|**/p:**|AdditionalDeploymentContributors=(STRING)|Spécifie des collaborateurs de déploiement supplémentaires qui doivent être en cours d’exécution quand le fichier dacpac est déployé. Il doit s'agir d'une liste d'ID ou de noms de collaborateurs de build complets délimitée par des points-virgules.|
|**/p:**|AdditionalDeploymentContributorPaths=(STRING)| Spécifie les chemins d’accès pour charger des contributeurs de déploiement supplémentaires. Il doit s'agir d'une liste de valeurs délimitée par des points-virgules. | 
|**/p:**|AllowDropBlockingAssemblies=(BOOLEAN)|Cette propriété est utilisée par le déploiement SqlClr afin de supprimer les assemblys bloquants dans le cadre du plan de déploiement. Par défaut, les assemblys bloquants/de référence bloquent la mise à jour d'assembly si l'assembly de référence doit être supprimé.|
|**/p:**|AllowIncompatiblePlatform=(BOOLEAN)|Spécifie s'il faut tenter l'action, en dépit de la possibilité d'une incompatibilité avec les plateformes SQL Server.|
|**/p:**|AllowUnsafeRowLevelSecurityDataMovement=(BOOLEAN)|Ne pas bloquer le déplacement des données sur une table qui a une sécurité au niveau des lignes si cette propriété a la valeur true. La valeur par défaut est false.|
|**/p:**|BackupDatabaseBeforeChanges=(BOOLEAN)|Sauvegarde la base de données avant le déploiement des modifications.|
|**/p:**|BlockOnPossibleDataLoss=(BOOLEAN 'True')| Spécifie que l’opération doit s’arrêter durant l’étape de validation du schéma si les modifications de schéma résultantes risquent de provoquer une perte de données, notamment à la suite d’une réduction de la précision des données ou d’une modification du type de données nécessitant une opération de cast. La valeur par défaut (`True`) provoque l’arrêt de l’opération, que la base de données cible contienne des données ou non.  Une exécution avec la valeur `False` affectée à BlockOnPossibleDataLoss peut toujours échouer durant l’exécution du plan de déploiement si la conversion des données présentes sur la cible ne prend pas en charge le nouveau type de colonne. |
|**/p:**|BlockWhenDriftDetected=(BOOLEAN 'True')|Spécifie s'il faut bloquer la mise à jour d'une base de données dont le schéma ne correspond plus à son inscription ou qui est désinscrite.|
|**/p:**|CommandTimeout=(INT32 '60')|Spécifie le délai d'expiration de la commande (en secondes) lors de l'exécution de requêtes SQL Server.|
|**/p:**|CommentOutSetVarDeclarations=(BOOLEAN)|Spécifie si la déclaration des variables SETVAR doivent être commentées dans le script de publication généré. Cela peut vous être utile si vous prévoyez d espécifier les valeurs de la ligne de commande au moment de la publication à l’aide d’un outil tel que SQLCMD.EXE.|
|**/p:**|CompareUsingTargetCollation=(BOOLEAN)|Ce paramètre détermine la façon dont le classement de la base de données est géré durant le déploiement ; par défaut, le classement de la base de données cible sera mis à jour s'il ne correspond pas à celui spécifié par la source. Lorsque cette option est définie, le classement de la base de données (ou du serveur) cible doit être utilisé.|
|**/p:**|CreateNewDatabase=(BOOLEAN)|Spécifie si la base de données cible doit être mise à jour ou bien supprimée, puis recréée lors de la publication vers une base de données.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Définit l’édition d’une base de données Azure SQL Database.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')|Spécifie le délai d’expiration (en secondes) du verrouillage de la base de données lors de l'exécution de requêtes dans SQL Server. Utilisez -1 pour attendre indéfiniment.|
|**/p:**|DatabaseMaximumSize=(INT32)|Définit la taille maximale, en Go, d’une base de données Azure SQL Database.|
|**/p:**|DatabaseServiceObjective=(STRING)|Définit le niveau de performances d’une base de données Azure SQL Database, par exemple « P0 » ou « S1 ».|
|**/p:**|DeployDatabaseInSingleUserMode=(BOOLEAN)|Si la valeur est True, la base de données est définie en mode mono-utilisateur avant le déploiement.|
|**/p:**|DisableAndReenableDdlTriggers=(BOOLEAN 'True')|Spécifie si les déclencheurs DDL (Data Definition Language) doivent être désactivés au début du processus de publication, puis réactivés à la fin de ce dernier.|
|**/p:**|DoNotAlterChangeDataCaptureObjects=(BOOLEAN 'True')|Si la valeur est True, les objets de capture de données modifiées ne sont pas altérés.|
|**/p:**|DoNotAlterReplicatedObjects=(BOOLEAN 'True')|Spécifie si les objets répliqués sont identifiés lors de la vérification.|
|**/p:**|DoNotDropObjectType=(STRING)|Type d'objet qui ne doit pas être supprimé quand DropObjectsNotInSource est vrai. Les noms de types d'objets valides sont Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DoNotDropObjectTypes=(STRING)|Liste de types d’objet séparés par des points-virgules qui ne doivent pas être supprimés quand DropObjectsNotInSource a la valeur true. Les noms de types d'objets valides sont Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|DropConstraintsNotInSource=(BOOLEAN 'True')|Spécifie si les contraintes qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) seront supprimées de la base de données cible au moment de la publication vers une base de données.|
|**/p:**|DropDmlTriggersNotInSource=(BOOLEAN 'True')|Spécifie si les déclencheurs DML qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) seront supprimés de la base de données cible au moment de la publication vers une base de données.|
|**/p:**|DropExtendedPropertiesNotInSource=(BOOLEAN 'True')|Spécifie si les propriétés étendues qui sont absentes du fichier d'instantané de base de données (.dacpac) doivent être supprimées de la base de données cible lors de la publication dans une base de données.|
|**/p:**|DropIndexesNotInSource=(BOOLEAN 'True')|Spécifie si les index qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) seront supprimés de la base de données cible au moment de la publication vers une base de données.|
|**/p:**|DropObjectsNotInSource=(BOOLEAN)|Spécifie si les objets qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) sont supprimés de la base de données cible au moment de la publication dans une base de données. Cette valeur est prioritaire sur DropExtendedProperties.|
|**/p:**|DropPermissionsNotInSource=(BOOLEAN)|Spécifie si les autorisations qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) seront supprimés de la base de données cible au moment de la publication de mises à jour vers une base de données.|
|**/p:**|DropRoleMembersNotInSource=(BOOLEAN)|Spécifie si les membres de rôle qui ne sont pas définis dans le fichier d'instantané de base de données (.dacpac) seront supprimés de la base de données cible au moment de la publication de mises à jour vers une base de données.|
|**/p:**|DropStatisticsNotInSource=(BOOLEAN 'True')|Spécifie si les statistiques qui n'existent pas dans le fichier d'instantané de base de données (.dacpac) sont supprimées de la base de données cible quand vous publiez dans une base de données.|
|**/p:**|ExcludeObjectType=(STRING)|Type d'objet qui doit être ignoré durant le déploiement. Les noms de types d'objets valides sont Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|ExcludeObjectTypes=(STRING)|Liste de types d’objets séparés par des points-virgules qui doivent être ignorés pendant le déploiement. Les noms de types d'objets valides sont Aggregates, ApplicationRoles, Assemblies, AsymmetricKeys, BrokerPriorities, Certificates, ColumnEncryptionKeys, ColumnMasterKeys, Contracts, DatabaseRoles, DatabaseTriggers, Defaults, ExtendedProperties, ExternalDataSources, ExternalFileFormats, ExternalTables, Filegroups, FileTables, FullTextCatalogs, FullTextStoplists, MessageTypes, PartitionFunctions, PartitionSchemes, Permissions, Queues, RemoteServiceBindings, RoleMembership, Rules, ScalarValuedFunctions, SearchPropertyLists, SecurityPolicies, Sequences, Services, Signatures, StoredProcedures, SymmetricKeys, Synonyms, Tables, TableValuedFunctions, UserDefinedDataTypes, UserDefinedTableTypes, ClrUserDefinedTypes, Users, Views, XmlSchemaCollections, Audits, Credentials, CryptographicProviders, DatabaseAuditSpecifications, DatabaseScopedCredentials, Endpoints, ErrorMessages, EventNotifications, EventSessions, LinkedServerLogins, LinkedServers, Logins, Routes, ServerAuditSpecifications, ServerRoleMembership, ServerRoles, ServerTriggers.|
|**/p:**|GenerateSmartDefaults=(BOOLEAN)|Fournit automatiquement une valeur par défaut lors de la mise à jour d'une table contenant des données et une colonne n'acceptant pas les valeurs Null.|
|**/p:**|IgnoreAnsiNulls=(BOOLEAN 'True')|Spécifie si les différences relatives au paramètre ANSI NULLS doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreAuthorizer=(BOOLEAN)|Spécifie si les différences dans l'agent d'autorisation doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreColumnCollation=(BOOLEAN)|Spécifie si les différences dans les classements de colonnes doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreColumnOrder=(BOOLEAN)|Indique si les différences dans l’ordre des colonnes de table doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreComments=(BOOLEAN)|Spécifie si les différences dans les commentaires doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreCryptographicProviderFilePath=(BOOLEAN 'True')|Spécifie si les différences dans le chemin de fichier pour le fournisseur de chiffrement doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreDdlTriggerOrder=(BOOLEAN)|Spécifie si les différences relatives à l'ordre des déclencheurs DDL (Data Definition Language) doivent être ignorées ou mises à jour lors de la publication sur un serveur ou dans une base de données.|
|**/p:**|IgnoreDdlTriggerState=(BOOLEAN)|Spécifie si les différences dans l'état activé ou désactivé des déclencheurs DDL doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreDefaultSchema=(BOOLEAN)|Spécifie si les différences dans le schéma par défaut doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreDmlTriggerOrder=(BOOLEAN)|Spécifie si les différences relatives à l'ordre des déclencheurs DML (Data Manipulation Language) doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreDmlTriggerState=(BOOLEAN)|Spécifie si les différences dans l'état activé ou désactivé des déclencheurs DML doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Spécifie si les différences dans les propriétés étendues doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreFileAndLogFilePath=(BOOLEAN 'True')|Spécifie si les différences dans les chemins des fichiers et fichiers journaux doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreFilegroupPlacement=(BOOLEAN 'True')|Spécifie si les différences dans le positionnement des objets dans les FILEGROUP doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreFileSize=(BOOLEAN 'True')|Spécifie si les différences dans les tailles de fichiers doivent être ignorées ou si un avertissement doit être émis lors de la publication dans une base de données.|
|**/p:**|IgnoreFillFactor=(BOOLEAN 'True')|Spécifie si les différences dans le taux de remplissage du stockage d'index doivent être ignorées ou si un avertissement doit être émis lors de la publication dans une base de données.|
|**/p:**|IgnoreFullTextCatalogFilePath=(BOOLEAN 'True')|Spécifie si les différences dans le chemin du catalogue de texte intégral doivent être ignorées ou si un avertissement doit être émis lors de la publication dans une base de données.|
|**/p:**|IgnoreIdentitySeed=(BOOLEAN)|Spécifie si les différences du seed d'une colonne d'identité doivent être ignorées ou mises à jour quand vous publiez des mises à jour dans une base de données.|
|**/p:**|IgnoreIncrement=(BOOLEAN)|Spécifie si les différences dans l'incrément d'une colonne d'identité doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreIndexOptions=(BOOLEAN)|Spécifie si les différences dans les options d'index doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreIndexPadding=(BOOLEAN 'True')|Spécifie si les différences dans le remplissage d'index doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreKeywordCasing=(BOOLEAN 'True')|Spécifie si les différences dans la casse des mots clés doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreLockHintsOnIndexes=(BOOLEAN)|Spécifie si les différences dans les indications de verrou sur les index doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreLoginSids=(BOOLEAN 'True')|Spécifie si les différences dans le numéro d'identification de sécurité (SID) doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreNotForReplication=(BOOLEAN)|Spécifie si le paramètre NotForReplication doit être ignoré ou mis à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreObjectPlacementOnPartitionScheme=(BOOLEAN 'True')|Spécifie si le positionnement d'un objet dans un schéma de partition doit être ignoré ou mis à jour lors de la publication dans une base de données.|
|**/p:**|IgnorePartitionSchemes=(BOOLEAN)|Spécifie si les différences entre les schémas et les fonctions de partition doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnorePermissions=(BOOLEAN)|Spécifie si les différences dans les autorisations doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreQuotedIdentifiers=(BOOLEAN 'True')|Spécifie si les différences dans le paramètre d'identificateurs entre guillemets doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreRoleMembership=(BOOLEAN)|Spécifie si les différences situées au niveau du membre de rôle des informations de connexion doivent être ignorées ou mises à jour au moment de la publication vers une base de données.|
|**/p:**|IgnoreRouteLifetime=(BOOLEAN 'True')|Indique si les différences dans la durée pendant laquelle SQL Server conserve l’itinéraire dans la table de routage doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreSemicolonBetweenStatements=(BOOLEAN 'True')|Spécifie si les différences dans les points-virgules des instructions T-SQL sont ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreTableOptions=(BOOLEAN)|Spécifie si les différences dans les options de table sont ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreTablePartitionOptions=(BOOLEAN)|Spécifie si les différences dans les options de partition de table sont ignorées ou mises à jour lors de la publication dans une base de données.  Cette option s’applique uniquement aux bases de données du pool SQL dédié Azure Synapse Analytics.|
|**/p:**|IgnoreUserSettingsObjects=(BOOLEAN)|Spécifie si les différences dans les objets de paramètres utilisateur sont ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreWhitespace=(BOOLEAN 'True')|Spécifie si les différences dans les espaces blancs sont ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IgnoreWithNocheckOnCheckConstraints=(BOOLEAN)|Indique si les différences dans la valeur de la clause WITH NOCHECK pour les contraintes Check sont ignorées ou mises à jour lors de la publication.|
|**/p:**|IgnoreWithNocheckOnForeignKeys=(BOOLEAN)|Spécifie si les différences dans la valeur de la clause WITH NOCHECK pour les clés étrangères sont ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|IncludeCompositeObjects=(BOOLEAN)|Inclure tous les éléments composites dans une seule et même opération de publication.|
|**/p:**|IncludeTransactionalScripts=(BOOLEAN)|Spécifie si les instructions transactionnelles doivent être utilisées si possible lors de la publication dans une base de données.|
|**/p:**|LongRunningCommandTimeout=(INT32)|Spécifie le délai d'expiration (en secondes) de la commande longue lors de l'exécution de requêtes dans SQL Server. Utilisez 0 pour attendre indéfiniment.|
|**/p:**|NoAlterStatementsToChangeClrTypes=(BOOLEAN)|Spécifie que la publication doit toujours supprimer, puis recréer un assembly en cas de différence, au lieu d'insérer une instruction ALTER ASSEMBLY.|
|**/p:**|PopulateFilesOnFileGroups=(BOOLEAN 'True')|Spécifie si un nouveau fichier est créé quand un FileGroup est créé dans la base de données cible.|
|**/p:**|RegisterDataTierApplication=(BOOLEAN)|Spécifie si le schéma est inscrit avec le serveur de la base de données.|
|**/p:**|RunDeploymentPlanExecutors=(BOOLEAN)|Spécifie si les collaborateurs DeploymentPlanExecutor doivent être exécutés quand d'autres opérations sont exécutées.|
|**/p:**|ScriptDatabaseCollation=(BOOLEAN)|Spécifie si les différences dans le classement de base de données doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|ScriptDatabaseCompatibility=(BOOLEAN)|Spécifie si les différences en matière de compatibilité de base de données doivent être ignorées ou mises à jour lors de la publication dans une base de données.|
|**/p:**|ScriptDatabaseOptions=(BOOLEAN 'True')|Spécifie si les propriétés de la base de données cible doivent être définies ou mises à jour dans le cadre de l'action de publication.|
|**/p:**|ScriptDeployStateChecks=(BOOLEAN)|Spécifie si les instructions sont générées dans le script de publication pour vérifier que le nom de la base de données et le nom du serveur correspondent aux noms spécifiés dans le projet de base de données.|
|**/p:**|ScriptFileSize=(BOOLEAN)|Contrôle si la taille est spécifiée lors de l'ajout d'un fichier à un groupe de fichiers.|
|**/p:**|ScriptNewConstraintValidation=(BOOLEAN 'True')|À la fin de la publication, toutes les contraintes sont vérifiées comme un ensemble, évitant ainsi les erreurs de données provoquées par une contrainte de validation ou de clé étrangère rencontrée durant l’action de publication. Si cette option a la valeur False, vos contraintes sont publiées sans que les données correspondantes ne soient vérifiées.|
|**/p:**|ScriptRefreshModule=(BOOLEAN 'True')|Inclure les instructions d'actualisation à la fin du script de publication.|
|**/p:**|Storage=({File&#124;Memory})|Spécifie comment les éléments sont stockés lors de l'élaboration du modèle de base de données. Pour des raisons de performance, la valeur par défaut est InMemory. Pour les bases de données très volumineuses, un stockage sauvegardé de fichiers (File) est exigé.|
|**/p:**|TreatVerificationErrorsAsWarnings=(BOOLEAN)|Spécifie si les erreurs rencontrées lors de la vérification de la publication doivent être considérées comme des avertissements. Cette vérification est effectuée conformément au plan de déploiement généré avant l'exécution de ce dernier dans votre base de données cible. La vérification du plan permet de détecter les problèmes, comme la perte d'objets cibles (tels que les index), qui doivent être supprimés pour que la modification soit effectuée. La vérification permet également de détecter les situations dans lesquelles les dépendances (table ou vue par exemple) existent en raison d'une référence à un projet composite, mais n'existent pas dans la base de données cible. Vous pouvez choisir de le faire pour obtenir une liste complète de tous les problèmes, plutôt que de laisser l’action de publication s’arrêter à la première erreur.
|**/p:**|UnmodifiableObjectWarnings=(BOOLEAN 'True')|Spécifie si des avertissements doivent être générés lorsque des différences sont trouvées au niveau d'objets ne pouvant pas être modifiés, par exemple au niveau de la taille du fichier ou de son chemin d'accès.|
|**/p:**|VerifyCollationCompatibility=(BOOLEAN 'True')|Spécifie si la compatibilité du classement est vérifiée.|
|**/p:**|VerifyDeployment=(BOOLEAN 'True')|Spécifie si des vérifications doivent être effectuées avant la publication dans le but de rechercher les problèmes susceptibles d'empêcher une publication correcte. Par exemple, l'action de publication peut être interrompue en raison de la présence de clés étrangères dans la base de données cible qui n'existent pas dans le projet de base de données, ce qui engendre des erreurs au cours de la publication.|
|

## <a name="sqlcmd-variables"></a>Variables SQLCMD

Le tableau suivant montre le format de l'option que vous pouvez utiliser pour remplacer la valeur d'une variable (**sqlcmd**) de commande SQL utilisée lors de l'action de publication. Les valeurs de variable spécifiées sur la ligne de commande remplacent les autres valeurs attribuées à la variable (dans le profil de publication, par exemple).  
  
|Paramètre|Default|Description|  
|-------------|-----------|---------------|  
|**/Variables:{PropertyName}={Value}**||Spécifie une paire nom-valeur pour une variable spécifique à l’action ; {VariableName}={Value}. Le fichier DACPAC contient la liste des variables SQLCMD valides. Si une valeur n’est pas fournie pour chaque variable, une erreur est générée.|  

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)