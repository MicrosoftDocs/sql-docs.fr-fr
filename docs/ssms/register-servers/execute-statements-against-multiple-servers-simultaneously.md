---
description: Exécuter des instructions sur plusieurs serveurs simultanément
title: Exécuter des instructions sur plusieurs serveurs simultanément
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- multiserver queries
- executing queries against multiple servers
- queries [SQL Server], multiserver
ms.assetid: 197760f3-0a06-43de-8162-69c27d3fbe56
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/18/2016
ms.openlocfilehash: 1c5de69d2a4167d03dcc5ea5c34ed6f0262c4678
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250413"
---
# <a name="execute-statements-against-multiple-servers-simultaneously"></a>Exécuter des instructions sur plusieurs serveurs simultanément

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment interroger simultanément plusieurs serveurs dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]en créant un groupe de serveurs locaux, ou un serveur de gestion centralisée et un ou plusieurs groupes de serveurs, et un ou plusieurs serveurs inscrits dans les groupes, puis en interrogeant le groupe complet. 

Les résultats retournés par la requête peuvent être combinés dans un volet de résultats unique ou retournés dans des volets de résultats distincts. Le jeu de résultats peut inclure des colonnes supplémentaires pour le nom du serveur et la connexion utilisée par la requête sur chaque serveur. Les serveurs de gestion centralisée et les serveurs subordonnés peuvent être inscrits uniquement à l'aide de l'authentification Windows. Les serveurs dans les groupes de serveurs locaux peuvent être inscrits à l'aide de l'authentification Windows ou de l'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> **REMARQUE !** Avant d’appliquer les procédures suivantes, créez un serveur de gestion centralisée et un groupe de serveurs. Pour plus d’informations, consultez [Créer un serveur d’administration centralisée et un groupe de serveurs &#40;SQL Server Management Studio&#41;](./create-a-central-management-server-and-server-group.md).  

  
##  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Dans la mesure où les connexions gérées par un serveur de gestion centralisée s'exécutent dans le contexte de l'utilisateur, avec l'authentification Windows, les autorisations effectives sur les serveurs inscrits peuvent varier. Par exemple, l'utilisateur peut être membre du rôle serveur fixe sysadmin sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] A, mais disposer d'autorisations limitées sur l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] B.  
  
 ## <a name="execute-statements-against-multiple-configuration-targets-simultaneously"></a>Exécuter des instructions contre plusieurs cibles de configuration simultanément  

1.  Dans SQL Server Management Studio, dans le menu **Affichage** , cliquez sur **Serveurs inscrits**.  
  
2.  Développez un serveur de gestion centralisée, cliquez avec le bouton droit sur un groupe de serveurs, pointez sur **Se connecter**, puis cliquez sur **Nouvelle requête**.  
  
3.  Dans l'Éditeur de requête, tapez et exécutez une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] , telle que la suivante :  
  
    ```  
    USE master  
    GO  
    SELECT * FROM sysdatabases;  
    GO  
    ```  
  
     Par défaut, le volet de résultats combine les résultats de la requête à partir de tous les serveurs appartenant au groupe de serveurs.  
  
#### <a name="to-change-the-multiserver-results-options"></a>Pour modifier les options de résultats multiserveurs  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], dans le menu **Outils** , cliquez sur **Options**.  
  
2.  Développez **Résultats de la requête**, **SQL Server**, puis cliquez sur **Résultats multiserveurs**.  
  
3.  Dans la page **Résultats multiserveurs** , spécifiez les paramètres d'options souhaités, puis cliquez sur **OK**.  
  
## <a name="see-also"></a>Voir aussi  
 [Administrer plusieurs serveurs à l’aide de serveurs de gestion centralisée](../../relational-databases/administer-multiple-servers-using-central-management-servers.md)  
  
