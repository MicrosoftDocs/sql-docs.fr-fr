---
description: MSSQLSERVER_1418
title: MSSQLSERVER_1418 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 1418 (Database Engine error)
ms.assetid: 6e9c7241-0201-44e0-9f8b-b3c4e293f0f6
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: cb8cff51e4b33f2d5aeb99c030073544e195b11d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197009"
---
# <a name="mssqlserver_1418"></a>MSSQLSERVER_1418
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|ID de l’événement|1418|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBM_PARTNERNOTFOUND|  
|Texte du message|L'adresse réseau du serveur « %.*ls » est impossible à atteindre ou elle n'existe pas. Vérifiez le nom de l'adresse réseau et que les ports des points de terminaison locaux et distants sont opérationnels.|  
  
## <a name="explanation"></a>Explication  
Le point de terminaison réseau du serveur n'a pas répondu car l'adresse réseau spécifiée pour le serveur ne peut pas être atteinte ou n'existe pas.  
  
> [!NOTE]  
> Par défaut, le système d'exploitation de [!INCLUDE[msCoName](../../includes/msconame-md.md)] bloque tous les ports.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Vérifiez le nom de l'adresse réseau et exécutez de nouveau la commande.  
  
Il peut être nécessaire d'appliquer des actions correctives sur les deux serveurs partenaires. Par exemple, si ce message apparaît lorsque vous essayez d'exécuter une instruction SET PARTNER sur l'instance du serveur principal, il peut impliquer que vous ne devez effectuer une action corrective que sur l'instance du serveur miroir. Toutefois, des actions correctives peuvent être requises sur les deux serveurs partenaires.  
  
### <a name="additional-corrective-actions"></a>Actions correctives supplémentaires  
  
-   Assurez-vous que la base de données miroir est prête pour la mise en miroir.  
  
-   Assurez-vous que le nom et le port de l'instance du serveur miroir sont corrects.  
  
-   Assurez-vous que l'instance du serveur miroir de destination ne se trouve pas derrière un pare-feu.  
  
-   Assurez-vous que l'instance du serveur principal ne se trouve pas derrière un pare-feu.  
  
-   Vérifiez que les points de terminaison sont démarrés sur les serveurs partenaires à l’aide de la colonne **state** ou **state_desc** de l’affichage catalogue **sys.database_mirroring_endpoints**. Si l'un des points de terminaison n'est pas démarré, exécutez une instruction ALTER ENDPOINT pour le démarrer.  
  
-   Assurez-vous que l'instance du serveur principal est à l'écoute sur le port attribué à son point de terminaison de mise en miroir de bases de données et que l'instance du serveur miroir est à l'écoute sur son port. Pour plus d'informations, consultez « Vérification de la disponibilité des ports », plus loin dans cette rubrique. Si un serveur partenaire n'est pas à l'écoute sur le port qui lui est attribué, modifiez le point de terminaison de mise en miroir de bases de données pour qu'il soit à l'écoute sur un autre port.  
  
    > [!IMPORTANT]  
    > Une sécurité configurée de manière incorrecte peut générer un message d'erreur général sur la configuration. En règle générale, l'instance de serveur supprime la demande de connexion incorrecte sans répondre. Pour l'appelant, une erreur de sécurité/configuration peut sembler due à plusieurs autres facteurs : la base de données miroir peut être dans un état incorrect ou peut ne pas exister, les autorisations peuvent être incorrectes, etc.  
  
### <a name="using-the-error-log-file-for-diagnosis"></a>Utilisation du fichier journal des erreurs pour le diagnostic  
Dans certains cas, seuls les fichiers journaux des erreurs sont disponibles pour une analyse approfondie. Si c'est le cas, déterminez si le journal des erreurs contient le message d'erreur 26023 pour le port TCP du point de terminaison de mise en miroir de bases de données. Cette erreur de niveau de gravité 16 peut indiquer que le point de terminaison de mise en miroir de bases de données n'est pas démarré. Ce message peut apparaître même si l’affichage catalogue **sys.database_mirroring_endpoints** indique que le point de terminaison est démarré.  
  
Après avoir résolu les problèmes rencontrés, exécutez de nouveau l’instruction ALTER DATABASE *nom_base_de_données* SET PARTNER sur le serveur principal.  
  
### <a name="verifying-port-availability"></a>Vérification de la disponibilité des ports  
Lors de la configuration du réseau pour une session de mise en miroir de bases de données, assurez-vous que le point de terminaison de mise en miroir de bases de données de chaque instance de serveur est utilisé uniquement par le processus de mise en miroir de bases de données. Si un autre processus est à l'écoute sur le port attribué à un point de terminaison de mise en miroir de bases de données, les processus de mise en miroir de bases de données des autres instances de serveur ne peuvent pas se connecter au point de terminaison.  
  
Pour afficher tous les ports sur lesquels un serveur Windows est à l’écoute, utilisez l’utilitaire d’invite de commandes **netstat**. La syntaxe à utiliser pour **netstat** dépend de la version du système d’exploitation Windows. Pour plus d'informations, consultez la documentation du système d'exploitation.  
  
#### <a name="windows-server-2003-service-pack-1-sp1"></a>Windows Server 2003 Service Pack 1 (SP1)  
Pour répertorier les ports d'écoute et les processus pour lesquels ces ports sont ouverts, entrez la commande ci-dessous à l'invite de commandes Windows :  
  
**netstat -abn**  
  
#### <a name="windows-server-2003-pre-sp1"></a>Windows Server 2003 (version antérieure à SP1)  
Pour identifier les ports d'écoute et les processus pour lesquels ces ports sont ouverts, procédez comme suit :  
  
1.  Obtenez l'ID de processus.  
  
    Pour connaître l'ID de processus d'une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], connectez-vous à cette instance et utilisez l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante :  
  
    ```  
    SELECT SERVERPROPERTY('ProcessID')   
    ```  
  
    Pour plus d'informations, consultez « SERVERPROPERTY (Transact-SQL) » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
2.  Mettez en corrélation l’ID de processus avec la sortie de la commande **netstat** suivante :  
  
    **netstat -ano**  
  
## <a name="see-also"></a>Voir aussi  
[ALTER ENDPOINT &#40;Transact-SQL&#41;](~/t-sql/statements/alter-endpoint-transact-sql.md)  
[Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
[Préparer une base de données miroir pour la mise en miroir &#40;SQL Server&#41;](~/database-engine/database-mirroring/prepare-a-mirror-database-for-mirroring-sql-server.md)  
[SERVERPROPERTY &#40;Transact-SQL&#41;](~/t-sql/functions/serverproperty-transact-sql.md)  
[Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](~/database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](~/relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
[Résoudre des problèmes de configuration de mise en miroir de bases de données &#40;SQL Server&#41;](~/database-engine/database-mirroring/troubleshoot-database-mirroring-configuration-sql-server.md)  
  
