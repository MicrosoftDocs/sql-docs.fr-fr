---
title: Comparer et synchroniser les données de deux bases de données
description: Découvrez comment comparer et synchroniser les données de deux bases de données. Découvrez comment configurer la comparaison, afficher les différences et mettre à jour la cible.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.datacompare.connection.datasources.f1
- sql.data.tools.datacompare.watermark.f1
- sql.data.tools.datacompare.connection.objectselection.f1
ms.assetid: 2148e517-ed42-41c6-b753-1ac625f594c8
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 73dcdd57b018f980291eceefa1be37d1b51df13a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018439"
---
# <a name="how-to-compare-and-synchronize-the-data-of-two-databases"></a>Procédure : Comparer et synchroniser les données de deux bases de données

Vous pouvez comparer les données contenues dans deux bases de données. Les bases de données que vous comparez sont appelées *source* et *cible*.  
  
> [!NOTE]  
> Les *projets de base de données* et les packages .dacpac ou .bacpac ne peuvent pas être la source ou la cible d'une comparaison de données.  
  
Lorsque les données sont comparées, un script de *langage de manipulation de données* (DML) est généré, qui permet de synchroniser les bases de données distinctes en mettant à jour une partie ou l'intégralité des données dans la base de données cible. Lorsque la comparaison de données se termine, les résultats s'affichent dans la fenêtre de comparaisons de données de Visual Studio.  
  
Une fois la comparaison terminée, vous pouvez effectuer d'autres actions :  
  
-   Vous pouvez afficher les différences entre les deux bases de données. Pour plus d'informations, consultez [Afficher les différences entre les données](#ViewDifferences).  
  
