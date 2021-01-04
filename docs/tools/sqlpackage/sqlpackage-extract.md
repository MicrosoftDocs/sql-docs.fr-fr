---
title: SqlPackage - Extraire
description: Découvrez comment automatiser les tâches de développement de bases de données avec l’action Extract de SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: abefb39814213426d863fa3839c4095eadc82249
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577851"
---
# <a name="sqlpackage-extract-parameters-and-properties"></a>Paramètres et propriétés de l’action Extract de SqlPackage
L’action Extract de SqlPackage.exe crée un schéma d’une base de données active à partir de SQL Server ou Azure SQL Database vers un package DACPAC (fichier .dacpac). Par défaut, les données ne sont pas incluses dans le fichier .dacpac. Pour inclure des données, utilisez [l’action Exporter](sqlpackage-export.md). 

## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

> [!NOTE]
> Quand une base de données assortie d’informations d’identification de mot de passe (par exemple, un utilisateur de l’authentification SQL) est extraite, le mot de passe est remplacé par un mot de passe différent d’une complexité appropriée. Les utilisateurs de SqlPackage ou DacFx doivent modifier le mot de passe après la publication du package d'application de la couche Données (dacpac).

## <a name="parameters-for-the-extract-action"></a>Paramètres de l’action Extract

|Paramètre|Forme abrégée|Valeur|Description|
|---|---|---|---|
|**/Action:**|**/a**|Extract|Indique l'action à effectuer. |
|**/AccessToken:**|**/at**|{string}| Dans le cadre de l'authentification par jeton, spécifie le jeton d'accès à utiliser pour se connecter à la base de données cible. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Spécifie si la journalisation des diagnostics est affichée dans la console. Valeur par défaut False. |
|**/DiagnosticsFile :**|**/df**|{string}|Spécifie un fichier où stocker les journaux de diagnostic. |
|**/MaxParallelism:**|**/mp**|{int}| Spécifie le degré de parallélisme d'opérations simultanées sur une base de données. La valeur par défaut est 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Indique si sqlpackage.exe doit remplacer les fichiers existants. Si vous choisissez False, sqlpackage.exe abandonne l'action si un fichier existant est rencontré. La valeur par défaut est True. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Spécifie une paire nom-valeur pour une propriété spécifique à l’action ; {PropertyName}={Value}. Reportez-vous à l'aide d'une action spécifique pour afficher le nom des propriétés relatives à cette action. Exemple : sqlpackage.exe /Action:Extract /?. |
|**/Quiet:**|**/q**|{True&#124;False}|Spécifie si les commentaires détaillés sont supprimés. Valeur par défaut False. |
|**/SourceConnectionString:**|**/scs**|{string}|Spécifie une chaîne de connexion SQL Server/Azure valide à la base de données source. Si ce paramètre est spécifié, la chaîne de connexion doit être utilisée exclusivement par tous les autres paramètres sources. |
|**/SourceDatabaseName:**|**/sdn**|{string}|Définit le nom la base de données source. |
|**/SourceEncryptConnection:**|**/sec**|{True&#124;False}|Spécifie si le chiffrement SQL doit être utilisé pour la connexion à la base de données source. |
|**/SourcePassword:**|**/sp**|{string}|Pour les scénarios d’authentification SQL Server, définit le mot de passe à utiliser pour accéder à la base de données source. |
|**/SourceServerName:**|**/ssn**|{string}|Définit le nom du serveur hébergeant la base de données source. |
|**/SourceTimeout :**|**/st**|{int}|Spécifie le délai d'attente (en secondes) pour l'établissement d'une connexion à la base de données source. |
|**/SourceTrustServerCertificate:**|**/stsc**|{True&#124;False}|Spécifie s'il faut utiliser TLS pour chiffrer la connexion de la base de données source et ignorer la vérification de la chaîne de certificats pour valider la chaîne d'approbation. |
|**/SourceUser:**|**/su**|{string}|Pour les scénarios d’authentification SQL Server, définit l’utilisateur SQL Server à utiliser pour accéder à la base de données source. |
|**/TargetFile :**|**/tf**|{string}| Spécifie un fichier cible (autrement dit, un fichier .dacpac) à utiliser comme cible d’action au lieu d’une base de données. Si ce paramètre est utilisé, aucun autre paramètre cible n’est valide. Ce paramètre ne doit pas être valide pour les actions qui prennent uniquement en charge les cibles de base de données.| 
|**/TenantId:**|**/tid**|{string}|Représente l’ID de locataire ou le nom de domaine Azure AD. Cette option est requise pour prendre en charge les utilisateurs invités ou importés Azure AD, ainsi que les comptes Microsoft tels que outlook.com, hotmail.com ou live.com. Si ce paramètre est omis, l’ID de locataire par défaut pour Azure AD sera utilisé, en supposant que l’utilisateur authentifié est un utilisateur natif pour cette instance AD. Toutefois, dans ce cas, les utilisateurs invités ou importés et/ou les comptes Microsoft hébergés dans cette instance Azure AD ne sont pas pris en charge et l’opération échoue. <br/> Pour plus d’informations sur l’authentification universelle avec Active Directory, consultez [Authentification universelle avec SQL Database et Azure Synapse Analytics (prise en charge de SSMS pour MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|
|**/UniversalAuthentication:**|**/ua**|{True&#124;False}|Spécifie si l’authentification universelle doit être utilisée. Quand la valeur est true, le protocole d’authentification interactive prend en charge l’authentification multifacteur. Cette option peut également être utilisée pour l’authentification Azure AD sans authentification multifacteur, à l’aide d’un protocole interactif qui oblige l’utilisateur à entrer son nom d’utilisateur et son mot de passe ou de l’authentification intégrée (informations d’identification Windows). Lorsque/UniversalAuthentication est défini sur true, aucune authentification Azure AD ne peut être spécifiée dans SourceConnectionString (/SCS). Quand/UniversalAuthentication est défini sur false, l’authentification Azure AD doit être spécifiée dans SourceConnectionString (/SCS). <br/> Pour plus d’informations sur l’authentification universelle avec Active Directory, consultez [Authentification universelle avec SQL Database et Azure Synapse Analytics (prise en charge de SSMS pour MFA)](/azure/sql-database/sql-database-ssms-mfa-authentication).|

## <a name="properties-specific-to-the-extract-action"></a>Propriétés spécifiques à l’action Extract

|Propriété|Valeur|Description|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Spécifie le délai d'expiration de la commande (en secondes) lors de l'exécution de requêtes SQL Server.|
|**/p:**|DacApplicationDescription=(STRING)|Définit la description de l'application à stocker dans les métadonnées DACPAC.|
|**/p:**|DacApplicationName=(STRING)|Définit le nom de l'application à stocker dans les métadonnées DACPAC. La valeur par défaut est le nom de la base de données.|
|**/p:**|DacMajorVersion=(INT32 '1')|Définit la version principale à stocker dans les métadonnées DACPAC.|
|**/p:**|DacMinorVersion=(INT32 '0')|Définit la version secondaire à stocker dans les métadonnées DACPAC.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Spécifie le délai d’expiration (en secondes) du verrouillage de la base de données lors de l'exécution de requêtes dans SQL Server. Utilisez -1 pour attendre indéfiniment.|
|**/p:**|ExtractAllTableData=(BOOLEAN)|Indique si les données de toutes les tables utilisateur sont extraites. Si la valeur est « true », les données de toutes les tables utilisateur sont extraites et vous ne pouvez pas spécifier des tables utilisateur individuelles pour l’extraction des données. Si la valeur est « false », spécifiez une ou plusieurs tables utilisateur à partir desquelles extraire des données.|
|**/p:**|ExtractApplicationScopedObjectsOnly=(BOOLEAN 'True')|Si la valeur est True, extrait uniquement les objets à portée d'application pour la source spécifiée. Si la valeur est False, extrait tous les objets pour la source spécifiée.|
|**/p:**|ExtractReferencedServerScopedElements=(BOOLEAN 'True')|Si la valeur est true, extrait les objets de connexion, d’audit de serveur et d’informations d’identification référencés par les objets de base de données source.|
|**/p:**|ExtractUsageProperties=(BOOLEAN)|Indique si les propriétés d'utilisation, telles que le nombre de lignes du tableau et la taille de l'index, seront extraites de la base de données.|
|**/p:**|IgnoreExtendedProperties=(BOOLEAN)|Spécifie si les propriétés étendues doivent être ignorées.|
|**/p:**|IgnorePermissions=(BOOLEAN 'True')|Spécifie si les autorisations doivent être ignorées.|
|**/p:**|IgnoreUserLoginMappings=(BOOLEAN)|Indique si les relations entre les utilisateurs et les connexions sont ignorées.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Spécifie le délai d'expiration (en secondes) de la commande longue lors de l'exécution de requêtes dans SQL Server. Utilisez 0 pour attendre indéfiniment.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Spécifie le type de stockage de sauvegarde pour le modèle de schéma utilisé lors de l'extraction.|
|**/p:**|TableData=(STRING)|Indique la table à partir de laquelle les données sont extraites. Spécifiez les parties du nom de la table avec ou sans crochets sous le format suivant : nom_schéma.identificateur_table. Cette option peut être spécifiée plusieurs fois.|
|**/p:**| TempDirectoryForTableData=(STRING)|Spécifie le répertoire temporaire utilisé pour la mise en mémoire tampon des données de table avant leur écriture dans le fichier de package.|
|**/p:**|VerifyExtraction=(BOOLEAN)|Spécifie si le fichier dacpac extrait doit être vérifié.|

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)