---
title: Audit local pour l’utilisation et collecte des données d’utilisation et de diagnostic
description: En savoir plus sur l’audit local utilisé par SQL Server pour collecter et envoyer des données d’utilisation et de diagnostic à Microsoft.
ms.custom: seo-lt-2019
ms.date: 03/27/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Local Audit
ms.assetid: a0665916-7789-4f94-9086-879275802cf3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6333f571ea271420b4044f7e3cf562d2f1edcc15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353542"
---
# <a name="local-audit-for-sql-server-usage-and-diagnostic-data-collection-ceip"></a>Audit local pour l’utilisation de SQL Server et collecte des données d’utilisation et de diagnostic (CEIP)

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sql-windows-only-asdbmi.md)]

## <a name="introduction"></a>Introduction

Microsoft SQL Server contient des fonctionnalités Internet capables de collecter et d’envoyer des informations sur votre ordinateur ou appareil. Il s’agit des *informations standard de l’ordinateur*. Le composant d’audit local de la [collecte de données d’utilisation et de diagnostic de SQL Server](usage-and-diagnostic-data-configuration-for-sql-server.md) écrit les données collectées par le service vers un dossier désigné, représentant les données (journaux) qui peuvent être envoyées à Microsoft. L’objectif de l’audit local est d’autoriser les clients à visualiser toutes les données collectées par Microsoft avec cette fonctionnalité, pour des raisons de conformité, de réglementation ou de validation de la confidentialité.  

Pour SQL Server 2016 CU2 et CU3, l’audit local est configurable au niveau de l’instance pour le moteur de base de données SQL Server et Analysis Services (SSAS). Pour SQL Server 2016 CU4, SQL Server 2016 SP1 et les versions ultérieures, l’audit local est également activé pour SQL Server Integration Services (SSIS). Les autres composants SQL Server installés pendant la configuration et les outils SQL Server qui sont téléchargés ou installés après la configuration ne possèdent pas de fonctionnalité d’audit local pour les données d’utilisation et de diagnostic.

## <a name="remarks"></a>Notes

 - La suppression ou la désactivation du service CEIP SQL n’est pas prise en charge. 
 - La suppression des ressources CEIP SQL à partir du groupe de cluster n’est pas prise en charge. 

