---
title: SqlPackage - Importer
description: Découvrez comment automatiser les tâches de développement de bases de données avec l’action Import de SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 0e9acab737de04b002debf9d8c1b230a5cb01b14
ms.sourcegitcommit: 866554663ca3191748b6e4eb4d8d82fa58c4e426
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97577846"
---
# <a name="sqlpackage-import-parameters-and-properties"></a>Paramètres et propriétés de l’action Import de SqlPackage
L’action Import de SqlPackage.exe importe le schéma et les données de table d’un package BACPAC (fichier .bacpac) dans une base de données nouvelle ou vide dans SQL Server ou Azure SQL Database. Au moment de l’opération d’importation dans une base de données existante, la base de données cible ne peut pas contenir d’objets de schéma définis par l’utilisateur.  

## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-import-action"></a>Paramètres de l’action Import

|Paramètre|Forme abrégée|Valeur|Description|
|---|---|---|---|
|**/Action:**|**/a**|Importer|Indique l'action à effectuer. |
|**/AccessToken:**|**/at**|{string}| Dans le cadre de l'authentification par jeton, spécifie le jeton d'accès à utiliser pour se connecter à la base de données cible. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Spécifie si la journalisation des diagnostics est affichée dans la console. Valeur par défaut False. |
|**/DiagnosticsFile :**|**/df**|{string}|Spécifie un fichier où stocker les journaux de diagnostic. |
|**/MaxParallelism:**|**/mp**|{int}| Spécifie le degré de parallélisme d'opérations simultanées sur une base de données. La valeur par défaut est 8. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Spécifie une paire nom-valeur pour une propriété spécifique à l’action ; {PropertyName}={Value}. Reportez-vous à l'aide d'une action spécifique pour afficher le nom des propriétés relatives à cette action. Exemple : sqlpackage.exe /Action:Import /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Spécifie si les commentaires détaillés sont supprimés. Valeur par défaut False.|
|**/SourceFile:**|**/sf**|{string}|Spécifie un fichier source à utiliser comme source d’action. Si ce paramètre est utilisé, aucun autre paramètre source ne doit être valide. |
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

## <a name="properties-specific-to-the-import-action"></a>Propriétés spécifiques de l’action Import

|Propriété|Valeur|Description|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Spécifie le délai d'expiration de la commande (en secondes) lors de l'exécution de requêtes SQL Server.|
|**/p:**|DatabaseEdition=({Basic&#124;Standard&#124;Premium&#124;DataWarehouse&#124;GeneralPurpose&#124;BusinessCritical&#124;Hyperscale&#124;Default} 'Default')|Définit l’édition d’une base de données Azure SQL Database.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Spécifie le délai d’expiration (en secondes) du verrouillage de la base de données lors de l'exécution de requêtes dans SQL Server. Utilisez -1 pour attendre indéfiniment.|
|**/p:**|DatabaseMaximumSize=(INT32)|Définit la taille maximale, en Go, d’une base de données Azure SQL Database.|
|**/p:**|DatabaseServiceObjective=(STRING)|Définit le niveau de performances d’une base de données Azure SQL Database, par exemple « P0 » ou « S1 ».|
|**/p:**|ImportContributorArguments=(STRING)|Spécifie des arguments de collaborateur de déploiement pour les collaborateurs de déploiement. Il doit s'agir d'une liste de valeurs délimitée par des points-virgules.|
|**/p:**|ImportContributors=(STRING)|Spécifie les collaborateurs de déploiement qui doivent être en cours d’exécution quand le fichier bacpac est importé. Il doit s'agir d'une liste d'ID ou de noms de collaborateurs de build complets délimitée par des points-virgules.|
|**/p:**|ImportContributorPaths=(STRING)|Spécifie les chemins d’accès pour charger des contributeurs de déploiement supplémentaires. Il doit s'agir d'une liste de valeurs délimitée par des points-virgules. |
|**/p:**|LongRunningCommandTimeout=(INT32)| Spécifie le délai d'expiration (en secondes) de la commande longue lors de l'exécution de requêtes dans SQL Server. Utilisez 0 pour attendre indéfiniment.|
|**/p:**|Storage=({File&#124;Memory})|Spécifie comment les éléments sont stockés lors de l'élaboration du modèle de base de données. Pour des raisons de performance, la valeur par défaut est InMemory. Pour les bases de données très volumineuses, un stockage sauvegardé de fichiers (File) est exigé.|
  

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)