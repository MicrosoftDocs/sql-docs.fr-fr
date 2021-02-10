---
description: Convertir des objets de base de données ASE Sybase (SybaseToSQL)
title: Conversion d’objets de base de données Sybase ASE (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Converting Database Objects
ms.assetid: 509cb65d-2f54-427a-83d7-37919cc4e3e3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 07560faaa3fb3316864216859f79cd5e1b7ff950
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056834"
---
# <a name="converting-sap-ase-database-objects-sybasetosql"></a>Conversion d’objets de base de données SAP ASE (SybaseToSQL)
Après vous être connecté à SAP Adaptive Server Enterprise (ASE), connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL et définir les options de mappage de projet et de données, vous pouvez convertir des objets de base de données SAP Adaptive Server Enterprise (ASE) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets de base de données SQL ou Azure.  
  
## <a name="the-conversion-process"></a>Processus de conversion  
La conversion d’objets de base de données prend les définitions d’objet de l’ASE, les convertit en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets similaires ou SQL Azure, puis charge ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans Azure SQL. Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de l [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 'ou de l’Explorateur de métadonnées Azure SQL.
  
Pendant la conversion, SSMA imprime les messages de sortie dans le volet de sortie et les messages d’erreur dans le volet de **liste d’erreurs** . Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données ASE ou votre processus de conversion pour obtenir les résultats de conversion souhaités.  
  
## <a name="setting-conversion-options"></a>Définition des options de conversion  
Avant de convertir des objets, passez en revue les options de conversion de projet dans la boîte de dialogue **paramètres du projet** . À l’aide de cette boîte de dialogue, vous pouvez définir la manière dont SSMA convertit des fonctions et des variables globales. Pour plus d’informations, consultez [Project settings &#40;Conversion&#41; &#40;SybaseToSQL&#41;](../../ssma/sybase/project-settings-conversion-sybasetosql.md).  
  
## <a name="converting-ase-database-objects"></a>Conversion d’objets de base de données ASE  
Pour convertir des objets de base de données ASE, commencez par sélectionner les objets que vous souhaitez convertir, puis faites en sorte que SSMA effectue la conversion. Pour afficher les messages de sortie pendant la conversion, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour convertir des objets ASE en syntaxe SQL Server ou SQL Azure**  
  
1.  Dans l’Explorateur de métadonnées Sybase, développez le serveur ASE, puis développez **bases de données**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir toutes les bases de données, activez la case à cocher en regard de **bases de données**.  
  
    -   Pour convertir ou omettre une base de données, activez ou désactivez la case à cocher en regard du nom de la base de données.  
  
    -   Pour convertir ou omettre des schémas individuels, développez la base de données, développez **schémas**, puis activez ou désactivez la case à cocher en regard du schéma.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez le schéma, puis activez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets individuels, développez le dossier Category, puis activez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **convertir le schéma**.  
  
    Vous pouvez également convertir des objets ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou dans le dossier qui les contient, puis en sélectionnant **convertir le schéma**.  
  
> [!NOTE]  
> Certaines fonctions du système SAP ASE ne correspondent pas exactement aux fonctions système SQL Server équivalentes dans le comportement. Pour émuler le comportement de SAP ASE, SSMA génère des fonctions définies par l’utilisateur dans la base de données SQL Server convertie sous un schéma appelé 2SS. Selon les paramètres du projet, certaines des fonctions système SQL Server sont remplacées par ces fonctions émulées. SSMA crée les fonctions définies par l’utilisateur suivantes :  

:::row:::
    :::column:::
        **char_length_nvarchar**  
        **char_length_varchar**  
        **charindex_nvarchar**  
        **charindex_varchar**  
        **hextoint**  
        **index_colorder**  
    :::column-end:::
    :::column:::
        **inttohex**  
        **ssma_current_time**  
        **ssma_datediff**  
        **ssma_datepart**  
        **substring_nvarchar**  
        **substring_varbinary**  
    :::column-end:::
    :::column:::
        **substring_varchar**  
        **to_unichar**  
        **uhighsurr**  
        **ulowsurr**  
    :::column-end:::
:::row-end:::

## <a name="objects-not-supported-in-azure-sql"></a>Objets non pris en charge dans Azure SQL  
Les mots clés T-SQL suivants sont utilisés par SSMA pour SAP ASE lors de la conversion en SQL Server localement, mais ces mots clés ne sont pas pris en charge par SQL Azure syntaxe T-SQL :  

:::row:::
    :::column:::
        CHECKPOINT  
        CREATE/ALTER/DROP DEFAULT  
        CREATE/DROP RULE  
        DBCC TRACEOFF  
        DBCC TRACEON  
    :::column-end:::
    :::column:::
        GRANT/REVOKE/DENY ALL  
        KILL  
        READTEXT  
        SELECT INTO  
        SET OFFSETS  
    :::column-end:::
    :::column:::
        SETUSER  
        SHUTDOWN  
        WRITETEXT  
    :::column-end:::
:::row-end:::

## <a name="viewing-conversion-problems"></a>Affichage des problèmes de conversion  
Certains objets SAP ASE peuvent ne pas être convertis. Vous pouvez déterminer les taux de réussite de la conversion en affichant le rapport de conversion Résumé.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées Sybase, sélectionnez **bases de données**.  
  
2.  Dans le volet droit, sélectionnez l’onglet **rapport** .  
  
    Ce rapport affiche le rapport d’évaluation Résumé pour tous les objets de base de données qui ont été évalués ou convertis. Vous pouvez également afficher un rapport de synthèse pour des objets individuels :  
  
    -   Pour afficher le rapport d’une base de données individuelle, sélectionnez la base de données dans l’Explorateur de métadonnées Sybase.  
  
    -   Pour afficher le rapport d’un objet de base de données individuel, sélectionnez l’objet dans l’Explorateur de métadonnées Sybase. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets qui n’ont pas pu être converties, vous pouvez afficher la syntaxe qui a provoqué l’échec de la conversion.  
  
**Pour afficher des problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées Sybase, développez **bases de données**.  
  
2.  Développez la base de données qui affiche une icône d’erreur rouge.  
  
3.  Développez le dossier **schémas** , puis développez le schéma qui affiche une icône d’erreur rouge.  
  
4.  Sous le schéma, développez un dossier avec une icône d’erreur rouge.  
  
5.  Sélectionnez l’objet avec une icône d’erreur rouge.  
  
6.  Dans le volet droit, sélectionnez l’onglet **rapport** .  
  
7.  En haut de l’onglet **rapport** se trouve une liste déroulante. Si la liste affiche des **statistiques**, modifiez la sélection en **source**.  
  
    SSMA affiche le code source et plusieurs boutons immédiatement au-dessus du code.  
  
8.  Sélectionnez **problème suivant**, une icône d’erreur rouge avec une flèche pointant vers la droite.  
  
    SSMA pour SAP ASE met en surbrillance le premier code source problématique trouvé dans l’objet actuel.  
  
Pour chaque élément qui n’a pas pu être converti, vous devez déterminer ce que vous souhaitez faire avec cet objet :  
  
-   Vous pouvez modifier le code source pour les procédures et les déclencheurs sous l’onglet **SQL** .  
  
-   Vous pouvez modifier l’objet SAP ASE pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans l’Explorateur de métadonnées Azure SQL et l’Explorateur de métadonnées Sybase, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL et de migrer des données à partir de SAP ASE.  
  
## <a name="next-steps"></a>Étapes suivantes  
L’étape suivante du processus de migration consiste à [charger des objets de base de données convertis dans SQL Server/SQL Azure (SybaseToSQL)](./loading-converted-database-objects-into-sql-server-sybasetosql.md).  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
