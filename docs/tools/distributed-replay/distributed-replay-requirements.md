---
description: Distributed Replay Requirements
title: Configuration requise pour Distributed Replay
titleSuffix: SQL Server Distributed Replay
ms.prod: sql
ms.technology: tools-other
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/18/2018
ms.openlocfilehash: 237a2ca3b7488688162b38ea3e7f916e2a4d3c6d
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836886"
---
# <a name="distributed-replay-requirements"></a>Distributed Replay Requirements

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Avant d’utiliser la fonctionnalité [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Distributed Replay, prenez connaissance des spécifications du produit présentées dans cette rubrique.  

## <a name="input-trace-requirements"></a>Spécifications des données de trace d'entrée  

Pour que les données de trace puissent être correctement relues, elles doivent répondre à des spécifications de version et de format et contenir des événements et des colonnes obligatoires.  

### <a name="input-trace-versions"></a>Versions de trace d'entrée  

Distributed Replay prend en charge les données de trace d'entrée recueillies dans les versions suivantes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:  

- [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]  Mise à jour cumulée 1 et versions ultérieures. Consultez : [SQL Server 2017, mises à jour cumulatives](https://aka.ms/sql2017cu).
- [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]   
- [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
- [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]  
- [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]  
- [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]    
- [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]  
  
### <a name="input-trace-formats"></a>Formats de trace d'entrée

Les données de trace d'entrée peuvent se présenter sous l'un des formats suivants :  

- Un fichier de trace unique ayant l'extension `.trc` .  

- Un jeu de fichiers de trace de substitution qui suivent la convention d’affectation des noms de substitution de fichier, par exemple : `<TraceFile>.trc`, `<TraceFile>_1.trc`, `<TraceFile>_2.trc`, `<TraceFile>_3.trc`, ... `<TraceFile>_n.trc`.

### <a name="input-trace-events-and-columns"></a>Événements et colonnes de trace d'entrée

Les données de trace d'entrée doivent contenir des événements et des colonnes spécifiques pour pouvoir être relues par Distributed Replay. Le modèle **TSQL_Replay** , dans [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , contient tous les événements et toutes les colonnes obligatoires, ainsi que des informations supplémentaires. Pour plus d’informations sur ce modèle, consultez [Conditions préalables à la relecture](../../tools/sql-server-profiler/replay-requirements.md).  
  
> [!WARNING]
> Si vous n’utilisez pas le modèle **TSQL_Replay** pour capturer les données de trace d’entrée, ou si les conditions d’entrée de trace ne sont pas satisfaites, vous pouvez recevoir des résultats de relecture inattendus.  

Vous pouvez également créer un modèle de trace personnalisé et l'utiliser pour relire des événements avec Distributed Replay, à condition que ce modèle contienne les événements suivants :
  
- Audit Login  
  
- Audit Logout  
  
- ExistingConnection  
  
- RPC Output Parameter  
  
- RPC:Completed  
  
- RPC:Starting  
  
- SQL:BatchCompleted  
  
- SQL:BatchStarting  
  
 Si vous relisez des curseurs côté serveur, les événements suivants sont également obligatoires :  
  
- CursorClose  
  
- CursorExecute  
  
- CursorOpen  
  
- CursorPrepare  
  
- CursorUnprepare  
  
 Si vous relisez des instructions SQL préparées côté serveur, les événements suivants sont également obligatoires :  
  
- Exec Prepared SQL  
  
- Prepare SQL  
  
 Toutes les données de trace d'entrée doivent contenir les colonnes suivantes :  
  
- Classe d'événements  
  
- EventSequence  
  
- TextData  
  
- Nom de l’application  
  
- LoginName  
  
- nom_base_de_données  
  
- ID de base de données  
  
- HostName  
  
- Binary Data  
  
- SPID  
  
- Heure de Début  
  
- EndTime  
  
- IsSystem  
  
## <a name="supported-input-trace-and-target-server-combinations"></a>Combinaisons de traces d'entrée et de serveurs cibles prises en charge  

Le tableau suivant répertorie les versions de données de trace prises en charge et, pour chacune d'entre elles, les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prises en charge avec lesquelles les données peuvent être relues.  
  
|Version de données de trace d'entrée|Versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] prises en charge pour l'instance de serveur cible|  
|---------------------------------|------------------------------------------------------------------------------------|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]|[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)], [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)], [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)],[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|  
  
## <a name="operating-system-requirements"></a>Systèmes d'exploitation requis

Les systèmes d'exploitation pris en charge pour exécuter l'outil d'administration et les services contrôleur et clients sont les mêmes que dans votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur les systèmes d’exploitation pris en charge pour votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [Configurations matérielle et logicielle requises pour l’installation de SQL Server 2016](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).  

Les fonctionnalités Distributed Replay sont prises en charge à la fois sur les systèmes d'exploitation basés sur des processeurs x86 et ceux basés sur des processeurs x64. Pour les systèmes d'exploitation basés sur des processeurs x64, seul le mode Windows on Windows (WOW) est pris en charge.  

## <a name="installation-limitations"></a>Limitations d'installation

Un ordinateur ne peut avoir qu'une seule instance de chaque fonctionnalité Distributed Replay installée. Le tableau suivant indique le nombre d'installations autorisées pour chaque fonctionnalité dans un même environnement Distributed Replay.  
  
|Fonctionnalité de Distributed Replay|Nombre maximal d'installations par environnement de relecture|  
|--------------------------------|--------------------------------------------------|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Distributed Replay Controller|1|  
|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Distributed Replay Client|16 (ordinateurs physiques ou virtuels)|  
|Outil d'administration|Illimité|  
  
> [!NOTE]  
>  Bien qu'une seule instance de l'outil d'administration puisse être installée sur un même ordinateur, vous pouvez en démarrer plusieurs instances. Les commandes émises depuis plusieurs outils d'administration sont résolues dans l'ordre de leur réception.  
  
## <a name="data-access-provider"></a>Fournisseur d'accès aux données

 Distributed Replay ne prend en charge que le fournisseur d'accès aux données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ODBC Native Client.  

## <a name="target-server-preparation-requirements"></a>Spécifications requises pour la préparation du serveur cible  

Nous conseillons de placer le serveur cible dans un environnement de test. Pour relire les données de trace avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] différente de celle qui a servi à les enregistrer, assurez-vous que les opérations suivantes ont été effectuées sur le serveur cible :  

