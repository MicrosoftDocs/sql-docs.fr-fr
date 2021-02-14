---
title: Étendre la génération du projet de base de données à la génération de statistiques de modèle
description: Découvrez comment créer, installer et tester un collaborateur du build qui génère des statistiques du modème de base de données SQL lorsque vous générez un projet de base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: d44935ce-63bf-46df-976a-5a54866c8119
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 5e3905a3ded61ac6ef99f9ada4e94ffaab77a36c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016679"
---
# <a name="walkthrough-extend-database-project-build-to-generate-model-statistics"></a>Procédure pas à pas : Étendre la génération du projet de base de données à la génération de statistiques de modèle

Vous pouvez créer un contributeur de génération pour effectuer des actions personnalisées lorsque vous générez un projet de base de données. Dans cette procédure pas à pas, vous allez créer un contributeur de génération nommé ModelStatistics qui génère des statistiques de base de données SQL lorsque vous créez un projet de base de données. Ce contributeur de génération acceptant des paramètres lorsque vous effectuez la génération, quelques étapes supplémentaires sont nécessaires.  
  
Au cours de cette procédure pas à pas, vous allez effectuer les tâches principales suivantes :  
  
-   [Créer un contributeur de génération](#CreateBuildContributor)  
  
-   [Installer un contributeur de génération](#InstallBuildContributor)  
  
-   [Tester votre contributeur de génération](#TestBuildContributor)  
  
## <a name="prerequisites"></a>Prérequis  
Vous devez disposer des éléments suivants pour exécuter cette procédure pas à pas :  
  
-   Vous devez avoir installé une version de Visual Studio qui inclut SQL Server Data Tools (SSDT) et qui prend en charge le développement en C# ou VB.  
  
-   Vous devez disposer d'un projet SQL qui contient des objets SQL.  
  
> [!NOTE]  
> Cette procédure pas à pas est destinée aux utilisateurs qui sont déjà familiarisés avec les fonctionnalités SQL de SSDT. Vous devez également être familiarisé avec les concepts de base de Visual Studio, comme la création d'une bibliothèque de classes et l'utilisation de l'éditeur de code pour ajouter du code à une classe.  
  
## <a name="build-contributor-background"></a>Informations contextuelles concernant le contributeur de génération  
Les contributeurs de génération sont exécutés pendant la génération du projet, lorsque le modèle qui représente le projet a été généré mais avant que le projet soit enregistré sur le disque. Ils peuvent être utilisés dans plusieurs scénarios, par exemple  
  
-   Validation du contenu du modèle et communication d'erreurs de validation à l'appelant. Pour ce faire, vous pouvez ajouter des erreurs dans une liste transmise comme paramètre à la méthode OnExecute.  
  
-   Génération de statistiques de modèle et communication à l'utilisateur. Il s'agit de l'exemple ci-après.  
  
Le point d'entrée principal pour les contributeurs de génération est la méthode OnExecute. Toutes les classes héritant de BuildContributor doivent implémenter cette méthode. Un objet BuildContributorContext est transmis à cette méthode. Il contient toutes les données pertinentes pour la génération, telles qu’un modèle de base de données, les propriétés de génération et les arguments et les fichiers que les contributeurs de génération doivent utiliser.  
  
**TSqlModel et API de la base de données modèle**  
  
L'objet le plus utile est le modèle de base de données, représenté par un objet TSqlModel. C'est une représentation logique d'une base de données, comprenant toutes les tables, vues et autres éléments, ainsi que les relations entre eux. Il existe un schéma fortement typé qui peut être utilisé pour rechercher des types spécifiques d'éléments et parcourir des relations intéressantes. Des exemples d'utilisation de ce paramètre sont présentés dans le code de cette procédure pas à pas.  
  
Voici certaines commandes utilisées par l'exemple de contributeur dans cette procédure pas à pas :  
  
|**Classe**|**Méthode/propriété**|**Description**|  
|-------------|------------------------|-------------------|  
|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel)|GetObjects()|Interroge le modèle d'objets, est le point d'entrée principal à l'API du modèle. Seuls les types de niveau supérieur comme Table et Vue peuvent être interrogés. Les types comme Colonnes peuvent être trouvés uniquement en parcourant le modèle. Si aucun filtre ModelTypeClass n'est spécifié, tous les types de niveau supérieur sont retournés.|  
|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|GetReferencedRelationshipInstances()|Recherche des relations aux éléments référencés par TSqlObject actuel. Par exemple, pour une table, cette méthode retourne des objets comme des colonnes de la table. Dans ce cas, un filtre ModelRelationshipClass peut être utilisé pour spécifier les relations exactes à interroger (par exemple le filtre « Table.Columns » garantirait que seules des colonnes sont retournées).<br /><br />Il existe plusieurs méthodes semblables, telles que GetReferencingRelationshipInstances, GetChildren et GetParent. Pour plus d'informations, consultez la documentation relative à API.|  
  
**Identifier un collaborateur de manière unique**  
  
Lors de la génération, les contributeurs personnalisés sont chargés à partir d'un répertoire d'extension standard. Les contributeurs de génération sont identifiés par un attribut [ExportBuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportbuildcontributorattribute). Cet attribut est requis afin que les contributeurs puissent être découverts. Cet attribut doit présenter un aspect similaire au suivant :  
  
```  
[ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
  
```  
  
Dans ce cas le premier paramètre de l’attribut doit être un identificateur unique qui sera utilisé pour identifier un contributeur dans des fichiers de projet. Il est recommandé d’associer l’espace de noms de la bibliothèque (ici, « ExampleContributors ») au nom de la classe (ici, « ModelStatistics ») pour générer l’identificateur. Vous pouvez voir comment cet espace de noms est utilisé pour spécifier que votre contributeur doit être exécuté ultérieurement dans la chronologie.  
  
## <a name="create-a-build-contributor"></a><a name="CreateBuildContributor"></a>Créer un contributeur de génération  
Pour créer un contributeur de génération, vous devez effectuer les tâches suivantes :  
  
-   Créez un projet Bibliothèque de classes et ajoutez les références requises.  
  
-   Définissez une classe nommée ModelStatistics qui hérite de [BuildContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.buildcontributor).  
  
-   Remplacer la méthode OnExecute.  
  
-   Ajouter quelques méthodes d'assistance privées.  
  
-   Générer l'assembly résultant.  
  
#### <a name="to-create-a-class-library-project"></a>Pour créer un projet de bibliothèque de classes  
  
1.  Créez un projet Bibliothèque de classes Visual Basic ou Visual C# nommé MyBuildContributor.  
  
2.  Renommez le fichier « Class1.cs » en « ModelStatistics.cs ».  
  
3.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**.  
  
4.  Sélectionnez l'entrée de **System.ComponentModel.Composition** puis cliquez sur **OK**.  
  
5.  Ajoutez les références SQL requises : cliquez avec le bouton droit sur le nœud de projet puis cliquez sur **Ajouter une référence**. Cliquez sur le bouton **Parcourir** . Accédez au dossier **C:\Program Files (x86)\Microsoft SQL Server\110\DAC\Bin**. Sélectionnez les entrées **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** et **Microsoft.Data.Tools.Schema.Sql.dll** , puis cliquez sur **OK**.  
  
    Commencez ensuite à ajouter le code à la classe.  
  
#### <a name="to-define-the-modelstatistics-class"></a>Pour définir la classe ModelStatistics  
  
1.  La classe ModelStatistics traite le modèle de base de données transmis à la méthode OnExecute, et produit un rapport XML détaillant son contenu.  
  
    Dans l'éditeur de code, mettez à jour le fichier ModelStatistics.cs de façon à ce qu'il corresponde à ce qui suit :  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.IO;  
    using System.Linq;  
    using System.Xml.Linq;  
    using Microsoft.Data.Schema;  
    using Microsoft.Data.Schema.Build;  
    using Microsoft.Data.Schema.Extensibility;  
    using Microsoft.Data.Schema.SchemaModel;  
    using Microsoft.Data.Schema.Sql;  
  
    namespace ExampleContributors  
    {  
    /// <summary>  
        /// A BuildContributor that generates statistics about a model and saves this to the output directory.  
        /// Will only run if a "GenerateModelStatistics=true" contributor argument is set in the project file, or a targets file.   
        /// Statistics can be sorted by "none, "name" or "value", with "none" being the default sort behavior.  
        ///   
        /// To set contributor arguments in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
        ///     </ContributorArguments>  
        /// <PropertyGroup>      
        ///   
        /// This will generate model statistics when building in Debug mode only - remove the condition to generate in all build modes.  
        /// </summary>  
        [ExportBuildContributor("ExampleContributors.ModelStatistics", "1.0.0.0")]  
        public class ModelStatistics : BuildContributor  
        {  
            public const string GenerateModelStatistics = "ModelStatistics.GenerateModelStatistics";  
            public const string SortModelStatisticsBy = "ModelStatistics.SortModelStatisticsBy";  
            public const string OutDir = "ModelStatistics.OutDir";  
            public const string ModelStatisticsFilename = "ModelStatistics.xml";  
            private enum SortBy { None, Name, Value };  
            private static Dictionary<string, SortBy> SortByMap = new Dictionary<string, SortBy>(StringComparer.OrdinalIgnoreCase)  
            {  
                { "none", SortBy.None },  
                { "name", SortBy.Name },  
                { "value", SortBy.Value },  
            };  
  
            private SortBy _sortBy = SortBy.None;  
  
            /// <summary>  
            /// Override the OnExecute method to perform actions when you build a database project.  
            /// </summary>  
            protected override void OnExecute(BuildContributorContext context, IList<ExtensibilityError> errors)  
            {  
                // handle related arguments, passed in as part of  
                // the context information.  
                bool generateModelStatistics;  
                ParseArguments(context.Arguments, errors, out generateModelStatistics);  
  
                // Only generate statistics if requested to do so  
                if (generateModelStatistics)  
                {  
                    // First, output model-wide information, such  
                    // as the type of database schema provider (DSP)  
                    // and the collation.  
                    StringBuilder statisticsMsg = new StringBuilder();  
                    statisticsMsg.AppendLine(" ")  
                                 .AppendLine("Model Statistics:")  
                                 .AppendLine("===")  
                                 .AppendLine(" ");  
                    errors.Add(new ExtensibilityError(statisticsMsg.ToString(), Severity.Message));  
  
                    var model = context.Model;  
  
                    // Start building up the XML that will later  
                    // be serialized.  
                    var xRoot = new XElement("ModelStatistics");  
  
                    SummarizeModelInfo(model, xRoot, errors);  
  
                    // First, count the elements that are contained   
                    // in this model.  
                    IList<TSqlObject> elements = model.GetObjects(DacQueryScopes.UserDefined).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "UserDefinedElements", xRoot, errors);  
  
                    // Now, count the elements that are defined in  
                    // another model. Examples include built-in types,  
                    // roles, filegroups, assemblies, and any   
                    // referenced objects from another database.  
                    elements = model.GetObjects(DacQueryScopes.BuiltIn | DacQueryScopes.SameDatabase | DacQueryScopes.System).ToList();  
                    Summarize(elements, element => element.ObjectType.Name, "OtherElements", xRoot, errors);  
  
                    // Now, count the number of each type  
                    // of relationship in the model.  
                    SurveyRelationships(model, xRoot, errors);  
  
                    // Determine where the user wants to save  
                    // the serialized XML file.  
                    string outDir;  
                    if (context.Arguments.TryGetValue(OutDir, out outDir) == false)  
                    {  
                        outDir = ".";  
                    }  
                    string filePath = Path.Combine(outDir, ModelStatisticsFilename);  
                    // Save the XML file and tell the user  
                    // where it was saved.  
                    xRoot.Save(filePath);  
                    ExtensibilityError resultArg = new ExtensibilityError("Result was saved to " + filePath, Severity.Message);  
                    errors.Add(resultArg);  
                }  
            }  
  
            /// <summary>  
            /// Examine the arguments provided by the user  
            /// to determine if model statistics should be generated  
            /// and, if so, how the results should be sorted.  
            /// </summary>  
            private void ParseArguments(IDictionary<string, string> arguments, IList<ExtensibilityError> errors, out bool generateModelStatistics)  
            {  
                // By default, we don't generate model statistics  
                generateModelStatistics = false;  
  
                // see if the user provided the GenerateModelStatistics   
                // option and if so, what value was it given.  
                string valueString;  
                arguments.TryGetValue(GenerateModelStatistics, out valueString);  
                if (string.IsNullOrWhiteSpace(valueString) == false)  
                {  
                    if (bool.TryParse(valueString, out generateModelStatistics) == false)  
                    {  
                        generateModelStatistics = false;  
  
                        // The value was not valid from the end user  
                        ExtensibilityError invalidArg = new ExtensibilityError(  
                            GenerateModelStatistics + "=" + valueString + " was not valid.  It can be true or false", Severity.Error);  
                        errors.Add(invalidArg);  
                        return;  
                    }  
                }  
  
                // Only worry about sort order if the user requested  
                // that we generate model statistics.  
                if (generateModelStatistics)  
                {  
                    // see if the user provided the sort option and  
                    // if so, what value was provided.  
                    arguments.TryGetValue(SortModelStatisticsBy, out valueString);  
                    if (string.IsNullOrWhiteSpace(valueString) == false)  
                    {  
                        SortBy sortBy;  
                        if (SortByMap.TryGetValue(valueString, out sortBy))  
                        {  
                            _sortBy = sortBy;  
                        }  
                        else  
                        {  
                            // The value was not valid from the end user  
                            ExtensibilityError invalidArg = new ExtensibilityError(  
                                SortModelStatisticsBy + "=" + valueString + " was not valid.  It can be none, name, or value", Severity.Error);  
                            errors.Add(invalidArg);  
                        }  
                    }  
                }  
            }  
  
            /// <summary>  
            /// Retrieve the database schema provider for the  
            /// model and the collation of that model.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SummarizeModelInfo(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // use a Dictionary to accumulate the information  
                // that will later be output.  
                var info = new Dictionary<string, string>();  
  
                // Two things of interest: the database schema  
                // provider for the model, and the language id and  
                // case sensitivity of the collation of that  
                // model  
                info.Add("Version", model.Version.ToString());  
  
                TSqlObject options = model.GetObjects(DacQueryScopes.UserDefined, DatabaseOptions.TypeClass).FirstOrDefault();  
                if (options != null)  
                {  
                    info.Add("Collation", options.GetProperty<string>(DatabaseOptions.Collation));  
                }  
  
                // Output the accumulated information and add it to   
                // the XML.  
                OutputResult("Basic model info", info, xContainer, errors);  
            }  
  
            /// <summary>  
            /// For a provided list of model elements, count the number  
            /// of elements for each class name, sorted as specified  
            /// by the user.  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private void Summarize<T>(IList<T> set, Func<T, string> groupValue, string category, XElement xContainer, IList<ExtensibilityError> errors)  
            { // Use a Dictionary to keep all summarized information  
                var statistics = new Dictionary<string, int>();  
  
                // For each element in the provided list,  
                // count items based on the specified grouping  
                var groups =  
                    from item in set  
                    group item by groupValue(item) into g  
                    select new { g.Key, Count = g.Count() };  
  
                // order the groups as requested by the user  
                if (this._sortBy == SortBy.Name)  
                {  
                    groups = groups.OrderBy(group => group.Key);  
                }  
                else if (this._sortBy == SortBy.Value)  
                {  
                    groups = groups.OrderBy(group => group.Count);  
                }  
  
                // build the Dictionary of accumulated statistics  
                // that will be passed along to the OutputResult method.  
                foreach (var item in groups)  
                {  
                    statistics.Add(item.Key, item.Count);  
                }  
  
                statistics.Add("subtotal", set.Count);  
                statistics.Add("total items", groups.Count());  
  
                // output the results, and build up the XML  
                OutputResult(category, statistics, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Iterate over all model elements, counting the  
            /// styles and types for relationships that reference each   
            /// element  
            /// Results are output to the console and added to the XML  
            /// being constructed.  
            /// </summary>  
            private static void SurveyRelationships(TSqlModel model, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                // get a list that contains all elements in the model  
                var elements = model.GetObjects(DacQueryScopes.All);  
                // We are interested in all relationships that  
                // reference each element.  
                var entries =  
                    from element in elements  
                    from entry in element.GetReferencedRelationshipInstances(DacExternalQueryScopes.All)  
                    select entry;  
  
                // initialize our counting buckets  
                var composing = 0;  
                var hierachical = 0;  
                var peer = 0;  
  
                // process each relationship, adding to the   
                // appropriate bucket for style and type.  
                foreach (var entry in entries)  
                {  
                    switch (entry.Relationship.Type)  
                    {  
                        case RelationshipType.Composing:  
                            ++composing;  
                            break;  
                        case RelationshipType.Hierarchical:  
                            ++hierachical;  
                            break;  
                        case RelationshipType.Peer:  
                            ++peer;  
                            break;  
                        default:  
                            break;  
                    }  
                }  
  
                // build a dictionary of data to pass along  
                // to the OutputResult method.  
                var stat = new Dictionary<string, int>  
                {  
                    {"Composing", composing},  
                    {"Hierarchical", hierachical},  
                    {"Peer", peer},  
                    {"subtotal", entries.Count()}  
                };  
  
                OutputResult("Relationships", stat, xContainer, errors);  
            }  
  
            /// <summary>  
            /// Performs the actual output for this contributor,  
            /// writing the specified set of statistics, and adding any   
            /// output information to the XML being constructed.  
            /// </summary>  
            private static void OutputResult<T>(string category, Dictionary<string, T> statistics, XElement xContainer, IList<ExtensibilityError> errors)  
            {  
                var maxLen = statistics.Max(stat => stat.Key.Length) + 2;  
                var format = string.Format("{{0, {0}}}: {{1}}", maxLen);  
  
                StringBuilder resultMessage = new StringBuilder();  
                //List<ExtensibilityError> args = new List<ExtensibilityError>();  
                resultMessage.AppendLine(category);  
                resultMessage.AppendLine("-----------------");  
  
                // Remove any blank spaces from the category name  
                var xCategory = new XElement(category.Replace(" ", ""));  
                xContainer.Add(xCategory);  
  
                foreach (var item in statistics)  
                {  
                    //Console.WriteLine(format, item.Key, item.Value);  
                    var entry = string.Format(format, item.Key, item.Value);  
                    resultMessage.AppendLine(entry);  
                    // Replace any blank spaces in the element key with  
                    // underscores.  
                    xCategory.Add(new XElement(item.Key.Replace(' ', '_'), item.Value));  
                }  
                resultMessage.AppendLine(" ");  
                errors.Add(new ExtensibilityError(resultMessage.ToString(), Severity.Message));  
            }  
        }  
    }  
  
    ```  
  
    Générez ensuite la bibliothèque de classes.  
  
### <a name="to-sign-and-build-the-assembly"></a>Pour signer et générer l'assembly  
  
1.  Dans le menu **Projet** , cliquez sur **Propriétés de MyBuildContributor**.  
  
2.  Cliquez sur l'onglet **Signature** .  
  
3.  Cliquez sur **Signer l'assembly**.  
  
4.  Dans **Choisir un fichier de clé de nom fort**, cliquez sur **<New>** .  
  
5.  Dans la boîte de dialogue **Créer une clé de nom fort** , dans **Nom du fichier de clé**, tapez **MyRefKey**.  
  
6.  (facultatif) Vous pouvez spécifier un mot de passe pour votre fichier de clé de nom fort.  
  
7.  Cliquez sur **OK**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
9. Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
    Ensuite, vous devez installer l'assembly afin qu'il soit chargé lorsque vous générez des projets SQL.  
  
## <a name="install-a-build-contributor"></a><a name="InstallBuildContributor"></a>Installer un contributeur de génération  
Pour installer un contributeur de génération, vous devez copier l'assembly et le fichier .pdb associé dans le dossier Extensions.  
  
#### <a name="to-install-the-mybuildcontributor-assembly"></a>Pour installer l'assembly MyBuildContributor  
  
1.  Puis, vous allez copier les informations d'assembly dans le répertoire Extensions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire et les sous-répertoires %Program Files%\\Microsoft SQL Server\110\DAC\Bin\Extensions\Microsoft\SQLDB\TestConditions et mises à disposition.  
  
2.  Copiez le fichier d'assembly de **MyBuildContributor.dll** du répertoire de sortie dans le répertoire %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions.  
  
    > [!NOTE]  
    > Par défaut, le chemin d'accès du fichier .dll compilé est le suivant : Chemin de votre solution\Chemin de votre projet\bin\Debug ou Chemin de votre solution\Chemin de votre projet\bin\Release.  
  
## <a name="run-or-test-your-build-contributor"></a><a name="TestBuildContributor"></a>Exécuter ou tester votre contributeur de génération  
Pour exécuter ou tester un contributeur de génération, vous devez effectuer les tâches suivantes :  
  
-   Ajouter des propriétés au fichier .sqlproj que vous envisagez de générer.  
  
-   Générer le projet de base de données à l'aide de MSBuild et en fournissant les paramètres appropriés.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Ajouter des propriétés au fichier de projet SQL (.sqlproj)  
Vous devez toujours mettre à jour le fichier de projet SQL pour spécifier l'ID des contributeurs à exécuter. En outre, ce contributeur de génération acceptant des paramètres de ligne de commande Msbuild, vous devez modifier le projet SQL pour permettre aux utilisateurs de transmettre ces paramètres via Msbuild.  
  
Vous pouvez le faire de deux façons :  
  
-   Vous pouvez modifier manuellement le fichier .sqlproj pour ajouter les arguments requis. Vous pouvez procéder de la sorte si vous n'envisagez pas de réutiliser le contributeur de génération sur un grand nombre de projets. Si vous choisissez cette option, ajoutez les instructions suivantes dans le fichier .sqlproj après le premier nœud d'importation dans le fichier  
  
    ```  
    /// <PropertyGroup>  
    ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
    ///         $(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy="name";  
    ///     </ContributorArguments>  
    /// <PropertyGroup>  
  
    ```  
  
-   La deuxième méthode consiste à créer un fichier de cibles qui contient les arguments de contributeur requis. Cela est utile si vous utilisez le même contributeur pour plusieurs projets, car il comprend les valeurs par défaut.  
  
    Dans ce cas, créez un fichier de cibles dans le chemin d'accès des extensions Msbuild :  
  
    1.  Accédez à %Program Files%\MSBuild\\.  
  
    2.  Créez un dossier « MyContributors » où vos fichiers de cibles seront stockés.  
  
    3.  Créez un fichier « MyContributors.targets » dans ce répertoire, ajoutez le texte suivant, puis enregistrez le fichier :  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <BuildContributors>$(BuildContributors);ExampleContributors.ModelStatistics</BuildContributors>  
            <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);ModelStatistics.GenerateModelStatistics=true;ModelStatistics.SortModelStatisticsBy=name;</ContributorArguments>  
          </PropertyGroup>  
        </Project>  
        ```  
  
    4.  Dans le fichier .sqlproj, pour n’importe quel projet, vous souhaitez exécuter des collaborateurs, importez le fichier de cibles en ajoutant la déclaration suivante au fichier .sqlproj après le nœud \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> dans le fichier :  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
        ```  
  
Après avoir suivi une de ces approches, vous pouvez utiliser Msbuild pour transmettre les paramètres des générations par ligne de commande.  
  
> [!NOTE]  
> Vous devez toujours mettre à jour la propriété « BuildContributors » pour spécifier votre ID de contributeur. Il s’agit du même ID que celui utilisé dans l’attribut « ExportBuildContributor » dans le fichier source du contributeur. Sans cet ID, le contributeur ne s'exécute pas lors de la génération du projet. La propriété « ContributorArguments » doit être mise à jour uniquement si des arguments sont nécessaires pour l’exécution du contributeur.  
  
### <a name="build-the-sql-project"></a>Générez le projet SQL.  
  
##### <a name="to-rebuild-your-database-project-by-using-msbuild-and-generate-statistics"></a>Pour reconstruire un projet de base de données à l'aide de Msbuild et générer des statistiques  
  
1.  Dans Visual Studio, cliquez avec le bouton droit sur le projet et sélectionnez « Regénérer ». Cela reconstruit le projet, puis les statistiques du modèle sont générées, et la sortie comprise dans la sortie de la génération et enregistrée dans ModelStatistics.xml. Notez que vous pouvez être amené à sélectionner « Afficher tous les fichiers » dans l’Explorateur de solutions pour visualiser le fichier XML.  
  
2.  Ouvrez une invite de commandes Visual Studio : dans le menu **Démarrer**, cliquez sur **Tous les programmes**, sur **Microsoft Visual Studio <Visual Studio Version>**, cliquez sur **Outils Visual Studio**, puis sur **Invite de commandes Visual Studio (<Visual Studio Version>)**.  
  
3.  À l'invite de commandes, accédez au dossier qui contient votre projet SQL.  
  
4.  Saisissez ensuite la commande suivante dans l’invite de commandes :  
  
    ```  
    MSBuild /t:Rebuild MyDatabaseProject.sqlproj /p:BuildContributors=$(BuildContributors);ExampleContributors.ModelStatistics /p:ContributorArguments=$(ContributorArguments);GenerateModelStatistics=true;SortModelStatisticsBy=name;OutDir=.\;  
    ```  
  
    Remplacez *MyDatabaseProject* par le nom du projet de base de données que vous souhaitez générer. Si vous avez modifié le projet après l'avoir généré, vous pouvez utiliser /t:Build au lieu de /t:Rebuild.  
  
    Dans la sortie vous devez voir les informations de génération comme suit :  
  
```  
Model Statistics:  
===  
  
Basic model info  
-----------------  
    Version: Sql110  
  Collation: SQL_Latin1_General_CP1_CI_AS  
  
UserDefinedElements  
-----------------  
  DatabaseOptions: 1  
         subtotal: 1  
      total items: 1  
  
OtherElements  
-----------------  
                Assembly: 1  
       BuiltInServerRole: 9  
           ClrTypeMethod: 218  
  ClrTypeMethodParameter: 197  
         ClrTypeProperty: 20  
                Contract: 6  
                DataType: 34  
                Endpoint: 5  
               Filegroup: 1  
             MessageType: 14  
                   Queue: 3  
                    Role: 10  
                  Schema: 13  
                 Service: 3  
                    User: 4  
         UserDefinedType: 3  
                subtotal: 541  
             total items: 16  
  
Relationships  
-----------------  
     Composing: 477  
  Hierarchical: 6  
          Peer: 19  
      subtotal: 502  
  
```  
  
1.  Ouvrez ModelStatistics.xml et examinez son contenu.  
  
    Les résultats enregistrés sont également conservés dans le fichier XML.  
  
## <a name="next-steps"></a>Étapes suivantes  
Vous pouvez créer des outils supplémentaires pour effectuer le traitement du fichier XML en sortie. Il s'agit d'un exemple de contributeur de génération. Vous pouvez, par exemple, créer un contributeur de génération pour générer un fichier de dictionnaire de données dans le cadre de la génération.  
  
## <a name="see-also"></a>Voir aussi  
[Personnaliser la génération et le déploiement de bases de données à l'aide de contributeurs de génération et de déploiement](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Procédure pas à pas : étendre le déploiement du projet de base de données pour analyser le plan de déploiement](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
