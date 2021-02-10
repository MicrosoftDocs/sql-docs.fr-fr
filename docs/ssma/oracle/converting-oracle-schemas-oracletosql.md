---
title: Conversion de schémas Oracle (OracleToSQL) | Microsoft Docs
description: Découvrez comment convertir des objets de base de données Oracle en SQL Server des objets de base de données avec SSMA pour Oracle, après avoir défini les options et vous être connecté à Oracle et à SQL Server.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Conversion Results
ms.assetid: e021182d-31da-443d-b110-937f5db27272
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 8e2ee101ec916a920d4b72d924456a52850e8bc7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058754"
---
# <a name="converting-oracle-schemas-oracletosql"></a>Conversion de schémas Oracle (OracleToSQL)
Après vous être connecté à Oracle, connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et définir les options de mappage de données et de projet, vous pouvez convertir des objets de base de données Oracle en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets de base de données.  
  
## <a name="the-conversion-process"></a>Processus de conversion  
La conversion d’objets de base de données prend les définitions d’objets d’Oracle, les convertit en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets similaires, puis charge ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées.  
  
Pendant la conversion, SSMA imprime les messages de sortie dans le volet de sortie et les messages d’erreur dans le volet de Liste d’erreurs. Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données Oracle ou votre processus de conversion pour obtenir les résultats de conversion souhaités.  
  
