---
description: Mappage des types de données DB2 et SQL Server (DB2ToSQL)
title: Mappage des types de données DB2 et SQL Server (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e7e939a8-5e76-4509-beaf-5acd1cab505e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 32cf19b192fe67802ae44eaf9ce6e4ca891d02ae
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072363"
---
# <a name="mapping-db2-and-sql-server-data-types-db2tosql"></a>Mappage des types de données DB2 et SQL Server (DB2ToSQL)
Les types de base de données DB2 diffèrent des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de base de données. Lorsque vous convertissez des objets de base de données DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets, vous devez spécifier comment mapper les types de données de DB2 à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les sections suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA possède un ensemble de mappages de types de données par défaut. Pour obtenir la liste des mappages par défaut, consultez [paramètres du projet &#40;mappage de Type&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-type-mapping-db2tosql.md).  
  
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
  
    Ou, pour personnaliser le mappage de type de données au niveau de la base de données, de la table, de la vue ou de la procédure stockée, sélectionnez la base de données, la catégorie d’objet ou l’objet dans l’Explorateur de métadonnées DB2 :  
  
    1.  Dans l’Explorateur de métadonnées DB2, sélectionnez le dossier ou l’objet à personnaliser.  
  
    2.  Dans le volet droit, cliquez sur l’onglet **mappage de type** .  
  
2.  Pour ajouter un nouveau mappage, procédez comme suit :  
  
    1.  Cliquez sur **Add**.  
  
    2.  Sous **type de source**, sélectionnez le type de données DB2 à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur de données minimale pour le mappage dans la zone **de** et la longueur de données maximale dans la zone **à** .  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** .  
  
    5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
3.  Pour modifier un mappage de type de données, procédez comme suit :  
  
    1.  Cliquez sur **Modifier**.  
  
    2.  Sous **type de source**, sélectionnez le type de données DB2 à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez la longueur de données minimale pour le mappage dans la zone **de** et la longueur de données maximale dans la zone **à** .  
  
        Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données.  
  
    4.  Sous **type de cible**, sélectionnez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type de données cible.  
  
        Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
4.  Pour supprimer un mappage de type de données personnalisé, procédez comme suit :  
  
    1.  Sélectionnez la ligne dans la liste mappage de type qui contient le mappage de type de données que vous souhaitez supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
        Vous ne pouvez pas supprimer les mappages hérités. Toutefois, les mappages hérités sont remplacés par des mappages personnalisés sur un objet spécifique ou une catégorie d’objet.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à effectuer un [rapport d’évaluation &#40;DB2ToSQL&#41;](../../ssma/db2/assessment-report-db2tosql.md) ou à [convertir des schémas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md). Si vous créez un rapport d’évaluation, les objets DB2 sont convertis automatiquement au cours de l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
