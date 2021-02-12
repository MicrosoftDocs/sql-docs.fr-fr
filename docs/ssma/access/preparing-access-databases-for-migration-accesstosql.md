---
title: Préparation des bases de données Access pour la migration (AccessToSQL) | Microsoft Docs
description: Découvrez comment déterminer les bases de données Access à migrer vers SQL Server ou Azure SQL Database et vérifier que ces bases de données sont prêtes pour la migration.
ms.prod: sql
ms.custom: ''
ms.date: 08/15/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, versions
- Access databases, when to migrate
- Access databases, workgroup security
- backing up databases
- documenting databases
- files, preparing
- migrating databases, versions
- migrating databases, when to migrate
- versions of Access
- workgroup security
ms.assetid: 9b80a9e0-08e7-4b4d-b5ec-cc998d3f5114
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 7c52b13a2a71afc005f19d1733e8dff435d2a582
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066584"
---
# <a name="preparing-access-databases-for-migration-accesstosql"></a>Préparation des bases de données Access pour la migration (AccessToSQL)
Avant de migrer des bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez déterminer les bases de données à migrer et vérifier que ces bases de données sont prêtes pour la migration.  
  
## <a name="determining-when-to-migrate-to-sql-server"></a>Détermination du moment où migrer vers SQL Server  
Le moteur de base de données Jet, qui est utilisé en tant que moteur de base de données pour l’accès, est une solution flexible et facile à utiliser pour la gestion des données. Toutefois, étant donné que les bases de données deviennent plus importantes et plus stratégiques, de nombreux utilisateurs trouvent qu’ils ont besoin de performances, de sécurité ou d’une disponibilité accrue. Pour les applications qui nécessitent une plateforme de données plus robuste, envisagez de déplacer les bases de données sous-jacentes pour ces applications vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur le choix du moment de la migration, consultez la [page informations](https://go.microsoft.com/fwlink/?LinkId=68571) sur la migration sur le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] site Web.  
  
Après la migration des bases de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez continuer à utiliser l’accès à l’aide de tables liées, ou vous pouvez migrer manuellement vos applications vers du [!INCLUDE[msCoName](../../includes/msconame_md.md)] code basé sur .NET Framework qui interagit directement avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="determining-which-databases-to-migrate"></a>Détermination des bases de données à migrer  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour Access peut localiser les bases de données Access pour vous. Vous pouvez ensuite exporter les métadonnées relatives à ces bases de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Pour plus d’informations sur l’exportation et l’interrogation des métadonnées, consultez [exportation d’un inventaire des accès](exporting-an-access-inventory-accesstosql.md).  

   > [!NOTE]
   > Les fonctionnalités et les paramètres d’accès ne sont pas tous pris en charge par, ou peuvent être facilement converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Avant de commencer à migrer des bases de données, consultez [fonctionnalités d’accès incompatible](incompatible-access-features-accesstosql.md).
  
## <a name="preparing-for-migration"></a>Préparation à la migration  
Utilisez les instructions suivantes pour vous aider à préparer vos bases de données Access à la migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="upgrading-older-access-databases"></a>Mise à niveau des bases de données Access plus anciennes  
SSMA pour Access prend en charge Access 97 et versions ultérieures. Si vous avez des bases de données de versions antérieures d’Access, ouvrez et enregistrez les bases de données dans Access 97 ou une version ultérieure.  
  
### <a name="removing-workgroup-protection"></a>Suppression de la protection de groupe de travail  
SSMA ne peut pas migrer les bases de données qui utilisent la protection de groupe de travail. Pour supprimer la protection d’un groupe de travail d’une base de données Access, procédez comme suit :  
  
1.  Copiez le fichier de base de données Access dans un autre emplacement.  
  
2.  Ouvrez la base de données copiée.  
  
3.  Dans le menu **Outils** , pointez sur **sécurité**, puis sélectionnez **autorisations pour les utilisateurs et les groupes**.  
  
4.  Sélectionnez l’option **utilisateurs** , sélectionnez l’utilisateur **administrateur** , puis vérifiez que l’autorisation **administrer** est sélectionnée.  
  
5.  Sélectionnez l’option **groupes** , sélectionnez le groupe **utilisateurs** , puis vérifiez que l’autorisation **administrer** est sélectionnée.  
  
6.  Cliquez sur **OK**, puis, dans le menu **fichier** , cliquez sur **quitter**.  
  
Vous pouvez maintenant utiliser SSMA pour migrer la base de données copiée. Après avoir chargé le schéma dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez sécuriser manuellement la base de données sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="backing-up-databases"></a>Sauvegarde de bases de données  
Avant de migrer vos bases de données Access vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez sauvegarder les bases de données Access que vous allez migrer ainsi que les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données dans lesquelles vous allez migrer les objets et les données d’accès.  
  
Pour sauvegarder une base de données Access, dans le menu **Outils** , pointez sur **utilitaires de base de données**, puis sélectionnez **sauvegarder la base de données**.  
  
Pour plus d’informations sur la sauvegarde des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données, consultez « sauvegarde et restauration de bases de données » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.  
  
### <a name="documenting-databases"></a>Documentation des bases de données  
Vous pouvez également documenter les propriétés, telles que les listes d’objets de base de données, les tailles de fichier et les autorisations, de vos bases de données Access. Pour générer cette documentation dans Access, dans le menu **Outils** , pointez sur **analyser**, puis cliquez sur **documenté**.  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
[Liaison des applications Access à SQL Server](linking-access-applications-to-sql-server-azure-sql-db-accesstosql.md)
