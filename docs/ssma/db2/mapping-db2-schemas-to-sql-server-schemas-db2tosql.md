---
description: Mappage de schémas DB2 à des schémas de SQL Server (DB2ToSQL)
title: Mappage de schémas DB2 à des schémas de SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 05ff7bd4-e60b-4f48-a893-bc2346aa9a8a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 489a026fe610b298803db088e7f8c9953215a7c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060581"
---
# <a name="mapping-db2-schemas-to-sql-server-schemas-db2tosql"></a>Mappage de schémas DB2 à des schémas de SQL Server (DB2ToSQL)
Dans DB2, chaque base de données contient un ou plusieurs schémas. Par défaut, SSMA migre tous les objets d’un schéma DB2 vers une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données nommée pour le schéma. Toutefois, vous pouvez personnaliser le mappage entre les schémas DB2 et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
## <a name="db2-and-sql-server-schemas"></a>Schémas DB2 et SQL Server  
Une base de données DB2 contient des schémas. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient plusieurs bases de données, chacune pouvant avoir plusieurs schémas.  
  
Le concept DB2 d’un schéma est mappé au [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] concept d’une base de données et à l’un de ses schémas. Par exemple, DB2 peut avoir un schéma nommé **HR**. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut avoir une base de données nommée **HR** et au sein de cette base de données sont des schémas. Un schéma est le schéma **dbo** (ou propriétaire de la base de données). Par défaut, le schéma DB2 **HR** est mappé à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données et au schéma **hr. dbo**. SSMA fait référence à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinaison base de données et schéma en tant que schéma.  
  
Vous pouvez modifier le mappage entre DB2 et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données et du schéma cibles  
Dans SSMA, vous pouvez mapper un schéma DB2 à n’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma disponible.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées DB2, sélectionnez **schémas**.  
  
    L’onglet **mappage de schéma** est également disponible lorsque vous sélectionnez une base de données individuelle, le dossier **schémas** ou des schémas individuels. La liste de l’onglet **mappage de schéma** est personnalisée pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur l’onglet **mappage de schéma** .  
  
    Vous verrez une liste de tous les schémas DB2, suivis d’une valeur cible. Cette cible est indiquée dans une notation en deux parties (*Database. Schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où vos objets et vos données seront migrés.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
    Dans la boîte de dialogue **choisir un schéma cible** , vous pouvez rechercher le schéma et la base de données cible disponibles, ou taper le nom de la base de données et du schéma dans la zone de texte en deux parties (Database. Schema), puis cliquer sur **OK**.  
  
4.  Les modifications apportées à la cible sous l’onglet **mappage de schéma** .  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper une base de données source à une base de données cible. Par défaut, la base de données source est mappée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible mappée n’est pas existante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous êtes invité à entrer un message **«la base de données et/ou le schéma n’existe pas dans les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ?»** Cliquez sur Oui. De même, vous pouvez mapper le schéma à un schéma non existant sous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible, qui sera créé lors de la synchronisation.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Rétablissement de la base de données et du schéma par défaut  
Si vous personnalisez le mappage entre un schéma DB2 et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma, vous pouvez rétablir les valeurs par défaut du mappage.  
  
**Pour rétablir la base de données et le schéma par défaut**  
  
1.  Sous l’onglet Mappage de schéma, sélectionnez n’importe quelle ligne et cliquez sur **rétablir les valeurs par défaut** pour rétablir la base de données et le schéma par défaut.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets, vous pouvez effectuer un [rapport de migration des données (SSMA commun)](../sybase/data-migration-report-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Connexion à SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-sql-server-db2tosql.md)  
[Migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
