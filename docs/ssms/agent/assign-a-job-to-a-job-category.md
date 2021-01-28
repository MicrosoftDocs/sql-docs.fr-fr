---
description: Affecter un travail à une catégorie de travaux
title: Affecter un travail à une catégorie de travaux
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], assigning
- SQL Server Agent jobs, categories
- jobs [SQL Server Agent], categories
- categories [SQL Server Agent jobs]
- SQL Server Agent jobs, assigning
- assigning job to category
ms.assetid: a9ea65a2-1d73-4582-a335-63adeb450cb6
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 06/03/2020
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 5c2ab0d3b985a619fbb31492d47aca88e932dbc7
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98765511"
---
# <a name="assign-a-job-to-a-job-category"></a>Affecter un travail à une catégorie de travaux

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique décrit comment affecter des travaux de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent aux catégories de travaux dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] ou SQL Server Management Objects.  
  
Les catégories de travaux permettent d'organiser les travaux afin d'en faciliter le filtrage et le regroupement. Par exemple, vous pouvez organiser tous vos travaux de sauvegarde de base de données dans la catégorie Maintenance de bases de données. Vous pouvez affecter des travaux à des catégories de travaux intégrées, ou vous pouvez créer une catégorie de travaux définie par l’utilisateur, à laquelle vous affectez ensuite des travaux.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
Pour plus d'informations, consultez [Implémenter la sécurité de SQL Server Agent](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>Pour affecter un travail à une catégorie de travaux  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus pour développer le serveur sur lequel vous souhaitez affecter un travail à une catégorie de travaux.  
  
2.  Cliquez sur le signe plus (+) pour développer **Agent SQL Server**.  
  
3.  Cliquez sur le signe plus (+) pour développer le dossier **Travaux** .  
  
4.  Cliquez avec le bouton droit sur le travail à modifier, puis sélectionnez **Propriétés**.  
  
5.  Dans la boîte de dialogue **Propriétés du travail -**_nom\_travail_, dans la liste **Catégorie**, sélectionnez la catégorie de travaux que vous voulez affecter au travail.  
  
6.  Cliquez sur **OK**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Utilisation de Transact-SQL  
  
#### <a name="to-assign-a-job-to-a-job-category"></a>Pour affecter un travail à une catégorie de travaux  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```sql
    -- adding a new job category to the "NightlyBackups" job  
    USE msdb ;  
    GO  
    EXEC dbo.sp_update_job  
        @job_name = N'NightlyBackups',  
        @category_name = N'[Uncategorized (Local)]';  
    GO  
    ```  
  
Pour plus d’informations, consultez [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Utilisation de SQL Server Management Objects  
**Pour affecter un travail à une catégorie de travaux**  
  
Utilisez la classe **JobCategory** à l’aide d’un langage de programmation que vous choisissez, tel que Visual Basic, Visual C# ou PowerShell.  
