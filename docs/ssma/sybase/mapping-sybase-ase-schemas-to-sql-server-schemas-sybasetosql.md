---
description: Mappage de schémas Sybase ASE à des schémas SQL Server (SybaseToSQL)
title: Mappage de schémas Sybase ASE à des schémas de SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Schema Mapping
ms.assetid: 2c927003-c49d-4fe1-8e3e-5b2899166268
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a0084551b2e1ab6de4c3731f33525ccd3a8a48d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017520"
---
# <a name="mapping-sybase-ase-schemas-to-sql-server-schemas-sybasetosql"></a>Mappage de schémas Sybase ASE à des schémas SQL Server (SybaseToSQL)
Dans Sybase Adaptive Server Enterprise (ASE), chaque base de données contient un ou plusieurs schémas. Par défaut, SSMA migre tous les objets d’une base de données et d’un schéma vers la même base de données et le même schéma dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Toutefois, vous pouvez personnaliser le mappage entre ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL Database.  
  
## <a name="ase-and-sql-server-or-sql-azure-schemas"></a>Schémas ASE et SQL Server ou SQL Azure  
ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure spécifient tous deux des bases de données et leurs schémas en utilisant la notation en deux parties comme *Database. Schema*. Par exemple, dans une base de données de **démonstration** ASE, il peut y avoir un schéma **dbo** . Cette paire base de données et schéma est spécifiée en tant que **Demo. dbo**. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure a les mêmes base de données et schéma, la paire est également spécifiée en tant que **Demo. dbo**.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données et du schéma cibles  
Dans SSMA, vous pouvez mapper un schéma ASE à n’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma disponible ou SQL Azure.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées Sybase, sélectionnez **bases de données**.  
  
    L’onglet **mappage de schéma** est également disponible lorsque vous sélectionnez une base de données individuelle, le dossier **schémas** ou des schémas individuels. La liste de l’onglet **mappage de schéma** est personnalisée pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur l’onglet **mappage de schéma** .  
  
    Vous verrez une liste de toutes les bases de données ASE avec leurs schémas, suivies d’une valeur cible. Cette cible est indiquée dans une notation en deux parties (*Database. Schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure où vos objets et vos données seront migrés.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
4.  Dans la boîte de dialogue **choisir un schéma cible** , vous pouvez rechercher le schéma et la base de données cible disponibles, ou taper le nom de la base de données et du schéma dans la zone de texte en deux parties (Database. Schema), puis cliquer sur **OK**.  
  
5.  Les modifications apportées à la cible sous l’onglet **mappage de schéma** .  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper une base de données source à une base de données cible. Par défaut, la base de données source est mappée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible mappée n’est pas existante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous êtes invité à entrer un message **«la base de données et/ou le schéma n’existe pas dans les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ?»** Cliquez sur Oui. De même, vous pouvez mapper le schéma à un schéma non existant sous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible, qui sera créé lors de la synchronisation.  
  
-   Mappage à SQL Azure  
  
Vous pouvez mapper la base de données source au Azure SQL Database cible connecté ou à n’importe quel schéma du Azure SQL Database cible connecté. Si vous mappez le schéma source à un schéma non existant dans une base de données cible connectée, vous êtes invité à entrer un message **«le schéma n’existe pas dans les métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ? «** Cliquez sur Oui.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Rétablissement de la base de données et du schéma par défaut  
Si vous personnalisez le mappage entre un schéma ASE et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma ou SQL Azure, vous pouvez rétablir les valeurs par défaut du mappage.  
  
**Pour rétablir la base de données et le schéma par défaut**  
  
1.  Sous l’onglet Mappage de schéma, sélectionnez n’importe quelle ligne et cliquez sur **rétablir les valeurs par défaut** pour rétablir la base de données et le schéma par défaut.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets Sybase ASE en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou SQL Azure, vous pouvez [créer un rapport de conversion](assessing-sybase-ase-database-objects-for-conversion-sybasetosql.md). Dans le cas contraire, vous pouvez [convertir les définitions d’objet de base de données ASE](converting-sybase-ase-database-objects-sybasetosql.md) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure définitions d’objets.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
