---
description: Évaluation des bases de données MySQL pour la conversion (MySQLToSQL)
title: Évaluation des bases de données MySQL pour la conversion (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Assessment reports
ms.assetid: 2a56a003-3b0f-453a-963c-00c9e40933ec
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 61e8cd99640b45cfdfd74de60beb2fcbd9393308
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100069194"
---
# <a name="assessing-mysql-databases-for-conversion-mysqltosql"></a>Évaluation des bases de données MySQL pour la conversion (MySQLToSQL)
Avant de charger des objets et de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, vous devez déterminer le niveau de complexité de la migration et le temps nécessaire à la migration. SSMA peut créer un rapport d’évaluation qui indique le pourcentage d’objets qui seront convertis avec succès. SSMA vous permet également d’afficher les problèmes spécifiques qui provoquent des échecs de conversion.  
  
## <a name="creating-assessment-reports"></a>Création de rapports d’évaluation  
Lorsqu’il crée ce rapport d’évaluation, SSMA convertit les objets de base de données MySQL sélectionnés en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure syntaxe, puis affiche les résultats.  
  
**Pour créer un rapport d’évaluation**  
  
1.  Dans l’Explorateur de métadonnées MySQL, sélectionnez les schémas à évaluer.  
  
2.  Pour omettre des objets spécifiques, désactivez les cases à cocher en regard de celles-ci.  
  
3.  Cliquez avec le bouton droit sur **schémas**, puis sélectionnez **créer un rapport**.  
  
    Cliquez avec le bouton droit sur un objet pour analyser des objets individuels. Sélectionnez ensuite **créer un rapport**.  
  
    SSMA affiche la progression dans la barre d’État en bas de la fenêtre. Si le volet de sortie est visible, vous verrez également des messages dans le volet de sortie.  
  
    Une fois l’évaluation terminée, la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fenêtre Assistant Migration pour MySQL, rapport d’évaluation s’affiche.  
  
## <a name="using-assessment-reports"></a>Utilisation des rapports d’évaluation  
La fenêtre rapport d’évaluation contient trois volets :  
  
-   Le volet gauche contient la hiérarchie des objets inclus dans le rapport d’évaluation. Vous pouvez parcourir la hiérarchie et sélectionner des objets et des catégories d’objets pour afficher les statistiques et le code de conversion.  
  
-   Le contenu du volet droit dépend de l’élément sélectionné dans le volet gauche.  
  
    Si un groupe d’objets est sélectionné, par exemple schéma, le volet droit contient un volet conversion des statistiques et des objets par catégories. Le volet statistiques de conversion affiche les statistiques de conversion pour les objets sélectionnés. Le volet objets par catégorie affiche les statistiques de conversion pour l’objet ou les catégories d’objets.  
  
    Si une fonction, une procédure, une table ou une vue est sélectionnée, le volet droit contient des statistiques, du code source et du code cible.  
  
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
  
4.  Passez en revue les messages d’erreur, puis déterminez ce que vous souhaitez faire avec l’objet qui a provoqué le problème de conversion.  
  
-   Mettez à jour la syntaxe MySQL dans SSMA. Vous pouvez mettre à jour la syntaxe uniquement pour les procédures et les fonctions. Pour mettre à jour la syntaxe, sélectionnez l’objet dans le volet Explorateur de métadonnées MySQL, cliquez sur l’onglet **SQL** , puis modifiez le code SQL. Lorsque vous quittez l’élément, vous êtes invité à enregistrer la syntaxe mise à jour. Vous pouvez afficher les erreurs signalées pour l’objet sous l’onglet **rapport** .  
  
-   Dans MySQL, vous pouvez modifier l’objet MySQL pour supprimer ou réviser le code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [connexion à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-mysql-mysqltosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure l’Explorateur de métadonnées et l’Explorateur de métadonnées MySQL, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure et migrez les données à partir de MySQL.  
  
## <a name="next-step"></a>étape suivante  
[Conversion de bases de données MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/converting-mysql-databases-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
