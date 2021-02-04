---
description: Désigner un opérateur de prévention de défaillance
title: Désigner un opérateur de prévention de défaillance
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- fail-safe operator [SQL Server]
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: 0f4eb513-5c0a-4523-974e-e85c1deeb57f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4b59e4a768f7c6cc5bdea3ca7784584816bee248
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236875"
---
# <a name="designate-a-fail-safe-operator"></a>Désigner un opérateur de prévention de défaillance
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Un opérateur de prévention de défaillance est un utilisateur qui reçoit l'alerte si l'opérateur désigné n'est pas joignable. Cette rubrique explique comment définir un opérateur de prévention de défaillance qui recevra les notifications d’alertes de [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Avant de commencer  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitations et restrictions  
  
-   Les options du récepteur de radiomessagerie et **net send** seront supprimées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent dans une version future de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Évitez d'utiliser ces fonctionnalités dans une nouvelle tâche de développement et prévoyez de modifier les applications qui les utilisent actuellement.  
  
-   Remarque : SQL Server Agent doit être configuré pour utiliser la messagerie de base de données pour envoyer des notifications aux opérateurs par messagerie électronique ou radiomessagerie. Pour plus d'informations, consultez [Affecter des alertes à un opérateur](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] est un outil dont l'interface graphique permet de gérer facilement les travaux. Son utilisation est recommandée pour créer et gérer l'infrastructure des travaux.  
  
### <a name="security"></a><a name="Security"></a>Sécurité  
  
#### <a name="permissions"></a><a name="Permissions"></a>Autorisations  
Seuls les membres du rôle serveur fixe **sysadmin** peuvent désigner des opérateurs de prévention de défaillance.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-designate-a-fail-safe-operator"></a>Pour désigner un opérateur de prévention de défaillance  
  
1.  Dans **l’Explorateur d’objets** , cliquez sur le signe plus pour développer le serveur qui contient l’opérateur de SQL Server Agent que vous souhaitez désigner comme opérateur de prévention de défaillance.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent** , puis sélectionnez **Propriétés**.  
  
3.  Dans la boîte de dialogue **Propriétés de SQL Server Agent -**_nom\_serveur_, sous **Sélectionner une page**, sélectionnez **Système d’alerte**.  
  
4.  Sous **Opérateur de prévention de défaillance**, sélectionnez **Activer l’opérateur de prévention de défaillance**.  
  
5.  Dans la liste **Opérateur** , sélectionnez l’opérateur que vous souhaitez définir comme opérateur de prévention de défaillance.  
  
6.  Cochez tout ou partie des cases suivantes pour spécifier comment l’opérateur doit être informé : **Messagerie électronique**, **Radiomessagerie** ou **NET SEND**.  
  
7.  Lorsque vous avez terminé, cliquez sur **OK**.  
