---
title: Créer des conditions de test pour le Concepteur de test unitaire SQL Server
description: Découvrez comment étendre la classe TestCondition pour créer une condition de test personnalisée pour le concepteur de tests unitaires SQL Server. Affichez un exemple d’une condition de test personnalisée.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 48076062-1ef5-419a-8a55-3c7b4234cc35
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d74fe60ff94d2893dc5a2dd899643cabfd0a4f30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018279"
---
# <a name="how-to-create-test-conditions-for-the-sql-server-unit-test-designer"></a>Procédure : Créer des conditions de test pour le Concepteur de test unitaire SQL Server

Utilisez la classe [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) extensible pour créer des conditions de test. Vous pouvez, par exemple, créer une condition de test qui vérifie le nombre de colonnes ou valeurs dans un jeu de résultats.  
  
## <a name="to-create-a-test-condition"></a>Pour créer une condition de test  
Cette procédure explique comment créer une condition de test de façon à ce qu'elle s'affiche dans le Concepteur de test unitaire SQL Server.  
  
1.  Pour créer un projet de bibliothèque de classes dans Visual Studio :  
  
2.  Dans le menu **Projet**, cliquez sur **Ajouter une référence**.  
  
3.  Cliquez sur l'onglet **.NET**.  
  
4.  Dans la liste **Nom du composant**, sélectionnez **System.ComponentModel.Composition**, puis cliquez sur **OK**.  
  
5.  Ajoutez les références d'assembly nécessaires. Cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**. Cliquez sur **Parcourir** et accédez au dossier C:\Program Files (x86)\\Microsoft SQL Server\110\DAC\Bin. Choisissez Microsoft.Data.Tools.Schema.Sql.dll et cliquez sur Ajouter, puis sur OK.  
  
6.  Dans le menu **Projet**, cliquez sur **Décharger le projet**.  
  
7.  Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Modifier <project name>.csproj**.  
  
8.  Ajoutez les instructions Import suivantes après l'importation de élément Microsoft.CSharp.targets :  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v10.0\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' == ''" />  
    <Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.Sql.UnitTesting.targets" Condition="'$(VisualStudioVersion)' != ''" />  
    ```  
  
9. Enregistrez le fichier et fermez-le. Dans l'**Explorateur de solutions**, cliquez avec le bouton droit sur le projet, puis sélectionnez **Recharger le projet**.  
  
10. Dérivez votre classe de la classe [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)).  
  
11. Signez l'assembly avec un nom fort. Pour plus d’informations, consultez [Procédure : signer l’assembly avec un nom fort](/dotnet/standard/assembly/sign-strong-name).  
  
12. Générez la bibliothèque de classes.  
  
13. Pour utiliser la nouvelle condition de test, copiez l'assembly signé dans le dossier %Program Files%\Microsoft Visual Studio <Version>\Common7\IDE\Extensions\Microsoft\SQLDB\TestConditions. Si ce dossier n'existe pas, créez-le. Vous devez disposer de privilèges d'administrateur sur votre ordinateur pour effectuer une copie dans ce répertoire.  
  
14. Installez la condition de test. Pour plus d’informations, consultez [Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md).  
  
15. Ajoutez un test unitaire SQL Server au projet pour créer une référence à la condition de test à ajouter au projet. Vous pouvez ajouter manuellement une référence à l'assembly de condition de test dans le projet. Rechargez le concepteur après cette étape.  
  
    > [!NOTE]  
    > Une classe de test doit être ajoutée pour créer la référence. Supprimez la classe de test après avoir ajouté la référence.  
  
Cet exemple crée une condition de test simple qui vérifie le nombre de colonnes retournées dans la propriété ResultSet. Utilisez cette condition de test simple pour vous assurer que le contrat d'une procédure stockée est correct.  
  
```  
using System;  
using System.ComponentModel;  
using System.Data;  
using System.Data.Common;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting;  
using Microsoft.Data.Tools.Schema.Sql.UnitTesting.Conditions;  
  
