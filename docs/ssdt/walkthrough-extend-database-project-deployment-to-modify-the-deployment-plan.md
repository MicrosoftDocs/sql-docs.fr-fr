---
title: Étendre le déploiement du projet de base de données pour modifier le plan de déploiement
description: Créez un contributeur de déploiement de type DeploymentPlanModifier qui programme la réexécution des lots de script de déploiement si des erreurs se produisent pendant l’exécution.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 22b077b1-fa25-49ff-94f6-6d0d196d870a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bdb9831caf2145b43340fdca9af3108ba20d3c34
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064954"
---
# <a name="walkthrough-extend-database-project-deployment-to-modify-the-deployment-plan"></a>Procédure pas à pas : Étendre le déploiement du projet de base de données pour modifier le plan de déploiement

Vous pouvez créer des contributeurs de déploiement pour effectuer des actions personnalisées lorsque vous déployez un projet SQL. Vous pouvez créer soit un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) ou un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). Utilisez un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) pour changer le plan avant son exécution et un [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor) pour effectuer des opérations alors que le plan est en cours d'exécution. Dans cette procédure pas à pas, vous allez créer un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) nommé SqlRestartableScriptContributor qui ajoute des instructions IF aux lots dans le script de déploiement pour permettre la réexécution du script jusqu'à son aboutissement si une erreur se produit pendant l'exécution.  
  
Au cours de cette procédure pas à pas, vous allez effectuer les tâches principales suivantes :  
  
