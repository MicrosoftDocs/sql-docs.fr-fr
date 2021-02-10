---
description: Conversion de schémas DB2 (DB2ToSQL)
title: Conversion de schémas DB2 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 7947efc3-ca86-4ec5-87ce-7603059c75a0
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: af91274d4e48ce99894316c730a58d68fd633242
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062874"
---
# <a name="converting-db2-schemas-db2tosql"></a>Conversion de schémas DB2 (DB2ToSQL)
Après vous être connecté à DB2, connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , et définir les options de mappage de données et de projet, vous pouvez convertir les objets de base de données DB2 en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets de base de données.  
  
## <a name="the-conversion-process"></a>Processus de conversion  
La conversion d’objets de base de données prend les définitions d’objets de DB2, les convertit en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] objets similaires, puis charge ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées.  
  
Pendant la conversion, SSMA imprime les messages de sortie dans le volet de sortie et les messages d’erreur dans le volet de Liste d’erreurs. Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données DB2 ou votre processus de conversion pour obtenir les résultats de conversion souhaités.  
  
## <a name="setting-conversion-options"></a>Définition des options de conversion  
Avant de convertir des objets, passez en revue les options de conversion de projet dans la boîte de dialogue **paramètres du projet** . À l’aide de cette boîte de dialogue, vous pouvez définir la manière dont SSMA convertit des fonctions et des variables globales. Pour plus d’informations, consultez [Project settings &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant indique les objets DB2 qui sont convertis et les objets qui en résultent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
|Objets DB2|Objets SQL Server résultants|  
|-----------|----------------------------|  
|Types de données|**SSMA mappe chaque type, à l’exception des éléments suivants :**<br /><br />CLOB : certaines fonctions natives pour travailler avec ce type ne sont pas prises en charge (par exemple, CLOB_EMPTY ())<br /><br />Objet BLOB : certaines fonctions natives pour travailler avec ce type ne sont pas prises en charge (par exemple, BLOB_EMPTY ())<br /><br />DBLOB : certaines fonctions natives pour travailler avec ce type ne sont pas prises en charge (par exemple, DBLOB_EMPTY ())|  
|Types définis par l'utilisateur|**SSMA mappe le suivant défini par l’utilisateur :**<br /><br />Type distinct<br /><br />Type structuré<br /><br />Types de données SQL PL-Remarque : le type de curseur faible n’est pas pris en charge.|  
|Registres spéciaux|**SSMA ne mappe que les registres listés ci-dessous :**<br /><br />HORODATEUR ACTUEL<br /><br />DATE ACTUELLE<br /><br />HEURE ACTUELLE<br /><br />FUSEAU HORAIRE ACTUEL<br /><br />UTILISATEUR ACTUEL<br /><br />SESSION_USER et utilisateur<br /><br />SYSTEM_USER<br /><br />CLIENT_APPLNAME ACTUEL<br /><br />CLIENT_WRKSTNNAME ACTUEL<br /><br />DÉLAI D’ATTENTE DE VERROUILLAGE ACTUEL<br /><br />SCHÉMA ACTUEL<br /><br />SERVEUR ACTUEL<br /><br />ISOLEMENT ACTUEL<br /><br />Les autres registres spéciaux ne sont pas mappés à la sémantique SQL Server.|  
|CREATE TABLE|**SSMA mappe CREATE TABLE avec les exceptions suivantes :**<br /><br />Tables MDC (multidimensionnelle clustering)<br /><br />Tables à plages groupées (RCT)<br /><br />tables partitionnées ;<br /><br />Table détachée<br /><br />Clause de CAPTURE de données<br /><br />Option IMPLICITement MASQUÉe<br /><br />VOLATILe (option)|  
|CREATE VIEW|SSMA Maps crée une vue avec « WITH LOCAL CHECK OPTION », mais d’autres options ne sont pas mappées à la sémantique SQL Server|  
|CREATE INDEX|**SSMA Maps crée un INDEX avec les exceptions suivantes :**<br /><br />Index XML<br /><br />BUSINESS_TIME sans option de CHEVAUCHement<br /><br />Clause PARTITIONNÉe<br /><br />SPÉCIFICATION uniquement (option)<br /><br />Option étendre l’utilisation<br /><br />Option MINPCTUSED<br /><br />Option de FRACTIONNEment de PAGE|  
|Déclencheurs|**SSMA mappe la sémantique de déclencheur suivante :**<br /><br />APRÈS/pour chaque déclencheur de ligne<br /><br />APRÈS chaque déclenchement de l’instruction<br /><br />AVANT/pour chaque ligne et au lieu de/pour chaque déclencheur de ligne|  
|Séquences|Sont mappés.|  
|Instruction SELECT|**Les mappages SSMA SELECT avec les exceptions suivantes :**<br /><br />Clause Data-change-table-Reference : partiellement mappée, mais les tables finales ne sont pas prises en charge<br /><br />Clause de référence de table-mappée partiellement, mais uniquement-table-Reference, externe-table-Reference, analyze_table-expression, collection-Derived-table, XMLTable-expression ne sont pas mappées à la sémantique SQL Server<br /><br />Period-clause de spécification-non mappée.<br /><br />Continue-clause Handler-non mappée.<br /><br />Clause de corrélation typée-non mappée.<br /><br />Clause de résolution d’accès simultanée-non mappée.|  
|Instruction VALUEs|Est mappé.|  
|Instruction INSERT|Est mappé.|  
|Instruction UPDATE|**La mise à jour des mappages SMA est mise à jour avec les exceptions suivantes :**<br /><br />Table-Reference clause-only-la référence de table n’est pas mappée à la sémantique SQL Server<br /><br />La clause period-n’est pas mappée.|  
|Instruction MERGE|**Les mappages SSMA sont FUSIONNés avec les exceptions suivantes :**<br /><br />Une seule ou plusieurs occurrences de chaque clause-est mappée à la sémantique SQL Server pour les occurrences limitées de chaque clause.<br /><br />SIGNAL, clause-n’est pas mappé à la sémantique SQL Server<br /><br />Clauses de mise à jour et de suppression mixtes-ne mappe pas à la sémantique SQL Server<br /><br />Period-clause-n’est pas mappé à la sémantique SQL Server|  
|DELETE (instruction)|**SSMA mappe DELETE avec les exceptions suivantes :**<br /><br />Table-Reference clause-only-la référence de table n’est pas mappée à la sémantique SQL Server<br /><br />Clause period-n’est pas mappé à la sémantique SQL Server|  
|Niveau d’isolation et type de verrou|Est mappé.|  
|Procédures (SQL)|Sont mappés.|  
|Procédures (externes)|Nécessite une mise à jour manuelle.|  
|Procédures (source)|Ne mappez pas à SQL Server sémantique.|  
|Instruction d’assignation|Est mappé.|  
|Instruction CALL pour une procédure|Est mappé.|  
|Instruction CASE|Est mappé.|  
|Instruction FOR|Est mappé.|  
|GOTO (instruction)|Est mappé.|  
|Instruction IF|Est mappé.|  
|Instruction de itération|Est mappé.|  
|LEAVE (instruction)|Est mappé.|  
|LOOP (instruction)|Est mappé.|  
|Instruction REPEAT|Est mappé.|  
|Instruction resigne|Les conditions ne sont pas prises en charge. Les messages peuvent être facultatifs.|  
|Instruction return|Est mappé.|  
|SIGNAL (instruction)|Les conditions ne sont pas prises en charge. Les messages peuvent être facultatifs.|  
|WHILe (instruction)|Est mappé.|  
|Instruction d’extraction des DIAGNOSTICs|**SSMA Maps obtient les DIAGNOSTICs avec les exceptions suivantes :**<br /><br />ROW_COUNT-est mappé.<br /><br />DB2_RETURN_STATUS-est mappé.<br /><br />MESSAGE_TEXT-est mappé.<br /><br />DB2_SQL_NESTING_LEVEL-ne mappe pas à la sémantique SQL Server<br /><br />DB2_TOKEN_STRING-ne mappe pas à la sémantique SQL Server|  
|Curseurs|**SSMA mappe les CURSEURs avec les exceptions suivantes :**<br /><br />Instruction ALLOCATE CURSOR-ne mappe pas à la sémantique SQL Server<br /><br />Instruction ASSOCIATE LOCATORs-ne mappe pas à la sémantique SQL Server<br /><br />Instruction DECLARE CURSOR-la clause returnabilité n’est pas mappée à la sémantique SQL Server<br /><br />Instruction FETCH-mappage partiel. Les variables en tant que cible sont prises en charge uniquement. Le descripteur SQLDA n’est pas mappé à la sémantique SQL Server|  
|Variables|Sont mappés.|  
|Exceptions, gestionnaires et conditions|**SSMA mappe « gestion des exceptions » avec les exceptions suivantes :**<br /><br />Les gestionnaires de sortie sont mappés.<br /><br />Gestionnaires d’annulation-sont mappés.<br /><br />Les gestionnaires de CONTINUation-ne sont pas mappés.<br /><br />Conditions : il ne correspond pas à la sémantique SQL Server.|  
|SQL dynamique|Non mappé.|  
|Alias|Sont mappés.|  
|Surnoms|Mappage partiel. Le traitement manuel est requis pour l’objet sous-jacent|  
|Synonymes|Sont mappés.|  
|Fonctions standard dans DB2|SSMA mappe les fonctions DB2 standard quand une fonction équivalente est disponible dans SQL Server :|  
|Autorisation|Non mappé.|  
|Prédicats|Sont mappés.|  
|SELECT INTO, instruction|Non mappé.|  
|VALEURS dans l’instruction|Non mappé.|  
|Contrôle de transaction|Non mappé.|  
  
## <a name="converting-db2-database-objects"></a>Conversion des objets de base de données DB2  
Pour convertir des objets de base de données DB2, vous devez d’abord sélectionner les objets que vous souhaitez convertir, puis demander à SSMA d’effectuer la conversion. Pour afficher les messages de sortie pendant la conversion, dans le menu **affichage** , sélectionnez **sortie**.  
  
**Pour convertir des objets DB2 en syntaxe SQL Server**  
  
1.  Dans l’Explorateur de métadonnées DB2, développez le serveur DB2, puis **schémas**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir tous les schémas, activez la case à cocher en regard de **schémas**.  
  
    -   Pour convertir ou omettre une base de données, activez la case à cocher en regard du nom du schéma.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez un schéma, puis activez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets individuels, développez le dossier Category, puis activez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez avec le bouton droit sur **schémas** , puis sélectionnez **convertir le schéma**.  
  
    Vous pouvez également convertir des objets ou des catégories d’objets en cliquant avec le bouton droit sur l’objet ou son dossier parent, puis en sélectionnant **convertir le schéma**.  
  
## <a name="viewing-conversion-problems"></a>Affichage des problèmes de conversion  
Certains objets DB2 peuvent ne pas être convertis. Vous pouvez déterminer les taux de réussite de la conversion en affichant le rapport de conversion Résumé.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées DB2, sélectionnez **schémas**.  
  
2.  Dans le volet droit, sélectionnez l’onglet **rapport** .  
  
    Ce rapport affiche le rapport d’évaluation Résumé pour tous les objets de base de données qui ont été évalués ou convertis. Vous pouvez également afficher un rapport de synthèse pour des objets individuels :  
  
    -   Pour afficher le rapport d’un schéma individuel, sélectionnez le schéma dans l’Explorateur de métadonnées DB2.  
  
    -   Pour afficher le rapport d’un objet individuel, sélectionnez l’objet dans l’Explorateur de métadonnées DB2. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets qui n’ont pas pu être converties, vous pouvez afficher la syntaxe qui a provoqué l’échec de la conversion.  
  
**Pour afficher des problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées DB2, développez **schémas**.  
  
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
  
-   Vous pouvez modifier l’objet dans la base de données DB2 pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à la base de données DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées et l’Explorateur de métadonnées DB2, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de migrer les données à partir de DB2.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste à [charger les objets convertis dans SQL Server](./loading-converted-database-objects-into-sql-server-db2tosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
