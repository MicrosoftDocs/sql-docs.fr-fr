---
title: Mappage de bases de données MySQL à des schémas de SQL Server (MySQLToSQL) | Microsoft Docs
description: Découvrez comment personnaliser les mappages SSMA pour MySQL entre les schémas MySQL et SQL Server ou Azure SQL Database ou accepter la valeur par défaut.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, Modifying target database and schema
- Mapping, reverting to default database and schema
ms.assetid: 5c6fb445-92ae-4933-b77d-80230931c024
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 44ac691a0fe9cb4b513a2f9b435457c699c7e3e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076071"
---
# <a name="mapping-mysql-databases-to-sql-server-schemas-mysqltosql"></a>Mappage de bases de données MySQL à des schémas SQL Server (MySQLToSQL)
Par défaut, SSMA pour MySQL migre tous les objets d’un schéma MySQL vers un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database ou nommé pour le schéma. Toutefois, vous pouvez personnaliser le mappage entre les schémas MySQL et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database.  
  
## <a name="mysql-and-sql-server-or-sql-azure-schemas"></a>Schémas MySQL et SQL Server ou SQL Azure  
Le concept MySQL d’un schéma correspond au concept SQL Server d’une base de données et à l’un de ses schémas. SSMA fait référence à la combinaison de SQL Server de base de données et de schéma en tant que schéma.  
  
Le concept MySQL d’un schéma correspond au concept SQL Server d’une base de données et à l’un de ses schémas. Par exemple, MySQL peut avoir un schéma nommé **HR**. Une instance de SQL Server peut avoir une base de données nommée **HR** et au sein de cette base de données sont des schémas. Un schéma est le schéma **dbo** (ou propriétaire de la base de données). Par défaut, le schéma MySQL **HR** est mappé à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données et au schéma **hr. dbo**. SSMA fait référence à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinaison base de données et schéma en tant que schéma.  
  
Vous pouvez modifier le mappage entre MySQL et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas Azure.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données et du schéma cibles  
Dans SSMA, vous pouvez mapper un schéma MySQL à n’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma disponible ou SQL Azure.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez **schémas**.  
  
    L’onglet **mappage de schéma** est également disponible lorsque vous sélectionnez des schémas individuels. La liste de l’onglet **mappage de schéma** est personnalisée pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur l’onglet **mappage de schéma** .  
  
    Vous verrez une liste de tous les schémas MySQL, suivis d’une valeur cible. Cette cible est indiquée dans une notation en deux parties (*Database. Schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure où vos objets et vos données seront migrés.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
    Dans la boîte de dialogue **choisir un schéma cible** , vous pouvez rechercher le schéma et la base de données cible disponibles, ou taper le nom de la base de données et du schéma dans la zone de texte en deux parties (Database. Schema), puis cliquer sur **OK**.  
  
4.  Les modifications apportées à la cible sous l’onglet **mappage de schéma** .  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper une base de données source à une base de données cible. Par défaut, la base de données source est mappée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible mappée n’est pas existante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous êtes invité à entrer un message **«la base de données et/ou le schéma n’existe pas dans les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ?»** Cliquez sur Oui. De même, vous pouvez mapper le schéma à un schéma non existant sous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible, qui sera créé lors de la synchronisation.  
  
-   Mappage à SQL Azure  
  
Vous pouvez mapper la base de données source à la base de données cible connectée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou au schéma de n’importe quel schéma dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données cible connectée. Si vous mappez le schéma source à un schéma non existant dans une base de données cible connectée, vous êtes invité à entrer un message **«le schéma n’existe pas dans les métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ? «** Cliquez sur Oui.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Rétablissement de la base de données et du schéma par défaut  
Si vous personnalisez le mappage entre un schéma MySQL et un schéma SQL Server, vous pouvez rétablir les valeurs par défaut du mappage.  
  
**Pour rétablir la base de données et le schéma par défaut**  
  
1.  Sous l’onglet Mappage de schéma, sélectionnez n’importe quelle ligne et cliquez sur **rétablir les valeurs par défaut** pour rétablir la base de données et le schéma par défaut.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets MySQL en objets SQL Server ou SQL Azure, vous pouvez [créer un rapport de conversion](assessing-mysql-databases-for-conversion-mysqltosql.md) , sinon vous pouvez [convertir les définitions d’objet de base de données mysql](converting-mysql-databases-mysqltosql.md) en schémas SQL Server ou SQL Azure  
  
## <a name="see-also"></a>Voir aussi  
[Paramètres du projet &#40;conversion&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
[Connexion à Azure SQL Database &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-azure-sql-db-mysqltosql.md)  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
[Connexion à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