-   [Créer le type de contributeur de déploiement DeploymentPlanModifier](#CreateDeploymentContributor)  
  
-   [Installer le contributeur de déploiement](#InstallDeploymentContributor)  
  
-   [Exécuter ou tester votre contributeur de déploiement](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>Prérequis  
Vous devez disposer des éléments suivants pour exécuter cette procédure pas à pas :  
  
-   Vous devez avoir installé une version de Visual Studio qui inclut SQL Server Data Tools et qui prend en charge le développement en C# ou VB.  
  
-   Vous devez disposer d'un projet SQL qui contient des objets SQL.  
  
-   Instance de SQL Server dans laquelle vous pouvez déployer un projet de base de données.  
  
> [!NOTE]  
> Cette procédure pas à pas est destinée aux utilisateurs qui sont déjà familiarisés avec les fonctionnalités SQL de SQL Server Data Tools. Vous devez également être familiarisé avec les concepts de base de Visual Studio, comme la création d'une bibliothèque de classes et l'utilisation de l'éditeur de code pour ajouter du code à une classe.  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>Créer un contributeur de déploiement  
Pour créer un contributeur de déploiement, vous devez effectuer les tâches suivantes :  
  
-   Créez un projet Bibliothèque de classes et ajoutez les références requises.  
  
-   Définissez une classe nommée SqlRestartableScriptContributor qui hérite de [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier).  
  
-   Remplacez la méthode [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute).  
  
-   Ajouter des méthodes d'assistance privées.  
  
-   Générer l'assembly résultant.  
  
#### <a name="to-create-a-class-library-project"></a>Pour créer un projet de bibliothèque de classes  
  
1.  Créez un projet Bibliothèque de classes Visual Basic ou Visual C# nommé MyOtherDeploymentContributor.  
  
2.  Renommez le fichier « Class1.cs » en « SqlRestartableScriptContributor.cs ».  
  
3.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**.  
  
4.  Sous l’onglet Infrastructure, sélectionnez **System.ComponentModel.Composition**.  
  
5.  Cliquez sur **Parcourir** et accédez au répertoire de **C:\Program Files (x86)\Microsoft SQL Server\110\SDK\Assemblies**, sélectionnez **Microsoft.SqlServer.TransactSql.ScriptDom.dll**, puis cliquez sur **OK**.  
  
6.  Ajoutez les références SQL requises : cliquez avec le bouton droit sur le nœud de projet puis cliquez sur **Ajouter une référence**. Cliquez sur **Parcourir** et accédez au dossier **C:\Program files (x86)\Microsoft SQL Server\110\DAC\bin**. Sélectionnez les entrées **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** et **Microsoft.Data.Tools.Schema.Sql.dll**, cliquez sur **Ajouter**, puis sur **OK**.  
  
Commencez ensuite à ajouter le code à la classe.  
  
#### <a name="to-define-the-sqlrestartablescriptcontributor-class"></a>Pour définir la classe SqlRestartableScriptContributor  
  
1.  Dans l'éditeur de code, mettez à jour le fichier class1.cs pour le faire correspondre aux instructions **using** suivantes :  
  
    ```csharp  
    using System;  
    using System.Collections.Generic;  
    using System.Globalization;  
    using System.Text;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Model;  
    using Microsoft.SqlServer.TransactSql.ScriptDom;  
    ```  
  
2.  Mettez à jour la définition de classe pour qu'elle corresponde à l'exemple suivant :  
  
    ```csharp  
        /// <summary>  
    /// This deployment contributor modifies a deployment plan by adding if statements  
    /// to the existing batches in order to make a deployment script able to be rerun to completion  
    /// if an error is encountered during execution  
    /// </summary>  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
    public class SqlRestartableScriptContributor : DeploymentPlanModifier  
    {  
    }  
  
    ```  
  
    Vous avez défini un contributeur de déploiement qui hérite de [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier). Lors de la génération et du déploiement, les contributeurs personnalisés sont chargés à partir d'un répertoire d'extension standard. Les contributeurs de modification du plan de déploiement sont identifiés par un attribut [ExportDeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanmodifierattribute). Cet attribut est requis afin que les contributeurs puissent être découverts. Cet attribut doit présenter un aspect similaire au suivant :  
  
    ```csharp  
    [ExportDeploymentPlanModifier("MyOtherDeploymentContributor.RestartableScriptContributor", "1.0.0.0")]  
  
    ```  
  
3.  Ajoutez les déclarations de membres suivantes :  
  
    ```vb  
         private const string BatchIdColumnName = "BatchId";  
            private const string DescriptionColumnName = "Description";  
  
            private const string CompletedBatchesVariableName = "CompletedBatches";  
            private const string CompletedBatchesVariable = "$(CompletedBatches)";  
            private const string CompletedBatchesSqlCmd = @":setvar " + CompletedBatchesVariableName + " __completedBatches_{0}_{1}";  
            private const string TotalBatchCountSqlCmd = @":setvar TotalBatchCount {0}";  
            private const string CreateCompletedBatchesTable = @"  
    if OBJECT_ID(N'tempdb.dbo." + CompletedBatchesVariable + @"', N'U') is null  
    begin  
    use tempdb  
    create table [dbo].[$(CompletedBatches)]  
    (  
    BatchId int primary key,  
    Description nvarchar(300)  
    )  
    use [$(DatabaseName)]  
    end  
    ";  
  
    ```  
  
    Ensuite, vous remplacez la méthode OnExecute pour ajouter le code à exécuter lors du déploiement d'un projet de base de données.  
  
#### <a name="to-override-onexecute"></a>Pour remplacer OnExecute  
  
1.  Ajoutez la méthode suivante à votre classe SqlRestartableScriptContributor :  
  
    ```csharp  
    /// <summary>  
    /// You override the OnExecute method to do the real work of the contributor.  
    /// </summary>  
    /// <param name="context"></param>  
    protected override void OnExecute(DeploymentPlanContributorContext context)  
    {  
         // Replace this with the method body  
    }  
  
    ```  
  
    Remplacez la méthode [OnExecute](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor.onexecute) de la classe de base [DeploymentPlanContributor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributor), qui est la classe de base pour [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) et [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). La méthode OnExecute se voit transmettre un objet [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) qui permet d'accéder à tous les arguments spécifiés, au modèle de base de données source et cible, au plan de déploiement et aux options de déploiement. Dans cet exemple, nous obtenons le plan de déploiement et nom de la base de données cible.  
  
2.  Ajoutez à présent le début d'un corps à la méthode de OnExecute :  
  
    ```vb  
    // Obtain the first step in the Plan from the provided context  
    DeploymentStep nextStep = context.PlanHandle.Head;  
    int batchId = 0;  
    BeginPreDeploymentScriptStep beforePreDeploy = null;  
  
    // Loop through all steps in the deployment plan  
    while (nextStep != null)  
    {  
        // Increment the step pointer, saving both the current and next steps  
        DeploymentStep currentStep = nextStep;  
        nextStep = currentStep.Next;  
  
        // Add additional step processing here  
    }  
  
    // if we found steps that required processing, set up a temporary table to track the work that you are doing  
    if (beforePreDeploy != null)  
    {  
        // Add additional post-processing here  
    }  
  
    // Cleanup and drop the table   
    DeploymentScriptStep dropStep = new DeploymentScriptStep(DropCompletedBatchesTable);  
    base.AddAfter(context.PlanHandle, context.PlanHandle.Tail, dropStep);  
  
    ```  
  
    Dans ce code, nous définissons quelques variables locales, et nous configurons la boucle qui gère le traitement de toutes les étapes du plan de déploiement. Une fois que la boucle est terminée, nous devrons effectuer des opérations post-traitement, puis nous supprimerons la table temporaire que nous avons créée lors du déploiement pour suivre la progression à mesure que le plan s'exécutait. Les types de clés sont ici : [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep) et [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep). AddAfter est une méthode de clé.  
  
3.  Ajoutez à présent l'étape de traitement supplémentaire, en remplacement du commentaire qui indique « Add additional step processing here » :  
  
    ```csharp  
    // Look for steps that mark the pre/post deployment scripts  
    // These steps will always be in the deployment plan even if the  
    // user's project does not have a pre/post deployment script  
    if (currentStep is BeginPreDeploymentScriptStep)  
    {  
        // This step marks the beginning of the predeployment script.  
        // Save the step and move on.  
        beforePreDeploy = (BeginPreDeploymentScriptStep)currentStep;  
        continue;  
    }  
    if (currentStep is BeginPostDeploymentScriptStep)  
    {  
        // This is the step that marks the beginning of the post deployment script.    
        // We do not continue processing after this point.  
        break;  
    }  
    if (currentStep is SqlPrintStep)  
    {  
        // We do not need to put if statements around these  
        continue;  
    }  
  
    // if we have not yet found the beginning of the pre-deployment script steps,   
    // skip to the next step.  
    if (beforePreDeploy == null)  
    {  
        // We only surround the "main" statement block with conditional  
        // statements  
        continue;  
    }  
  
    // Determine if this is a step that we need to surround with a conditional statement  
    DeploymentScriptDomStep domStep = currentStep as DeploymentScriptDomStep;  
    if (domStep == null)  
    {  
        // This step is not a step that we know how to modify,  
        // so skip to the next step.  
        continue;  
    }  
  
    TSqlScript script = domStep.Script as TSqlScript;  
    if (script == null)  
    {  
        // The script dom step does not have a script with batches - skip  
        continue;  
    }  
  
        // Loop through all the batches in the script for this step.  All the statements  
        // in the batch will be enclosed in an if statement that will check the  
        // table to ensure that the batch has not already been executed  
        TSqlObject sqlObject;  
        string stepDescription;  
        GetStepInfo(domStep, out stepDescription, out sqlObject);  
        int batchCount = script.Batches.Count;  
  
    for (int batchIndex = 0; batchIndex < batchCount; batchIndex++)  
    {  
        // Add batch processing here  
    }  
  
    ```  
  
    Les commentaires de code expliquent le traitement. De façon générale, ce code recherche les étapes qui vous intéressent, en ignore d'autres et s'arrête lorsque vous atteignez le début des étapes post-déploiement. Si l'étape contient des instructions que nous devons entourer de conditions, nous effectuerons un traitement supplémentaire. Les types de clés, les méthodes et les propriétés sont les suivants : [BeginPreDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpredeploymentscriptstep), [BeginPostDeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.beginpostdeploymentscriptstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [TSqlScript](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlscript), Script, [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep) et [SqlPrintStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlprintstep).  
  
4.  Maintenant, ajoutez le code de traitement par lots en remplaçant le commentaire qui indique « Add batch processing here » :  
  
    ```csharp  
        // Create the if statement that will contain the batch's contents  
        IfStatement ifBatchNotExecutedStatement = CreateIfNotExecutedStatement(batchId);  
        BeginEndBlockStatement statementBlock = new BeginEndBlockStatement();  
        ifBatchNotExecutedStatement.ThenStatement = statementBlock;  
        statementBlock.StatementList = new StatementList();  
  
        TSqlBatch batch = script.Batches[batchIndex];  
        int statementCount = batch.Statements.Count;  
  
        // Loop through all statements in the batch, embedding those in an sp_execsql  
        // statement that must be handled this way (schemas, stored procedures,   
        // views, functions, and triggers).  
        for (int statementIndex = 0; statementIndex < statementCount; statementIndex++)  
        {  
            // Add additional statement processing here  
        }  
  
        // Add an insert statement to track that all the statements in this  
        // batch were executed.  Turn on nocount to improve performance by  
        // avoiding row inserted messages from the server  
        string batchDescription = string.Format(CultureInfo.InvariantCulture,  
            "{0} batch {1}", stepDescription, batchIndex);  
  
        PredicateSetStatement noCountOff = new PredicateSetStatement();  
        noCountOff.IsOn = false;  
        noCountOff.Options = SetOptions.NoCount;  
  
        PredicateSetStatement noCountOn = new PredicateSetStatement();  
        noCountOn.IsOn = true;  
        noCountOn.Options = SetOptions.NoCount;   
        InsertStatement batchCompleteInsert = CreateBatchCompleteInsert(batchId, batchDescription);  
        statementBlock.StatementList.Statements.Add(noCountOn);  
    statementBlock.StatementList.Statements.Add(batchCompleteInsert);  
        statementBlock.StatementList.Statements.Add(noCountOff);  
  
        // Remove all the statements from the batch (they are now in the if block) and add the if statement  
        // as the sole statement in the batch  
        batch.Statements.Clear();  
        batch.Statements.Add(ifBatchNotExecutedStatement);  
  
        // Next batch  
        batchId++;  
  
    ```  
  
    Ce code crée une instruction IF avec un bloc BEGIN/END. Nous effectuons le traitement supplémentaire sur les instructions du lot. Une fois qu'il est terminé, nous ajoutons une instruction INSERT pour ajouter des informations dans la table temporaire qui suit l'avancement de l'exécution du script. Enfin, mettez à jour le lot, en remplaçant les instructions par le nouveau IF qui contient les instructions. Les types de clés, méthodes et propriétés sont les suivantes : [IfStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.ifstatement), [BeginEndBlockStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.beginendblockstatement), [StatementList](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.statementlist), [TSqlBatch](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlbatch), [PredicateSetStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.predicatesetstatement), [SetOptions](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.setoptions) et [InsertStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.insertstatement).  
  
5.  Maintenant, ajoutez le corps de la boucle de traitement de l'instruction. Remplacez le commentaire qui indique « Add additional statement processing here » :  
  
    ```csharp  
    TSqlStatement smnt = batch.Statements[statementIndex];  
  
    if (IsStatementEscaped(sqlObject))  
    {  
        // "escape" this statement by embedding it in a sp_executesql statement  
        string statementScript;  
        domStep.ScriptGenerator.GenerateScript(smnt, out statementScript);  
        ExecuteStatement spExecuteSql = CreateExecuteSql(statementScript);  
        smnt = spExecuteSql;  
    }  
  
    statementBlock.StatementList.Statements.Add(smnt);  
  
    ```  
  
    Pour chaque instruction du lot, si l'instruction est d'un type qui doit être intégré dans une instruction sp_executesql, modifiez l'instruction en conséquence. Le code ajoute ensuite l'instruction à la liste d'instructions pour le bloc BEGIN/END que vous avez créé. Les types de clés, méthodes et propriétés sont [TSqlStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.tsqlstatement) et [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement).  
  
6.  Enfin, ajoutez la partie de post-traitement en remplacement du commentaire qui indique « Add additional post-processing here » :  
  
    ```csharp  
    // Declare a SqlCmd variables.  
    //  
    // CompletedBatches variable - defines the name of the table in tempdb that will track  
    // all the completed batches.  The temporary table's name has the target database name and  
    // a guid embedded in it so that:  
    // * Multiple deployment scripts targeting different DBs on the same server  
    // * Failed deployments with old tables do not conflict with more recent deployments  
    //  
    // TotalBatchCount variable - the total number of batches surrounded by if statements.  Using this  
    // variable pre/post deployment scripts can also use the CompletedBatches table to make their  
    // script rerunnable if there is an error during execution  
    StringBuilder sqlcmdVars = new StringBuilder();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, CompletedBatchesSqlCmd,  
        context.Options.TargetDatabaseName, Guid.NewGuid().ToString("D"));  
    sqlcmdVars.AppendLine();  
    sqlcmdVars.AppendFormat(CultureInfo.InvariantCulture, TotalBatchCountSqlCmd, batchId);  
  
    DeploymentScriptStep completedBatchesSetVarStep = new DeploymentScriptStep(sqlcmdVars.ToString());  
    base.AddBefore(context.PlanHandle, beforePreDeploy, completedBatchesSetVarStep);  
  
    // Create the temporary table we will use to track the work that we are doing  
    DeploymentScriptStep createStatusTableStep = new DeploymentScriptStep(CreateCompletedBatchesTable);  
    base.AddBefore(context.PlanHandle, beforePreDeploy, createStatusTableStep);  
  
    ```  
  
    Si notre traitement a détecté une ou plusieurs étapes que nous avons entourées par une instruction conditionnelle, nous devons ajouter des instructions pour que le script de déploiement définisse les variables SQLCMD. La première variable, CompletedBatches, contient un nom unique pour la table temporaire que le script de déploiement utilise pour gérer les lots correctement finalisés lorsque le script a été exécuté. La deuxième variable, TotalBatchCount, contient le nombre de lots dans le script de déploiement.  
  
    D'autres types, propriétés et méthodes dignes d'intérêt sont les suivantes :  
  
    StringBuilder, [DeploymentScriptStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptstep) et AddBefore.  
  
    Vous définissez ensuite les méthodes d'assistance appelées par cette méthode.  
  
#### <a name="to-add-the-helper-methods"></a>Pour ajouter des méthodes d'assistance  
  
-   Plusieurs méthodes d'assistance doivent être définies. Les méthodes importantes sont les suivantes :  
  
    |**Méthode**|**Description**|  
    |--------------|-------------------|  
    |CreateExecuteSQL|Définit la méthode CreateExecuteSQL qui va entourer une instruction fournie avec une instruction EXEC sp_executesql. Les types de clés, les méthodes et les propriétés sont les suivantes : [ExecuteStatement](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executestatement), [ExecutableProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executableprocedurereference), [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname), [ProcedureReference](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.procedurereference) et [ExecuteParameter](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.executeparameter).|  
    |CreateCompletedBatchesName|Définit la méthode CreateCompletedBatchesName. Cette méthode crée le nom qui sera inséré dans la table temporaire d'un lot. Les types de clés, les méthodes et les propriétés sont les suivantes : [SchemaObjectName](/dotnet/api/microsoft.sqlserver.transactsql.scriptdom.schemaobjectname).|  
    |IsStatementEscaped|Définit la méthode IsStatementEscaped. Cette méthode détermine si le type d'élément de modèle requiert l'instruction à inclure dans une instruction EXEC sp_executesql avant son placement dans une instruction IF. Les méthodes, les propriétés et les types de clés sont les suivants : TSqlObject.ObjectType, ModelTypeClass et la propriété TypeClass pour les types de modèles suivants : Schema, Procedure, View,  TableValuedFunction, ScalarFunction, DatabaseDdlTrigger, DmlTrigger, ServerDdlTrigger.|  
    |CreateBatchCompleteInsert|Définit la méthode CreateBatchCompleteInsert. Cette méthode crée l'instruction INSERT qui sera ajoutée au script de déploiement pour assurer le suivi de l'exécution du script. Les types de clés, méthodes et propriétés sont les suivants : InsertStatement, NamedTableReference, ColumnReferenceExpression, ValuesInsertSource, et RowValue.|  
    |CreateIfNotExecutedStatement|Définit la méthode CreateIfNotExecutedStatement. Cette méthode génère une instruction IF qui vérifie si la table d'exécution des lots temporaires indique que ce lot a déjà été exécuté. Les méthodes, les propriétés et les types de clés sont les suivants : IfStatement, ExistsPredicate, ScalarSubquery, NamedTableReference, WhereClause, ColumnReferenceExpression, IntegerLiteral, BooleanComparisonExpression et BooleanNotExpression.|  
    |GetStepInfo|Définit la méthode GetStepInfo. Cette méthode extrait les informations sur l’élément de modèle utilisé pour créer le script de l’étape, en plus du nom de l’étape. Les types et méthodes dignes d'intérêt sont les suivants : [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext), [DeploymentScriptDomStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentscriptdomstep), [TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject), [CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep), [AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep) et [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep).|  
    |GetElementName|Crée un nom mis en forme pour un TSqlObject.|  
  
1.  Ajoutez le code suivant pour définir les méthodes d'assistance :  
  
    ```csharp  
  
    /// <summary>  
    /// The CreateExecuteSql method "wraps" the provided statement script in an "sp_executesql" statement  
    /// Examples of statements that must be so wrapped include: stored procedures, views, and functions  
    /// </summary>  
    private static ExecuteStatement CreateExecuteSql(string statementScript)  
    {  
        // define a new Exec statement  
        ExecuteStatement executeSp = new ExecuteStatement();  
        ExecutableProcedureReference spExecute = new ExecutableProcedureReference();  
        executeSp.ExecuteSpecification = new ExecuteSpecification { ExecutableEntity = spExecute };  
  
        // define the name of the procedure that you want to execute, in this case sp_executesql  
        SchemaObjectName procName = new SchemaObjectName();  
        procName.Identifiers.Add(CreateIdentifier("sp_executesql", QuoteType.NotQuoted));  
        ProcedureReference procRef = new ProcedureReference { Name = procName };  
  
        spExecute.ProcedureReference = new ProcedureReferenceName { ProcedureReference = procRef };  
  
        // add the script parameter, constructed from the provided statement script  
        ExecuteParameter scriptParam = new ExecuteParameter();  
        spExecute.Parameters.Add(scriptParam);  
        scriptParam.ParameterValue = new StringLiteral { Value = statementScript };  
        scriptParam.Variable = new VariableReference { Name = "@stmt" };  
        return executeSp;  
    }  
  
    /// <summary>  
    /// The CreateIdentifier method returns a Identifier with the specified value and quoting type  
    /// </summary>  
    private static Identifier CreateIdentifier(string value, QuoteType quoteType)  
    {  
        return new Identifier { Value = value, QuoteType = quoteType };  
    }  
  
    /// <summary>  
    /// The CreateCompletedBatchesName method creates the name that will be inserted  
    /// into the temporary table for a batch.  
    /// </summary>  
    private static SchemaObjectName CreateCompletedBatchesName()  
    {  
        SchemaObjectName name = new SchemaObjectName();  
        name.Identifiers.Add(CreateIdentifier("tempdb", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier("dbo", QuoteType.SquareBracket));  
        name.Identifiers.Add(CreateIdentifier(CompletedBatchesVariable, QuoteType.SquareBracket));  
        return name;  
    }  
  
    /// <summary>  
    /// Helper method that determins whether the specified statement needs to  
    /// be escaped  
    /// </summary>  
    /// <param name="sqlObject"></param>  
    /// <returns></returns>  
    private static bool IsStatementEscaped(TSqlObject sqlObject)  
    {  
        HashSet<ModelTypeClass> escapedTypes = new HashSet<ModelTypeClass>  
        {  
            Schema.TypeClass,  
            Procedure.TypeClass,  
            View.TypeClass,  
            TableValuedFunction.TypeClass,  
            ScalarFunction.TypeClass,  
            DatabaseDdlTrigger.TypeClass,  
            DmlTrigger.TypeClass,  
            ServerDdlTrigger.TypeClass  
        };  
        return escapedTypes.Contains(sqlObject.ObjectType);  
    }  
  
    /// <summary>  
    /// Helper method that creates an INSERT statement to track a batch being completed  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <param name="batchDescription"></param>  
    /// <returns></returns>  
    private static InsertStatement CreateBatchCompleteInsert(int batchId, string batchDescription)  
    {  
        InsertStatement insert = new InsertStatement();  
        NamedTableReference batchesCompleted = new NamedTableReference();  
        insert.InsertSpecification = new InsertSpecification();  
        insert.InsertSpecification.Target = batchesCompleted;  
        batchesCompleted.SchemaObject = CreateCompletedBatchesName();  
  
        // Build the columns inserted into  
        ColumnReferenceExpression batchIdColumn = new ColumnReferenceExpression();  
        batchIdColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        batchIdColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(BatchIdColumnName, QuoteType.NotQuoted));  
  
        ColumnReferenceExpression descriptionColumn = new ColumnReferenceExpression();  
        descriptionColumn.MultiPartIdentifier = new MultiPartIdentifier();  
        descriptionColumn.MultiPartIdentifier.Identifiers.Add(CreateIdentifier(DescriptionColumnName, QuoteType.NotQuoted));  
  
        insert.InsertSpecification.Columns.Add(batchIdColumn);  
        insert.InsertSpecification.Columns.Add(descriptionColumn);  
  
        // Build the values inserted  
        ValuesInsertSource valueSource = new ValuesInsertSource();  
        insert.InsertSpecification.InsertSource = valueSource;  
  
        RowValue values = new RowValue();  
        values.ColumnValues.Add(new IntegerLiteral { Value = batchId.ToString() });  
        values.ColumnValues.Add(new StringLiteral { Value = batchDescription });  
        valueSource.RowValues.Add(values);  
  
        return insert;  
    }  
  
    /// <summary>  
    /// This is a helper method that generates an if statement that checks the batches executed  
    /// table to see if the current batch has been executed.  The if statement will look like this  
    ///   
    /// if not exists(select 1 from [tempdb].[dbo].[$(CompletedBatches)]   
    ///                where BatchId = batchId)  
    /// begin  
    /// end  
    /// </summary>  
    /// <param name="batchId"></param>  
    /// <returns></returns>  
    private static IfStatement CreateIfNotExecutedStatement(int batchId)  
    {  
        // Create the exists/select statement  
        ExistsPredicate existsExp = new ExistsPredicate();  
        ScalarSubquery subQuery = new ScalarSubquery();  
        existsExp.Subquery = subQuery;  
  
        subQuery.QueryExpression = new QuerySpecification  
        {  
            SelectElements =  
            {  
                new SelectScalarExpression  { Expression = new IntegerLiteral { Value ="1" } }  
            },  
            FromClause = new FromClause  
            {  
                TableReferences =  
                    {  
                        new NamedTableReference() { SchemaObject = CreateCompletedBatchesName() }  
                    }  
            },  
            WhereClause = new WhereClause  
            {  
                SearchCondition = new BooleanComparisonExpression  
                {  
                    ComparisonType = BooleanComparisonType.Equals,  
                    FirstExpression = new ColumnReferenceExpression  
                    {  
                        MultiPartIdentifier = new MultiPartIdentifier  
                        {  
                            Identifiers = { CreateIdentifier(BatchIdColumnName, QuoteType.SquareBracket) }  
                        }  
                    },  
                    SecondExpression = new IntegerLiteral { Value = batchId.ToString() }  
                }  
            }  
        };  
  
        // Put together the rest of the statement  
        IfStatement ifNotExists = new IfStatement  
        {  
            Predicate = new BooleanNotExpression  
            {  
                Expression = existsExp  
            }  
        };  
  
        return ifNotExists;  
    }  
  
    /// <summary>  
    /// Helper method that generates a useful description of the step.  
    /// </summary>  
    private static void GetStepInfo(  
        DeploymentScriptDomStep domStep,  
        out string stepDescription,  
        out TSqlObject element)  
    {  
        element = null;  
  
        // figure out what type of step we've got, and retrieve  
        // either the source or target element.  
        if (domStep is CreateElementStep)  
        {  
            element = ((CreateElementStep)domStep).SourceElement;  
        }  
        else if (domStep is AlterElementStep)  
        {  
            element = ((AlterElementStep)domStep).SourceElement;  
        }  
        else if (domStep is DropElementStep)  
        {  
            element = ((DropElementStep)domStep).TargetElement;  
        }  
  
        // construct the step description by concatenating the type and the fully qualified  
        // name of the associated element.  
        string stepTypeName = domStep.GetType().Name;  
        if (element != null)  
        {  
            string elementName = GetElementName(element);  
  
            stepDescription = string.Format(CultureInfo.InvariantCulture, "{0} {1}",  
                stepTypeName, elementName);  
        }  
        else  
        {  
            // if the step has no associated element, just use the step type as the description  
            stepDescription = stepTypeName;  
        }  
    }  
  
    private static string GetElementName(TSqlObject element)  
    {  
        StringBuilder name = new StringBuilder();  
        if (element.Name.HasExternalParts)  
        {  
            foreach (string part in element.Name.ExternalParts)  
            {  
                if (name.Length > 0)  
                {  
                    name.Append('.');  
                }  
                name.AppendFormat("[{0}]", part);  
            }  
        }  
  
        foreach (string part in element.Name.Parts)  
        {  
            if (name.Length > 0)  
            {  
                name.Append('.');  
            }  
            name.AppendFormat("[{0}]", part);  
        }  
  
        return name.ToString();  
    }  
  
    ```  
  
2.  Enregistrez les changements apportés à SqlRestartableScriptContributor.cs.  
  
Générez ensuite la bibliothèque de classes.  
  
#### <a name="to-sign-and-build-the-assembly"></a>Pour signer et générer l'assembly  
  
1.  Dans le menu **Projet**, cliquez sur **Propriétés de MyOtherDeploymentContributor**.  
  
2.  Cliquez sur l'onglet **Signature** .  
  
3.  Cliquez sur **Signer l'assembly**.  
  
4.  Dans **Choisir un fichier de clé de nom fort**, cliquez sur **<New>** .  
  
5.  Dans la boîte de dialogue **Créer une clé de nom fort** , dans **Nom du fichier de clé**, tapez **MyRefKey**.  
  
6.  (facultatif) Vous pouvez spécifier un mot de passe pour votre fichier de clé de nom fort.  
  
7.  Cliquez sur **OK**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
9. Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
    Ensuite, vous devez installer l'assembly afin qu'il soit chargé lorsque vous déployez des projets SQL.  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>Installer un contributeur de déploiement  
Pour installer un contributeur de déploiement, vous devez copier l'assembly et le fichier .pdb associé dans le dossier Extensions.  
  
#### <a name="to-install-the-myotherdeploymentcontributor-assembly"></a>Pour installer l'assembly MyOtherDeploymentContributor  
  
1.  Puis, vous allez copier les informations d'assembly dans le répertoire Extensions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire et les sous-répertoires %Program Files%\\Microsoft SQL Server\110\DAC\Bin\Extensions\Microsoft\SQLDB\TestConditions et mises à disposition.  
  
2.  Copiez le fichier d'assembly de **MyOtherDeploymentContributor.dll** du répertoire de sortie dans le répertoire %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions. Par défaut, le chemin d'accès du fichier .dll compilé est le suivant : Chemin de votre solution\Chemin de votre projet\bin\Debug ou Chemin de votre solution\Chemin de votre projet\bin\Release.  
  
## <a name="run-or-test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>Exécuter ou tester votre contributeur de déploiement  
Pour exécuter ou tester un contributeur de déploiement, vous devez effectuer les tâches suivantes :  
  
-   Ajouter des propriétés au fichier .sqlproj que vous envisagez de générer.  
  
-   Déployer la base de données du projet à l'aide de MSBuild et en fournissant les paramètres appropriés.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Ajouter des propriétés au fichier de projet SQL (.sqlproj)  
Vous devez toujours mettre à jour le fichier de projet SQL pour spécifier l'ID des contributeurs à exécuter. Vous pouvez le faire de deux façons :  
  
1.  Vous pouvez modifier manuellement le fichier .sqlproj pour ajouter les arguments requis. Vous pouvez décider de procéder de la sorte si un contributeur n'a pas d'arguments de contributeur requis pour la configuration ou si vous n'envisagez pas de réutiliser ce contributeur de génération sur un grand nombre de projets. Si vous choisissez cette option, ajoutez les instructions suivantes dans le fichier .sqlproj après le premier nœud d'importation dans le fichier :  
  
    ```  
    <PropertyGroup>  
      <DeploymentContributors>  
        $(DeploymentContributors); MyOtherDeploymentContributor.RestartableScriptContributor  
      </DeploymentContributors>  
    </PropertyGroup>  
    ```  
  
2.  La deuxième méthode consiste à créer un fichier de cibles qui contient les arguments de contributeur requis. Cela est utile si vous utilisez le même contributeur pour plusieurs projets et que vous possédez les arguments de contributeur requis, car il comprend les valeurs par défaut. Dans ce cas, créez un fichier de cibles dans le chemin d'accès des extensions Msbuild :  
  
    1.  Accédez à %Program Files%\Msbuild.  
  
    2.  Créez un dossier « MyContributors » où vos fichiers de cibles seront stockés.  
  
    3.  Créez un fichier « MyContributors.targets » dans ce répertoire, ajoutez le texte suivant, puis enregistrez le fichier :  
  
        ```  
        <?xml version="1.0" encoding="utf-8"?>  
  
        <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
          <PropertyGroup>  
            <DeploymentContributors>$(DeploymentContributors);MyOtherDeploymentContributor.RestartableScriptContributor</DeploymentContributors>  
          </PropertyGroup>  
        </Project>  
  
        ```  
  
    4.  Dans le fichier .sqlproj d’un projet pour lequel vous souhaitez exécuter des contributeurs, importez le fichier de cibles en ajoutant l’instruction suivante au fichier .sqlproj après le nœud \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> dans le fichier :  
  
        ```  
        <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
  
        ```  
  
Après avoir suivi une de ces approches, vous pouvez utiliser Msbuild pour transmettre les paramètres des générations par ligne de commande.  
  
> [!NOTE]  
> Vous devez toujours mettre à jour la propriété « DeploymentContributors » pour spécifier votre ID de contributeur Il s’agit du même ID que celui utilisé dans l’attribut « ExportDeploymentPlanModifier » dans le fichier source du contributeur. Sans cet ID le contributeur ne s'exécute pas lors de la génération du projet. La propriété « ContributorArguments » doit être mise à jour uniquement si des arguments sont nécessaires à l’exécution de votre contributeur.  
  
## <a name="deploy-the-database-project"></a>Déployer le projet de base de données  
  
#### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>Pour déployer votre projet SQL et générer un rapport de déploiement  
  
-   Votre projet peut être publié ou déployé normalement dans Visual Studio. Ouvrez une solution qui contient votre projet SQL et sélectionnez l’option Publier dans le menu contextuel du projet, ou appuyez sur la touche F5 pour un déploiement de débogage dans LocalDB. Dans cet exemple nous utiliserons la boîte de dialogue « Publier… » pour générer un script de déploiement.  
  
    1.  Ouvrez Visual Studio et ouvrez la solution qui contient votre projet SQL.  
  
    2.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le projet, puis sélectionnez l’option **Publier...** .  
  
    3.  Définissez le nom du serveur et le nom de la base de données sur lesquels publier.  
  
    4.  Sélectionnez **Générer un script** dans les options au bas de la boîte de dialogue. Cela crée un script qui peut être utilisé pour le déploiement. Vérifier que les instructions IF ont été ajoutées pour rendre le script redémarrable.  
  
    5.  Vérifiez le script de déploiement généré. Juste avant la section intitulée « Modèle de scripts de prédéploiement », vous devez obtenir quelque chose qui ressemble à la syntaxe Transact-SQL suivante :  
  
        ```  
        :setvar CompletedBatches __completedBatches_CompareProjectDB_cd1e348a-8f92-44e0-9a96-d25d65900fca  
        :setvar TotalBatchCount 17  
        GO  
  
        if OBJECT_ID(N'tempdb.dbo.$(CompletedBatches)', N'U') is null  
        begin  
        use tempdb  
        create table [dbo].[$(CompletedBatches)]  
        (  
        BatchId int primary key,  
        Description nvarchar(300)  
        )  
        use [$(DatabaseName)]  
        end  
  
        ```  
  
        Ultérieurement dans le script de déploiement, autour de chaque lot, vous voyez une instruction IF qui entoure l'instruction d'origine. Par exemple, pour une instruction CREATE SCHEMA :  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 0)  
            BEGIN  
                EXECUTE sp_executesql @stmt = N'CREATE SCHEMA [Sales]  
            AUTHORIZATION [dbo]';  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (0, N'CreateElementStep Sales batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        Notez que CREATE SCHEMA est une des instructions qui doivent être placées dans une instruction EXECUTE sp_executesql statement dans l'instruction IF. Les instructions telles que CREATE TABLE ne nécessitent pas l'instruction EXECUTE sp_executesql statement et ressemblent à l'exemple suivant :  
  
        ```  
        IF NOT EXISTS (SELECT 1  
                       FROM   [tempdb].[dbo].[$(CompletedBatches)]  
                       WHERE  [BatchId] = 1)  
            BEGIN  
                CREATE TABLE [Sales].[Customer] (  
                    [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
                    [CustomerName] NVARCHAR (40) NOT NULL,  
                    [YTDOrders]    INT           NOT NULL,  
                    [YTDSales]     INT           NOT NULL  
                );  
                SET NOCOUNT ON;  
                INSERT  [tempdb].[dbo].[$(CompletedBatches)] (BatchId, Description)  
                VALUES                                      (1, N'CreateElementStep Sales.Customer batch 0');  
                SET NOCOUNT OFF;  
            END  
  
        ```  
  
        > [!NOTE]  
        > Si vous déployez un projet de base de données identique à la base de données cible, le rapport résultant ne sera pas très explicite. Pour obtenir des résultats significatifs, déployez les modifications sur une base de données ou déployez une nouvelle base de données.  
  
## <a name="command-line-deployment-using-generated-dacpac-file"></a>Déploiement de ligne de commande à l'aide du fichier dacpac généré  
Une fois qu'un projet SQL a été créé, un fichier dacpac est créé qui peut être utilisé pour déployer le schéma à partir de la ligne de commande et qui peut activer le déploiement à partir d'un ordinateur différent tel qu'un ordinateur de génération. SqlPackage est un utilitaire de ligne de commande permettant le déploiement de fichiers dacpacs avec diverses options qui permettent aux utilisateurs de déployer un dacpac ou de générer un script de déploiement, entre autres actions. Pour plus d'informations, reportez-vous à [SqlPackage.exe](../tools/sqlpackage/sqlpackage.md).  
  
> [!NOTE]  
> Pour déployer avec succès des dacpacs à partir de projets créés avec la propriété DeploymentContributors définie, la ou les DLL contenant les contributeurs de déploiement doivent être installées sur l'ordinateur utilisé. Cela est dû au fait qu'elles ont été marquées spécifiquement pour que le déploiement aboutisse.  
>   
> Pour éviter cette condition, excluez un contributeur de déploiement du fichier .sqlproj. Spécifiez à place les contributeurs à exécuter pendant le déploiement à l'aide de SqlPackage avec le paramètre **AdditionalDeploymentContributors**. Cela s'avère utile lorsque vous souhaitez utiliser un contributeur uniquement dans des circonstances particulières, telles que le déploiement sur un serveur spécifique.  
  
## <a name="next-steps"></a>Étapes suivantes  
Vous pouvez essayer d'apporter d'autres types de modifications aux plans de déploiement avant leur exécution. Voici d'autres types de modifications que vous pouvez effectuer :  
  
-   Ajout d'une propriété étendue à tous les objets de base de données qui associent un numéro de version.  
  
-   Ajout ou suppression d'instructions PRINT de diagnostic ou de commentaires supplémentaires dans les scripts de déploiement.  
  
## <a name="see-also"></a>Voir aussi  
[Personnaliser la génération et le déploiement de bases de données à l'aide de contributeurs de génération et de déploiement](../ssdt/use-deployment-contributors-to-customize-database-build-and-deployment.md)  
[Procédure pas à pas : étendre la génération du projet de base de données à la génération de statistiques de modèle](../ssdt/walkthrough-extend-database-project-build-to-generate-model-statistics.md)  
[Procédure pas à pas : étendre le déploiement du projet de base de données pour analyser le plan de déploiement](../ssdt/walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan.md)  
