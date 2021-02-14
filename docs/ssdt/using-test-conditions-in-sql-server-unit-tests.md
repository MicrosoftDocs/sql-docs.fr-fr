---
title: Utilisation de conditions de test dans les tests unitaires SQL Server
description: En savoir plus sur les conditions de test dans des tests unitaires SQL Server. Découvrez comment utiliser des conditions prédéfinies et des tests négatifs et afficher des informations sur les conditions personnalisées.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.unittesting.testconditions
ms.assetid: e3d1c86c-1e58-4d2c-b625-d1b591b221aa
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 18021d85c9b490664411b7bb98f1b55972a13022
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082410"
---
# <a name="using-test-conditions-in-sql-server-unit-tests"></a>Utilisation de conditions de test dans les tests unitaires SQL Server

Dans un test unitaire SQL Server, un ou plusieurs scripts de test Transact\-SQL sont exécutés. Les résultats sont évalués dans le script Transact\-SQL et l'instruction THROW ou RAISERROR utilisée pour retourner une erreur et faire échouer le test, ou des conditions de test peuvent être définies dans le test pour évaluer les résultats. Le test retourne une instance de la classe [SqlExecutionResult](/previous-versions/sql/sql-server-data-tools/jj856590(v=vs.103)). L'instance de cette classe contient un ou plusieurs DataSets, l'heure d'exécution et les lignes affectées par le script. Ces informations sont collectées pendant l'exécution du script. Vous pouvez évaluer ces résultats à l'aide de conditions de test. SQL Server Data Tools fournit un ensemble de conditions de test prédéfinies. Vous pouvez également créer et utiliser des conditions personnalisées ; consultez [Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="predefined-test-conditions"></a>Conditions de test prédéfinies  
Le tableau suivant répertorie les conditions de test prédéfinies que vous pouvez ajouter à l'aide du volet Conditions de test du Concepteur de test unitaire SQL Server.  
  
|**Condition de test**|**Description de la condition de test**|  
|----------------------|----------------------------------|  
|Checksum de données|Échoue si la somme de contrôle du jeu de résultats retourné par le script Transact\-SQL ne correspond pas à la somme de contrôle attendue. Pour plus d'informations, consultez [Spécification d'un checksum de données](#SpecifyDataChecksum).<br /><br />**REMARQUE :** cette condition de test n’est pas recommandée si vous retournez des données qui varient entre les séries de test. Par exemple, si votre jeu de résultats contient des dates ou des heures générées, ou contient des colonnes d'identité, vos tests échouent, car la somme de contrôle est différente à chaque exécution.|  
|Jeu de résultats vide|Échoue si le jeu de résultats retourné par le script Transact\-SQL n'est pas vide.|  
|Durée d'exécution|Échoue si le script de test Transact\-SQL prend plus de temps que prévu pour s'exécuter. La durée d'exécution par défaut est de 30 secondes.<br /><br />La durée d'exécution s'applique au test de script de test uniquement, et non pas au script d'avant test ou d'après test.|  
|Schéma attendu|Échoue si les colonnes et les types de données du jeu de résultats ne correspondent pas à ceux de la condition de test. Vous devez spécifier un schéma via les propriétés de la condition de test. Pour plus d'informations, consultez [Spécification d'un schéma attendu](#SpecifyExpectedSchema).|  
|Non concluant|Génère toujours un test avec un résultat Non concluant. Il s'agit de la condition par défaut ajoutée à chaque test. Cette condition de test est incluse pour indiquer que la vérification du test n'a pas été implémentée. Supprimez cette condition de test de votre test après avoir ajouté d'autres conditions de test.|  
|Jeu de résultats non vide|Échoue si le jeu de résultats est vide. Utilisez cette condition de test ou EmptyResultSet avec la fonction Transact\-SQL @@RAISERROR dans le script de test pour tester si une mise à jour a été effectuée. Par exemple, enregistrez des valeurs avant la mise à jour, exécutez la mise à jour, comparez les valeurs après mise à jour et générez une erreur si vous n'obtenez pas les résultats attendus.|  
|Nombre de lignes|Échoue si le jeu de résultats ne contient pas le nombre de lignes attendu.|  
|Valeur scalaire|Échoue si une valeur spécifique du jeu de résultats n'est pas égale à la valeur spécifiée. La **Valeur attendue** par défaut est Null.|  
  
> [!NOTE]  
> La condition de test Durée d'exécution spécifie une limite de temps pendant laquelle le script de test Transact\-SQL doit s'exécuter. Si cette limite de temps est atteinte, le test échoue. Les résultats des tests incluent également des statistiques de durée, qui diffèrent de la condition de test Durée d'exécution. Les statistiques de durée comprennent non seulement la durée d'exécution, mais également l'heure de connexion à la base de données deux fois ; la durée d'exécuter d'autres scripts de test, tels que le script d'avant test et le script d'après test ; et la durée d'exécution des conditions de test. Par conséquent, un test réussit même si sa durée est plus longue que sa durée d'exécution.  
>   
> La durée enregistrée ne contient pas le temps utilisé pour la génération de données et le déploiement du schéma, car ces opérations se produisent avant l'exécution des tests. Pour afficher la durée des tests, sélectionnez une série de tests dans la fenêtre **Résultats des tests**, cliquez avec le bouton droit, puis choisissez **Afficher les détails des résultats des tests**.  
  
Ajoutez d'autres conditions de test à des tests unitaires SQL Server à l'aide du volet Conditions de test du Concepteur de test unitaire SQL Server. Pour plus d’informations, consultez [Procédure : ajouter des conditions de test à des tests unitaires SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
Vous pouvez également modifier le code de la méthode de test directement pour ajouter des fonctionnalités. Pour plus d’informations, consultez [Procédure : ouvrir un test unitaire SQL Server à modifier](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md) et [Procédure : écrire un test unitaire SQL Server qui s’exécute dans l’étendue d’une seule transaction](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md). Par exemple, ajoutez des fonctionnalités à une méthode de test en ajoutant des instructions Assert. Pour plus d’informations, consultez [Utilisation d'assertions Transact-SQL dans les tests unitaires SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md).  
  
## <a name="expected-failures"></a>Échecs attendus  
Créez des tests unitaires SQL Server pour tester le comportement qui doit échouer. Ces échecs attendus sont parfois appelés test négatif. Voici quelques exemples de cette situation :  
  
-   Vérifier qu'une procédure stockée qui supprime les données d'un client échoue si vous spécifiez un ID de client valide.  
  
-   Vérifier qu'une procédure stockée qui passe une commande échoue si la commande n'a jamais été passée ou si la commande a déjà été passée.  
  
-   Vérifier qu'une procédure stockée qui annule une commande ne peut pas annuler les commandes passées ou les commandes qui ont déjà été annulées.  
  
Définissez des tests unitaires SQL Server pour les procédures stockées qui lèvent des exceptions attendues. Ajoutez un attribut à la méthode de test unitaire pour indiquer la ou les exceptions prévues. Ce faisant, vous empêchez le test d'échouer lorsque l'exception se produit.  
  
Pour marquer une méthode de test unitaire SQL Server par des exceptions attendues, ajoutez l'attribut suivant :  
  
```  
[ExpectedSqlException(MessageNumber = nnnnn, Severity = x, MatchFirstError = false, State = y)]  
```  
  
Où :  
  
-   *nnnnn* est le numéro du message attendu, par exemple 14025.  
  
-   *x* est le niveau de gravité de l'exception attendue.  
  
-   *y* est l'état de l'exception attendue.  
  
Tous les paramètres non spécifiés sont ignorés. Vous passez ces paramètres à l'instruction **THROW** dans votre code de base de données. Si vous spécifiez MatchFirstError = false, l'attribut correspond à une des erreurs SqlErrors dans l'exception. Le comportement par défaut (MatchFirstError = truei) n'est utilisé que pour correspondre à la première erreur qui se produit.  
  
Pour obtenir un exemple montrant comment utiliser les exceptions attendues et un test unitaire de SQL Server négatif, consultez [Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="specifying-a-data-checksum"></a><a name="SpecifyDataChecksum"></a>Spécification d'un checksum de données  
Pour afficher le Concepteur de test unitaire SQL Server, double-cliquez sur le fichier de code source d'un test unitaire dans l'**Explorateur de solutions**.  
  
Après avoir ajouté une condition de test Checksum de données à un test unitaire de base de données, vous devez configurer la somme de contrôle attendue à l'aide de la procédure suivante :  
  
#### <a name="to-specify-an-expected-checksum"></a>Pour spécifier une somme de contrôle attendue  
  
1.  Dans la liste des conditions de test, cliquez sur la condition de test Checksum de données pour laquelle vous souhaitez spécifier une somme de contrôle.  
  
2.  Ouvrez la fenêtre **Propriétés** en appuyant sur F4. Vous pouvez également ouvrir le menu **Affichage** et cliquez sur Fenêtre **Propriétés** .  
  
3.  (Facultatif) Vous devrez peut-être modifier la propriété **(Nom)** de la condition de test de façon à ce qu'elle soit plus descriptive.  
  
4.  Dans la propriété **Configuration**, cliquez sur le bouton Parcourir ( **...** ).  
  
    La boîte de dialogue **Configuration de TestConditionName** s'affiche.  
  
5.  Spécifiez une connexion à la base de données à tester. Pour plus d’informations, consultez [Procédure : créer une connexion de base de données](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Par défaut, le corps Transact\-SQL de votre test s'affiche dans le volet d'édition. Modifiez le code si nécessaire, pour produire les résultats attendus. Par exemple, si le test possède du code dans l'avant test, vous devrez peut-être ajouter ce code.  
  
    > [!IMPORTANT]  
    > Si vous modifiez une condition de somme de contrôle pour laquelle vous aviez précédemment spécifié une somme de contrôle, les modifications apportées dans le volet d'édition ne sont pas enregistrées. Vous devez réeffectuer ces modifications avant de cliquer sur **Récupérer**.  
  
7.  Cliquez sur **Récupérer**.  
  
    Transact\-SQL s'exécute sur la connexion de base de données spécifiée et les résultats s'affichent dans la boîte de dialogue.  
  
8.  Si les résultats correspondent aux résultats attendus du test, cliquez sur **OK**. Sinon, modifiez le corps Transact\-SQL et répétez les étapes 6, 7 et 8 jusqu'à ce que les résultats correspondent à ceux attendus.  
  
    La somme de contrôle attendue apparaît dans la colonne **Valeur** de la condition de test.  
  
## <a name="specifying-an-expected-schema"></a><a name="SpecifyExpectedSchema"></a>Spécification d'un schéma attendu  
Après avoir ajouté une condition de test Schéma attendu à un test unitaire SQL Server, vous devez configurer le schéma attendu à l'aide de la procédure suivante :  
  
#### <a name="to-specify-an-expected-schema"></a>Pour spécifier un schéma attendu  
  
1.  Dans la liste des conditions de test, cliquez sur la condition de test Schéma attendu pour laquelle vous souhaitez spécifier un schéma.  
  
2.  Ouvrez la fenêtre **Propriétés** en appuyant sur F4. Vous pouvez également ouvrir le menu **Affichage** et cliquez sur fenêtre **Propriétés** .  
  
3.  (Facultatif) Vous devrez peut-être modifier la propriété **(Nom)** de la condition de test de façon à ce qu'elle soit plus descriptive.  
  
4.  Dans la propriété **Configuration**, cliquez sur le bouton Parcourir ( **...** ).  
  
    La boîte de dialogue **Configuration de TestConditionName** s'affiche.  
  
5.  Spécifiez une connexion à la base de données à tester. Pour plus d’informations, consultez [Procédure : créer une connexion de base de données](/previous-versions/visualstudio/visual-studio-2010/aa833420(v=vs.100)).  
  
6.  Par défaut, le corps Transact\-SQL de votre test s'affiche dans le volet d'édition. Modifiez le code si nécessaire, pour produire les résultats attendus. Par exemple, si le test possède du code dans l'avant test, vous devrez peut-être ajouter ce code.  
  
    > [!IMPORTANT]  
    > Si vous modifiez une condition de schéma attendu pour laquelle vous aviez précédemment spécifié un schéma, les modifications apportées dans le volet d'édition ne sont pas enregistrées. Vous devez réeffectuer ces modifications avant de cliquer sur **Récupérer**.  
  
7.  Cliquez sur **Récupérer**.  
  
    Transact\-SQL s'exécute sur la connexion de base de données spécifiée et les résultats s'affichent dans la boîte de dialogue. Étant donné que vous vérifiez le schéma, ou la forme, du jeu de résultats et non pas les valeurs des résultats, vous ne devez voir aucune donnée dans les résultats, tant que les colonnes apparaissent comme prévu.  
  
8.  Si les résultats correspondent aux résultats attendus du test, cliquez sur **OK**. Sinon, modifiez le corps Transact\-SQL et répétez les étapes 6, 7 et 8 jusqu'à ce que les résultats correspondent à ceux attendus.  
  
    Des informations sur le schéma attendu apparaissent dans la colonne **Valeur** de la condition de test. Par exemple, « Attendu : 2 tables » peut s’afficher.  
  
## <a name="extensible-test-conditions"></a>Conditions de test extensibles  
Outre les six conditions de test prédéfinies, vous pouvez écrire de nouvelles conditions de test. Ces conditions de test seront affichées dans le volet Conditions de test du Concepteur de test unitaire SQL Server. Pour plus d’informations, consultez [Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Utilisation d'assertions Transact-SQL dans les tests unitaires SQL Server](../ssdt/using-transact-sql-assertions-in-sql-server-unit-tests.md)  
[Scripts des tests unitaires SQL Server](../ssdt/scripts-in-sql-server-unit-tests.md)  
