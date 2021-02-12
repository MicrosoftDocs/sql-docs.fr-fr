---
title: Condition de test personnalisée pour vérifier les résultats d’une procédure stockée
description: Suivez les étapes de configuration d’une condition de test personnalisée qui vérifie si une procédure stockée retourne ou pas le nombre correct de colonnes.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 4c33b494-a85e-4dd2-97b6-c88ee858a99c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d434df88cae0a5fb28add27cc9785e659f059ced
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042489"
---
# <a name="walkthrough-using-a-custom-test-condition-to-verify-the-results-of-a-stored-procedure"></a>Procédure pas à pas : Utiliser une condition de test personnalisée pour vérifier les résultats d’une procédure stockée

Dans cette procédure pas à pas d'extension de fonctionnalité, vous allez créer une condition de test, puis vérifier cette fonctionnalité en créant un test unitaire SQL Server. Cette procédure inclut la création d'un projet de bibliothèque de classes pour la condition de test, sa signature et son installation. Si vous disposez déjà d'une condition de test à mettre à jour, consultez [Procédure : mettre à niveau une condition de test personnalisée Visual Studio 2010 d'une version antérieure vers SQL Server Data Tools](../ssdt/how-to-upgrade-visual-studio-2010-custom-test-condition-to-ssdt.md).  
  
Cette procédure pas à pas décrit les tâches suivantes :  
  
-   Procédure : créer une condition de test  
  
-   Procédure : signer l'assembly avec un nom fort  
  
-   Procédure : ajouter les références nécessaires au projet  
  
-   Procédure : générer une condition de test  
  
-   Procédure : installer la nouvelle condition de test  
  
-   Procédure : tester la nouvelle condition de test  
  
Vous devez disposer de Visual Studio 2010 ou Visual Studio 2012 avec la dernière version de SQL Server Data Tools pour effectuer cette procédure pas à pas. Pour plus d’informations, consultez [Installer les outils de données SQL Server](./download-sql-server-data-tools-ssdt.md).  
  
## <a name="creating-a-custom-test-condition"></a>Création d'une condition de test personnalisée  
Vous allez commencer par créer une bibliothèque de classes.  
  
1.  Dans le menu **Fichier**, cliquez sur **Nouveau**, puis cliquez sur **Projet**.  
  
2.  Dans la boîte de dialogue **Nouveau projet**, sous **Types de projets**, cliquez sur Visual C\#.  
  
3.  Sous **Modèles**, sélectionnez **Bibliothèque de classes**.  
  
4.  Dans la zone de texte **Nom**, tapez **ColumnCountCondition**, puis cliquez sur **OK**.  
  
Ensuite, signez le projet.  
  
1.  Dans le menu **Projet**, cliquez sur **Propriétés de ColumnCountCondition**.  
  
2.  Sous l'onglet **Signature**, activez la case à cocher **Signer l'assembly**.  
  
3.  Dans la boîte **Choisir un fichier de clé de nom fort**, cliquez sur **\<New...>** .  
  
    La boîte de dialogue **Créer une clé de nom fort** s'affiche.  
  
4.  Dans la zone **Nom du fichier de clé**, tapez **SampleKey**.  
  
5.  Entrez et confirmez le mot de passe, puis cliquez sur **OK**. Lorsque vous construisez la solution, le fichier de clé est utilisé pour signer l'assembly.  
  
6.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
7.  Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
Ensuite, vous allez ajouter les références nécessaires au projet  
  
1.  Dans l'**Explorateur de solutions**, sélectionnez le projet **ColumnCountCondition**.  
  
2.  Dans le menu **Projet**, cliquez sur **Ajouter une référence** pour afficher la boîte de dialogue **Ajouter une référence**.  
  
3.  Sélectionnez l'onglet **.NET**.  
  
4.  Dans la colonne **Nom du composant**, recherchez et sélectionnez le composant **System.ComponentModel.Composition**. Après avoir sélectionné le composant, cliquez sur **OK**.  
  
5.  Ajoutez les références d'assembly nécessaires. Cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**. Cliquez sur **Parcourir** et accédez au dossier C:\Program Files (x86)\\Microsoft SQL Server\110\DAC\Bin. Choisissez Microsoft.Data.Tools.Schema.Sql.dll et cliquez sur Ajouter, puis sur OK.  
  
6.  Dans le menu **Projet**, cliquez sur **Décharger le projet**.  
  
7.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Modifier <project name>.csproj**.  
  
8.  Ajoutez les instructions Import suivantes après l'importation de élément **Microsoft.CSharp.targets** :  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Enregistrez le fichier et fermez-le. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Recharger le projet**.  
  
    Les références nécessaires s'affichent sous le nœud **Références** du projet dans l'**Explorateur de solutions**.  
  
## <a name="creating-the-resultsetcolumncountcondition-class"></a>Création de la classe ResultSetColumnCountCondition  
Vous allez renommer **Class1** **ResultSetColumnCountCondition** et la dériver de [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). La classe **ResultSetColumnCountCondition** est une condition de test simple qui vérifie le nombre de colonnes retourné dans la propriété ResultSet. Utilisez cette condition de test pour vous assurer que le contrat d'une procédure stockée est correct.  
  
