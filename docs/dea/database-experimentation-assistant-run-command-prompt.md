---
title: Exécuter Assistant Expérimentation de base de données à partir d’une invite de commandes
description: Découvrez comment capturer une trace dans Assistant Expérimentation de base de données (DEA), puis analyser les résultats, le tout à partir d’une invite de commandes.
ms.custom: seo-lt-2019
ms.date: 02/25/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: 0e52cf31a94f2fd63f92305b80f7f7ae049c2809
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066444"
---
# <a name="run-database-experimentation-assistant-at-a-command-prompt"></a>Exécuter Assistant Expérimentation de base de données à partir d’une invite de commandes

Cet article explique comment capturer une trace dans Assistant Expérimentation de base de données (DEA), puis analyser les résultats, le tout à partir d’une invite de commandes.

   > [!NOTE]
   > Pour en savoir plus sur chaque opération de DEA, essayez d’exécuter la commande suivante :
   >
   > `Deacmd.exe -o <operation> --help`
   >
   > Un nom d’opération est requis ; les opérations valides sont **Analysis**, **StartCapture** et **StopCapture**.

## <a name="start-a-new-workload-capture-by-using-the-dea-command"></a>Démarrer une nouvelle capture de la charge de travail à l’aide de la commande DEA

Pour démarrer une nouvelle capture de la charge de travail, à l’invite de commandes, exécutez la commande suivante :

`Deacmd.exe -o StartCapture -n <Trace FileName> -x <Trace Format> -h <SQLServerInstance> -f <database name> -e <Encrypt Connection> -m <Authetication Mode> -u <user name> -p <password> -l <Location of Output Folder> -d <duration>`

**Exemple**

`Deacmd.exe -o StartCapture -n sql2008capture -x 0 -h localhost -f adventureworks -e --trust -m 0 -l c:\test  -d 60`

**Options supplémentaires**

Lors du démarrage d’une nouvelle capture de la charge de travail avec la `Deacmd.exe` commande, vous pouvez utiliser les options supplémentaires suivantes :

| Option| Description |  
| --- | --- |
| -n, --name | Souhaitée nom du fichier de trace |
| -x,--format | Souhaitée format de la trace (trace = 0, XEvents = 1) |
| -d,--Duration | Souhaitée durée maximale de la capture, en minutes |
| -l, --location | Souhaitée emplacement du dossier de sortie pour le stockage des fichiers trace/XEvent sur l’ordinateur hôte |
| -t,--type | (Valeur par défaut : 0) type/édition de SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Managed Instance = 2) |
| -h, --host | Souhaitée SQL Server nom d’hôte et/ou nom d’instance pour démarrer la capture |
| -e,--Encrypt | (Valeur par défaut : true) Chiffrer la connexion à SQL Server instance. La valeur par défaut est true |
| --confiance | (Valeur par défaut : false) Faire confiance au certificat de serveur lors de la connexion à SQL Server instance. La valeur par défaut est false |
| -f,--DatabaseName | (Par défaut :) Nom de la base de données pour filtrer vos suivis, si elle n’est pas spécifiée, la capture démarre sur toutes les bases de données |
| -m,--authmode | (Valeur par défaut : 0) mode d’authentification (Windows = 0, authentification SQL = 1) |
| -u,--username | Nom d’utilisateur pour la connexion au SQL Server |
| -p,--mot de passe | Mot de passe pour la connexion au SQL Server |

## <a name="replay-a-workload-capture"></a>Relire une capture de la charge de travail

**Utilisation de Distributed Replay**

Si vous utilisez Distributed Replay, procédez comme suit.

1. Connectez-vous à l’ordinateur du contrôleur de Distributed Replay.
2. Pour convertir la trace de charge de travail que vous avez capturée à l’aide de la commande de DEA dans un fichier IRF, à l’invite de commandes, exécutez la commande suivante :

    `DReplay preprocess -m "dreplaycontroller" -i "Path to first trace file" -d "<Folder path on controller>\IrfFolder"`

3. Démarrez une capture de trace sur l’ordinateur cible qui exécute SQL Server à l’aide de StartReplayCaptureTrace. Sql.

    a.  Dans SQL Server Management Studio (SSMS), ouvrez <Dea_InstallPath \> \Scripts\StartReplayCaptureTrace.Sql.

    b.  Exécutez `Set @durationInMins=0` afin que la capture de trace ne s’arrête pas automatiquement après une heure spécifiée.

    c.  Pour définir la taille de fichier maximale par fichier de trace, exécutez `Set @maxfilesize` . La taille recommandée est de 200 (en Mo).

    d.  Modifiez `@Tracefile` pour définir un nom unique pour votre fichier de trace.

    e.  Modifiez `@dbname` pour spécifier un nom de base de données si la charge de travail doit être capturée uniquement sur une base de données spécifique. Par défaut, la charge de travail sur l’ensemble du serveur est capturée.

