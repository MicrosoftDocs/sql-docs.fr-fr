---
title: Création d'un assembly de règle d'analyse statique du code personnalisée pour SQL Server
description: Découvrez comment créer des règles de Code Analysis SQL Server. Configurez une règle pour éviter des instructions WAITFOR dans des procédures stockées, des déclencheurs et des fonctions.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: f7b6ed8c-a4e0-4e33-9858-a8aa40aef309
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 3cae3183bef3d6bd40a9d30805f3923df8d70679
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082340"
---
# <a name="walkthrough-authoring-a-custom-static-code-analysis-rule-assembly-for-sql-server"></a>Procédure pas à pas : création d'un assembly de règle d'analyse statique du code personnalisée pour SQL Server

Cette procédure pas à pas montre les étapes permettant de créer une règle d’analyse du code SQL Server. La règle créée lors de cette procédure pas à pas sert à éviter la présence d'instructions WAITFOR DELAY dans les procédures stockées, les déclencheurs et les fonctions.  
  
Lors de cette procédure pas à pas, vous allez créer une règle personnalisée pour l’analyse statique du code Transact\-SQL en effectuant les tâches suivantes :  
  
1. Créer une bibliothèque de classes, activer la signature pour ce projet et ajouter les références nécessaires.  
  
2. Créer une classe de règle personnalisée Visual C\#.  
  
3. Créer deux classes d’assistance Visual C\#.  
  
4. Copier la DLL résultante dans le répertoire Extensions pour l'installer.  
  
5. Vérifier que la nouvelle règle d'analyse du code est en place.  
  
**Composants requis**
  
Vous devez disposer des éléments suivants pour exécuter cette procédure pas à pas :  
  
- Vous devez avoir installé une version de Visual Studio qui inclut SQL Server Data Tools et qui prend en charge le développement en Visual C\# ou Visual Basic.  
  
- Vous devez disposer d'un projet SQL Server qui contient des objets SQL Server.  
  
- Instance de SQL Server dans laquelle vous pouvez déployer un projet de base de données.  
  
> [!NOTE]  
> Cette procédure pas à pas est destinée aux utilisateurs qui sont déjà familiarisés avec les fonctionnalités SQL Server de SQL Server Data Tools. Vous devez également être familiarisé avec les concepts de Visual Studio, comme la création d'une bibliothèque de classes et l'utilisation de l'éditeur de code pour ajouter du code à une classe.  
  
## <a name="creating-a-custom-code-analysis-rule-for-sql-server"></a>Création d’une règle d’analyse du code personnalisée pour SQL Server  

Créez d’abord une bibliothèque de classes. Pour créer un projet de bibliothèque de classes :  
  
1. Créez un projet Bibliothèque de classes Visual C\# ou Visual Basic nommé SampleRules.  
  
2. Renommez le fichier Class1.cs AvoidWaitForDelayRule.cs.  
  
3. Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**.  
  
4. Sous l’onglet Infrastructure, sélectionnez System.ComponentModel.Composition.  
  
5. Cliquez sur **Parcourir** et accédez au répertoire de C:\Program Files (x86)\\MicrosoftSQL Server\120\SDK\Assemblies directory, select Microsoft.SqlServer.TransactSql.ScriptDom.dll, puis cliquez sur OK.  
  
6. Ensuite, installez les références DACFx nécessaires. Cliquez sur **Parcourir** et accédez au répertoire <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120. Sélectionnez les entrées Microsoft.SqlServer.Dac.dll, Microsoft.SqlServer.Dac.Extensions.dll et Microsoft.Data.Tools.Schema.Sql.dll, cliquez sur **Ajouter**, puis sur **OK**.  
  
    Les binaires DACFx sont maintenant installés dans votre répertoire d’installation Visual Studio. Pour Visual Studio 2012, le <Visual Studio Install Dir> est généralement C:\Program Files (x86)\\MicrosoftVisual Studio 11.0. Pour Visual Studio 2013, il s’agit généralement de C:\Program Files (x86)\\MicrosoftVisual Studio 12.0.  
  
