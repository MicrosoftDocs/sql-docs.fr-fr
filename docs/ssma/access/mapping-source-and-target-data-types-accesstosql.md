---
description: Mappage des types de données source et cible (AccessToSQL)
title: Mappage des types de données source et cible (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- customizing data type mappings
- data types, mapping
- mapping, data types
- source data types
- target data types
ms.assetid: b362a075-16e7-423f-b63f-e1e9f02844a9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f5a725d0e2c6d9651884c399caabe2a8833826f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059044"
---
# <a name="mapping-source-and-target-data-types-accesstosql"></a>Mappage des types de données source et cible (AccessToSQL)
Les types de base de données Access diffèrent des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de base de données. Lorsque vous convertissez des objets de base de données Access en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets, vous devez spécifier comment mapper les types de données de l’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les procédures suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA possède un ensemble de mappages de types de données par défaut. Pour obtenir la liste des mappages par défaut, consultez [paramètres du projet (mappage de type)](./project-settings-type-mapping-accesstosql.md).  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation des mappages de types de données  
La boîte de dialogue **paramètres du projet** vous permet de personnaliser la façon dont les types sont mappés pour toutes les bases de données et les objets de base de données d’un projet. Les mappages de type pour un projet s’appliquent à toutes les bases de données et objets de base de données qui n’ont pas de mappages de types personnalisés.  
  
Vous pouvez également personnaliser le mappage de type de données au niveau de la base de données ou de la table.  
  
La procédure suivante montre comment mapper des types de données au niveau du projet, de la base de données ou de l’objet de base de données.  
  
**Pour mapper des types de données**  
  
1.  Pour personnaliser le mappage de type de données pour l’ensemble du projet, ouvrez la boîte de dialogue **paramètres du projet** :  
  
    1.  Dans le menu **Outils** , sélectionnez **paramètres du projet**.  
  
    2.  Dans le volet gauche, sélectionnez **mappage de type**.  
  
        Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    Ou, pour personnaliser le mappage de type de données au niveau de la base de données ou de la table, sélectionnez la base de données ou la table dans le volet Explorateur de métadonnées d’accès :  
  
    1.  Dans le volet Explorateur de métadonnées Access, développez **accès-métabase**, puis développez **bases de données**.  
  
    2.  Sélectionnez la base de données ou la table pour laquelle vous souhaitez personnaliser le mappage de type de données.  
  
    3.  Dans le volet droit, cliquez sur **mappage de type**.  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Dans le volet mappage de type, cliquez sur **Ajouter**.  
  
    2.  Dans la boîte de dialogue **nouveau mappage de type** , sous **type de source**, sélectionnez le type de données Access à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez les longueurs de données minimale et maximale pour le mappage en activant les cases à cocher **de** et à, puis **en** entrant les valeurs.  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Dans le volet mappage de type, cliquez sur **modifier**.  
  
    2.  Dans la boîte de dialogue **liste de mappage de type** , sous type de **source**, sélectionnez le type de données Access à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez les longueurs de données minimale et maximale pour le mappage en activant les cases à cocher **de** et à, puis **en** entrant les valeurs.  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
4.  Pour supprimer un mappage de type de données, procédez comme suit :  
  
    1.  Dans le volet mappage de type, sélectionnez la ligne dans la liste mappage de type qui contient le mappage de type de données que vous souhaitez supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste [à convertir les objets de base de données Access en objets SQL Server](converting-access-database-objects-accesstosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