## <a name="setting-conversion-options"></a>Définition des options de conversion  
Avant de convertir des objets, passez en revue les options de conversion de projet dans la boîte de dialogue **paramètres du projet** . À l’aide de cette boîte de dialogue, vous pouvez définir la manière dont SSMA convertit des fonctions et des variables globales. Pour plus d’informations, consultez [Project settings &#40;Conversion&#41; &#40;OracleToSQL&#41;](../../ssma/oracle/project-settings-conversion-oracletosql.md).  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant répertorie les objets Oracle qui sont convertis et les objets qui en résultent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objets Oracle|Objets SQL Server résultants|  
|-|-|  
|Fonctions|Si la fonction peut être convertie directement en [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crée une fonction.<br /><br />Dans certains cas, la fonction doit être convertie en procédure stockée. Dans ce cas, SSMA crée une procédure stockée et une fonction qui appelle la procédure stockée.|  
|Procédures|Si la procédure peut être convertie directement en [!INCLUDE[tsql](../../includes/tsql-md.md)] , SSMA crée une procédure stockée.<br /><br />Dans certains cas, une procédure stockée doit être appelée dans une transaction autonome. Dans ce cas, SSMA crée deux procédures stockées : une qui implémente la procédure et une autre qui est utilisée pour appeler la procédure stockée d’implémentation.|  
|.|SSMA crée un ensemble de procédures stockées et de fonctions unifiées par des noms d’objets similaires.|  
|Séquences|SSMA crée des objets séquences (SQL Server 2012 ou SQL Server 2014) ou émule des séquences Oracle.|  
|Tables avec des objets dépendants, tels que des index et des déclencheurs|SSMA crée des tables avec des objets dépendants.|  
|Affichage avec des objets dépendants, tels que des déclencheurs|SSMA crée des affichages avec des objets dépendants.|  
|Vues matérialisées|**SSMA crée des vues indexées sur SQL Server, à quelques exceptions près. La conversion échoue si la vue matérialisée comprend une ou plusieurs des constructions suivantes :**<br /><br />Fonction définie par l'utilisateur<br /><br />Champ/fonction/expression non déterministe dans les clauses SELECT, WHERE ou GROUP BY<br /><br />Utilisation d’une colonne de type float dans les clauses SELECT *, WHERE ou GROUP BY (cas particuliers du précédent problème)<br /><br />Type de données personnalisé (notamment les tableaux imbriqués)<br /><br />COUNT (&lt;champ&gt; distinct)<br /><br />FETCH<br /><br />Jointures OUTER (LEFT, RIGHT ou FULL)<br /><br />Sous-requête, autre affichage<br /><br />OVER, RANK, LEAD, LOG<br /><br />MIN, MAX<br /><br />UNION, MINUS, INTERSECT<br /><br />HAVING|  
|Déclencheur|**SSMA crée des déclencheurs en fonction des règles suivantes :**<br /><br />Les déclencheurs BEFORE sont convertis en déclencheurs INSTEAD OF.<br /><br />Les déclencheurs AFTER sont convertis en déclencheurs AFTER.<br /><br />Les déclencheurs INSTEAD OF sont convertis en déclencheurs INSTEAD OF. Plusieurs déclencheurs INSTEAD OF définis sur la même opération sont combinés en un seul déclencheur.<br /><br />Les déclencheurs de niveau ligne sont émulés à l’aide de curseurs.<br /><br />Les déclencheurs en cascade sont convertis en plusieurs déclencheurs individuels.|  
|Synonymes|**Des synonymes sont créés pour les types d’objets suivants :**<br /><br />Tables et tables d’objets<br /><br />Vues et vues d’objets<br /><br />Procédures stockées<br /><br />Fonctions<br /><br />**Les synonymes pour les objets suivants sont résolus et remplacés par des références d’objet directes :**<br /><br />Séquences<br /><br />.<br /><br />Objets de schéma de classe Java<br /><br />Types d'objets définis par l'utilisateur<br /><br />Les synonymes d’un autre synonyme ne peuvent pas être migrés et sont marqués comme des erreurs.<br /><br />Les synonymes ne sont pas créés pour les vues matérialisées.|  
|Types définis par l’utilisateur|**SSMA ne prend pas en charge la conversion de types définis par l’utilisateur. Les types définis par l’utilisateur, y compris son utilisation dans les programmes PL/SQL, sont signalés par des erreurs de conversion spéciales guidées par les règles suivantes :**<br /><br />La colonne de table d’un type défini par l’utilisateur est convertie en VARCHAR (8000).<br /><br />L’argument de type défini par l’utilisateur pour une procédure stockée ou une fonction est converti en VARCHAR (8000).<br /><br />La variable de type défini par l’utilisateur dans le bloc PL/SQL est convertie en VARCHAR (8000).<br /><br />La table des objets est convertie en table standard.<br /><br />L’affichage des objets est converti en vue standard.|  
  
## <a name="converting-oracle-database-objects"></a>Conversion d’objets Oracle Database  
Pour convertir des objets de base de données Oracle, vous devez d’abord sélectionner les objets que vous souhaitez convertir, puis demander à SSMA d’effectuer la conversion. Pour afficher les messages de sortie pendant la conversion, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour convertir des objets Oracle en SQL Server syntaxe**  
  
1.  Dans l’Explorateur de métadonnées Oracle, développez le serveur Oracle, puis **schémas**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir tous les schémas, activez la case à cocher en regard de **schémas**.  
  
    -   Pour convertir ou omettre une base de données, activez la case à cocher en regard du nom du schéma.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez un schéma, puis activez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets individuels, développez le dossier Category, puis activez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez avec le bouton droit sur **schémas** , puis sélectionnez **convertir le schéma**.  
  
    Vous pouvez également convertir des objets ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en sélectionnant **convertir le schéma**.  
  
## <a name="viewing-conversion-problems"></a>Affichage des problèmes de conversion  
Certains objets Oracle ne sont peut-être pas convertis. Vous pouvez déterminer les taux de réussite de la conversion en affichant le rapport de conversion Résumé.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées Oracle, sélectionnez **schémas**.  
  
2.  Dans le volet droit, sélectionnez l’onglet **rapport** .  
  
    Ce rapport affiche le rapport d’évaluation Résumé pour tous les objets de base de données qui ont été évalués ou convertis. Vous pouvez également afficher un rapport de synthèse pour des objets individuels :  
  
    -   Pour afficher le rapport d’un schéma individuel, sélectionnez le schéma dans l’Explorateur de métadonnées Oracle.  
  
    -   Pour afficher le rapport d’un objet individuel, sélectionnez l’objet dans l’Explorateur de métadonnées Oracle. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets qui n’ont pas pu être converties, vous pouvez afficher la syntaxe qui a provoqué l’échec de la conversion.  
  
**Pour afficher des problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées Oracle, développez **schémas**.  
  
2.  Développez le schéma qui affiche une icône d’erreur rouge.  
  
3.  Sous le schéma, développez un dossier avec une icône d’erreur rouge.  
  
4.  Sélectionnez l’objet avec une icône d’erreur rouge.  
  
5.  Dans le volet droit, cliquez sur l’onglet **rapport** .  
  
6.  En haut de l’onglet **rapport** se trouve une liste déroulante. Si la liste affiche des **statistiques**, modifiez la sélection en **source**.  
  
    SSMA affiche le code source et plusieurs boutons immédiatement au-dessus du code.  
  
7.  Cliquez sur le bouton **problème suivant** . Il s’agit d’une icône d’erreur rouge avec une flèche pointant vers la droite.  
  
    SSMA met en surbrillance le premier code source problématique trouvé dans l’objet actuel.  
  
Pour chaque élément qui n’a pas pu être converti, vous devez déterminer ce que vous souhaitez faire avec cet objet :  
  
-   Vous pouvez modifier le code source des procédures sous l’onglet **SQL** .  
  
-   Vous pouvez modifier l’objet dans la base de données Oracle pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à Oracle Database &#40;OracleToSQL&#41;](../../ssma/oracle/connecting-to-oracle-database-oracletosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées et l’Explorateur de métadonnées Oracle, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de migrer des données à partir d’Oracle.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [charger les objets convertis dans SQL Server](loading-converted-database-objects-into-sql-server-oracletosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données Oracle vers SQL Server &#40;OracleToSQL&#41;](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)  
  
