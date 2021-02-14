---
title: SqlPackage - Exporter
description: Découvrez comment automatiser les tâches de développement de bases de données avec l’action Export de SqlPackage.exe. Affichez des exemples et des paramètres, des propriétés et des variables SQLCMD.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
ms.assetid: 198198e2-7cf4-4a21-bda4-51b36cb4284b
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 12/11/2020
ms.openlocfilehash: 942d0c84e697654e67d48ce708792eb8bcf770aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081480"
---
# <a name="sqlpackage-export-parameters-and-properties"></a>Paramètres et propriétés de l’action Export de SqlPackage
L’action Export de SqlPackage.exe exporte une base de données connectée dans un fichier BACPAC (.bacpac). Par défaut, les données de toutes les tables sont incluses dans le fichier .bacpac. Si vous le souhaitez, vous pouvez spécifier uniquement un sous-ensemble de tables pour lequel exporter des données. La validation de l'action d'exportation garantit la compatibilité avec Azure SQL Database pour la base de données ciblée complète même si un sous-ensemble de tables est spécifié pour l'exportation. 

## <a name="command-line-syntax"></a>Syntaxe de ligne de commande

**SqlPackage.exe** initie les actions spécifiées à l'aide des paramètres, des propriétés et des variables SQLCMD indiqués sur la ligne de commande.  
  
```bash
SqlPackage {parameters}{properties}{SQLCMD Variables}  
```

## <a name="parameters-for-the-export-action"></a>Paramètres de l’action Export

|Paramètre|Forme abrégée|Valeur|Description|
|---|---|---|---|
|**/Action:**|**/a**|Exporter|Indique l'action à effectuer. |
|**/AccessToken:**|**/at**|{string}| Dans le cadre de l'authentification par jeton, spécifie le jeton d'accès à utiliser pour se connecter à la base de données cible. |
|**/Diagnostics:**|**/d**|{True&#124;False}|Spécifie si la journalisation des diagnostics est affichée dans la console. Valeur par défaut False. |
|**/DiagnosticsFile :**|**/df**|{string}|Spécifie un fichier où stocker les journaux de diagnostic. |
|**/MaxParallelism:**|**/mp**|{int}| Spécifie le degré de parallélisme d'opérations simultanées sur une base de données. La valeur par défaut est 8. |
|**/OverwriteFiles:**|**/of**|{True&#124;False}|Indique si sqlpackage.exe doit remplacer les fichiers existants. Si vous choisissez False, sqlpackage.exe abandonne l'action si un fichier existant est rencontré. La valeur par défaut est True. |
|**/Properties:**|**/p**|{PropertyName}={Value}|Spécifie une paire nom-valeur pour une propriété spécifique à l’action ; {PropertyName}={Value}. Reportez-vous à l'aide d'une action spécifique pour afficher le nom des propriétés relatives à cette action. Exemple : sqlpackage.exe /Action:Export /?.|
|**/Quiet:**|**/q**|{True&#124;False}|Spécifie si les commentaires détaillés sont supprimés. Valeur par défaut False.|
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

## <a name="properties-specific-to-the-export-action"></a>Propriétés spécifiques à l’action Export

|Propriété|Valeur|Description|
|---|---|---|
|**/p:**|CommandTimeout=(INT32 '60')|Spécifie le délai d'expiration de la commande (en secondes) lors de l'exécution de requêtes SQL Server.|
|**/p:**|DatabaseLockTimeout=(INT32 '60')| Spécifie le délai d’expiration (en secondes) du verrouillage de la base de données lors de l'exécution de requêtes dans SQL Server. Utilisez -1 pour attendre indéfiniment.|
|**/p:**|LongRunningCommandTimeout=(INT32)| Spécifie le délai d'expiration (en secondes) de la commande longue lors de l'exécution de requêtes dans SQL Server. Utilisez 0 pour attendre indéfiniment.|
|**/p:**|Storage=({File&#124;Memory} 'File')|Spécifie le type de stockage de sauvegarde pour le modèle de schéma utilisé lors de l'extraction.|
|**/p:**|TableData=(STRING)|Indique la table à partir de laquelle les données sont extraites. Spécifiez les parties du nom de la table avec ou sans crochets sous le format suivant : nom_schéma.identificateur_table. Cette option peut être spécifiée plusieurs fois.|
|**/p:**|TargetEngineVersion=({Default&#124;Latest&#124;V11&#124;V12} 'Latest')|Spécifie la version de moteur cible attendue. Ce paramètre permet d’autoriser ou non les objets pris en charge par les serveurs Azure SQL Database avec les fonctionnalités V12, comme les tables à mémoire optimisée, dans le fichier BACPAC généré.|
|**/p:**|TempDirectoryForTableData=(STRING)|Spécifie le répertoire temporaire utilisé pour la mise en mémoire tampon des données de table avant leur écriture dans le fichier de package.|
|**/p:**|VerifyFullTextDocumentTypesSupported=(BOOLEAN)|Indique si les types de document de texte intégral pris en charge pour Microsoft Azure SQL Database v12 doivent être vérifiés.|

## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)