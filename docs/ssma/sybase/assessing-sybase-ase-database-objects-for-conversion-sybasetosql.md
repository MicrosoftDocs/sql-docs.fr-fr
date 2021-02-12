---
description: Évaluation des objets de base de données SAP ASE pour la conversion (SybaseToSQL)
title: Évaluation des objets de base de données SAP ASE pour la conversion (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: eb996b7c-1eef-4f73-b5e6-2fa6faf7336c
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e0926c350d02bf2cdbb5ae7fe4fdefbda3ef6863
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077220"
---
# <a name="assessing-sap-ase-database-objects-for-conversion-sybasetosql"></a>Évaluation des objets de base de données SAP ASE pour la conversion (SybaseToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL, vous devez déterminer la complexité de la migration et le temps nécessaire. SSMA peut créer un rapport d’évaluation qui indique le pourcentage d’objets et de procédures qui seront correctement convertis en [!INCLUDE[tsql](../../includes/tsql-md.md)] . SSMA vous permet également de consulter les problèmes spécifiques qui peuvent provoquer des échecs de conversion.  
  
## <a name="create-assessment-reports"></a>Créer des rapports d’évaluation  
Lors de la création de ce rapport d’évaluation, SSMA convertit les objets de base de données SAP Adaptive Server Enterprise (ASE) sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou la syntaxe SQL Azure, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées Sybase, sélectionnez les bases de données que vous souhaitez évaluer.  
  
2.  Pour omettre des objets spécifiques, désactivez les cases à cocher en regard des objets que vous ne souhaitez pas évaluer.  
  
3.  Cliquez avec le bouton droit sur **bases de données**, puis sélectionnez **créer un rapport**.  
  
    Vous pouvez également analyser des objets individuels en cliquant avec le bouton droit sur un objet, puis en sélectionnant **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’État en bas de la fenêtre. Si le volet de sortie est visible, vous verrez également tous les messages associés.  
  
    Une fois l’évaluation terminée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fenêtre rapport de Assistant Migration pour Sybase : évaluation s’affiche.  
  
## <a name="use-assessment-reports"></a>Utiliser les rapports d’évaluation  
La fenêtre rapport d’évaluation contient trois volets :  
  
-   Le volet gauche contient la hiérarchie des objets inclus dans le rapport d’évaluation. Vous pouvez parcourir la hiérarchie et sélectionner des objets et des catégories d’objets pour afficher les statistiques et le code de conversion.  
  
-   Le contenu du volet droit varie en fonction de l’élément sélectionné dans le volet gauche.  
  
    Si un groupe d’objets (tel qu’un schéma) ou une table est sélectionné, le volet droit affiche deux volets. Le volet **statistiques de conversion** affiche les statistiques de conversion pour les objets sélectionnés. Le volet **objets par catégorie** affiche les statistiques de conversion pour l’objet ou les catégories d’objets.  
  
    Si une procédure stockée, une vue ou un déclencheur est sélectionné, le volet droit contient des statistiques, du code source et du code cible.  
  
    -   La zone supérieure affiche les statistiques globales de l’objet. Vous devrez peut-être développer les **statistiques** pour afficher ces informations. 
    -   La zone source affiche le code source de l’objet sélectionné dans le volet gauche. Les zones mises en surbrillance indiquent un code source problématique.  
    -   La zone cible affiche le code converti. Le texte en rouge montre le code et les messages d’erreur les plus problématiques.  
  
-   Le volet inférieur affiche des messages de conversion, regroupés par numéro de message. Sélectionnez **Erreurs**, **avertissements** ou **informations** pour afficher les catégories de messages, puis développez un groupe de messages. Cliquez sur un message pour sélectionner l’objet dans le volet gauche, puis affichez les détails dans le volet droit.  
  
## <a name="analyze-conversion-problems-by-using-the-assessment-report"></a>Analyser les problèmes de conversion à l’aide du rapport d’évaluation  
Les **volets des statistiques de conversion** affichent les statistiques de conversion. Si le pourcentage pour une catégorie est inférieur à 100%, vous devez déterminer la raison pour laquelle la conversion a échoué.  
  
**Pour afficher les problèmes de conversion**  
  
1.  Créez le rapport d’évaluation à l’aide des instructions de la procédure précédente.  
  
2.  Dans le volet gauche, développez schémas ou dossiers présentant une icône d’erreur rouge. Continuez à développer les éléments jusqu’à ce que vous ayez sélectionné un élément individuel qui n’a pas pu être converti.  
  
3.  En haut du volet source, sélectionnez **problème suivant**.  
    Le code problématique est mis en surbrillance, tout comme le code connexe dans le volet de **navigation cible** .  
  
4.  Passez en revue les messages d’erreur, puis déterminez ce que vous souhaitez faire avec l’objet qui a provoqué le problème de conversion :  
  
    -   Mettez à jour la syntaxe ASE dans SSMA. Vous pouvez mettre à jour la syntaxe uniquement pour les procédures stockées et les déclencheurs. Pour mettre à jour la syntaxe, sélectionnez l’objet dans le volet de l’Explorateur de métadonnées Sybase, cliquez sur l’onglet **SQL** , puis modifiez le code SQL. Lorsque vous quittez l’élément, vous êtes invité à enregistrer la syntaxe mise à jour. Affichez les erreurs signalées pour l’objet sous l’onglet **rapport** .  
  
    -   Dans ASE, vous pouvez modifier l’objet ASE pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).  
  
    -   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans l’Explorateur de métadonnées Azure SQL et l’Explorateur de métadonnées Sybase, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Azure SQL et migrer les données à partir de ASE.
  
## <a name="next-steps"></a>Étapes suivantes  
[Conversion d’objets de base de données SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/converting-sybase-ase-database-objects-sybasetosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données SAP ASE vers SQL Server Azure SQL Database &#40;SybaseToSQL&#41;](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)  
  
