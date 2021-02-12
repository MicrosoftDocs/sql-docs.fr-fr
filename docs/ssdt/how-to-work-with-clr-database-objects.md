---
title: Travailler avec des objets de base de données CLR
description: Découvrez comment utiliser les langages .NET Framework pour récupérer et mettre à jour des données. Consultez comment créer et utiliser un objet de base de données CLR (Common Language Run) dans un projet.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.allowsqlclrdebugging
ms.assetid: 4a28d43d-eb5e-444d-aace-5df691f38709
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 02f5aa47948ee5cdbf2cc7f95d749a5f56e47328
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017949"
---
# <a name="how-to-work-with-clr-database-objects"></a>Procédure : Travailler avec des objets de base de données CLR

Outre le langage de programmation Transact\-SQL, vous pouvez utiliser les langages .NET Framework pour créer des objets de base de données et récupérer et mettre à jour des données pour les bases de données SQL Server. Les objets de base de données écrits en code managé sont appelés objets de base de données Common Language Runtime (CLR) de SQL Server. Pour obtenir une explication des avantages de l'utilisation des objets de base de données CLR hébergés dans SQL Server, ainsi que sur le choix entre Transact\-SQL et CLR, consultez [Avantages de l'intégration du CLR](../relational-databases/clr-integration/clr-integration-overview.md) et [Avantages de l'utilisation du code managé pour créer des objets de base de données](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100)).  
  
Pour créer un objet de base de données CLR avec SQL Server Data Tools, vous créez un projet de base de données, puis vous lui ajoutez un objet de base de données CLR. Contrairement aux versions précédentes de Visual Studio, vous n'avez pas besoin de créer un projet CLR distinct, puis de lui ajouter une référence à partir du projet de base de données. Lorsque vous générez et publiez le projet de base de données, vous publiez automatiquement les objets CLR dans le projet en même temps. Une fois ces objets CLR publiés, ils peuvent être appelés et exécutés comme les autres objets de base de données.  
  
Les pages de propriétés CLR et Build CLR contiennent de nombreux paramètres d'utilisation des objets de base de données CLR dans votre projet. Plus particulièrement, la page de propriétés CLR dispose d'un paramètre de niveau d'autorisation pour définir les autorisations sur l'assembly CLR. Elle possède aussi le paramètre Générer le DDL pour contrôler si le DDL des objets de base de données CLR ajoutés au projet doit être généré. La page de propriétés Build CLR contient toutes les options du compilateur que vous pouvez définir pour configurer la compilation du code CLR dans le projet. Ces pages de propriétés sont accessibles en cliquant avec le bouton droit sur votre projet dans l'**Explorateur de solutions** et en sélectionnant **Propriétés**.  
  
Pour activer le débogage des objets de base de données CLR, ouvrez l'**Explorateur d'objets SQL Server**. Cliquez avec le bouton droit sur le serveur contenant les artefacts de base de données CLR à déboguer, puis sélectionnez **Autoriser le débogage SQL CLR**. Une boîte de message affiche l’avertissement suivant : « Notez que pendant le débogage, tous les threads gérés sur ce serveur s’arrêtent. Voulez-vous activer le débogage SQL CLR sur ce serveur ? ». Lorsque vous déboguez des objets de base de données CLR, l'interruption de l'exécution interrompt tous les threads sur le serveur, ce qui affecte les autres utilisateurs. C'est pourquoi vous ne devez pas déboguer les applications pour des objets de base de données CLR sur un serveur de production. Notez également qu'une fois que vous avez commencé à déboguer, il est trop tard pour modifier des paramètres dans l'**Explorateur d'objets SQL Server**. Les modifications effectuées dans l'**Explorateur d'objets SQL Server** ne prendront pas effet avant le démarrage de la session de débogage suivante.  
  
Pour plus d'informations sur les conditions de création d'objets de base de données CLR requises, consultez les rubriques appropriées dans [Création d'objets de base de données avec l'intégration CLR (Common Language Runtime)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md).  
  
> [!WARNING]  
> Les procédures suivantes utilisent les entités créées dans les procédures précédentes des sections [Développement de base de données connectée](../ssdt/connected-database-development.md) et [Développement de base de données hors connexion orientée projet](../ssdt/project-oriented-offline-database-development.md).  
  
### <a name="to-add-a-clr-database-object-to-your-project"></a>Pour ajouter un objet de base de données CLR à votre projet  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet de base de données **TradeDev**, sélectionnez **Ajouter**, puis **Nouvel élément**.  
  
2.  Sélectionnez le modèle **CLR SQL C#** , puis **Fonction définie par l'utilisateur CLR SQL**. Acceptez le nom par défaut, puis cliquez sur **Ajouter**.  
  
3.  Ajoutez le code suivant au corps de la classe. Cette fonction valide un numéro de téléphone aux États-Unis. Il doit être composé de trois caractères numériques, éventuellement entre parenthèses, suivis par un jeu de trois caractères numériques, puis par un jeu de quatre caractères numériques. Exemples de formats pris en charge : (425) 555-0123, 425-555-0123, 425 555 0123 et 1-425-555-0123.  
  
    ```  
  
    [SqlFunction(IsDeterministic = true, IsPrecise = true)]  
    public static SqlBoolean validatePhone(SqlString phone)  
    {  
        string aNorthAmericanPhoneNumberPattern = @"^[01]?[- .]?(\([2-9]\d{2}\)|[2-9]\d{2})[- .]?\d{3}[- .]?\d{4}$";  
        if (!phone.IsNull)  
        {  
           Regex regex = new Regex(aNorthAmericanPhoneNumberPattern);  
           return regex.IsMatch(phone.Value);  
        }  
        return true;  
     }  
    ```  
  
4.  Notez que `Regex` est souligné en rouge. Cliquez avec le bouton droit sur `Regex`, sélectionnez **Résoudre**, **à l'aide de System.Text.RegularExpressions**.  
  
5.  Si vous développez avec une instance de serveur Microsoft SQL Server 2012, vous pouvez ignorer cette étape. Sinon, SQL Server 2005 et SQL Server 2008 ne prennent en charge que les projets de base de données générés avec la version 2.0, 3.0 ou 3.5 du .NET Framework. Pour vérifier que la plateforme cible .NET est configurée correctement, cliquez avec le bouton droit sur le projet de base de données **TradeDev** dans l'**Explorateur de solutions**, puis sélectionnez **Propriétés**. Dans la page de propriétés **SQLCLR**, remplacez la **Plateforme cible** par **.NET Framework 3.5** ou version inférieure. Cliquez sur **Oui** dans le dernier écran pour fermer et rouvrir le projet.  
  
6.  Cliquez avec le bouton droit sur le projet **TradeDev** et sélectionnez **Générer** pour générer le projet.  
  
7.  Double-cliquez sur Suppliers.sql et sélectionnez **Concepteur de vues** pour ouvrir la table Suppliers dans le Concepteur de tables.  
  
8.  Cliquez sur la ligne vide dans la Grille Colonnes pour ajouter une nouvelle colonne à la table. Entrez **téléphone** pour le champ **Nom**, **nvarchar (128)** pour **Type de données** et laissez le champ **Autoriser les valeurs NULL** activé.  
  
9. Cliquez avec le bouton droit sur le nœud **Contraintes de validation** dans le volet contextuel et sélectionnez **Ajouter une nouvelle contrainte de validation**.  
  
10. Remplacez la définition par défaut de la contrainte dans le volet de script par la définition suivante :  
  
    ```  
    CONSTRAINT [CK_Suppliers_CheckPhone] CHECK (dbo.validatePhone(phone)=1),  
    ```  
  
    Ainsi, toute entrée du nouveau champ téléphone sera validée à l'aide de la fonction définie par l'utilisateur CLR ajoutée précédemment.  
  
11. Appuyez sur F5 pour générer et déployer le projet dans la base de données locale.  
  
### <a name="to-use-clr-database-objects"></a>Pour utiliser les objets de base de données CLR  
  
1.  Dans l'**Explorateur d'objets SQL Server**, accédez à la base de données locale dans laquelle vous avez déployé votre projet.  
  
2.  Par défaut, la fonctionnalité d'intégration du CLR est désactivée dans SQL Server. Pour utiliser des objets de base de données CLR, vous devez activer l'intégration du CLR. Pour cela, utilisez l’option « clr enabled » (CLR activé) de la procédure stockée sp_configure. Pour plus d'informations, consultez la rubrique [Option CLR activé](../relational-databases/clr-integration/clr-integration-enabling.md).  
  
    Cliquez avec le bouton droit sur la base de données et sélectionnez **Nouvelle requête**. Collez le code suivant dans le volet de requête et cliquez sur le bouton **Exécuter la requête**.  
  
    ```  
  
    sp_configure 'clr enabled', 1;  
    GO  
    RECONFIGURE;  
    GO  
    ```  
  
3.  Cliquez avec le bouton droit sur la table Suppliers et sélectionnez **Afficher les données**.  
  
4.  Entrez **5** pour le champ **id**, **Contoso** pour **nom**, laissez le champ **Adresse** vide et entrez **425 3122 1222** pour le champ **téléphone**. Accédez par tabulation à un champ autre que le champ **téléphone`INSERT` et notez qu'un message s'affiche, indiquant que l'instruction** est en conflit avec votre contrainte de validation existante, qui valide l'entrée du champ **téléphone** à l'aide d'un modèle de numéro de téléphone prédéfini.  
  
5.  Remplacez l'entrée par **425 312 1222** et accédez par tabulation à un autre champ. Notez que cette fois-ci, l'entrée est acceptée.  
  
## <a name="see-also"></a>Voir aussi  
[Avantages de l'intégration du CLR](../relational-databases/clr-integration/clr-integration-overview.md)  
[Avantages de l'utilisation du code managé pour créer des objets de base de données](/previous-versions/visualstudio/visual-studio-2010/k2e1fb36(v=vs.100))  
[Création d’objets de base de données avec intégration du Common Language Runtime (CLR)](../relational-databases/clr-integration/database-objects/building-database-objects-with-common-language-runtime-clr-integration.md)  
