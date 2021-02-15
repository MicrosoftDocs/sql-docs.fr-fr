---
title: Supprimer une alerte
description: Supprimer une alerte
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], deleting
- deleting alerts
- canceling alerts
- dropping alerts
- disabling alerts
- removing alerts
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: bf4cefa3d6bcd307b80ae4f4dc0944cdb18bcdba
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978782"
---
# <a name="delete-an-alert"></a>Supprimer une alerte

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cet article explique comment supprimer les alertes de Microsoft SQL Server Agent à l’aide de SQL Server Management Studio ou de Transact-SQL.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions

En supprimant une alerte, vous supprimez également toute notification associée à celle-ci.

### <a name="security"></a><a name="Security"></a>Sécurité

#### <a name="permissions"></a><a name="Permissions"></a>Autorisations

Par défaut, seuls les membres du rôle serveur fixe **sysadmin** peuvent supprimer des alertes.  

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio

### <a name="to-delete-an-alert"></a>Pour supprimer une alerte

1. Dans l’**Explorateur d’objets**, sélectionnez le signe plus (+) pour développer le serveur qui contient l’alerte de SQL Server Agent à supprimer.

2. Sélectionnez le signe plus (+) pour développer **SQL Server Agent**.

3. Cliquez sur le signe plus (+) pour développer le dossier **Alertes**.

4. Cliquez avec le bouton droit sur l’alerte à supprimer, puis sélectionnez **Supprimer**.

5. Dans la boîte de dialogue **Supprimer un objet**, vérifiez que vous avez choisi la bonne alerte, puis sélectionnez **OK**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL

### <a name="to-delete-an-alert"></a>Pour supprimer une alerte

1. Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].

2. Dans la barre d’outils standard, sélectionnez **Nouvelle requête**.  

3. Copiez et collez l’exemple suivant dans la fenêtre de requête, puis sélectionnez **Exécuter**.

    ```
    -- deletes the SQL Server Agent alert called 'Test Alert.'
    USE msdb ;
    GO
  
    EXEC dbo.sp_delete_alert
       @name = N'Test Alert' ;
    GO
    ```

Pour plus d’informations, consultez [sp_delete_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md).