namespace Ssdt.Samples.SqlUnitTesting  
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
  
        // method you need to override  
        // to perform the condition verification  
        public override void Assert(DbConnection validationConnection, SqlExecutionResult[] results)  
        {  
            // call base for parameter validation  
            base.Assert(validationConnection, results);  
  
            // verify batch exists  
            if (results.Length < _batch)  
                throw new DataException(String.Format("Batch {0} does not exist", _batch));  
  
            SqlExecutionResult result = results[_batch - 1];  
  
            // verify resultset exists  
            if (result.DataSet.Tables.Count < ResultSet)  
                throw new DataException(String.Format("ResultSet {0} does not exist", ResultSet));  
  
            DataTable table = result.DataSet.Tables[ResultSet - 1];  
  
            // actual condition verification  
            // verify resultset column count matches expected  
            if (table.Columns.Count != Count)  
                throw new DataException(String.Format(  
                    "ResultSet {0}: {1} columns did not match the {2} columns expected",  
                    ResultSet, table.Columns.Count, Count));  
        }  
  
        // this method is called to provide the string shown in the  
        // test conditions panel grid describing what the condition tests  
        public override string ToString()  
        {  
            return String.Format(  
                "Condition fails if ResultSet {0} does not contain {1} columns",  
                ResultSet, Count);  
        }  
  
        // below are the test condition properties  
        // that are exposed to the user in the property browser  
        #region Properties  
  
        // property specifying the resultset for which  
        // you want to check the column count  
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
  
        // property specifying  
        // expected column count  
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
  
La classe de la condition de test personnalisée hérite de la classe [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)) de base. En raison des propriétés supplémentaires sur la condition de test personnalisée, les utilisateurs peuvent configurer la condition dans la fenêtre Propriétés après l'avoir installée.  
  
[ExportTestConditionAttribute](/previous-versions/sql/sql-server-data-tools/jj856578(v=vs.103)) doit être ajouté aux classes d’extension de [TestCondition](/previous-versions/sql/sql-server-data-tools/jj856583(v=vs.103)). Cet attribut permet la découverte de la classe par SQL Server Data Tools et son utilisation au moment de la conception et de l'exécution du test unitaire. L'attribut accepte deux paramètres :  
  
|Paramètre d'attribut|Position|Description|  
|-----------------------|------------|---------------|  
|DisplayName|1|Identifie la chaîne dans la zone de liste déroulante « Conditions de test ». Ce nom doit être unique. Si deux conditions portent le même nom complet, la première condition détectée sera affichée et un avertissement apparaîtra dans le Gestionnaire d'erreurs Visual Studio.|  
|ImplementingType|2|Ce paramètre s'utilise pour identifier de manière unique l'extension. Vous devez le modifier de façon à ce qu'il corresponde au type sur lequel vous placez l'attribut. Cet exemple utilise le type **ResultSetColumnCountCondition**, donc utilisez **typeof(ResultSetColumnCountCondition)** . Si votre type est **NewTestCondition**, utilisez **typeof(NewTestCondition)** .|  
  
Dans cet exemple, vous ajoutez deux propriétés. Les utilisateurs de la condition de test personnalisée peuvent utiliser la propriété ResultSet de façon à indiquer pour quel jeu de résultats le nombre de colonnes doit être vérifié. Ensuite, ils peuvent utiliser la propriété Count pour indiquer le nombre de colonnes attendu.  
  
Trois attributs sont ajoutés pour chaque propriété :  
  
-   Le nom de la catégorie qui permet d'organiser les propriétés.  
  
-   Le nom complet de la propriété.  
  
-   Une description de la propriété.  
  
La validation s'effectue sur les propriétés, pour vérifier que la valeur de la propriété ResultSet n'est pas inférieure à un et la valeur de la propriété Count est supérieure à zéro.  
  
La méthode Assert exécute la tâche principale de la condition de test. Substituez la méthode Assert pour confirmer que la condition attendue est satisfaite. Cette méthode fournit deux paramètres :  
  
-   Le premier paramètre correspond à la connexion de base de données utilisée pour valider la condition de test.  
  
-   Le deuxième paramètre et le plus important correspond au tableau de résultats, qui retourne un seul élément du tableau pour chaque lot exécuté.  
  
Un seul lot est pris en charge pour chaque script de test. Par conséquent, les conditions de test examineront toujours le premier élément du tableau. L'élément du tableau contient un DataSet qui, à son tour, contient les jeux de résultats retournés pour le script de test. Dans cet exemple, le code vérifie que la table de données du DataSet contient le nombre de colonnes approprié. Pour plus d'informations, consultez DataSet.  
  
Vous devez définir la bibliothèque de classes qui contient la condition de test à signer. Cette opération peut être effectuée dans les propriétés du projet sous l'onglet Signature.  
  
## <a name="see-also"></a>Voir aussi  
[Conditions de test personnalisées pour les tests unitaires SQL Server](../ssdt/custom-test-conditions-for-sql-server-unit-tests.md)  