Pour refuser la collecte de données, consultez [Activation ou désactivation de l’audit local](#turning-local-audit-on-or-off)

## <a name="prerequisites"></a>Prérequis 

Les éléments suivants sont nécessaires pour activer l’audit local sur chaque instance SQL Server : 

1. L’instance est corrigée sur SQL Server 2016 RTM CU2 ou version ultérieure. Pour Integration Services, l’instance est corrigée sur SQL 2016 RTM CU4, SQL 2016 SP1 et les versions ultérieures.

1. L’utilisateur doit être un administrateur système ou doit bénéficier d’un rôle ayant accès à l’ajout et à la modification de la clé de Registre, la création des dossiers, la gestion de la sécurité des dossiers et l’arrêter/le démarrage d’un service Windows.  

## <a name="pre-configuration-steps-prior-to-turning-on-local-audit"></a>Étapes de préconfiguration avant l’activation de l’audit local

Avant d’activer l’audit local, un administrateur système doit :

1. connaître le nom de l’instance SQL Server et le compte de connexion de service SQL Server CEIP ; 

1. Configurez un nouveau dossier pour les fichiers d’audit local.

1. accorder des autorisations au compte de connexion du service SQL Server CEIP ;

1. créer un paramètre de clé de Registre pour configurer le répertoire cible d’audit local ; 


### <a name="get-the-sql-server-ceip-service-logon-account"></a>obtenir le compte de connexion de service SQL Server CEIP. 

Procédez comme suit pour obtenir le compte de connexion de service SQL Server CEIP
 
1. Lancez la console **Services**. Pour ce faire, utilisez les **touches Windows + R** de votre clavier pour ouvrir la boîte de dialogue **Exécuter**. Ensuite, tapez *services.msc* dans le champ de texte et sélectionnez **OK** pour lancer la console **Services**.  

2. Accédez au service approprié. Par exemple, pour le moteur de base de données, recherchez **Service CEIP SQL Server** **(*Nom-de-votre-instance*)** . Pour Analysis Services, recherchez **CEIP SQL Server Analysis Services** **(*Nom-de-votre-instance*)** . Pour Integration Services, recherchez **Service CEIP SQL Server Integration Services**.

3. Cliquez avec le bouton droit sur le service et sélectionnez **Propriétés**. 

4. Sélectionnez l’onglet **Ouvrir une session**. Le compte d’ouverture de session apparaît dans **Ce compte**. 

### <a name="configure-a-new-folder-for-the-local-audit-files"></a>Configurez un nouveau dossier pour les fichiers d’audit local.    

Créez un dossier (répertoire d’audit local) où l’audit local écrit les journaux. Par exemple, le chemin complet du répertoire d’audit local pour une instance par défaut du moteur de base de données serait : *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* . 
 
  >[!NOTE] 
  >Configurez le chemin du répertoire pour l’audit local en dehors du chemin d’installation de SQL Server pour éviter que la fonctionnalité d’audit et la correction entraînent des problèmes éventuels avec SQL Server.

|Décision de conception|Recommandation|  
|-----------------|----------|  
|Disponibilité de l’espace |Pour une charge de travail modérée d’environ 10 bases de données, prévoyez environ 2 Mo d’espace disque par base de données et par instance.|  
|Répertoires distincts | Créez un répertoire pour chaque instance. Par exemple, utilisez *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* pour une instance SQL Server nommée `MSSQLSERVER`. Cela simplifie la gestion des fichiers.
|Dossiers séparés |Utilisez un dossier spécifique pour chaque service. Par exemple, pour un nom d’instance donné, vous devez avoir un dossier pour le moteur de base de données. Si une instance d’Analysis Services utilise le même nom d’instance, créez un dossier distinct pour Analysis Services. Lorsque le moteur de base de données et les instances Analysis Services sont configurés dans le même dossier, l’audit local écrit dans le même fichier journal pour les deux instances.| 
|Accorder des autorisations au compte de connexion du service SQL Server CEIP|Activer l’option **Lister le contenu des dossiers**, et l’accès **En lecture** et **En écriture** au compte de connexion de service SQL Server CEIP|


### <a name="grant-permissions-to-the-sql-server-ceip-service-logon-account"></a>Accorder des autorisations au compte de connexion du service SQL Server CEIP
  
1. Dans **l’Explorateur de fichiers**, accédez à l’emplacement où se trouve le nouveau dossier.

1. Cliquez avec le bouton droit sur le nom du dossier, puis sélectionnez **Propriétés**. 

1. Sous l’onglet **Sécurité**, sélectionnez **Modifier**, puis gérez l’autorisation.

1. Sélectionnez **Ajouter** et tapez les informations d’identification du service SQL Server CEIP. Par exemple, `NT Service\SQLTELEMETRY`.

1. Sélectionnez **Vérifier les noms** pour valider le nom que vous avez fourni, puis sélectionnez **OK**.

1. Dans la boîte de dialogue **Autorisation**, choisissez le compte de connexion au service SQL Server CEIP, puis sélectionnez **Lister le contenu des dossiers**, **En lecture** et **En écriture**.

1. Sélectionnez **OK** pour appliquer immédiatement les modifications d’autorisation. 
  
### <a name="create-a-registry-key-setting-to-configure-local-audit-target-directory"></a>Créer un paramètre de clé de Registre pour configurer le répertoire cible d’audit local

1. Lancez regedit.

1. Accédez au chemin CPE approprié :

   | Version | ***Moteur de base de données*** - Clé de Registre |
   | :------ | :----------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **13**.*Nom-de-votre-instance*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **14**.*Nom-de-votre-instance*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSSQL **15**.*Nom-de-votre-instance*\\CPE |
   | &nbsp; | &nbsp; |

   | Version | ***Analysis Services*** - Clé de Registre |
   | :------ | :------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **13**.*Nom-de-votre-instance*\\CPE |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **14**.*Nom-de-votre-instance*\\CPE |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\MSAS **15**.*Nom-de-votre-instance*\\CPE |  
   | &nbsp; | &nbsp; |

   | Version | ***Integration Services*** - Clé de Registre |
   | :------ | :---------------------------------- |
   | 2016    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**130** |
   | 2017    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**140** |
   | 2019    | HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\Microsoft SQL Server\\**150** |
   | &nbsp; | &nbsp; |

1. Cliquez avec le bouton droit sur le chemin CPE et choisissez **Nouveau**. Sélectionnez **Valeur de chaîne**.

1. Nommez la nouvelle clé de Registre `UserRequestedLocalAuditDirectory`. 
 
## <a name="turning-local-audit-on-or-off"></a>Activation ou désactivation de l’audit local

Après avoir effectué les étapes de préconfiguration, vous pouvez activer l’audit local. Pour ce faire, utilisez un compte d’administrateur système ou un rôle similaire avec accès à la modification des clés de Registre pour activer ou désactiver l’audit local en suivant les étapes ci-dessous. 

1. Lancez **regedit**.  

1. Accédez au [chemin](#create-a-registry-key-setting-to-configure-local-audit-target-directory) CPE approprié. 

1. Cliquez avec le bouton droit sur **UserRequestedLocalAuditDirectory**, puis sélectionnez *Modifier*. 

1. Pour activer l’audit local, tapez le chemin de l’audit local, par exemple *C:\\SQLCEIPAudit\\MSSQLSERVER\\DB\\* .
 
    Pour désactiver l’audit local, effacez la valeur contenue dans **UserRequestedLocalAuditDirectory**.

1. Fermez **regedit**. 

SQL Server CEIP doit reconnaître le paramètre d’audit local immédiatement si le service est déjà en cours d’exécution. Pour démarrer le service CEIP SQL Server, un administrateur système ou une personne ayant accès au démarrage ou à l’arrêt des services Windows peut suivre les étapes ci-dessous : 

1. Lancez la console **Services**. Pour ce faire, utilisez les **touches Windows + R** de votre clavier pour ouvrir la boîte de dialogue **Exécuter**. Ensuite, tapez *services.msc* dans le champ de texte et sélectionnez **OK** pour lancer la console **Services**.  

1. Accédez au service approprié. 

    - Pour le moteur de base de données, utilisez **Service SQL Server CEIP (*Nom-de-votre-instance*)**.     
    - Pour Analysis Services, utilisez **CEIP SQL Server Analysis Services (*Nom-de-votre-instance*)**.
    - Pour Integration Services, 
        - Pour SQL 2016, utilisez *Service CEIP SQL Server Integration Services 13.0*.
        - Pour SQL 2017, utilisez *Service CEIP SQL Server Integration Services 14.0*.
    - Pour SQL 2019, utilisez *Service CEIP SQL Server Integration Services 15.0*.

1. Cliquez avec le bouton droit sur le service et sélectionnez Redémarrer. 

1. Vérifiez que l’état du service est **En cours d’exécution**. 

L’audit local va générer un fichier journal par jour. Les fichiers journaux seront au format `<YYYY-MM-DD>.json`. Par exemple, *2016-07-12.json*. Si un fichier existe déjà pour le jour dans le répertoire désigné, l’audit local va s’y ajouter. Dans le cas contraire, il créera un fichier pour le jour en question. 

  >[!NOTE]
  > Après avoir activé l’audit local, la première écriture dans le fichier journal peut prendre jusqu’à 5 minutes. 

## <a name="maintenance"></a>Maintenance 

1. Pour limiter l’espace disque utilisé par les fichiers écrits par l’audit local, configurez une stratégie ou une tâche régulière pour nettoyer le répertoire d’audit local en supprimant les fichiers anciens et inutiles.  

2. Sécurisez le chemin d’accès du répertoire d’audit local afin qu’il soit accessible uniquement par les personnes concernées. Notez que les fichiers journaux contiennent les informations décrites dans [Configurer SQL Server 2016 pour envoyer des commentaires à Microsoft](https://support.microsoft.com/kb/3153756). L’accès à ce fichier doit être configuré pour empêcher la plupart des membres de votre organisation de le lire.  

## <a name="data-dictionary-of-local-audit-output-data-structure"></a>Dictionnaire de données de la structure de données de sortie d’audit local 

- Les fichiers journaux d’audit local sont au format JSON, contenant un ensemble d’objets (lignes) représentant des points de données qui sont envoyés à Microsoft à **emitTime**.
- Chaque ligne suit un schéma spécifique identifié par **schemaVersion**.
- Chaque ligne est une sortie d’une session de service SQLCEIP identifiée comme **sessionID**.
- Les lignes sont émises dans l’ordre identifié par **sequence**.
- Chaque ligne de point de données contient la sortie d’un **queryIdentifier**, qui peut être une requête T-SQL, une session XE ou un message lié à un type de trace, identifié par **traceName**.
- Les **queryIdentifiers** sont regroupés et un contrôle de version leur est affecté avec **querySetVersion**.
- **data** contient la sortie de l’exécution de la requête correspondante, qui a pris **queryTimeInTicks**.
- Les **queryIdentifiers** pour les requêtes T-SQL ont la définition de la requête T-SQL stockée dans la requête.

| Hiérarchie logique des informations d’audit local | Colonnes associées |
| ------ | -------|
| En-tête | emitTime, schemaVersion 
| Machine | operatingSystem 
| Instance | instanceUniqueID, correlationID, clientVersion 
| session | sessionID, traceName 
| Query | sequence, querySetVersion, queryIdentifier, query, queryTimeInTicks 
| Données |  data 

### <a name="namevalue-pairs-definition-and-examples"></a>Définition et exemples des paires nom/valeur 

Les colonnes répertoriées ci-dessous représentent l’ordre de la sortie du fichier d’audit local. Un hachage à sens unique avec SHA 256 est utilisé pour rendre anonyme les valeurs pour certaines colonnes ci-dessous.  

| Nom | Description | Exemples de valeurs
|-------|--------| ----------|
|instanceUniqueID| Identificateur d’instance rendu anonyme | 888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD 
|schemaVersion| Version du schéma de SQLCEIP |  3 
|emitTime |Heure d’émission UTC du point de données | 2016-09-08T17:20:22.1124269Z 
|sessionID | Identificateur de session pour la maintenance du service SQLCEIP | 89decf9a-ad11-485c-94a7-fefb3a02ed86 
|correlationId | Espace réservé pour un identificateur supplémentaire | 0 
|sequence | Numéro de séquence des points de données envoyés au sein de la session | 15 
|clientVersion | Version d’instance SQL Server | 13.0.2161.3 ((SQL16_RTM_QFE-CU).160907-1223) 
|operatingSystem | La version du système d’exploitation sur lequel l’instance SQL Server est installée | Microsoft Windows Server 2012 R2 Datacenter 
|querySetVersion | Version d’un groupe de définitions de requête | 1.0.0.0 
|traceName | Catégories de traces : (SQLServerXeQueries, SQLServerPeriodicQueries, SQLServerOneSettingsException) | SQLServerPeriodicQueries 
|queryIdentifier | Un identificateur de la requête | SQLServerProperties.002 
|data   | La sortie des informations collectées sur queryIdentifier en tant que sortie de requête T-SQL, de session XE ou de l’application |  [{"Collation": "SQL_Latin1_General_CP1_CI_AS","SqlFTinstalled": "0" "SqlIntSec": "1","IsSingleUser": "0","SqlFilestreamMode": "0","SqlPbInstalled": "0","SqlPbNodeRole": "","SqlVersionMajor": "13","SqlVersionMinor": "0","SqlVersionBuild": "2161","ProductBuildType": "","ProductLevel": "RTM","ProductUpdateLevel": "CU2","ProductUpdateReference": "KB3182270","ProductRevision": "3","SQLEditionId": "-1534726760","IsClustered": "0","IsHadrEnabled": "0","SqlAdvAInstalled": "0","PacketReceived": "1210","Version": "Microsoft SQL Server 2016 (RTM-CU2) (KB3182270) - 13.0.2161.3 (X64) \n\tSep  7 2016 14:24:16 \n\tCopyright (c) Microsoft Corporation\n\tÉdition Standard (64 bits) sur Windows Server 2012 R2 Datacenter 6.3 \u003cX64\u003e (Build 9600: ) (Hypervisor)\n"}],
|query| Le cas échéant, la définition de requêtes T-SQL liée au queryIdentifier qui génère des données.        Ce composant n’est pas téléchargé par le service SQL Server CEIP. Il est inclus dans l’audit local en tant que référence pour les clients uniquement.| SELECT\n      SERVERPROPERTY(\u0027Collation\u0027) AS [Collation],\n      SERVERPROPERTY(\u0027IsFullTextInstalled\u0027) AS [SqlFTinstalled],\n      SERVERPROPERTY(\u0027IsIntegratedSecurityOnly\u0027) AS [SqlIntSec],\n      SERVERPROPERTY(\u0027IsSingleUser\u0027) AS [IsSingleUser],\n      SERVERPROPERTY (\u0027FileStreamEffectiveLevel\u0027) AS [SqlFilestreamMode],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbInstalled],\n      SERVERPROPERTY(\u0027IsPolyBaseInstalled\u0027) AS [SqlPbNodeRole],\n      SERVERPROPERTY(\u0027ProductMajorVersion\u0027) AS [SqlVersionMajor],\n      SERVERPROPERTY(\u0027ProductMinorVersion\u0027) AS [SqlVersionMinor],\n      SERVERPROPERTY(\u0027ProductBuild\u0027) AS [SqlVersionBuild],\n      SERVERPROPERTY(\u0027ProductBuildType\u0027) AS ProductBuildType,\n      SERVERPROPERTY(\u0027ProductLevel\u0027) AS ProductLevel,\n      SERVERPROPERTY(\u0027ProductUpdateLevel\u0027) AS ProductUpdateLevel,\n      SERVERPROPERTY(\u0027ProductUpdateReference\u0027) AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)),CHARINDEX(\u0027.\u0027, REVERSE(CAST(SERVERPROPERTY(\u0027ProductVersion\u0027) AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY(\u0027EditionID\u0027) AS SQLEditionId,\n      SERVERPROPERTY(\u0027IsClustered\u0027) AS IsClustered,\n      SERVERPROPERTY(\u0027IsHadrEnabled\u0027) AS IsHadrEnabled,\n      SERVERPROPERTY(\u0027IsAdvancedAnalyticsInstalled\u0027) AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version
|queryTimeInTicks | La durée nécessaire à l’exécution de la requête avec la catégorie de trace suivante : (SQLServerXeQueries, SQLServerPeriodicQueries) |  0 
 
### <a name="trace-categories"></a>Catégories de trace 
Actuellement, nous collectons les catégories de trace suivantes : 

- **SQLServerXeQueries**: contient les points de données collectés via la session d’événements étendus.
- **SQLServerPeriodicQueries**: contient les points de données collectés via les requêtes périodiques exécutées dans une instance SQL Server.
- **SQLServerPerDBPeriodicQueries**: contient les points de données collectés via les requêtes périodiques exécutées sur 30 bases de données maximum dans une instance SQL Server.
- **SQLServerOneSettingsException**: contient les messages d’exception liés à la mise à jour du jeu de schéma et/ou du jeu de requêtes.
- **DigitalProductID**: contient les points de pour l’agrégation d’ID de produits numériques hachés (SHA-256) anonymes d’instances SQL Server. 

### <a name="local-audit-file-examples"></a>Exemples de fichiers d’audit local



Voici un extrait d’une sortie de fichier JSON d’audit local.

```JSON
[
  {
    "instanceUniqueId": "888770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:27:59.7031518Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 18,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "SQLServerProperties.002",
    "data": [
      {
        "Collation": "SQL_Latin1_General_CP1_CI_AS",
        "SqlFTinstalled": "0",
        "SqlIntSec": "1",
        "IsSingleUser": "0",
        "SqlFilestreamMode": "2",
        "SqlPbInstalled": "1",
        "SqlPbNodeRole": "Head",
        "SqlVersionMajor": "14",
        "SqlVersionMinor": "0",
        "SqlVersionBuild": "3025",
        "ProductBuildType": "",
        "ProductLevel": "RTM",
        "ProductUpdateLevel": "CU6",
        "ProductUpdateReference": "KB4101464",
        "ProductRevision": "34",
        "SQLEditionId": "1872460670",
        "IsClustered": "0",
        "IsHadrEnabled": "0",
        "SqlAdvAInstalled": "1",
        "PacketReceived": "422",
        "Version": "Microsoft SQL Server 2017 (RTM-CU6) (KB4101464) - 14.0.3025.34 (X64) \n\tApr  9 2018 18:00:41 \n\tCopyright (C) 2017 Microsoft Corporation\n\tEnterprise Edition: Core-based Licensing (64-bit) on Windows 10 Enterprise 10.0 <X64> (Build 16299: )\n"
      }
    ],
    "query": "SELECT\n      SERVERPROPERTY('Collation') AS [Collation],\n      SERVERPROPERTY('IsFullTextInstalled') AS [SqlFTinstalled],\n      SERVERPROPERTY('IsIntegratedSecurityOnly') AS [SqlIntSec],\n      SERVERPROPERTY('IsSingleUser') AS [IsSingleUser],\n      SERVERPROPERTY ('FileStreamEffectiveLevel') AS [SqlFilestreamMode],\n      SERVERPROPERTY('IsPolyBaseInstalled') AS [SqlPbInstalled],\n      SERVERPROPERTY('PolyBaseRole') AS [SqlPbNodeRole],\n      SERVERPROPERTY('ProductMajorVersion') AS [SqlVersionMajor],\n      SERVERPROPERTY('ProductMinorVersion') AS [SqlVersionMinor],\n      SERVERPROPERTY('ProductBuild') AS [SqlVersionBuild],\n      SERVERPROPERTY('ProductBuildType') AS ProductBuildType,\n      SERVERPROPERTY('ProductLevel') AS ProductLevel,\n      SERVERPROPERTY('ProductUpdateLevel') AS ProductUpdateLevel,\n      SERVERPROPERTY('ProductUpdateReference') AS ProductUpdateReference,\n      RIGHT(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)),CHARINDEX('.', REVERSE(CAST(SERVERPROPERTY('ProductVersion') AS NVARCHAR(30)))) - 1) AS ProductRevision,\n      SERVERPROPERTY('EditionID') AS SQLEditionId,\n      SERVERPROPERTY('IsClustered') AS IsClustered,\n      SERVERPROPERTY('IsHadrEnabled') AS IsHadrEnabled,\n      SERVERPROPERTY('IsAdvancedAnalyticsInstalled') AS [SqlAdvAInstalled],\n      @@PACK_RECEIVED AS PacketReceived,\n      @@VERSION AS Version",
    "queryTimeInTicks": 0
  },
  {
    "instanceUniqueId": "8884F770C4D5A8C6729F76F33D472B28883AE518C92E1999888B171A085059FD",
    "isSSEIInstance": "0",
    "schemaVersion": "5",
    "emitTime": "2018-05-04T15:28:00.9025999Z",
    "sessionId": "c3cd1b56-ab61-462f-8363-8881779aa223",
    "correlationId": 0,
    "sequence": 23,
    "clientVersion": "14.0.3025.34 ((SQLServer2017-CU6).180410-0033)",
    "isInternalMachine": "1",
    "operatingSystem": "Microsoft Windows 10 Enterprise",
    "querySetVersion": "14.0.3025.34",
    "traceName": "SQLServerPeriodicQueries",
    "queryIdentifier": "OsSysInfo.003",
    "data": [
      {
        "LogicalCPUCount": "8",
        "HyperthreadRatio": "8",
        "PhysicalMemoryMB": "32710.902343",
        "SQLServerStartTime": "05/04/2018 08:22:30",
        "AffinityTypeDesc": "AUTO",
        "VirtualMachineType": "0",
        "SocketCount": "1",
        "CoresPerSocket": "4",
        "NumaNodeCount": "1",
        "ContainerType": "0",
        "ContainerDescription": "NONE"
      }
    ],
    "query": "SELECT\n      cpu_count AS LogicalCPUCount,\n      hyperthread_ratio AS HyperthreadRatio,\n      physical_memory_kb/1024.0 AS PhysicalMemoryMB,\n      sqlserver_start_time AS SQLServerStartTime,\n      affinity_type_desc AS AffinityTypeDesc,\n      virtual_machine_type AS VirtualMachineType,\n      socket_count as SocketCount,\n      cores_per_socket as CoresPerSocket,\n      numa_node_count as NumaNodeCount,\n      container_type as ContainerType,\n      container_type_desc as ContainerDescription\n      FROM sys.dm_os_sys_info WITH(nolock)",
    "queryTimeInTicks": 0
  }
]
```
## <a name="frequently-asked-questions"></a>Forum aux questions

**Comment les DBA lisent-ils les fichiers journaux d’audit local ?**
Ces fichiers journaux sont écrits au format JSON. Chaque ligne est un objet JSON qui représente un élément de données d’utilisation/de diagnostic chargé vers Microsoft. Les noms de champs doivent être explicites.

**Que se passe-t-il si le DBA désactive la collecte de données d’utilisation et de diagnostic ?**
Aucun fichier d’audit local ne sera écrit.

**Que se passe-t-il en cas d’absence de connectivité Internet ou de machine derrière le pare-feu ?**
Les données d’utilisation et de diagnostic de SQL Server ne seront pas envoyées à Microsoft. Une tentative d’écriture des journaux d’audit local sera effectuée si la configuration est correcte.

**Comment les DBA désactivent-ils l’audit local ?**
Supprimez l’entrée de clé de Registre UserRequestedLocalAuditDirectory.

**Qui peut lire les fichiers journaux d’audit local ?**
Toute personne de votre organisation ayant accès au répertoire d’audit local.

**Comment les DBA gèrent-ils les fichiers journaux écrits dans le répertoire désigné ?**
Les DBA devront gérer eux-mêmes le nettoyage des fichiers dans le répertoire pour éviter de consommer trop d’espace disque.

**Y a-t-il un client ou un outil que je peux utiliser pour lire cette sortie JSON ?**
La sortie peut être lue avec le Bloc-notes, Visual Studio ou le lecteur JSON de votre choix.
Vous pouvez également lire le fichier JSON et analyser les données dans une instance SQL Server comme illustré ci-dessous. Pour plus d’informations sur la lecture de fichiers JSON dans SQL Server, visitez [Importation de fichiers JSON dans SQL Server à l’aide de OPENROWSET (BULK) et OPENJSON (Transact-SQL)](/archive/blogs/sqlserverstorageengine/bulk-importing-json-files-into-sql-server).

```Transact-SQL
DECLARE @JSONFile AS VARCHAR(MAX)

-- Read the JSON file into variable 
SELECT @JSONFile = BulkColumn 
FROM OPENROWSET (BULK 'C:\SQLCEIPAudit\MSSQLSERVER\2016-09-08.json', SINGLE_CLOB) MyFile 

-- Check if the JSON file has been read properly and if it's in a JSON format
SELECT 
    @JSONFile LocalAuditOutput, 
    ISJSON(@JSONFile) IsFileInJSONFormat

-- Get the query identifier, query and the data (output of the query)   
SELECT 
    sequence,
    queryIdentifier,
    query,
    data
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON)
-- Get specific details about the output of "DatabaseProperties.001" query  
SELECT 
    QueryIdentifier,
    DatabaseID,
    CompatibilityLevel,
    IsQueryStoreOn
FROM OPENJSON(@JSONFile) 
    WITH (sessionId VARCHAR(64)
         ,sequence INT
         ,queryIdentifier VARCHAR(128)
         ,query VARCHAR(MAX)
         ,data NVARCHAR(MAX) AS JSON) 
    CROSS APPLY OPENJSON(data) 
        WITH (   DatabaseID varchar(128) '$.database_id'
                ,CompatibilityLevel varchar(128) '$.compatibility_level'
                ,IsQueryStoreOn varchar(128) '$.QS'
             )
WHERE queryIdentifier = 'DatabaseProperties.001'
```

## <a name="see-also"></a>Voir aussi
[Audit local pour l’utilisation de SSMS et collecte des données d’utilisation et de diagnostic](../ssms/sql-server-management-studio-telemetry-ssms.md)