Ensuite, vous allez ajouter des classes de prise en charge qui seront utilisées par la règle.  
  
## <a name="creating-the-custom-code-analysis-rule-supporting-classes"></a>Création des classes de prise en charge de la règle d'analyse du code personnalisée

Avant de créer la classe pour la règle proprement dite, vous allez ajouter une classe Visitor et une classe d'attributs au projet. Ces classes peuvent être utiles pour la création de règles personnalisées supplémentaires.  
  
La première classe que vous devez définir est la classe WaitForDelayVisitor, dérivée de [TSqlConcreteFragmentVisitor](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlconcretefragmentvisitor). Cette classe fournit l'accès aux instructions WAITFOR DELAY dans le modèle. Les classes Visitor utilisent les API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom) fournies par SQL Server. Dans cette API, le code Transact\-SQL est représenté sous la forme d’une arborescence de syntaxe abstraite et les classes Visitor peuvent être utiles quand vous souhaitez rechercher des objets de syntaxe spécifiques, tels que des instructions WAITFORDELAY. Ceux-ci peuvent être difficiles à trouver à l’aide du modèle objet, car ils ne sont associés à aucune relation ou propriété d’objet spécifique. Toutefois, il est facile de les trouver à l’aide du modèle Visitor et de l’API [ScriptDom](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom).  
  
### <a name="defining-the-waitfordelayvisitor-class"></a>Définition de la classe WaitForDelayVisitor  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRules.  
  
2. Dans le menu **Projet**, sélectionnez **Ajouter une classe**. La boîte de dialogue **Ajouter un nouvel élément** s'affiche.  
  
3. Dans la zone de texte **Nom**, tapez WaitForDelayVisitor.cs, puis cliquez sur le bouton **Ajouter**. Le fichier WaitForDelayVisitor.cs est ajouté au projet dans **l’Explorateur de solutions**.  
  
4. Ouvrez le fichier WaitForDelayVisitor.cs et mettez à jour le contenu pour qu'il corresponde au code suivant :  
  
    ```  
    using System.Collections.Generic;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    namespace SampleRules {  
        class WaitForDelayVistor {}  
    }  
    ```  
  
5. Dans la déclaration de classe, définissez « internal » comme modificateur d'accès et dérivez la classe de TSqlConcreteFragmentVisitor :  
  
    ```  
    internal class WaitForDelayVisitor : TSqlConcreteFragmentVisitor {}  
    ```  
  
6. Ajoutez le code suivant pour définir la variable membre List :  
  
    ```  
    public IList<WaitForStatement> WaitForDelayStatements { get; private set; }  
    ```  
  
7. Définissez le constructeur de classe en ajoutant le code suivant :  
  
    ```  
    public WaitForDelayVisitor() {  
       WaitForDelayStatments = new List<WaitForStatement>();  
    }  
    ```  
  
8. Substituez la méthode ExplicitVisit en ajoutant le code suivant :  
  
    ```  
    public override void ExplicitVisit(WaitForStatement node) {  
       // We are only interested in WAITFOR DELAY occurrences  
       if (node.WaitForOption == WaitForOption.Delay)  
          WaitForDelayStatments.Add(node);  
    }  
    ```  
  
    Cette méthode visite les instructions WAITFOR dans le modèle et ajoute celles qui ont l'option DELAY spécifiée à la liste d'instructions WAITFOR DELAY. La principale classe référencée ici est [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement).  
  
9. Dans le menu **Fichier** , cliquez sur **Enregistrer**.  
  
La deuxième classe est LocalizedExportCodeAnalysisRuleAttribute.cs. Il s’agit d’une extension du Microsoft.SqlServer.Dac.CodeAnalysis.ExportCodeAnalysisRuleAttribute intégré fourni par l’infrastructure, qui prend en charge la lecture du DisplayName et de la Description utilisés par votre règle à partir d’un fichier de ressources. Cette classe est utile si jamais vous souhaitez utiliser vos règles dans plusieurs langues.  
  
### <a name="defining-the-localizedexportcodeanalysisruleattribute-class"></a>Définition de la classe LocalizedExportCodeAnalysisRuleAttribute  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRules.  
  
