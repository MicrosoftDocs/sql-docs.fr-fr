---
title: Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP | Microsoft Docs
description: Familiarisez-vous avec les points de terminaison tabular data stream (TDS). Découvrez comment les utiliser pour configurer le moteur de base de données SQL Server sur plusieurs ports TCP.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- ports [SQL Server], multiple
- TDS
- listen on multiple ports
- endpoints [SQL Server], TDS
- adding ports
- tabular data stream
- multiple ports
ms.assetid: 8e955033-06ef-403f-b813-3d8241b62f1f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 28c73233d8a7b38ec2d14fa92c40f69d9ae0af05
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783278"
---
# <a name="configure-the-database-engine-to-listen-on-multiple-tcp-ports"></a>Configurer le moteur de base de données de manière à écouter sur plusieurs ports TCP
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment configurer le [!INCLUDE[ssDE](../../includes/ssde-md.md)] pour écouter sur plusieurs ports TCP dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide du Gestionnaire de configuration SQL Server. Lorsque TCP/IP est activé pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le [!INCLUDE[ssDE](../../includes/ssde-md.md)] écoute les connexions entrantes sur un point de connexion composé d'une adresse IP et d'un numéro de port TCP. Les procédures suivantes créent un point de terminaison TDS (Tabular Data Stream, flux de données tabulaires), afin que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] écoute sur un port TCP supplémentaire.  
  
 Les raisons pouvant conduire à créer un second point de terminaison TDS sont les suivantes :  
  
-   Renforcement de la sécurité en configurant le pare-feu de manière à restreindre l'accès au point de terminaison par défaut aux ordinateurs clients locaux appartenant à un sous-réseau spécifique. Conservez l'accès Internet à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l'équipe de support technique en créant un nouveau point de terminaison que le pare-feu expose à Internet et en limitant les droits de connexion à ce point de terminaison à l'équipe de support technique.  
  
-   Affinage des connexions sur des processeurs spécifiques lors de l'utilisation d'un accès NUMA (Non-Uniform Memory Access).  
  
 La configuration d'un point de terminaison TDS comprend les étapes suivantes, que vous pouvez réaliser dans l'ordre de votre choix :  
  
-   Créer le point de terminaison TDS du port TCP et, le cas échéant, restaurer l'accès au point de terminaison par défaut.  
  
-   Accorder l'accès au point de terminaison aux principaux de serveur souhaités.  
  
-   Spécifier le numéro de port TCP de l'adresse IP sélectionnée.  
  
 Pour plus d’informations sur les paramètres par défaut du Pare-feu Windows et pour obtenir une description des ports TCP qui affectent le moteur de base de données, Analysis Services, Reporting Services et Integration Services, consultez [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
##  <a name="SSMSProcedure"></a>  
  
#### <a name="to-create-a-tds-endpoint"></a>Pour créer un point de terminaison TDS  
  
-   Émettez l'instruction suivante afin de créer un point de terminaison nommé **CustomConnection** pour le port 1500 pour toutes les adresses TCP disponibles sur le serveur.  
  
    ```  
    USE master;  
    GO  
    CREATE ENDPOINT [CustomConnection]  
    STATE = STARTED  
    AS TCP  
       (LISTENER_PORT = 1500, LISTENER_IP =ALL)  
    FOR TSQL() ;  
    GO  
    ```  
  
 Lorsque vous créez un nouveau point de terminaison [!INCLUDE[tsql](../../includes/tsql-md.md)] , les autorisations de connexion de **public** sont révoquées pour le point de terminaison TDS par défaut. Si l'accès au groupe **public** est nécessaire pour le point de terminaison par défaut, réappliquez cette autorisation à l'aide de l'instruction `GRANT CONNECT ON ENDPOINT::[TSQL Default TCP] to [public];` .  
  
#### <a name="to-grant-access-to-the-endpoint"></a>Pour accorder l'accès au point de terminaison  
  
-   Émettez l'instruction suivante afin d'accorder l'accès au point de terminaison **CustomConnection** au groupe SQLSupport du domaine corp.  
  
    ```  
    GRANT CONNECT ON ENDPOINT::[CustomConnection] to [corp\SQLSupport] ;  
    GO  
    ```  
  
#### <a name="to-configure-the-sql-server-database-engine-to-listen-on-an-additional-tcp-port"></a>Pour configurer le moteur de base de données SQL Server de manière à écouter sur un port TCP supplémentaire  
  
1.  Dans le Gestionnaire de configuration SQL Server, développez **Configuration du réseau SQL Server**, puis cliquez sur **Protocoles pour** _<nom_instance>_ .  
  
2.  Développez **Protocoles pour** _<nom_instance>_ , puis cliquez sur **TCP/IP**.  
  
3.  Dans le volet droit, cliquez avec le bouton droit sur chaque adresse IP désactivée à activer, puis cliquez sur **Activer**.  
  
4.  Cliquez avec le bouton droit sur **IPAll**, puis cliquez sur **Propriétés**.  
  
5.  Dans la zone **Port TCP** , tapez les ports sur lesquels le [!INCLUDE[ssDE](../../includes/ssde-md.md)] doit écouter, en les séparant par des virgules. Dans notre exemple, si le port par défaut 1433 est répertorié, tapez **,1500** afin que la zone affiche **1433,1500**, puis cliquez sur **OK**.  
  
    > [!NOTE]  
    >  Si vous n'activez pas le port sur toutes les adresses IP, configurez le port supplémentaire dans la zone des propriétés pour uniquement l'adresse de votre choix. Ensuite, dans le volet de la console, cliquez avec le bouton droit sur **TCP/IP**, cliquez sur **Propriétés** puis, dans la zone **Écouter tout** , sélectionnez **Non**.  
  
6.  Dans le volet gauche, cliquez sur **Services SQL Server**.  
  
7.  Dans le volet droit, cliquez avec le bouton droit sur **<nom_instance>** _SQL Server_, puis cliquez sur **Redémarrer**.  
  
     Lorsque le [!INCLUDE[ssDE](../../includes/ssde-md.md)] redémarre, le journal des erreurs répertorie les ports sur lesquels [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est à l'écoute.  
  
#### <a name="to-connect-to-the-new-endpoint"></a>Pour établir la connexion au nouveau point de terminaison  
  
-   Émettez l’instruction suivante pour vous connecter au point de terminaison **CustomConnection** de l’instance par défaut de SQL Server sur le serveur nommé ACCT, à l’aide d’une connexion approuvée et en supposant que l’utilisateur est membre du groupe [corp\SQLSupport].  
  
    ```  
    sqlcmd -SACCT,1500  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [DROP ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/drop-endpoint-transact-sql.md)   
 [Autorisations GRANT sur point de terminaison &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md)   
 [Mapper les ports TCP/IP aux nœuds NUMA &#40;SQL Server&#41;](../../database-engine/configure-windows/map-tcp-ip-ports-to-numa-nodes-sql-server.md)  
  
  