-   Vous pouvez mettre à jour une partie ou l'intégralité de la cible pour qu'elle corresponde à la source. Pour plus d'informations, consultez [Synchronisation des données d'une base de données](#Synchronize).  
  
Pour plus d'informations, consultez [Comparer et synchroniser des données d'une ou plusieurs tables avec des données d'une base de données de référence](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
> [!NOTE]  
> Vous pouvez également comparer le *schéma* de deux bases de données ou de deux versions de la même base de données. Pour plus d’informations, consultez [Procédure : utiliser le schéma pour comparer différentes définitions de base de données](../ssdt/how-to-use-schema-compare-to-compare-different-database-definitions.md).  
  
## <a name="comparing-database-data"></a><a name="CompareDatabaseData"></a>Comparaison des données d'une base de données  
  
#### <a name="to-compare-data-by-using-the-new-data-comparison-wizard"></a>Pour comparer des données à l'aide de l'Assistant Nouvelle comparaison de données  
  
1.  Dans le menu **SQL**, pointez sur **Comparaison de données**, puis cliquez sur **Nouvelle comparaison de données**.  
  
    L'Assistant Nouvelle comparaison de données s'affiche. En outre, la fenêtre Comparaison de données s'ouvre et Visual Studio lui assigne automatiquement un nom, tel que DataCompare1.  
  
2.  Identifiez les bases de données source et cible.  
  
    Si la liste de la **Base de données source** ou la liste de la **Base de données cible** est vide, cliquez sur **Nouvelle connexion**. Dans la boîte de dialogue **Propriétés de connexion**, identifiez le serveur sur lequel la base de données réside et le type d'authentification à utiliser lors de la connexion à la base de données. Cliquez ensuite sur **OK** pour fermer la boîte de dialogue **Propriétés de la connexion** et pour retourner à l'Assistant Comparaison de données.  
  
    Dans la première page de l'Assistant Comparaison de données, vérifiez que les informations pour chaque base de données sont correctes, spécifiez les enregistrements que vous souhaitez inclure dans les résultats, puis cliquez sur **Suivan**. La deuxième page de l'Assistant Comparaison de données apparaît et affiche une liste hiérarchique des tables et des vues de la base de données.  
  
3.  Activez les cases à cocher correspondant aux tables et vues à comparer. Éventuellement, développez les nœuds des objets de base de données, puis activez les cases à cocher des colonnes dans les objets à comparer.  
  
    > [!NOTE]  
    > Les tables et les vues doivent répondre à deux critères pour apparaître dans la liste. En premier lieu, les schémas des objets doivent correspondre entre les bases de données source et cible. Ensuite, seules les tables et les vues qui ont une clé primaire, une clé unique, un index unique ou une seule contrainte figurent dans la liste. Si aucune table ou vue ne répond aux deux critères, la liste est vide.  
  
4.  En présence de plusieurs clés, vous pouvez utiliser la colonne **Clé de comparaison** pour spécifier la clé sur laquelle baser la comparaison de données. Par exemple, vous pouvez spécifier si la comparaison doit se faire selon la colonne de clé primaire ou selon une autre colonne clé (à identification unique).  
  
5.  Cliquez sur **Terminer**.  
  
    La comparaison démarre.  
  
    > [!NOTE]  
    > Vous pouvez arrêter une opération de comparaison de données en cours en ouvrant le menu **SQL**, en cliquant sur **Comparaison de données**, puis en cliquant sur **Arrêter la comparaison des données**.  
  
    Lorsque la comparaison est terminée, vous pouvez afficher les différences de données entre les deux bases de données. Vous pouvez également mettre à jour une partie ou l'ensemble des données dans la base de données cible pour qu'elles correspondent aux données dans la base de données source.  
  
#### <a name="to-compare-data-by-using-the-visual-studio-automation-model"></a>Pour comparer des données à l'aide du modèle d'automatisation de Visual Studio  
  
1.  Ouvrez le menu **Affichage**, pointez sur **Autres fenêtres**, puis cliquez sur **Fenêtre Commande**.  
  
2.  Dans la fenêtre Commande, tapez la commande suivante :  
  
    ```  
    Sql.NewDataComparison /SrcServerName sServerName /SrcDatabaseName sDatabaseName /SrcUserName sUserName /SrcPassword sPassword /SrcDisplayName sDisplayName /TargetServerName tServerName /TargetDatabaseName tDatabaseName /TargeUserName tUserName /TargetPassword tPassword /TargetDisplayName tDisplayName  
    ```  
  
    Remplacez les espaces réservés (*sServerName*, *sDatabaseName*, *sUserName*, *sPassword*, *sDisplayName*, *tServerName*, *tDatabaseName*, *tUserName*, *tPassword* et *tDisplayName*) par les valeurs de vos bases de données source et cible.  
  
    Si vous ne spécifiez pas une source et une cible, la boîte de dialogue **Nouvelle comparaison de données** s'affiche. Pour plus d'informations sur les paramètres de la commande Sql.NewDataComparison, consultez [Référence des commandes Automation pour les fonctionnalités de base de données de Visual Studio Team System](/previous-versions/visualstudio/visual-studio-2010/dd470565(v=vs.100)).  
  
    Les données des bases de données source et cibles spécifiées sont comparées. Les résultats s'affichent dans une session de comparaisons de données. Pour plus d'informations sur l'affichage des résultats ou la synchronisation des données, consultez [Afficher les différences entre les données](#ViewDifferences) et [Synchronisation des données d'une base de données](#Synchronize).  
  
## <a name="viewing-data-differences"></a><a name="ViewDifferences"></a>Afficher les différences entre les données  
Après avoir comparé les données dans les deux bases de données, Comparaison de données répertorie chaque *objet de base de données* que vous avez comparé et son état. Vous pouvez également afficher les résultats des enregistrements dans chaque objet, regroupés par état. Pour plus d'informations sur les désignations d’état, consultez [Comparer et synchroniser des données d'une ou plusieurs tables avec des données d'une base de données de référence](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
Après avoir affiché les différences, vous pouvez mettre à jour la cible pour qu'elle corresponde à la source pour une partie ou l'ensemble des objets ou des enregistrements qui sont différents, manquants ou nouveaux. Pour plus d'informations, consultez [Synchronisation des données d'une base de données](#Synchronize).  
  
#### <a name="to-view-data-differences"></a>Pour afficher les différences entre les données  
  
1.  Comparer des données dans une base de données source et une base de données cible. Pour plus d'informations, consultez [Comparer des données d'une base de données](#CompareDatabaseData).  
  
2.  (Facultatif) Effectuez une des actions suivantes ou les deux :  
  
    -   Par défaut, les résultats pour tous les objets apparaissent, indépendamment de leur état. Pour afficher uniquement les objets qui ont un état particulier, cliquez sur une option dans la liste **Filtrer**.  
  
    -   Pour afficher les résultats des enregistrements dans un objet particulier, cliquez sur l'objet dans le volet de résultats principal, puis cliquez sur un onglet dans le volet de vue des enregistrements. Chaque onglet affiche tous les enregistrements de cet objet qui ont un état particulier : différent, uniquement dans la source, uniquement dans la cible et en double. Les données s'affichent par enregistrement et colonne.  
  
## <a name="synchronizing-database-data"></a><a name="Synchronize"></a>Synchronisation des données d'une base de données  
Après avoir comparé les données dans deux bases de données, vous pouvez les synchroniser en mettant à jour l'ensemble ou une partie de la cible pour qu'elle corresponde à la source. Vous pouvez comparer les données dans deux types d'objets de base de données : les tables et les vues.  
  
#### <a name="to-update-target-data-by-using-the-write-updates-command"></a>Pour mettre à jour les données cibles à l'aide de la commande d'écriture de mises à jour  
  
1.  Comparer des données dans une base de données source et une base de données cible. Pour plus d'informations, consultez [Comparer des données d'une base de données](#CompareDatabaseData).  
  
    Une fois la comparaison terminé, la fenêtre de comparaisons de données affiche les résultats pour les objets qui ont été comparés. Quatre colonnes (nommées Enregistrements différents, Uniquement dans la source, Uniquement dans la cible et Enregistrements en double) affichent des informations sur les objets différents. Pour chacun de ces objets, ces colonnes affichent le nombre d'enregistrements différents et le nombre d'enregistrements qu'une opération de mise à jour modifierait. Ces deux chiffres correspondent au début, mais à l'étape 4 vous pouvez changer les objets à mettre à jour.  
  
    Pour plus d'informations, consultez [Comparer et synchroniser des données d'une ou plusieurs tables avec des données d'une base de données de référence](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  Dans la table de la fenêtre Comparaison de données, cliquez sur une ligne.  
  
    Le volet détails donne les résultats des enregistrements dans l'objet de base de données sélectionné. Les enregistrements sont regroupées par état dans des onglets, qui permettent de spécifier les données qui seront transmises de la source à la cible.  
  
3.  Dans le volet d'informations, cliquez sur un onglet dont le nom contient un nombre différent de zéro (0).  
  
    La colonne **Mettre à jour** de la table de **Uniquement dans la cible** contient des cases à cocher que vous pouvez activer pour sélectionner les lignes à mettre à jour. Par défaut, chaque case est cochée.  
  
4.  Désactivez les cases des enregistrements dans la cible dont vous ne souhaitez pas mettre à jour des données de la source.  
  
    Lorsque vous désactivez une case à cocher, vous réduisez le nombre d'enregistrements à mettre à jour, et l'affichage change pour refléter vos actions. Ce nombre apparaît dans la ligne d'état du volet d'informations et dans la colonne correspondante dans le volet de résultats principal, comme indiqué à l'étape 1.  
  
5.  (Facultatif) cliquez sur **Générer un script**.  
  
    Une fenêtre d'éditeur Transact\-SQL s'ouvre et affiche le script en *Langage de manipulation de données* (DML) qui sera utilisé pour mettre à jour la cible.  
  
6.  Pour synchroniser les enregistrements qui sont différents, manquants ou nouveaux, cliquez sur **Mettre à jour la cible**.  
  
    > [!NOTE]  
    > Lorsque la base de données cible est mise à jour, vous pouvez annuler l'opération en cliquant sur **Arrêter l'écriture dans la cible**.  
  
    Les données des enregistrements sélectionnés dans la cible sont mises à jour avec les données des enregistrements correspondants dans la source.  
  
    > [!NOTE]  
    > Si vous choisissez de mettre à jour des vues indexées, l'opération **Mettre à jour la cible** peut échouer si cette action entraîne l'insertion de doublons de clés dans la même table.  
  
#### <a name="to-update-target-data-by-using-a-transact-sql-script"></a>Pour mettre à jour les données cibles à l'aide d'un script Transact\-SQL  
  
1.  Comparer des données dans une base de données source et une base de données cible. Pour plus d'informations, consultez [Comparer des données d'une base de données](#CompareDatabaseData).  
  
    Une fois la comparaison terminé, la fenêtre Comparaison de données affiche les objets qui ont été comparés. Pour plus d'informations, consultez [Comparer et synchroniser des données d'une ou plusieurs tables avec des données d'une base de données de référence](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md).  
  
2.  (Facultatif) Dans le volet d'informations, désactivez les cases à cocher pour les enregistrements dans la cible que vous ne souhaitez pas mettre à jour, comme décrit dans la procédure précédente.  
  
3.  Cliquez sur **Générer un script**.  
  
    Une nouvelle fenêtre affiche le script Transact\-SQL qui transmettrait les modifications nécessaires à apporter aux données dans la base de données cible pour qu'elles correspondent à celles de la base de données source. Une nouvelle fenêtre porte un nom tel que **DataUpdate_Database_1.sql**.  
  
    Ce script reflète les modifications apportées dans le volet d'informations. Par exemple, vous pouvez avoir désactivé la case à cocher pour une ligne donnée dans la page Uniquement dans la cible pour la table [dbo].[Shippers]. Dans ce cas, le script ne mettrait pas à jour la ligne.  
  
4.  (Facultatif) Modifiez ce script dans la fenêtre **DataUpdate_Database_1.sql**.  
  
5.  (Facultatif mais recommandé) Sauvegardez la base de données cible.  
  
6.  Cliquez sur **Exécuter** pour mettre à jour la base de données cible.  
  
    Spécifiez une connexion à la base de données cible à mettre à jour.  
  
    > [!IMPORTANT]  
    > Par défaut, les mises à jour se produisent dans la portée d'une transaction. En cas d'erreur, vous pouvez restaurer la mise à jour entière. Vous pouvez modifier ce comportement.  
  
    Les données des enregistrements sélectionnés dans la cible sont mises à jour avec les données des enregistrements correspondants dans la source.  
  
## <a name="see-also"></a>Voir aussi  
[Comparer et synchroniser des données d'une ou plusieurs tables avec des données d'une base de données de référence](../ssdt/compare-and-synchronize-data-in-tables-with-data-in-reference-database.md)  
