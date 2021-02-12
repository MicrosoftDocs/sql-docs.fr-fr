---
title: Conversion de bases de données MySQL (MySQLToSQL) | Microsoft Docs
description: Découvrez comment convertir des objets de base de données MySQL en objets SQL Server ou Azure SQL Database avec SSMA, après vous être connecté et définir les options de mappage de données et de projet.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: ac21850b-fb32-4704-9985-5759b7c688c7
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61298bb7e2c65e85959b52ab3750dc49a5105504
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068986"
---
# <a name="converting-mysql-databases-mysqltosql"></a>Conversion de bases de données MySQL (MySQLToSQL)
Après vous être connecté à MySQL, connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure et définir les options de mappage de projet et de données, vous pouvez convertir des objets de base de données MySQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets ou Azure SQL Database.  
  
## <a name="the-conversion-process"></a>Processus de conversion  
La conversion d’objets de base de données prend les définitions d’objets de MySQL, les convertit en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets similaires ou SQL Azure, puis charge ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées ou SQL Azure.  
  
Pendant la conversion, SSMA imprime les messages de sortie dans le volet de sortie et les messages d’erreur dans le volet de Liste d’erreurs. Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données MySQL ou votre processus de conversion pour obtenir les résultats de conversion souhaités.  
  
