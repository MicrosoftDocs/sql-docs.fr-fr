---
description: Évaluation des schémas DB2 pour la conversion (DB2ToSQL)
title: Évaluation des schémas DB2 pour la conversion (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8892f5a4-72ba-4406-8649-7a9d67f4c1d9
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4fd80f6dda35ffbdbb28bbe783047bd787bc81ac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065974"
---
# <a name="assessing-db2-schemas-for-conversion-db2tosql"></a>Évaluation des schémas DB2 pour la conversion (DB2ToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous devez déterminer le niveau de complexité de la migration et le temps nécessaire à la migration. SSMA peut créer un rapport d’évaluation qui indique le pourcentage d’objets qui seront convertis avec succès. SSMA vous permet également d’afficher les problèmes spécifiques qui provoquent des échecs de conversion.  
  
## <a name="creating-assessment-reports"></a>Création de rapports d’évaluation  
Lorsqu’il crée ce rapport d’évaluation, SSMA convertit les objets de base de données DB2 sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] syntaxe, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées DB2, sélectionnez les schémas à évaluer.  
  
2.  Pour omettre des objets spécifiques, désactivez les cases à cocher en regard de celles-ci.  
  
3.  Cliquez avec le bouton droit sur **schémas**, puis sélectionnez **créer un rapport**.  
  
    Vous pouvez également analyser des objets individuels en cliquant avec le bouton droit sur un objet, puis en sélectionnant **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’État en bas de la fenêtre. Si le volet de sortie est visible, vous verrez également des messages dans le volet de sortie.  
  
    Une fois l’évaluation terminée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fenêtre Assistant Migration pour DB2 : rapport d’évaluation s’affiche.  
  
## <a name="using-assessment-reports"></a>Utilisation des rapports d’évaluation  
La fenêtre rapport d’évaluation contient trois volets :  
  
-   Le volet gauche contient la hiérarchie des objets inclus dans le rapport d’évaluation. Vous pouvez parcourir la hiérarchie et sélectionner des objets et des catégories d’objets pour afficher les statistiques et le code de conversion.  
  
-   Le contenu du volet droit dépend de l’élément sélectionné dans le volet gauche.  
  
    Si un groupe d’objets est sélectionné, tel qu’un schéma, ou si une table est sélectionnée, le volet de droite contient un volet des statistiques de conversion et un volet objets par catégories. Le volet statistiques de conversion affiche les statistiques de conversion pour les objets sélectionnés. Le volet objets par catégorie affiche les statistiques de conversion pour l’objet ou les catégories d’objets.  
  
    Si une fonction, un package, une procédure, une séquence ou une vue est sélectionné, le volet droit contient des statistiques, du code source et du code cible.  
  
    -   La zone supérieure affiche les statistiques globales de l’objet. Vous devrez peut-être développer les **statistiques** pour afficher ces informations.  
  
    -   La zone source affiche le code source de l’objet sélectionné dans le volet gauche. Les zones mises en surbrillance indiquent un code source problématique.  
  
    -   La zone cible affiche le code converti. Le texte en rouge montre le code et les messages d’erreur les plus problématiques.  
  
-   Le volet inférieur affiche des messages de conversion, regroupés par numéro de message. Vous pouvez cliquer sur **Erreurs**, **avertissements** ou **informations** pour afficher les catégories de messages, puis développer un groupe de messages. Cliquez sur un message pour sélectionner l’objet dans le volet gauche et afficher les détails dans le volet droit.  
  
## <a name="analyzing-conversion-problems-by-using-the-assessment-report"></a>Analyse des problèmes de conversion à l’aide du rapport d’évaluation  
Le volet statistiques de conversion affiche les statistiques de conversion. Si le pourcentage pour une catégorie est inférieur à 100%, vous devez déterminer la raison pour laquelle la conversion a échoué.  
  
**Pour afficher les problèmes de conversion**  
  
1.  Créez le rapport d’évaluation à l’aide des instructions de la procédure précédente.  
  
2.  Dans le volet gauche, développez schémas ou dossiers présentant une icône d’erreur rouge. Continuez à développer les éléments jusqu’à ce que vous ayez sélectionné un élément individuel qui n’a pas pu être converti.  
  
3.  En haut du volet source, cliquez sur **problème suivant**.  
  
    Le code problématique est mis en surbrillance, tout comme le code connexe dans le volet de navigation cible.  
  
4.  Passez en revue les messages d’erreur, puis déterminez ce que vous souhaitez faire avec l’objet qui a provoqué le problème de conversion :  
  
    -   Mettez à jour la syntaxe DB2 dans SSMA. Vous pouvez mettre à jour la syntaxe des procédures, des fonctions, des déclencheurs, des fonctions packagées et des procédures empaquetées. Pour mettre à jour la syntaxe, sélectionnez l’objet dans le volet Explorateur de métadonnées DB2, cliquez sur l’onglet **SQL** , puis modifiez le code SQL. Lorsque vous quittez l’élément, vous êtes invité à enregistrer la syntaxe mise à jour. Vous pouvez afficher les erreurs signalées pour l’objet sous l’onglet **rapport** .  
  
    -   Dans DB2, vous pouvez modifier l’objet DB2 pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à la base de données DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md).  
  
    -   Vous pouvez exclure l’objet de la migration. Dans l' [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Explorateur de métadonnées et l’Explorateur de métadonnées DB2, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de migrer les données à partir de DB2.  
  
## <a name="next-step"></a>étape suivante  
[Conversion de schémas DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/converting-db2-schemas-db2tosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données DB2 vers SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-databases-to-sql-server-db2tosql.md)  
  
