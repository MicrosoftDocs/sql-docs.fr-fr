---
title: Utilisation d'assertions Transact-SQL dans les tests unitaires SQL Server
description: Découvrez les assertions Transact-SQL. Découvrez quand utiliser des assertions dans des tests unitaires SQL Server et quand utiliser des conditions de test et affichez des exemples d’utilisation d’assertion.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 55d8be9c-9282-47d3-be7f-e2c26f00c95e
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 23dff8f246d741896a042486f0dbe6c95067a8d9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066760"
---
# <a name="using-transact-sql-assertions-in-sql-server-unit-tests"></a>Utilisation d'assertions Transact-SQL dans les tests unitaires SQL Server

Dans un test unitaire SQL Server, un script de test Transact\-SQL s'exécute et retourne un résultat. Parfois, les résultats sont retournés sous la forme d'un jeu de résultats. Validez les résultats à l'aide de conditions de test. Par exemple, utilisez une condition de test pour vérifier le nombre de lignes qui ont été retournées dans un jeu de résultats spécifique ou pour vérifier le temps qui a été nécessaire à l'exécution d'un test spécifique. Pour plus d’informations sur les conditions de test, consultez [Utilisation de conditions de test dans les tests unitaires SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md).  
  
Au lieu d'utiliser des conditions de test, vous pouvez également utiliser des assertions Transact\-SQL, qui sont des instructions THROW ou RAISERROR dans un script Transact\-SQL. Dans certaines circonstances, vous devrez peut-être utiliser une assertion Transact\-SQL plutôt qu'une condition de test.  
  
## <a name="using-transact-sql-assertions"></a>Utilisation d'assertions Transact-SQL  
Vous devez tenir compte des points suivants avant de décider de valider des données en utilisant des assertions Transact\-SQL ou des conditions de test.  
  
-   **Performances**. Il est plus rapide d'exécuter une assertion Transact\-SQL sur le serveur que de déplacer les données vers un ordinateur client et de les manipuler localement.  
  
-   **Connaissance du langage**. Vous pouvez préférer un langage particulier selon vos compétences et donc choisir des assertions Transact\-SQL, ou bien des conditions de test Visual C\# ou Visual Basic.  
  
-   **Validation compliquée**. Dans certains cas, vous pouvez créer des tests plus complexes dans Visual C\# ou Visual Basic et valider vos tests sur le client.  
  
-   **Simplicité**. Il est souvent plus simple d'utiliser une condition de test prédéfinie que d'écrire le script équivalent dans Transact\-SQL.  
  
-   **Bibliothèques de validation héritées**. Si vous disposez déjà du code qui effectue la validation, utilisez-le dans un test unitaire SQL Server plutôt que d'utiliser des conditions de test.  
  
## <a name="mark-unit-test-methods-with-the-expected-exception"></a>Marquer les méthodes de test unitaire par l'exception attendue  
Pour marquer une méthode de test unitaire SQL Server par des exceptions attendues, ajoutez l'attribut suivant :  
  
```vb  
<ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)> _  
```  
  
```csharp  
[ExpectedSqlException(MessageNumber=nnnnn, Severity=x, MatchFirstError=false, State=y)]  
```  
  
Où :  
  
-   *nnnnn* est le numéro du message attendu, par exemple 14025.  
  
-   *x* est le niveau de gravité de l'exception attendue.  
  
-   *y* est l'état de l'exception attendue.  
  
Tous les paramètres non spécifiés sont ignorés. Vous passez ces paramètres à l'instruction RAISERROR dans votre code de base de données. Si vous spécifiez MatchFirstError = true, l'attribut correspond à une des erreurs SqlErrors dans l'exception. Le comportement par défaut (MatchFirstError = truei) n'est utilisé que pour correspondre à la première erreur qui se produit.  
  
Pour obtenir un exemple montrant comment utiliser les exceptions attendues et un test unitaire de SQL Server négatif, consultez [Procédure pas à pas : création et exécution d’un test unitaire SQL Server](../ssdt/walkthrough-creating-and-running-a-sql-server-unit-test.md).  
  
## <a name="the-raiserror-statement"></a>Instruction RAISERROR  
  
> [!NOTE]  
> Utilisez l'instruction THROW plutôt que RAISERROR. L'instruction RAISERROR est désormais déconseillée.  
  
Utilisez directement des assertions Transact\-SQL sur le serveur à l'aide de l'instruction RAISERROR dans le script Transact\-SQL. Sa syntaxe est la suivante :  
  
**RAISERROR (\@ErrorMessage, \@ErrorSeverity, \@ErrorState)**  
  
où :  
  
@ErrorMessage est un message d'erreur défini par l'utilisateur. Vous pouvez mettre en forme cette chaîne de message de la même façon que la fonction printf_s.  
  
@ErrorSeverity est un niveau de gravité défini par l’utilisateur compris entre 0 et 18.  
  
> [!NOTE]  
> Les valeurs « 0 » et « 10 » du niveau de gravité ne provoquent pas l'échec du test unitaire SQL Server. Utilisez toute autre valeur comprise entre 0 et 18 pour provoquer l'échec du test.  
  
@ErrorState est un entier aléatoire compris entre 1 et 127. Utilisez cet entier pour distinguer les occurrences d'une erreur qui se produit à différents emplacements dans le code.  
  
Pour plus d'informations, consultez [RAISERROR (Transact-SQL)](../t-sql/language-elements/raiserror-transact-sql.md). Un exemple d’utilisation de RAISERROR dans un test unitaire SQL Server est fourni dans la rubrique,[Procédure : écrire un test unitaire SQL Server qui s'exécute dans l'étendue d'une seule transaction](../ssdt/how-to-write-sql-server-unit-test-that-runs-in-single-transaction-scope.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création et définition de tests unitaires SQL Server](../ssdt/creating-and-defining-sql-server-unit-tests.md)  
[Utilisation de conditions de test dans les tests unitaires SQL Server](../ssdt/using-test-conditions-in-sql-server-unit-tests.md)  
[Vérifier le code de la base de données à l’aide de tests unitaires SQL Server](../ssdt/verifying-database-code-by-using-sql-server-unit-tests.md)  
[Procédure : ouvrir un test unitaire SQL Server à modifier](../ssdt/how-to-open-a-sql-server-unit-test-to-edit.md)  