## <a name="setting-conversion-options"></a>Définition des options de conversion  
Avant de convertir des objets, passez en revue les options de conversion de projet dans la boîte de dialogue **paramètres du projet** . À l’aide de cette boîte de dialogue, vous pouvez définir la manière dont SSMA convertit les tables et les index. Pour plus d’informations, consultez [paramètres du projet &#40;Conversion&#41; &#40;MySQLToSQL&#41;](../../ssma/mysql/project-settings-conversion-mysqltosql.md)  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant répertorie les objets MySQL qui sont convertis et les objets qui en résultent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objets MySQL|Objets SQL Server résultants|  
|-|-|  
|Tables avec objets dépendants, tels que les index|SSMA crée des tables avec des objets dépendants. La table est convertie avec tous les index et contraintes. Les index sont convertis en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets distincts.<br /><br />Le **mappage de type de données spatiales** ne peut être effectué qu’au niveau du nœud de table.<br /><br />Pour plus d’informations sur les paramètres de conversion de table, consultez [paramètres de conversion](conversion-settings-mysqltosql.md) .|  
|Fonctions|Si la fonction peut être convertie directement en Transact-SQL, SSMA crée une fonction. Dans certains cas, la fonction doit être convertie en procédure stockée. Pour ce faire, vous pouvez utiliser la **conversion de fonction** dans les paramètres du projet. Dans ce cas, SSMA crée une procédure stockée et une fonction qui appelle la procédure stockée.<br /><br />**Choix donnés :**<br /><br />Convertir en fonction des paramètres du projet<br /><br />Convertir en fonction<br /><br />Convertir en procédure stockée<br /><br />Pour plus d’informations sur les paramètres de conversion des fonctions, consultez [paramètres de conversion](conversion-settings-mysqltosql.md)|  
|Procédures|Si la procédure peut être convertie directement en Transact-SQL, SSMA crée une procédure stockée. Dans certains cas, une procédure stockée doit être appelée dans une transaction autonome. Dans ce cas, SSMA crée deux procédures stockées : une qui implémente la procédure et une autre qui est utilisée pour appeler la procédure stockée d’implémentation.|  
|Conversion de base de données|Les bases de données en tant qu’objets MySQL ne sont pas directement converties par SSMA pour MySQL. Les bases de données MySQL sont traitées plus comme des noms de schéma et tous les paramètres physiques sont perdus pendant la conversion. SSMA pour MySQL utilise le [mappage des bases de données MySQL aux schémas de SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/mapping-mysql-databases-to-sql-server-schemas-mysqltosql.md) pour mapper les objets de la base de données MySQL à la paire de base de données/schéma appropriée SQL Server.|  
|Conversion de déclencheur|**SSMA crée des déclencheurs en fonction des règles suivantes :**<br /><br />Les déclencheurs BEFORe sont convertis en déclencheurs INSTEAD OF T-SQL<br /><br />Les déclencheurs AFTER sont convertis en déclencheurs T-SQL avec ou sans itérations par ligne.|  
|Afficher la conversion|SSMA crée des vues avec des objets dépendants|  
|Conversion d’instruction|-Chaque objet instruction SQL peut contenir une seule instruction MySQL (comme DDL, DML et d’autres types d’instructions) ou BEGIN... Bloc de fin.<br />-   **Conversion à États multiversions : Begin...** L’instruction SQL de conversion de bloc end peut également contenir un Begin... Bloc END comme un bloc dans la définition de la procédure, de la fonction ou du déclencheur. Ces blocs doivent être convertis de la même façon que pour les objets d’instruction MySQL uniques.|  
  
## <a name="converting-mysql-database-objects"></a>Conversion d’objets de base de données MySQL  
Pour convertir des objets de base de données MySQL, vous devez d’abord sélectionner les objets que vous souhaitez convertir, puis demander à SSMA d’effectuer la conversion. Pour afficher les messages de sortie pendant la conversion, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour convertir des objets MySQL en syntaxe SQL Server ou SQL Azure**  
  
1.  Dans l’Explorateur de métadonnées MySQL, développez le serveur MySQL, puis développez **bases de données**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir tous les schémas, activez la case à cocher en regard de **bases de données**.  
  
    -   Pour convertir ou omettre une base de données, activez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez un schéma, puis activez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets individuels, développez le dossier Category, puis activez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **convertir le schéma**.  
  
    Vous pouvez également convertir des objets ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en sélectionnant **convertir le schéma**.  
  
## <a name="viewing-conversion-problems"></a>Affichage des problèmes de conversion  
Certains objets MySQL ne sont peut-être pas convertis. Vous pouvez déterminer les taux de réussite de la conversion en affichant le rapport de conversion Résumé.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez **bases de données**.  
  
2.  Dans le volet droit, sélectionnez l’onglet **rapport** .  
  
    Ce rapport affiche le rapport d’évaluation Résumé pour tous les objets de base de données qui ont été évalués ou convertis. Vous pouvez également afficher un rapport de synthèse pour des objets individuels :  
  
    -   Pour afficher le rapport d’un schéma individuel, sélectionnez la base de données dans l’Explorateur de métadonnées MySQL.  
  
    -   Pour afficher le rapport d’un objet individuel, sélectionnez l’objet dans l’Explorateur de métadonnées MySQL. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets qui n’ont pas pu être converties, vous pouvez afficher la syntaxe qui a provoqué l’échec de la conversion.  
  
**Pour afficher des problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées MySQL, développez **bases de données**.  
  
2.  Développez la base de données qui affiche une icône d’erreur rouge.  
  
3.  Sous la base de données, développez un dossier avec une icône d’erreur rouge.  
  
4.  Sélectionnez l’objet avec une icône d’erreur rouge.  
  
5.  Dans le volet droit, cliquez sur l’onglet **rapport** .  
  
6.  En haut de l’onglet **rapport** se trouve une liste déroulante. Si la liste affiche des **statistiques**, modifiez la sélection en **source**.  
  
    SSMA affiche le code source et plusieurs boutons immédiatement au-dessus du code.  
  
7.  Cliquez sur le bouton **problème suivant** . Il s’agit d’une icône d’erreur rouge avec une flèche pointant vers la droite.  
  
    SSMA met en surbrillance le premier code source problématique trouvé dans l’objet actuel.  
  
Pour chaque élément qui n’a pas pu être converti, vous devez déterminer ce que vous souhaitez faire avec cet objet :  
  
-   Vous pouvez modifier l’objet dans la base de données MySQL pour supprimer ou modifier le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md)  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées et l’Explorateur de métadonnées MySQL, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure et de migrer des données à partir de MySQL.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [charger des objets de base de données convertis dans SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/loading-converted-database-objects-into-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