1.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur Class1.cs, cliquez sur **Renommer**, puis tapez **ResultSetColumnCountCondition.cs**.  
  
2.  Cliquez sur **Oui** pour confirmer le changement de nom de toutes les références en Class1.  
  
3.  Ouvrez le fichier **ResultSetColumnCountCondition.cs** et ajoutez les instructions using suivantes dans le fichier :  
  
    ```  
    using System;  
    using System.ComponentModel;  
    using System.Data;  
    using System.Data.Common;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
    using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
    namespace ColumnCountCondition {  
        public class ResultSetColumnCountCondition  
    ```  
  
4.  Dérivez la classe de [testcondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) :  
  
    ```  
    public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
5.  Ajoutez [ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)). Consultez [Procédure : créer des conditions de test pour le Concepteur de test unitaire SQL Server](../ssdt/how-to-create-test-conditions-for-the-sql-server-unit-test-designer.md) pour plus d’informations sur UnitTesting.Conditions.ExportTestConditionAttribute.  
  
    ```  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
        public class ResultSetColumnCountCondition : TestCondition  
    ```  
  
6.  Créez les variables membres et le constructeur :  
  
    ```  
            private int _resultSet;  
            private int _count;  
            private int _batch;  
  
            public ResultSetColumnCountCondition() {  
                _resultSet = 1;  
                _count = 0;  
                _batch = 1;  
            }  
    ```  
  
7.  Substituez la méthode **Assert**. La méthode contient les arguments de **IDbConnection**, qui représente la connexion à la base de données et **SqlExecutionResult**. La méthode utilise **DataSchemaException** pour la gestion des erreurs :  
  
    ```  
           //method you need to override  
            //to perform the condition verification  
            public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
            {  
                //call base for parameter validation  
                base.Assert(validationConnection, results);  
  
                //verify batch exists  
                if (results.Length < _batch)  
                    throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
                SqlExecutionResult result = results[_batch - 1];  
  
                //verify resultset exists  
                if (result.DataSet.Tables.Count < ResultSet)  
                    throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
                DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
                //actual condition verification  
                //verify resultset column count matches expected  
                if (table.Columns.Count != Count)  
                    throw new DataException(String.Format(  
                        "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                        ResultSet, table.Columns.Count, Count));  
            }  
  
    Add the following method, which overrides the ToString method:  
    C#  
            //this method is called to provide the string shown in the  
            //test conditions panel grid describing what the condition tests  
            public override string ToString()  
            {  
                return String.Format(  
                    "Condition fails if ResultSet {0} does not contain {1} columns",  
                    ResultSet, Count);  
            }  
    ```  
  
8.  Ajoutez les propriétés de condition de test suivantes à l'aide des attributs **CategoryAttribute**, **DisplayNameAttribute** et **DescriptionAttribute** :  
  
    ```  
            //below are the test condition properties  
            //that are exposed to the user in the property browser  
            #region Properties  
  
            //property specifying the resultset for which  
            //you want to check the column count  
            [Category("Test Condition")]  
            [DisplayName("ResultSet")]  
            [Description("ResultSet Number")]  
            public int ResultSet  
            {  
                get { return _resultSet; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 1)  
                        throw new ArgumentException("ResultSet cannot be less than 1");  
  
                    _resultSet = value;  
                }  
            }  
  
            //property specifying  
            //expected column count  
            [Category("Test Condition")]  
            [DisplayName("Count")]  
            [Description("Column Count")]  
            public int Count  
            {  
                get { return _count; }  
  
                set  
                {  
                    //basic validation  
                    if (value < 0)  
                        throw new ArgumentException("Count cannot be less than 0");  
  
                    _count = value;  
                }  
            }  
             #endregion  
    ```  
  
Voici le code final :  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace ColumnCountCondition  
{  
  
    [ExportTestCondition("ResultSet Column Count", typeof(ResultSetColumnCountCondition))]  
    public class ResultSetColumnCountCondition : TestCondition  
    {  
        private int _resultSet;  
        private int _count;  
        private int _batch;  
  
        public ResultSetColumnCountCondition()  
        {  
            _resultSet = 1;  
            _count = 0;  
            _batch = 1;  
        }  
  
        //method you need to override  
        //to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            //call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            //verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            //verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            //actual condition verification  
            //verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        //this method is called to provide the string shown in the  
        //test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        //below are the test condition properties  
        //that are exposed to the user in the property browser  
        #region Properties  
  
        //property specifying the resultset for which  
        //you want to check the column count  
        [Category("Test Condition")]  
        [DisplayName("ResultSet")]  
        [Description("ResultSet Number")]  
        public int ResultSet  
        {  
            get { return _resultSet; }  
  
            set  
            {  
                //basic validation  
                if (value < 1)  
                    throw new ArgumentException("ResultSet cannot be less than 1");  
  
                _resultSet = value;  
            }  
        }  
  
        //property specifying  
        //expected column count  
        [Category("Test Condition")]  
        [DisplayName("Count")]  
        [Description("Column Count")]  
        public int Count  
        {  
            get { return _count; }  
  
            set  
            {  
                //basic validation  
                if (value < 0)  
                    throw new ArgumentException("Count cannot be less than 0");  
  
                _count = value;  
            }  
        }  
  
        #endregion  
    }  
}  
  
```  
  
Vous allez ensuite générer le projet.  
  
## <a name="compiling-the-project-and-installing-your-test-condition"></a><a name="xxx"></a>Compilation du projet et installation de votre condition de test  
Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
Puis, vous allez copier les informations d'assembly dans le répertoire Extensions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire et les sous-répertoires %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions et mises à disposition :  
  
Copiez le fichier d'assembly **ColumnCountCondition.dll** à partir du répertoire de sortie vers le répertoire %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions.  
  
Par défaut, le chemin d'accès du fichier .dll compilé est le suivant : *Chemin de votre solution*\\*Chemin de votre projet*\bin\Debug ou *Chemin de votre solution*\\*Chemin de votre projet*\bin\Release.  
  
Vous allez ensuite démarrer une nouvelle session de Visual Studio et créer un projet de base de données. Pour démarrer une nouvelle session de Visual Studio et créer un projet de base de données :  
  
1.  Démarrez une deuxième session de Visual Studio.  
  
2.  Dans le menu **Fichier**, cliquez sur **Nouveau**, puis cliquez sur **Projet**.  
  
3.  Dans la boîte de dialogue **Nouveau projet**, dans la liste de modèles installés, sélectionnez le nœud **SQL Server**.  
  
4.  Dans le volet d'informations, cliquez sur **Projet de base de données SQL Server**.  
  
5.  Dans la zone de texte **Nom**, tapez **SampleConditionDB**, puis cliquez sur **OK**.  
  
Vous devez ensuite créer un test unitaire. Pour créer un test unitaire SQL Server dans une nouvelle classe de test :  
  
1.  Dans le menu **Test**, cliquez sur **Nouveau test** pour afficher la boîte de dialogue **Ajouter un nouveau test**.  
  
    Ou, ouvrez l'**Explorateur de solutions**, cliquez avec le bouton droit sur un projet de test, cliquez sur **Ajouter**, puis sur **Nouveau test**.  
  
2.  Dans la liste de modèles, cliquez sur **Test unitaire SQL Server**.  
  
3.  Dans **Nom du test**, tapez **SampleUnitTest**.  
  
4.  Dans **Ajouter au projet de test**, cliquez sur **Créer un nouveau projet de test Visual C\#** . Ensuite, cliquez sur **OK** pour afficher la boîte de dialogue **Nouveau projet de test**.  
  
5.  Tapez **SampleUnitTest** comme nom du projet.  
  
6.  Cliquez sur **Annuler** pour créer le test unitaire sans configurer le projet de test de façon à utiliser une connexion de base de données. Le test vide apparaît dans le Concepteur de test unitaire SQL Server. Un fichier de code source Visual C\# est ajouté au projet de test.  
  
    Pour plus d’informations sur la création et la configuration de tests unitaires de base de données avec des connexions de base de données, consultez [Procédure : Créer un test unitaire SQL Server vide](../ssdt/how-to-create-an-empty-sql-server-unit-test.md).  
  
7.  Cliquez sur **Cliquez ici pour créer** pour terminer la création du test unitaire. La nouvelle condition de test s'affiche dans le projet SQL Server.  
  
> [!NOTE]  
> Pour utiliser la condition de test personnalisée avec des projets de test unitaire existants, vous devez créer au moins une classe de test unitaire SQL Server. La référence nécessaire à l'assembly de condition de test est ajoutée au projet de test lors de la création de la classe de test.  
  
Pour afficher la nouvelle condition de test :  
  
1.  Dans le **Concepteur de test unitaire SQL Server**, sous **Conditions de test**, dans la colonne **Nom**, cliquez sur le test inconclusiveCondition1.  
  
2.  Cliquez sur le bouton de barre d'outils **Supprimer la condition de test** pour supprimer le test inconclusiveCondition1.  
  
3.  Cliquez sur la liste déroulante **Conditions de test**, puis sélectionnez **ResultSet Column Count**.  
  
4.  Cliquez sur le bouton de barre d'outils **Ajouter une condition de test** pour ajouter la condition de test personnalisée.  
  
5.  Dans la fenêtre **Propriétés**, configurez les propriétés Count, Enabled et ResultSet.  
  
    Pour plus d’informations, consultez [Procédure : ajouter des conditions de test à des tests unitaires SQL Server](../ssdt/how-to-add-test-conditions-to-sql-server-unit-tests.md).  
  
## <a name="see-also"></a>Voir aussi  
[Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
