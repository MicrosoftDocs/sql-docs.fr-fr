---
description: Inscrire un serveur cible dans un serveur maître
title: Inscrire un serveur cible dans un serveur maître
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- enlisting target servers [SQL Server]
- SQL Server Agent jobs, target servers
- master servers [SQL Server], enlisting target servers
- SQL Server Agent jobs, master servers
- target servers [SQL Server], enlisting
ms.assetid: 7633adb5-d140-4e58-a8f2-5b4b50c2f95b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 482da129c638daafe7d5a86a1d4e8d33810824ae
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/02/2021
ms.locfileid: "99251067"
---
# <a name="enlist-a-target-server-to-a-master-server"></a>Inscrire un serveur cible dans un serveur maître
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> Dans [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), la plupart, mais pas toutes les fonctionnalités SQL Server Agent sont actuellement prises en charge. Pour plus d’informations, consultez [Différences T-SQL entre Azure SQL Managed Instance et SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent).

Cette rubrique explique comment ajouter des serveurs cibles à une configuration d'administration multiserveur. Exécutez la procédure suivante à partir du serveur maître : dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], de [!INCLUDE[tsql](../../includes/tsql-md.md)], ou d'objets SMO (SQL Server Management Objects).  
  
Pour plus d’informations sur la manière dont le compte Windows utilisé pour le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Agent affecte un environnement multiserveur, consultez [Créer un environnement multi-serveur](../../ssms/agent/create-a-multiserver-environment.md).  
  
Le chiffrement TLS (Transport Layer Security), anciennement SSL (Secure Sockets Layer), complet et la validation de certificats sont activés pour les connexions entre les serveurs maîtres et les serveurs cible par défaut. Pour plus d’informations, consultez [Définir des options de chiffrement sur des serveurs cibles](../../ssms/agent/set-encryption-options-on-target-servers.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Utilisation de SQL Server Management Studio  
  
#### <a name="to-enlist-a-target-server"></a>Pour inscrire un serveur cible  
  
1.  Dans l' **Explorateur d'objets**, développez un serveur configuré en tant que serveur maître.  
  
2.  Cliquez avec le bouton droit sur **SQL Server Agent**, pointez sur **Administration multiserveur**, puis cliquez sur **Ajouter des serveurs cibles**.  
  
3.  Exécutez l'Assistant Création d'un serveur cible, qui vous guide tout au long du processus.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Utilisation de Transact-SQL  
  
#### <a name="to-enlist-a-target-server"></a>Pour inscrire un serveur cible  
  
1.  Utilisez la procédure stockée **sp_msx_enlist** .  Pour plus d’informations, consultez [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md).  
  
## <a name="see-also"></a> Voir aussi  
[Administration automatisée à l'échelle d'une entreprise](../../ssms/agent/automated-administration-across-an-enterprise.md)  
