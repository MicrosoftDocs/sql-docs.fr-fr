---
description: Mappage des types de données MySQL et SQL Server (MySQLToSQL)
title: Mappage de types de données MySQL et SQL Server (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Mapping, customize data type mapping
- Mapping, Type mapping
ms.assetid: 14f98054-13b4-4231-a6b0-2452f3b9941d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: f7024f944030714e7aa2431815e326b1be5be701
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077200"
---
# <a name="mapping-mysql-and-sql-server-data-types-mysqltosql"></a>Mappage des types de données MySQL et SQL Server (MySQLToSQL)
Les types de base de données MySQL diffèrent des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] types de Azure SQL Database ou. Lorsque vous convertissez des objets de base de données MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou SQL Azure, vous devez spécifier comment mapper les types de données de MySQL à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Vous pouvez accepter les mappages de type de données par défaut, ou vous pouvez personnaliser les mappages comme indiqué dans les procédures suivantes.  
  
## <a name="default-mappings"></a>Mappages par défaut  
SSMA possède un ensemble de mappages de types de données par défaut. Pour obtenir la liste des mappages par défaut, consultez [paramètres du projet &#40;mappage de Type&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-type-mapping-mysqltosql.md).  
  
## <a name="type-mapping-inheritance"></a>Héritage de mappage de type  
Vous pouvez personnaliser les mappages de type au niveau du projet, au niveau de la catégorie d’objet (par exemple, toutes les procédures stockées) ou au niveau de l’objet. Les paramètres sont hérités du niveau supérieur, sauf s’ils sont remplacés à un niveau inférieur. Par exemple, si vous mappez **smallint** à **int** au niveau du projet, tous les objets du projet utiliseront ce mappage, sauf si vous personnalisez le mappage au niveau de l’objet ou de la catégorie.  
  
Lorsque vous affichez l’onglet **mappage de type** dans SSMA, l’arrière-plan est codé par couleur pour indiquer les mappages de type hérités. L’arrière-plan d’un mappage de type est jaune pour tout mappage de type hérité et blanc pour tout mappage spécifié au niveau actuel.  
  
## <a name="customizing-data-type-mappings"></a>Personnalisation des mappages de types de données  
  
-   **Pour mapper les types de données :**  
  
    Les procédures suivantes montrent comment mapper des types de données au niveau du projet, de la base de données ou de l’objet de base de données :  
  
    1.  Pour personnaliser le mappage de type de données pour l’ensemble du projet, ouvrez la boîte de dialogue **paramètres du projet** . Dans le menu Outils, sélectionnez **paramètres du projet**.  
  
        Dans le volet gauche, sélectionnez **mappage de type**. Le graphique de mappage de type et les boutons s’affichent dans le volet droit.  
  
    2.  Pour personnaliser les mappages de type de données au niveau de la base de données ou de la table, sélectionnez la base de données ou la table dans l’Explorateur de métadonnées MySQL. Dans l’Explorateur de métadonnées MySQL, sélectionnez le dossier ou l’objet à personnaliser.  
  
        Dans le volet droit, cliquez sur **mappage de type**.  
  
-   **Pour ajouter un nouveau mappage, procédez comme suit :**  
  
    1.  Dans le volet mappage de type, cliquez sur **Ajouter** .  
  
    2.  Dans la boîte de dialogue Nouveau mappage de type, sous **type de source**, sélectionnez le type de données MySQL à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez les longueurs de données minimale et maximale pour le mappage en activant les cases à cocher **de** et à, puis **en** entrant les valeurs.  
  
    4.  Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données. Sous **type de cible**, sélectionnez le SQL Server cible ou le type de données SQL Azure.  
  
        1.  Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
        2.  Certains types nécessitent une **précision** et une **échelle** de type de données cible. Si nécessaire, entrez la nouvelle précision et l’échelle dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
-   **Pour modifier un mappage de type, procédez comme suit :**  
  
    1.  Dans le volet mappage de type, cliquez sur **modifier**.  
  
    2.  Dans la boîte de dialogue Liste de mappage de type, sous **type de source**, sélectionnez le type de données MySQL à mapper.  
  
    3.  Si le type requiert une longueur, spécifiez les longueurs de données minimale et maximale pour le mappage en activant les cases à cocher **de** et à, puis **en** entrant les valeurs.  
  
    Cela vous permet de personnaliser le mappage des données pour les valeurs plus petites et plus grandes du même type de données. Sous **type de cible**, sélectionnez le SQL Server cible ou le type de données SQL Azure.  
  
    -  Certains types nécessitent une longueur de type de données cible. Si nécessaire, entrez la nouvelle longueur des données dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
    -  Certains types nécessitent une **précision** et une **échelle** de type de données cible. Si nécessaire, entrez la nouvelle précision et l’échelle dans la zone **remplacer par** , puis cliquez sur **OK**.  
  
-   **Pour supprimer un mappage de type de données, procédez comme suit :**  
  
    1.  Dans le volet mappage de type, sélectionnez la ligne dans la liste mappage de type qui contient le mappage de type de données que vous souhaitez supprimer.  
  
    2.  Cliquez sur **Supprimer**.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [créer un rapport d’évaluation](assessing-mysql-databases-for-conversion-mysqltosql.md) ou à [convertir les objets de base de données MySQL en SQL Server ou SQL Azure syntaxe](converting-mysql-databases-mysqltosql.md). Si vous créez un rapport, les objets MySQL sont convertis automatiquement au cours de l’évaluation.  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
