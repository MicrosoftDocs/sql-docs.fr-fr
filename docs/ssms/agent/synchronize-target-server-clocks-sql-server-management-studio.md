---
description: Synchroniser les horloges des serveurs cibles
title: Synchroniser les horloges des serveurs cibles
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, target servers
- clocks [SQL Server]
- master servers [SQL Server], clock synchronization
- synchronization [SQL Server], target server clocks
- target servers [SQL Server], clock synchronization
ms.assetid: 4fb80502-d271-4d06-bcbc-bfbbceb5f2a2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: ebce7874aa7f21066d4424bff61f6f588dab1e0b
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250975"
---
# <a name="synchronize-target-server-clocks"></a>Synchroniser les horloges des serveurs cibles

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique décrit la procédure de synchronisation des horloges des serveurs cibles dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] avec celle du serveur maître à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. La synchronisation de ces horloges système gère vos planifications de travail.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Nécessite l'appartenance au rôle serveur fixe **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-synchronize-target-server-clocks"></a>Pour synchroniser les horloges des serveurs cibles  
  
1.  Dans l' **Explorateur d'objets** , cliquez sur le signe plus (+) pour développer le serveur sur lequel vous souhaitez synchroniser les horloges des serveurs cibles avec celle du serveur maître.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent**, pointez sur **Administration multiserveur**, puis sélectionnez **Gérer les serveurs cibles**.  
  
3.  Dans la boîte de dialogue **Gérer les serveurs cibles** , cliquez sur **Publier les instructions**.  
  
4.  Dans la liste **Type d'instruction** , cliquez sur **Synchroniser les horloges**.  
  
5.  Sous **Destinataires**, activez l'une des options suivantes :  
  
    -   Cliquez sur **Tous les serveurs cibles** pour synchroniser les horloges de tous les serveurs cibles avec celle du serveur maître.  
  
    -   Cliquez sur **Serveurs cible sélectionnés** pour synchroniser l'horloge de certains serveurs, puis sélectionnez chacun des serveurs cibles dont vous souhaitez synchroniser l'horloge avec celle du serveur maître.  
  
6.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-synchronize-target-server-clocks"></a>Pour synchroniser les horloges des serveurs cibles  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE msdb ;  
    GO  
    -- resynchronizes the SEATTLE1 target server  
    EXEC dbo.sp_resync_targetserver  
        N'SEATTLE1' ;  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_resync_targetserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-resync-targetserver-transact-sql.md).  
