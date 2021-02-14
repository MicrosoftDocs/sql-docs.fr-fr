---
title: Mappage de types de données Oracle et SQL Server (OracleToSQL) | Microsoft Docs
description: Découvrez comment personnaliser les mappages SSMA pour Oracle entre les types de données Oracle et SQL Server ou accepter la valeur par défaut.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Type Mapping Inheritance
ms.assetid: 05da1495-63b9-47b7-86e2-6746394a2d8a
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8058123020571ad43e3142fccb920aa22a4b5af5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080778"
---
# <a name="mapping-oracle-and-sql-server-data-types-oracletosql"></a>Mappage de types de données Oracle et SQL Server (OracleToSQL)
Les types de base de données Oracle diffèrent des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de base de données. Lorsque vous convertissez des objets de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets, vous devez spécifier comment mapper des types de données d’Oracle vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les sections suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA possède un ensemble de mappages de types de données par défaut. Pour obtenir la liste des mappages par défaut, consultez [paramètres du projet &#40;mappage de Type&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-type-mapping-oracletosql.md).  
  
## <a name="type-mapping-inheritance"></a>Héritage de mappage de type  
Vous pouvez personnaliser les mappages de type au niveau du projet, au niveau de la catégorie d’objet (par exemple, toutes les procédures stockées) ou au niveau de l’objet. Les paramètres sont hérités du niveau supérieur, sauf s’ils sont remplacés à un niveau inférieur. Par exemple, si vous mappez **smallmoney** à **Money** au niveau du projet, tous les objets du projet utiliseront ce mappage, sauf si vous personnalisez le mappage au niveau de l’objet ou de la catégorie.  
  
Lorsque vous affichez l’onglet **mappage de type** dans SSMA, l’arrière-plan est codé par couleur pour indiquer les mappages de type hérités. L’arrière-plan d’un mappage de type est jaune pour tout mappage de type hérité et blanc pour tout mappage spécifié au niveau actuel.  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation des mappages de types de données  
La procédure suivante montre comment mapper des types de données au niveau du projet, de la base de données ou de l’objet :  
  
**Pour mapper des types de données**  
  
1.  Pour personnaliser le mappage de type de données pour l’ensemble du projet, ouvrez la boîte de dialogue **paramètres du projet** :  
  
    1.  Dans le menu **Outils** , sélectionnez **paramètres du projet**.  
  
    2.  Dans le volet gauche, sélectionnez **mappage de type**.  
  
        Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    Ou, pour personnaliser le mappage de type de données au niveau de la base de données, de la table, de la vue ou de la procédure stockée, sélectionnez la base de données, la catégorie d’objet ou l’objet dans l’Explorateur de métadonnées Oracle :  
  
    1.  Dans l’Explorateur de métadonnées Oracle, sélectionnez le dossier ou l’objet à personnaliser.  
  
    2.  Dans le volet droit, cliquez sur l’onglet **mappage de type** .  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Cliquez sur **Add**.  
  
    2.  Sous **type de source**, sélectionnez le type de données Oracle à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur de données minimale pour le mappage dans la zone **de** et la longueur de données maximale dans la zone **à** .  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** .  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Cliquez sur **Modifier**.  
  
    2.  Sous **type de source**, sélectionnez le type de données Oracle à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur de données minimale pour le mappage dans la zone **de** et la longueur de données maximale dans la zone **à** .  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Pour supprimer un mappage de type de données personnalisé, procédez comme suit :  
  
    1.  Sélectionnez la ligne dans la liste mappage de type qui contient le mappage de type de données que vous souhaitez supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
        Vous ne pouvez pas supprimer les mappages hérités. Toutefois, les mappages hérités sont remplacés par des mappages personnalisés sur un objet spécifique ou une catégorie d’objet.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à [créer un rapport d’évaluation](assessing-oracle-schemas-for-conversion-oracletosql.md) ou [à convertir des objets de base de données Oracle en SQL Server syntaxe](converting-oracle-schemas-oracletosql.md). Si vous créez un rapport d’évaluation, les objets Oracle sont convertis automatiquement au cours de l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
