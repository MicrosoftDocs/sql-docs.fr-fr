---
title: Mappage de schémas Oracle à des schémas de SQL Server (OracleToSQL) | Microsoft Docs
description: Découvrez comment personnaliser les mappages SSMA pour Oracle entre les schémas Oracle et SQL Server ou accepter la valeur par défaut.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 0edeaa08-9c5d-4e3a-bc15-b9a1f0c8a9dc
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 16c4de0bdf08be3bfdbc8644ae61d300284a0c25
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080770"
---
# <a name="mapping-oracle-schemas-to-sql-server-schemas-oracletosql"></a>Mappage de schémas Oracle à des schémas SQL Server (OracleToSQL)
Dans Oracle, chaque base de données contient un ou plusieurs schémas. Par défaut, SSMA migre tous les objets d’un schéma Oracle vers une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données nommée pour le schéma. Toutefois, vous pouvez personnaliser le mappage entre les schémas Oracle et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bases de données.  
  
## <a name="oracle-and-sql-server-schemas"></a>Schémas Oracle et SQL Server  
Une base de données Oracle contient des schémas. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contient plusieurs bases de données, chacune pouvant avoir plusieurs schémas.  
  
Le concept Oracle d’un schéma est mappé au [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] concept d’une base de données et à l’un de ses schémas. Par exemple, Oracle peut avoir un schéma nommé **HR**. Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut avoir une base de données nommée **HR** et au sein de cette base de données sont des schémas. Un schéma est le schéma **dbo** (ou propriétaire de la base de données). Par défaut, le schéma Oracle **HR** est mappé à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données et au schéma **hr. dbo**. SSMA fait référence à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] combinaison base de données et schéma en tant que schéma.  
  
Vous pouvez modifier le mappage entre Oracle et les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schémas.  
  
## <a name="modifying-the-target-database-and-schema"></a>Modification de la base de données et du schéma cibles  
Dans SSMA, vous pouvez mapper un schéma Oracle à n’importe quel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma disponible.  
  
**Pour modifier la base de données et le schéma**  
  
1.  Dans l’Explorateur de métadonnées Oracle, sélectionnez **schémas**.  
  
    L’onglet **mappage de schéma** est également disponible lorsque vous sélectionnez une base de données individuelle, le dossier **schémas** ou des schémas individuels. La liste de l’onglet **mappage de schéma** est personnalisée pour l’objet sélectionné.  
  
2.  Dans le volet droit, cliquez sur l’onglet **mappage de schéma** .  
  
    Vous verrez une liste de tous les schémas Oracle, suivis d’une valeur cible. Cette cible est indiquée dans une notation en deux parties (*Database. Schema*) dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] où vos objets et vos données seront migrés.  
  
3.  Sélectionnez la ligne qui contient le mappage que vous souhaitez modifier, puis cliquez sur **modifier**.  
  
    Dans la boîte de dialogue **choisir un schéma cible** , vous pouvez rechercher le schéma et la base de données cible disponibles, ou taper le nom de la base de données et du schéma dans la zone de texte en deux parties (Database. Schema), puis cliquer sur **OK**.  
  
4.  Les modifications apportées à la cible sous l’onglet **mappage de schéma** .  
  
**Modes de mappage**  
  
-   Mappage à SQL Server  
  
Vous pouvez mapper une base de données source à une base de données cible. Par défaut, la base de données source est mappée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible avec laquelle vous vous êtes connecté à l’aide de SSMA. Si la base de données cible mappée n’est pas existante sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous êtes invité à entrer un message **«la base de données et/ou le schéma n’existe pas dans les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] métadonnées cibles. Il serait créé au cours de la synchronisation. Voulez-vous continuer ?»** Cliquez sur Oui. De même, vous pouvez mapper le schéma à un schéma non existant sous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la base de données cible, qui sera créé lors de la synchronisation.  
  
## <a name="reverting-to-the-default-database-and-schema"></a>Rétablissement de la base de données et du schéma par défaut  
Si vous personnalisez le mappage entre un schéma Oracle et un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] schéma, vous pouvez rétablir les valeurs par défaut du mappage.  
  
**Pour rétablir la base de données et le schéma par défaut**  
  
1.  Sous l’onglet Mappage de schéma, sélectionnez n’importe quelle ligne et cliquez sur **rétablir les valeurs par défaut** pour rétablir la base de données et le schéma par défaut.  
  
## <a name="next-steps"></a>Étapes suivantes  
Si vous souhaitez analyser la conversion d’objets Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets, vous pouvez [créer un rapport de conversion](assessing-oracle-schemas-for-conversion-oracletosql.md). Dans le cas contraire, vous pouvez [convertir les définitions d’objet de base de données Oracle](converting-oracle-schemas-oracletosql.md) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions d’objets.  
  
## <a name="see-also"></a>Voir aussi  
[Connexion à SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-sql-server-oracletosql.md)  
[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