4. Pour relire le fichier IRF sur l’instance de SQL Server cible, à l’invite de commandes, exécutez la commande suivante :

    `DReplay replay -m "dreplaycontroller" -d "<Folder Path on Dreplay Controller>\IrfFolder" -o -s "SQL2016Target" -w "dreplaychild1,dreplaychild2,dreplaycild3,dreplaychild4"`

    a.  Pour surveiller l’État, à l’invite de commandes, exécutez `DReplay status -f 1` .

    b.  Pour arrêter la relecture, par exemple si vous constatez que le% de réussite est inférieur à la valeur attendue, à l’invite de commandes, exécutez `DReplay cancel` .

5. Arrêtez la capture de trace sur l’instance de SQL Server cible.
6. Dans SSMS, ouvrez `<Dea_InstallPath>\Scripts\StopCaptureTrace.sql` .
7. Modifiez `@Tracefile` pour qu’il corresponde au chemin d’accès du fichier de trace sur l’ordinateur cible qui exécute SQL Server.
8. Exécutez le script sur l’ordinateur cible qui exécute SQL Server.

**Utilisation de la relecture intégrée**

Si vous utilisez la relecture intégrée, vous n’êtes pas obligé de configurer Distributed Replay. La possibilité d’utiliser la relecture intégrée via la ligne de commande est en cours d’utilisation, mais dans l’intervalle, vous pouvez utiliser notre interface utilisateur graphique pour exécuter la relecture à l’aide de la relecture intégrée.

## <a name="analyze-traces-using-the-dea-command"></a>Analyser les traces à l’aide de la commande DEA

Pour démarrer une nouvelle analyse de trace, à l’invite de commandes, exécutez la commande suivante :

`Deacmd.exe -o analysis -a <Target1 trace filepath> -b <Target2 trace filepath> -r reportname -h <SQLserverInstance> -e <encryptconnection> -u <username>`

**Exemple**

`Deacmd.exe -o analysis -a C:\Trace\SQL2008Source\Trace.trc -b C:\ Trace\SQL2014Trace\Trace.trc -r upgrade20082014 -h localhost -e`

Pour afficher les rapports d’analyse de ces fichiers de trace, vous devez utiliser l’interface graphique utilisateur pour afficher des graphiques et des mesures organisées.  Toutefois, la base de données d’analyse est écrite dans l’instance SQL Server spécifiée, ce qui vous permet également d’interroger directement les tables d’analyse générées.

**Options supplémentaires**

Lorsque vous analysez des traces à l’aide de la commande DEA, vous pouvez utiliser les options supplémentaires suivantes :

| Option| Description |  
| --- | --- |
| -a,--tracea | Souhaitée chemin d’accès au fichier d’événements pour une instance. Exemple de C:\traces\Sql2008trace.trc.  S’il existe un lot de fichiers, sélectionnez le premier fichier, et DEA recherche automatiquement les fichiers de substitution. Si les fichiers sont dans un objet BLOB, indiquez le chemin d’accès au dossier où vous souhaitez stocker les fichiers d’événements localement.  Exemple de C:\traces\ |
| -b,--traceB | Souhaitée chemin d’accès au fichier d’événements pour l’instance B. Exemple de C:\traces\Sql2014trace.trc. S’il existe un lot de fichiers, sélectionnez le premier fichier, et DEA recherche automatiquement les fichiers de substitution. Si les fichiers sont dans un objet BLOB, indiquez le chemin d’accès au dossier où vous souhaitez stocker les fichiers d’événements localement.  Exemple de C:\traces\ |
| -r,--ReportName | Souhaitée nom de l’analyse actuelle. Le rapport d’analyse qui est généré est identifié par ce nom. |
| -t,--type | (Valeur par défaut : 0) type/édition de SQL Server (SqlServer = 0, AzureSQLDB = 1, Azure SQL Managed Instance = 2) |
| -h, --host | Souhaitée Nom d’hôte et/ou nom d’instance SQL Server |
| -e,--Encrypt | (Valeur par défaut : true) Chiffrer la connexion à SQL Server instance. La valeur par défaut est true |
| --confiance | (Valeur par défaut : false) Faire confiance au certificat de serveur lors de la connexion à SQL Server instance. La valeur par défaut est false |
| -m,--authmode | (Valeur par défaut : 0) mode d’authentification (Windows = 0, authentification SQL = 1) |
| -u,--username | Nom d’utilisateur pour la connexion au SQL Server |
| --p | Mot de passe pour la connexion au SQL Server |
| --AB | (Valeur par défaut : false) L’emplacement de stockage de trace A est dans l’objet BLOB. S’il est utilisé, doit également spécifier--Abu (suivi d’une URL d’objet BLOB) |
| --BB | (Valeur par défaut : false) L’emplacement de stockage de la trace B est dans l’objet BLOB. S’il est utilisé, doit également spécifier--les batteries de secours (URL de l’objet blob de trace B) |
| --Abu | URL de l’objet BLOB pour une instance avec la clé SAS |
| --les blocs-secours | URL de l’objet BLOB pour l’instance B avec la clé SAS |

## <a name="see-also"></a>Voir également

- Pour plus d’informations sur l’utilisation de DEA, consultez [vue d’ensemble des Assistant expérimentation de base de données](database-experimentation-assistant-overview.md).