2. Dans le menu **Projet**, sélectionnez **Ajouter une classe**. La boîte de dialogue **Ajouter un nouvel élément** s'affiche.  
  
3. Dans la zone de texte **Nom**, tapez LocalizedExportCodeAnalysisRuleAttribute.cs, puis cliquez sur le bouton **Ajouter**. Le fichier est ajouté au projet dans **l’Explorateur de solutions**.  
  
4. Ouvrez le fichier et mettez à jour le contenu pour qu’il corresponde au code suivant :  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using System;  
    using System.Globalization;  
    using System.Reflection;  
    using System.Resources;  
  
    namespace SampleRules  
    {  
  
        internal class LocalizedExportCodeAnalysisRuleAttribute : ExportCodeAnalysisRuleAttribute  
        {  
            private readonly string _resourceBaseName;  
            private readonly string _displayNameResourceId;  
            private readonly string _descriptionResourceId;  
  
            private ResourceManager _resourceManager;  
            private string _displayName;  
            private string _descriptionValue;  
  
            /// <summary>  
            /// Creates the attribute, with the specified rule ID, the fully qualified  
            /// name of the resource file that will be used for looking up display name  
            /// and description, and the Ids of those resources inside the resource file.  
            /// </summary>  
            public LocalizedExportCodeAnalysisRuleAttribute(  
                string id,  
                string resourceBaseName,  
                string displayNameResourceId,  
                string descriptionResourceId)  
                : base(id, null)  
            {  
                _resourceBaseName = resourceBaseName;  
                _displayNameResourceId = displayNameResourceId;  
                _descriptionResourceId = descriptionResourceId;  
            }  
  
            /// <summary>  
            /// Rules in a different assembly would need to overwrite this  
            /// </summary>  
            /// <returns></returns>  
            protected virtual Assembly GetAssembly()  
            {  
                return GetType().Assembly;  
            }  
  
            private void EnsureResourceManagerInitialized()  
            {  
                var resourceAssembly = GetAssembly();  
  
                try  
                {  
                    _resourceManager = new ResourceManager(_resourceBaseName, resourceAssembly);  
                }  
                catch (Exception ex)  
                {  
                    var msg = String.Format(CultureInfo.CurrentCulture, RuleResources.CannotCreateResourceManager, _resourceBaseName, resourceAssembly);  
                    throw new RuleException(msg, ex);  
                }  
            }  
  
            private string GetResourceString(string resourceId)  
            {  
                EnsureResourceManagerInitialized();  
                return _resourceManager.GetString(resourceId, CultureInfo.CurrentUICulture);  
            }  
  
            /// <summary>  
            /// Overrides the standard DisplayName and looks up its value inside a resources file  
            /// </summary>  
            public override string DisplayName  
            {  
                get  
                {  
                    if (_displayName == null)  
                    {  
                        _displayName = GetResourceString(_displayNameResourceId);  
                    }  
                    return _displayName;  
                }  
            }  
  
            /// <summary>  
            /// Overrides the standard Description and looks up its value inside a resources file  
            /// </summary>  
            public override string Description  
            {  
                get  
                {  
                    if (_descriptionValue == null)  
                    {   
                        _descriptionValue = GetResourceString(_descriptionResourceId);  
                    }  
                    return _descriptionValue;  
                }  
            }  
        }  
    }  
    ```  
  
Ensuite, vous ajoutez un fichier de ressources qui définit le nom de la règle, la description de la règle et la catégorie dans laquelle la règle apparaîtra dans l'interface de configuration de règle.  
  
### <a name="to-add-a-resource-file-and-three-resource-strings"></a>Pour ajouter un fichier de ressources et trois chaînes de ressources  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRules.  
  
2. Dans le menu **Projet**, sélectionnez **Ajouter un nouvel élément**. La boîte de dialogue **Ajouter un nouvel élément** s'affiche.  
  
3. Dans la liste des **Modèles installés**, cliquez sur **Général**.  
  
4. Dans le volet d’informations, cliquez sur **Fichier de ressources**.  
  
5. Dans **Nom**, tapez RuleResources.resx. L'éditeur de ressources s'affiche, sans aucune ressource définie.  
  
6. Définissez quatre chaînes de ressources comme suit :  
  
    |Nom|Valeur|  
    |--------|---------|  
    |AvoidWaitForDelay_ProblemDescription|L'instruction WAITFOR DELAY a été trouvée dans {0}.|  
    |AvoidWaitForDelay_RuleName|Évitez d'utiliser des instructions WaitFor Delay dans des procédures stockées, des fonctions et des déclencheurs.|  
    |CategorySamples|SamplesCategory|  
    |CannotCreateResourceManager|Impossible de créer ResourceManager pour {0} à partir de {1}.|  
  
7. Dans le menu **Fichier**, cliquez sur **Enregistrer RuleResources.resx**.  
  
Ensuite, définissez une classe qui fait référence aux ressources du fichier de ressources qui sont utilisées par Visual Studio pour afficher des informations à propos de votre règle dans l’interface utilisateur.  
  
### <a name="defining-the-sampleconstants-class"></a>Définition de la classe SampleConstants  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRules.  
  
2. Dans le menu **Projet**, sélectionnez **Ajouter une classe**. La boîte de dialogue **Ajouter un nouvel élément** s'affiche.  
  
3. Dans la zone de texte **Nom**, tapez SampleRuleConstants.cs, puis cliquez sur le bouton **Ajouter**. Le fichier SampleRuleConstants.cs est ajouté au projet dans **l’Explorateur de solutions**.  
  
4. Ouvrez le fichier SampleRuleConstants.cs et ajoutez les instructions using suivantes dans le fichier :  
  
    ```  
    namespace SampleRules  
    {   
        internal static class RuleConstants  
        {  
            /// <summary>  
            /// The name of the resources file to use when looking up rule resources  
            /// </summary>  
            public const string ResourceBaseName = "Public.Dac.Samples.Rules.RuleResources";  
  
            /// <summary>  
            /// Lookup name inside the resources file for the select asterisk rule name  
            /// </summary>  
            public const string AvoidWaitForDelay_RuleName = "AvoidWaitForDelay_RuleName";  
            /// <summary>  
            /// Lookup ID inside the resources file for the select asterisk description  
            /// </summary>  
            public const string AvoidWaitForDelay_ProblemDescription = "AvoidWaitForDelay_ProblemDescription";  
  
            /// <summary>  
            /// The design category (should not be localized)  
            /// </summary>  
            public const string CategoryDesign = "Design";  
  
            /// <summary>  
            /// The performance category (should not be localized)  
            /// </summary>  
            public const string CategoryPerformance = "Design";  
        }  
    }  
    ```  
  
5. Cliquez sur **Fichier** > **Enregistrer**.  
  
## <a name="creating-the-custom-code-analysis-rule-class"></a>Création de la classe de règle d’analyse du code personnalisée

Maintenant que vous avez ajouté les classes d'assistance qui seront utilisées par la règle d'analyse du code personnalisée, vous allez créer une classe de règle personnalisée et la nommer AvoidWaitForDelayRule. La règle personnalisée AvoidWaitForDelayRule servira à éviter que les développeurs de base de données utilisent des instructions WAITFOR DELAY dans des procédures stockées, des déclencheurs et des fonctions.  
  
### <a name="creating-the-avoidwaitfordelayrule-class"></a>Création de la classe AvoidWaitForDelayRule  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRules.  
  
2. Dans le menu **Projet**, sélectionnez **Ajouter une classe**. La boîte de dialogue **Ajouter un nouvel élément** s'affiche.  
  
3. Dans la zone de texte **Nom**, tapez AvoidWaitForDelayRule.cs, puis cliquez sur **Ajouter**. Le fichier AvoidWaitForDelayRule.cs est ajouté au projet dans **l’Explorateur de solutions**.  
  
4. Ouvrez le fichier AvoidWaitForDelayRule.cs et ajoutez les instructions using suivantes dans le fichier :  
  
    ```  
    using Microsoft.SqlServer.Dac.CodeAnalysis;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;   
    namespace SampleRules {  
        class AvoidWaitForDelayRule {}  
    }  
    ```  
  
5. Dans la déclaration de classe AvoidWaitForDelayRule, rendez public le modificateur d'accès :  
  
    ```  
    /// <summary>  
    /// This is a rule that returns a warning message   
    /// whenever there is a WAITFOR DELAY statement appears inside a subroutine body.  
    /// This rule only applies to stored procedures, functions and triggers.  
    /// </summary>  
    public sealed class AvoidWaitForDelayRule  
    ```  
  
6. Dérivez la classe AvoidWaitForDelayRule de la classe de base Microsoft.SqlServer.Dac.CodeAnalysis.SqlCodeAnalysisRule :  
  
    ```  
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    ```  
  
7. Ajoutez LocalizedExportCodeAnalysisRuleAttribute à votre classe.  
  
    LocalizedExportCodeAnalysisRuleAttribute permet au service d'analyse du code de découvrir les règles d'analyse du code personnalisées. Seules les classes marquées avec un ExportCodeAnalysisRuleAttribute (ou un attribut qui en hérite) peuvent être utilisées dans l'analyse du code.  
  
    LocalizedExportCodeAnalysisRuleAttribute fournit des métadonnées nécessaires utilisées par le service, notamment un ID unique pour cette règle, un nom complet qui sera affiché dans l’interface utilisateur Visual Studio et une description qui peut être utilisée par votre règle lors de l’identification des problèmes.  
  
    ```  
    [LocalizedExportCodeAnalysisRule(AvoidWaitForDelayRule.RuleId,  
        RuleConstants.ResourceBaseName,  
        RuleConstants.AvoidWaitForDelay_RuleName,   
        RuleConstants.AvoidWaitForDelay_ProblemDescription  
        Category = RuleConstants.CategoryPerformance,   
        RuleScope = SqlRuleScope.Element)]   
    public sealed class AvoidWaitForDelayRule : SqlCodeAnalysisRule  
    {  
       /// <summary>  
       /// The Rule ID should resemble a fully-qualified class name. In the Visual Studio UI  
       /// rules are grouped by "Namespace + Category", and each rule is shown using "Short ID: DisplayName".  
       /// For this rule, that means the grouping will be "Public.Dac.Samples.Performance", with the rule  
       /// shown as "SR1004: Avoid using WaitFor Delay statements in stored procedures, functions and triggers."  
       /// </summary>  
       public const string RuleId = "RuleSamples.SR1004";  
    }  
    ```  
  
    La propriété RuleScope doit être Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Element, car cette règle permet d’analyser des éléments spécifiques. La règle sera appelée une fois pour chaque élément correspondant dans le modèle. Si vous souhaitez analyser un modèle entier, vous pouvez utiliser Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleScope.Model à la place.  
  
8. Ajoutez un constructeur permettant de configurer les Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.SupportedElementTypes. Il est nécessaire pour les règles de portée d'élément. Il définit les types d'éléments auxquels cette règle sera appliquée. Dans notre exemple, la règle doit être appliquée aux procédures stockées, aux fonctions et aux déclencheurs. Notez que la classe Microsoft.SqlServer.Dac.Model.ModelSchema répertorie tous les types d’éléments disponibles qui peuvent être analysés.  
  
    ```  
    public AvoidWaitForDelayRule()  
    {  
       // This rule supports Procedures, Functions and Triggers. Only those objects will be passed to the Analyze method  
       SupportedElementTypes = new[]  
       {  
          // Note: can use the ModelSchema definitions, or access the TypeClass for any of these types  
          ModelSchema.ExtendedProcedure,  
          ModelSchema.Procedure,  
          ModelSchema.TableValuedFunction,  
          ModelSchema.ScalarFunction,  
  
          ModelSchema.DatabaseDdlTrigger,  
          ModelSchema.DmlTrigger,  
          ModelSchema.ServerDdlTrigger  
       };  
    }  
    ```  
  
9. Ajoutez une substitution pour la méthode Microsoft.SqlServer.Dac.CodeAnalysis.SqlAnalysisRule.Analyze(Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext), qui utilise Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleExecutionContext comme paramètres d’entrée. Cette méthode retourne une liste de problèmes potentiels.  
  
    La méthode obtient Microsoft.SqlServer.Dac.Model.TSqlModel, Microsoft.SqlServer.Dac.Model.TSqlObject et [TSqlFragment](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlfragment) à partir du paramètre de contexte. La classe WaitForDelayVisitor est ensuite utilisée pour obtenir une liste de toutes les instructions WAITFOR DELAY dans le modèle.  
  
    Pour chaque [WaitForStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.waitforstatement) de cette liste, un Microsoft.SqlServer.Dac.CodeAnalysis.SqlRuleProblem est créé.  
  
    ```  
    /// <summary>  
    /// For element-scoped rules the Analyze method is executed once for every matching   
    /// object in the model.   
    /// </summary>  
    /// <param name="ruleExecutionContext">The context object contains the TSqlObject being   
    /// analyzed, a TSqlFragment  
    /// that's the AST representation of the object, the current rule's descriptor, and a   
    /// reference to the model being  
    /// analyzed.  
    /// </param>  
    /// <returns>A list of problems should be returned. These will be displayed in the Visual   
    /// Studio error list</returns>  
    public override IList<SqlRuleProblem> Analyze(  
        SqlRuleExecutionContext ruleExecutionContext)  
    {  
         IList<SqlRuleProblem> problems = new List<SqlRuleProblem>();  
  
         TSqlObject modelElement = ruleExecutionContext.ModelElement;  
  
         // this rule does not apply to inline table-valued function  
         // we simply do not return any problem in that case.  
         if (IsInlineTableValuedFunction(modelElement))  
         {  
             return problems;  
         }  
  
         string elementName = GetElementName(ruleExecutionContext, modelElement);  
  
         // The rule execution context has all the objects we'll need, including the   
         // fragment representing the object,  
         // and a descriptor that lets us access rule metadata  
         TSqlFragment fragment = ruleExecutionContext.ScriptFragment;  
         RuleDescriptor ruleDescriptor = ruleExecutionContext.RuleDescriptor;  
  
         // To process the fragment and identify WAITFOR DELAY statements we will use a   
         // visitor   
         WaitForDelayVisitor visitor = new WaitForDelayVisitor();  
         fragment.Accept(visitor);  
         IList<WaitForStatement> waitforDelayStatements = visitor.WaitForDelayStatements;  
  
         // Create problems for each WAITFOR DELAY statement found   
         // When creating a rule problem, always include the TSqlObject being analyzed. This   
         // is used to determine  
         // the name of the source this problem was found in and a best guess as to the   
         // line/column the problem was found at.  
         //  
         // In addition if you have a specific TSqlFragment that is related to the problem   
         //also include this  
         // since the most accurate source position information (start line and column) will   
         // be read from the fragment  
         foreach (WaitForStatement waitForStatement in waitforDelayStatements)  
         {  
            SqlRuleProblem problem = new SqlRuleProblem(  
                String.Format(CultureInfo.CurrentCulture,   
                    ruleDescriptor.DisplayDescription, elementName),  
                modelElement,  
                waitForStatement);  
            problems.Add(problem);  
        }  
        return problems;  
    }  
  
    private static string GetElementName(  
        SqlRuleExecutionContext ruleExecutionContext,   
        TSqlObject modelElement)  
    {  
        // Get the element name using the built in DisplayServices. This provides a number of   
        // useful formatting options to  
        // make a name user-readable  
        var displayServices = ruleExecutionContext.SchemaModel.DisplayServices;  
        string elementName = displayServices.GetElementName(  
            modelElement, ElementNameStyle.EscapedFullyQualifiedName);  
        return elementName;  
    }  
  
    private static bool IsInlineTableValuedFunction(TSqlObject modelElement)  
    {  
        return TableValuedFunction.TypeClass.Equals(modelElement.ObjectType)  
                       && FunctionType.InlineTableValuedFunction ==   
            modelElement.GetMetadata<FunctionType>(TableValuedFunction.FunctionType);  
    }  
    ```  
  
10. Cliquez sur **Fichier** > **Enregistrer**.  
  
### <a name="building-the-class-library"></a>Création de la bibliothèque de classes  
  
1. Dans le menu **Projet**, cliquez sur **Propriétés de SampleRules**.  
  
2. Cliquez sur l'onglet **Signature** .  
  
3. Cliquez sur **Signer l'assembly**.  
  
4. Dans **Choisir un fichier de clé de nom fort**, cliquez sur **<New>** .  
  
5. Dans la boîte de dialogue **Créer une clé de nom fort** , dans **Nom du fichier de clé**, tapez MyRefKey.  
  
6. (facultatif) Vous pouvez spécifier un mot de passe pour votre fichier de clé de nom fort.  
  
7. Cliquez sur **OK**.  
  
8. Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
9. Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
Ensuite, vous devez installer l'assembly afin qu'il soit chargé lorsque vous générez et déployez des projets SQL Server.  
  
## <a name="install-a-static-code-analysis-rule"></a>Installer une règle d’analyse statique du code

Pour installer une règle, vous devez copier l’assembly et le fichier .pdb qui lui est associé dans le dossier Extensions.  
  
### <a name="to-install-the-samplerules-assembly"></a>Pour installer l'assembly SampleRules

Puis, vous allez copier les informations d'assembly dans le répertoire Extensions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions, et mises à disposition.  
  
Pour Visual Studio 2012, le <Visual Studio Install Dir> est généralement C:\Program Files (x86)\\MicrosoftVisual Studio 11.0. Pour Visual Studio 2013, il s’agit généralement de C:\Program Files (x86)\\MicrosoftVisual Studio 12.0.  
  
Copiez le fichier d’assembly SampleRules.dll du répertoire de sortie vers le répertoire <Visual Studio Install Dir>\Common7\IDE\Extensions\\Microsoft\SQLDB\DAC\120\Extensions. Par défaut, le chemin d'accès du fichier .dll compilé est le suivant : Chemin de votre solution\Chemin de votre projet\bin\Debug ou Chemin de votre solution\Chemin de votre projet\bin\Release.  
  
Votre règle doit maintenant être installée et apparaîtra une fois que vous aurez redémarré Visual Studio. Vous allez ensuite démarrer une nouvelle session de Visual Studio et créer un projet de base de données.  
  
### <a name="starting-a-new-visual-studio-session-and-creating-a-database-project"></a>Démarrage d’une nouvelle session de Visual Studio et création d’un projet de base de données  
  
1. Démarrez une deuxième session de Visual Studio.  
  
2. Cliquez sur **Fichier** > **Nouveau** > **Projet**.  
  
3. Dans la boîte de dialogue **Nouveau projet**, dans la liste **Modèles installés**, développez le nœud **SQL Server**, puis cliquez sur le nœud **Projet de base de données SQL Server**.  
  
4. Dans la zone de texte **Nom**, tapez SampleRulesDB, puis cliquez sur **OK**.  
  
Enfin, la nouvelle règle s’affiche dans le projet SQL Server. Pour afficher la nouvelle règle d'analyse du code AvoidWaitForRule  
  
1. Dans **l’Explorateur de solutions**, sélectionnez le projet SampleRulesDB.  
  
2. Dans le menu **Projet** , cliquez sur **Propriétés**. La page de propriétés SampleRulesDB s'affiche.  
  
3. Cliquez sur **Analyse du code**. Vous devez voir une nouvelle catégorie nommée RuleSamples.CategorySamples.  
  
4. Développez RuleSamples.CategorySamples. Vous devez voir SR1004 : éviter d’utiliser des instructions WAITFOR DELAY dans des procédures stockées, des fonctions et des déclencheurs.  
  
## <a name="see-also"></a>Voir aussi

[Vue d’ensemble de l’extensibilité pour les règles d’analyse du code de base de données](../ssdt/overview-of-extensibility-for-database-code-analysis-rules.md)