---
title: Mettre à niveau une condition de test personnalisée Visual Studio 2010 d’une version antérieure
description: Découvrez comment mettre à niveau une condition de test personnalisée Visual Studio 2010 pour l’utiliser dans SQL Server Data Tools. Consultez les modifications à apporter et la procédure d’installation de la condition.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 44c895a3-dee0-4032-a60f-812f5fe3c713
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 2e0f9813db19e06729f7164f0ca419801364a365
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018049"
---
# <a name="how-to-upgrade-a-visual-studio-2010-custom-test-condition-from-a-previous-release-to-sql-server-data-tools"></a>Procédure : Mettre à niveau une condition de test personnalisée Visual Studio 2010 d’une version antérieure vers SQL Server Data Tools

Pour utiliser une condition de test créée dans une version antérieure à SQL Server Data Tools, vous devez la mettre à niveau :  
  
-   [Mettre à jour les références](#UpdateReferences)  
  
-   [Mettre à jour les attributs de classe et les références de type](#UpdateClassAttributesandTypeReference)  
  
-   [Installer la condition de test mise à niveau](#ApplytheNewRegistrationProcess)  
  
## <a name="update-references"></a><a name="UpdateReferences"></a>Mettre à jour les références  
Pour mettre à jour les références de projet :  
  
1.  Visual Basic uniquement, dans l'**Explorateur de solutions**, cliquez sur **Afficher tous les fichiers**.  
  
2.  Dans l'**Explorateur de solutions**, développez le nœud **Références**.  
  
3.  Cliquez avec le bouton droit sur les références d'assembly suivantes, puis cliquez sur **Supprimer** :  
  
    1.  Microsoft.Data.Schema.UnitTesting  
  
    2.  Microsoft.Data.Schema  
  
4.  Dans le menu **Projet**, ou dans l'**Explorateur de solutions**, en cliquant avec le bouton droit sur le dossier de projet, cliquez sur **Ajouter une référence**.  
  
5.  Cliquez sur l'onglet **.NET**.  
  
6.  Dans la liste **Nom du composant**, sélectionnez **System.ComponentModel.Composition** et cliquez sur **OK**.  
  
7.  Ajoutez les références d'assembly nécessaires. Cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**. Cliquez sur **Parcourir** et accédez au dossier C:\Program Files (x86)\\Microsoft SQL Server\110\DAC\Bin. Choisissez Microsoft.Data.Tools.Schema.Sql.dll et cliquez sur Ajouter, puis sur OK.  
  
8.  Dans le menu **Projet**, cliquez sur **Décharger le projet**.  
  
9. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le **Projet**, puis sélectionnez **Modifier**`project_name` **.csproj**.  
  
10. Ajoutez l'instruction Import suivante après l'importation de élément `Microsoft.CSharp.targets` :  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
11. Enregistrez le fichier et fermez-le. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Recharger le projet**.  
  
12. Ouvrez la classe de condition de test et supprimez les instructions USING qui commencent par **Microsoft.Data.Schema**. La solution la plus simple consiste à cliquer avec le bouton droit sur le fichier et à sélectionner **Organiser les instructions Using**, puis **Supprimer et trier**. Supprimez les instructions using suivantes :  
  
    ```  
    using Microsoft.Data.Schema.UnitTesting;  
    using Microsoft.Data.Schema.UnitTesting.Conditions;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema;  
    ```  
  
13. Ajoutez les instructions using suivantes au fichier si elles n'existent pas :  
  
    ```  
    using System.ComponentModel;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
    ```  
  
La condition de test utilise à présent les références SQL Server d'assembly de test unitaire.  
  
## <a name="update-class-attributes-and-type-references"></a><a name="UpdateClassAttributesandTypeReference"></a>Mettre à jour les attributs de classe et les références de type  
Remplacez les anciens attributs de classe de test unitaire par un nouvel attribut. L'extensibilité des tests unitaires SQL Server est maintenant basée sur le Managed Extensibility Framework (MEF). Vous devez également mettre à jour certaines références de type.  
  
### <a name="update-class-attributes"></a>Mettre à jour les attributs de classe  
Mettez le code à jour comme suit :  
  
1.  Supprimez le ou les attributs `DatabaseSchemaProviderCompatibility`. Ce ou ces attributs étaient requis par la fonctionnalité d'extensibilité de la version antérieure et ne sont pas pris en charge dans les tests unitaires SQL Server.  
  
2.  Supprimez l'attribut `DisplayName`. Le nom complet est inclus dans le nouvel attribut.  
  
3.  Ajoutez le nouvel attribut `ExportTestCondition`. Cet attribut doit être présent afin de découvrir la condition de test et de l'utiliser dans un test unitaire SQL Server. `ExportTestCondition` et remplace le ou les attributs `DatabaseSchemaProviderCompatibility`. `ExportTestCondition` accepte deux paramètres :  
  
    -   `DisplayName` est le premier paramètre. Il remplace l'attribut `DisplayName` et est utilisé pour décrire toutes les conditions de test de ce type.  
  
    -   Le deuxième paramètre s'utilise uniquement pour identifier l'extension. Passez simplement le type à l'aide de `typeof(NewTestCondition)`, car il doit être unique. Cependant, il est également possible de passer un ID de chaîne.  
  
4.  La définition de classe doit être modifiée comme suit :  
  
    Avant :  
  
    ```  
    [DatabaseSchemaProviderCompatibility(typeof(DatabaseSchemaProvider))]  
    [DatabaseSchemaProviderCompatibility(null)]  
    [DisplayName("NewTestCondition")]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
    Après :  
  
    ```  
    [ExportTestCondition("NewTestCondition ", typeof(NewTestCondition))]  
    public class NewTestCondition:TestCondition  
    {  
       // Additional implementation to be added here  
    }  
    ```  
  
### <a name="update-type-references"></a>Mettre à jour les références de type  
Certains noms de types ont été modifiés dans l'infrastructure de test unitaire SQL Server. Pour mettre à jour le code de façon à utiliser les nouveaux noms de types, utilisez **Rechercher et remplacer** dans le menu **Edition**. Les noms de types commencent désormais par **Sql**. Les noms de classes doivent être mis à jour comme suit :  
  
|Ancien nom de type|Nouveau nom de type|  
|-----------------|-----------------|  
|`ExecutionResult`|`SqlExecutionResult`|  
  
## <a name="install-the-upgraded-test-condition"></a><a name="ApplytheNewRegistrationProcess"></a>Installer la condition de test mise à niveau  
Dans les versions antérieures de test unitaire de base de données, il se peut que vous ayez installé la condition de test dans le Global Assembly Cache ou créé un fichier XML contenant les informations d'assembly. Avec le test unitaire SQL Server, cette procédure supplémentaire n'est plus nécessaire. (Pour plus d’informations, consultez [Compilation du projet et installation de votre condition de test](../ssdt/walkthrough-use-custom-test-condition-to-verify-stored-procedure-results.md#xxx).  
  
Après avoir mis à jour les références, vérifiez que l'assembly est signé et compilé.  
  
Ensuite, copiez le fichier d'assembly à partir du répertoire de sortie, par défaut My Documents\Visual Studio 2010\Projects\\<yoursolutionname>\\<yourprojectname>\bin\Debug\\) vers %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire TestConditions et rendues disponibles pour une utilisation dans la session :  
  
## <a name="upgrade-existing-tests-that-need-to-use-the-new-test-condition"></a>Mettre à niveau les tests existants qui doivent utiliser la nouvelle condition de test  
Recherchez tous les projets qui utilisent l'ancienne condition de test et doivent utiliser la nouvelle. Assurez-vous que ces projets de test sont mis à niveau. Pour plus d’informations, consultez [Mettre à niveau un projet de test antérieur contenant des tests unitaires de base de données](../ssdt/upgrade-an-older-test-project-containing-database-unit-tests.md).  
  
Supprimez la référence d'assembly à l'ancienne condition de test.  
  
Ajoutez un test unitaire SQL Server au projet pour créer une référence d'assembly à la condition de test mise à niveau dans le projet. Une classe de test doit être ajoutée pour créer la référence. Supprimez la classe de test après avoir ajouté la référence.  
  
## <a name="see-also"></a>Voir aussi  
[Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
  