- Toutes les connexions et tous les utilisateurs contenus dans les données de trace doivent être présents dans la même base de données sur le serveur cible.  

- Toutes les connexions et tous les utilisateurs présents sur le serveur cible doivent avoir les mêmes autorisations que sur le serveur d'origine.  

- Les ID de base de données sur la cible doivent idéalement être identiques à ceux qui sont sur la source. Si ce n’est pas le cas, la mise en correspondance peut être effectuée sur la base du **DatabaseName** s’il est présent dans la trace.

- La base de données par défaut de chaque connexion contenue dans les données de trace doit être définie (sur le serveur cible) en tant que base de données cible relative à la connexion. Par exemple, les données de trace à relire contiennent les activités de la connexion **Fred** dans la base de données **Fred_Db** située sur l’instance d’origine de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Par conséquent, sur le serveur cible, la base de données par défaut de la connexion **Fred** doit être la base de données correspondant à **Fred_Db** (même si le nom de la base de données est différent). Pour définir la base de données par défaut de la connexion, utilisez la procédure stockée système `sp_defaultdb` .  
  
 La relecture d'événements associés à des connexions manquantes ou incorrectes va entraîner des erreurs de relecture, mais l'opération de relecture va se poursuivre.  
  
## <a name="see-also"></a>Voir aussi

- [SQL Server Distributed Replay](../../tools/distributed-replay/sql-server-distributed-replay.md)
- [Sécurité de Distributed Replay](../../tools/distributed-replay/distributed-replay-security.md)
- [Install Distributed Replay - Présentation](../../tools/distributed-replay/install-distributed-replay-overview.md)