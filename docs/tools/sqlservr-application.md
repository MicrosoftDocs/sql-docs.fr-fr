---
title: Application sqlservr
description: L’application sqlservr démarre, arrête, suspend et poursuit une instance de SQL Server à partir d’une invite de commandes.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], sqlservr
- command prompt [SQL Server], pausing/resuming instance of SQL Server
- starting instance of SQL Server
- command prompt [SQL Server], continuing instance of SQL Server
- sqlservr utility
- pausing instance of SQL Server
- stopping instance of SQL Server
- resuming SQL Server
- command prompt [SQL Server], stopping instance of SQL Server
- command prompt [SQL Server], starting instance of SQL Server
- continuing instance of SQL Server
ms.assetid: 60e8ef0a-0851-41cf-a6d8-cca1e04cbcdb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 9422651db34ab9c423a960b9d945d1c748f25d93
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99075951"
---
# <a name="sqlservr-application"></a>Application sqlservr

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

L’application **sqlservr** démarre, arrête, suspend et poursuit une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à partir d’une invite de commandes.

## <a name="syntax"></a>Syntaxe

```cmd
sqlservr [-s instance_name] [-c] [-d master_path] [-f] 
     [-e error_log_path] [-l master_log_path] [-m]
     [-n] [-T trace#] [-v] [-x]
```

## <a name="arguments"></a>Arguments

**-s** *instance_name* Spécifie l'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à laquelle établir une connexion. Si aucune instance nommée n’est spécifiée, **sqlservr** démarre l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

> [!IMPORTANT]
>Lorsque vous démarrez une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vous devez utiliser l’application **sqlservr** dans le répertoire approprié de cette instance. Si vous utilisez l’instance par défaut, exécutez **sqlservr** depuis le répertoire \MSSQL\Binn. Si vous utilisez l’instance nommée, exécutez **sqlservr** depuis le répertoire \MSSQL$*nom_instance*\Binn.

 **-c** Indique qu'une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est démarrée indépendamment du Gestionnaire de contrôle des services Windows. En cas de démarrage de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à partir d’une invite de commandes, cette option réduit le délai de démarrage de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

> [!NOTE]
>Avec cette option, vous ne pouvez pas arrêter [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant le Gestionnaire des services [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou la commande **net stop** . De plus, si vous vous déconnectez de l’ordinateur, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est arrêté.

**-d** *master_path* indique le chemin complet du fichier de base de données **master**. Il n’y a pas d’espace entre **-d** et *chemin_master*. Si vous ne spécifiez pas cette option, les paramètres du Registre existant sont utilisés.

**-f** Démarre une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec une configuration minimale. Cette option est utile lorsqu'une valeur de configuration définie (espace mémoire insuffisant, par exemple) a empêché le serveur de démarrer.

**-e** *error_log_path* Indique le chemin complet au fichier journal des erreurs. S’il n’est pas spécifié, l’emplacement par défaut est `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL\Log\Errorlog` pour l'instance par défaut et `*\<Drive>*:\Program Files\Microsoft SQL Server\MSSQL$*instance_name*\Log\Errorlog` pour une instance nommée. Il n’existe aucun espace entre **-e** et *chemin_du_journal_des_erreurs*.

**-l** *master_log_path* Indique le chemin complet du fichier journal des transactions de la base de données **master**. Il n’existe aucun espace entre **-l** et *chemin_du_journal_master*.

**-m** Spécifie le démarrage d'une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en mode mono-utilisateur. Dans ce mode, un seul utilisateur peut se connecter au démarrage de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Le mécanisme CHECKPOINT (qui garantit le transfert régulier des transactions terminées du cache disque vers l'unité de bases de données) n'est pas lancé. Cette option est généralement utilisée en cas de problème au niveau de bases de données système requérant une réparation. Active l’option **sp_configure allow updates**. Par défaut, l'option **allow updates** est désactivée.

**-n** Permet de démarrer une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Si le paramètre **-s** n’est pas spécifié, l’instance par défaut tente de démarrer. Vous devez accéder au répertoire BINN de l’instance, dans l’invite de commandes, avant de démarrer **sqlservr.exe**. Par exemple, si Instance1 doit utiliser \mssql$Instance1 pour ses fichiers binaires, l’utilisateur doit être dans le répertoire \mssql$Instance1\binn pour démarrer **sqlservr.exe -s instance1**. Si vous démarrez une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec l’option **-n** , il est également recommandé d’utiliser l’option **-e** , sinon les événements [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne sont pas consignés.

**-T** *trace#* Indique qu’une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] doit être démarrée avec un indicateur de trace spécifique (*trace#* ) en vigueur. Les indicateurs de trace permettent de démarrer le serveur avec un comportement non standard. Pour plus d’informations, consultez [Indicateurs de trace &#40;Transact-SQL&#41;](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

>[!IMPORTANT]
>Lorsque vous spécifiez un indicateur de trace, utilisez **-T** pour transmettre le numéro d’indicateur de trace. **accepte un t minuscule (** -t [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), mais **-t** définit d’autres indicateurs de trace internes requis par les ingénieurs du support technique de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

**-v** Affiche le numéro de version du serveur.

**-x** Désactive le suivi des statistiques temps UC et taux d'accès au cache. Optimise les performances au maximum.

## <a name="remarks"></a>Notes
Dans la plupart des cas, le programme sqlservr.exe est uniquement utilisé pour le dépannage ou pour une maintenance majeure. Lorsque [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] est démarré à partir de l’invite de commandes avec sqlservr.exe, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ne démarre pas en tant que service et vous ne pouvez donc pas arrêter [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec des commandes **net** . Les utilisateurs peuvent se connecter à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], mais les outils [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] montrent l'état du service et le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] indique correctement que le service est arrêté. [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] peut se connecter au serveur, mais indique également que le service est arrêté.

## <a name="compatibility-support"></a>Prise en charge de la compatibilité
Les paramètres suivants sont obsolètes et ne sont pas pris en charge dans [!INCLUDE[sssql19-md](../includes/sssql19-md.md)].

|Paramètre | Informations complémentaires|
|:-----|:-----|
|**-h** | Utilis dans les versions antérieures des instances 32 bits de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour réserver l'espace d'adressage de mémoire virtuelle pour les métadonnées d'ajout de mémoire à chaud lorsque AWE est activé. Prise en charge via [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Pour plus d’informations, consultez [Fonctionnalités SQL Server supprimées dans SQL Server 2016](../database-engine/discontinued-database-engine-functionality-in-sql-server.md).|
|**-g** | *memory_to_reserve*<br/><br>S’applique aux versions antérieures des instances 32 bits de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Prise en charge via [!INCLUDE[sssql14](../includes/sssql14-md.md)]. Spécifie un nombre entier de mégaoctets (Mo) de mémoire que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] laisse disponible pour des allocations de mémoire à l'intérieur du processus de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , mais hors du pool de mémoire de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour en savoir plus, consultez la [documentation 2014 SQL Server sur les options de configuration de la mémoire du serveur](/previous-versions/sql/2014/database-engine/configure-windows/server-memory-server-configuration-options?view=sql-server-2014&preserve-view=true).|
| &nbsp; | &nbsp; |

## <a name="see-also"></a>Voir aussi
 [Options de démarrage du service moteur de base de données](../database-engine/configure-windows/database-engine-service-startup-options.md)
