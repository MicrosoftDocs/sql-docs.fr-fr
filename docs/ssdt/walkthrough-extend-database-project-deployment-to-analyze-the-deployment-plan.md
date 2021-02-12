---
title: Étendre le déploiement du projet de base de données pour analyser le plan de déploiement
description: Créer un contributeur de déploiement DeploymentPlanExecutor. Configurez un collaborateur qui conserve un enregistrement des événements qui se produisent lorsque vous déployez un projet de base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 9ead8470-93ba-44e3-8848-b59322e37621
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d389d9f71b08914385cdf3bedef5bf613e6700d1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016669"
---
# <a name="walkthrough-extend-database-project-deployment-to-analyze-the-deployment-plan"></a>Procédure pas à pas : Étendre le déploiement du projet de base de données pour analyser le plan de déploiement

Vous pouvez créer des contributeurs de déploiement pour effectuer des actions personnalisées lorsque vous déployez un projet SQL. Vous pouvez créer soit un DeploymentPlanModifier soit un DeploymentPlanExecutor. Utilisez un DeploymentPlanModifier pour changer le plan avant son exécution et un DeploymentPlanExecutor pour effectuer des opérations alors que le plan est en cours d'exécution. Dans cette procédure pas à pas, vous allez créer un DeploymentPlanExecutor nommé DeploymentUpdateReportContributor qui crée un rapport sur les actions qui sont effectuées lorsque vous déployez un projet de base de données. Ce contributeur de génération acceptant un paramètre pour contrôler si le rapport est généré, vous devez effectuer une étape supplémentaire requise.  
  
Au cours de cette procédure pas à pas, vous allez effectuer les tâches principales suivantes :  
  
-   [Créer le type de contributeur de déploiement DeploymentPlanExecutor](#CreateDeploymentContributor)  
  
-   [Installer le contributeur de déploiement](#InstallDeploymentContributor)  
  
-   [Tester votre contributeur de déploiement](#TestDeploymentContributor)  
  
## <a name="prerequisites"></a>Prérequis  
Vous devez disposer des éléments suivants pour exécuter cette procédure pas à pas :  
  
-   Vous devez avoir installé une version de Visual Studio qui inclut SQL Server Data Tools (SSDT) et qui prend en charge le développement en C# ou VB.  
  
-   Vous devez disposer d'un projet SQL qui contient des objets SQL.  
  
-   Instance de SQL Server dans laquelle vous pouvez déployer un projet de base de données.  
  
> [!NOTE]  
> Cette procédure pas à pas est destinée aux utilisateurs qui sont déjà familiarisés avec les fonctionnalités SQL de SSDT. Vous devez également être familiarisé avec les concepts de base de Visual Studio, comme la création d'une bibliothèque de classes et l'utilisation de l'éditeur de code pour ajouter du code à une classe.  
  
## <a name="create-a-deployment-contributor"></a><a name="CreateDeploymentContributor"></a>Créer un contributeur de déploiement  
Pour créer un contributeur de déploiement, vous devez effectuer les tâches suivantes :  
  
-   Créez un projet Bibliothèque de classes et ajoutez les références requises.  
  
-   Définissez une classe nommée DeploymentUpdateReportContributor qui hérite de [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor).  
  
-   Remplacer la méthode OnExecute.  
  
-   Ajouter une classe d'assistance privée.  
  
-   Générer l'assembly résultant.  
  
#### <a name="to-create-a-class-library-project"></a>Pour créer un projet de bibliothèque de classes  
  
1.  Créez un projet Bibliothèque de classes Visual Basic ou Visual C# nommé MyDeploymentContributor.  
  
2.  Renommez le fichier « Class1.cs » en « DeploymentUpdateReportContributor.cs ».  
  
3.  Dans l’Explorateur de solutions, cliquez avec le bouton droit sur le nœud du projet, puis cliquez sur **Ajouter une référence**.  
  
4.  Sous l’onglet Infrastructure, sélectionnez **System.ComponentModel.Composition**.  
  
5.  Ajoutez les références SQL requises : cliquez avec le bouton droit sur le nœud de projet puis cliquez sur **Ajouter une référence**. Cliquez sur **Parcourir** et accédez au dossier **C:\Program files (x86)\Microsoft SQL Server\110\DAC\bin**. Sélectionnez les entrées **Microsoft.SqlServer.Dac.dll**, **Microsoft.SqlServer.Dac.Extensions.dll** et **Microsoft.Data.Tools.Schema.Sql.dll**, cliquez sur **Ajouter**, puis sur **OK**.  
  
    Commencez ensuite à ajouter le code à la classe.  
  
#### <a name="to-define-the-deploymentupdatereportcontributor-class"></a>Pour définir la classe DeploymentUpdateReportContributor  
  
1.  Dans l'éditeur de code, mettez à jour le fichier DeploymentUpdateReportContributor.cs pour le faire correspondre aux instructions **using** suivantes :  
  
    ```csharp  
    using System;  
    using System.IO;  
    using System.Text;  
    using System.Xml;  
    using Microsoft.SqlServer.Dac.Deployment;  
    using Microsoft.SqlServer.Dac.Extensibility;  
    using Microsoft.SqlServer.Dac.Model;  
  
    ```  
  
2.  Mettez à jour la définition de classe pour qu'elle corresponde à ce qui suit :  
  
    ```csharp  
    /// <summary>  
        /// An executor that generates a report detailing the steps in the deployment plan. Will only run  
        /// if a "GenerateUpdateReport=true" contributor argument is set in the project file, in a targets file or  
        /// passed as an additional argument to the DacServices API. To set in a project file, add the following:  
        ///   
        /// <PropertyGroup>  
        ///     <ContributorArguments Condition="'$(Configuration)' == 'Debug'">  
        /// $(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;  
        ///     </ContributorArguments>  
        /// <PropertyGroup>  
        ///   
        /// </summary>  
        [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
        public class DeploymentUpdateReportContributor : DeploymentPlanExecutor  
        {  
        }  
  
    ```  
  
    Vous avez défini un contributeur de déploiement qui hérite de DeploymentPlanExecutor. Lors de la génération et du déploiement, les contributeurs personnalisés sont chargés à partir d'un répertoire d'extension standard. Les contributeurs de l'exécuteur de plan de déploiement sont identifiés par un attribut [ExportDeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.exportdeploymentplanexecutorattribute).  
  
    Cet attribut est requis afin que les contributeurs puissent être découverts. Celui-ci doit se présenter comme suit :  
  
    ```csharp  
    [ExportDeploymentPlanExecutor("MyDeploymentContributor.DeploymentUpdateReportContributor", "1.0.0.0")]  
  
    ```  
  
    Dans ce cas le premier paramètre de l’attribut doit être un identificateur unique qui sera utilisé pour identifier un contributeur dans des fichiers de projet. Il est recommandé d’associer l’espace de noms de la bibliothèque (ici, MyDeploymentContributor) au nom de la classe (ici, DeploymentUpdateReportContributor) pour générer l’identificateur.  
  
3.  Ensuite, ajoutez le membre suivant que vous utiliserez pour permettre à ce fournisseur d'accepter un paramètre de ligne de commande :  
  
    ```csharp  
    public const string GenerateUpdateReport = "DeploymentUpdateReportContributor.GenerateUpdateReport";  
    ```  
  
    Ce membre permet à l'utilisateur de spécifier si le rapport doit être généré à l'aide de l'option de GenerateUpdateReport.  
  
    Ensuite, vous remplacez la méthode OnExecute pour ajouter le code à exécuter lors du déploiement d'un projet de base de données.  
  
#### <a name="to-override-onexecute"></a>Pour remplacer OnExecute  
  
-   Ajoutez la méthode suivante à votre classe DeploymentUpdateReportContributor :  
  
    ```csharp  
    /// <summary>  
            /// Override the OnExecute method to perform actions when you execute the deployment plan for  
            /// a database project.  
            /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                ExtensibilityError reportMsg = new ExtensibilityError(msg, Severity.Message);  
                base.PublishMessage(reportMsg);  
            }  
        /// <summary>  
        /// Override the OnExecute method to perform actions when you execute the deployment plan for  
        /// a database project.  
        /// </summary>  
            protected override void OnExecute(DeploymentPlanContributorContext context)  
            {  
                // determine whether the user specified a report is to be generated  
                bool generateReport = false;  
                string generateReportValue;  
                if (context.Arguments.TryGetValue(GenerateUpdateReport, out generateReportValue) == false)  
                {  
                    // couldn't find the GenerateUpdateReport argument, so do not generate  
                    generateReport = false;  
                }  
                else  
                {  
                    // GenerateUpdateReport argument was specified, try to parse the value  
                    if (bool.TryParse(generateReportValue, out generateReport))  
                    {  
                        // if we end up here, the value for the argument was not valid.  
                        // default is false, so do nothing.  
                    }  
                }  
  
                if (generateReport == false)  
                {  
                    // if user does not want to generate a report, we are done  
                    return;  
                }  
  
                // We will output to the same directory where the deployment script  
                // is output or to the current directory  
                string reportPrefix = context.Options.TargetDatabaseName;  
                string reportPath;  
                if (string.IsNullOrEmpty(context.DeploymentScriptPath))  
                {  
                    reportPath = Environment.CurrentDirectory;  
                }  
                else  
                {  
                    reportPath = Path.GetDirectoryName(context.DeploymentScriptPath);  
                }  
                FileInfo summaryReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".summary.xml"));  
                FileInfo detailsReportFile = new FileInfo(Path.Combine(reportPath, reportPrefix + ".details.xml"));  
  
                // Generate the reports by using the helper class DeploymentReportWriter  
                DeploymentReportWriter writer = new DeploymentReportWriter(context);  
                writer.WriteReport(summaryReportFile);  
                writer.IncludeScripts = true;  
                writer.WriteReport(detailsReportFile);  
  
                string msg = "Deployment reports ->"  
                    + Environment.NewLine + summaryReportFile.FullName  
                    + Environment.NewLine + detailsReportFile.FullName;  
  
                DataSchemaError reportMsg = new DataSchemaError(msg, ErrorSeverity.Message);  
                base.PublishMessage(reportMsg);  
            }  
    ```  
  
    La méthode OnExecute se voit transmettre un objet [DeploymentPlanContributorContext](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplancontributorcontext) qui permet d'accéder à tous les arguments spécifiés, au modèle de base de données source et cible, aux propriétés de génération et aux fichiers d'extension. Dans cet exemple, nous obtenons le modèle, puis appelons les fonctions d'assistance pour obtenir des informations sur le modèle. Nous utilisons la méthode d'assistance PublishMessage dans la classe de base pour signaler des erreurs qui se produisent.  
  
    Les types et méthodes supplémentaires dignes d’intérêt sont les suivants : [TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel), [ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult), [DeploymentPlanHandle](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanhandle) et [SqlDeploymentOptions](/dotnet/api/microsoft.sqlserver.dac.deployment.sqldeploymentoptions).  
  
    Vous définissez ensuite la classe d'assistance qui entre dans les détails du plan de déploiement.  
  
#### <a name="to-add-the-helper-class-that-generates-the-report-body"></a>Pour ajouter la classe d'assistance qui génère le corps du rapport  
  
-   Ajoutez la classe d'assistance et ses méthodes en ajoutant le code suivant :  
  
    ```csharp  
    /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                readonly TSqlModel _sourceModel;  
                readonly ModelComparisonResult _diff;  
                readonly DeploymentStep _planHead;  
  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
                    if (context == null)  
                    {  
                        throw new ArgumentNullException("context");  
                    }  
  
                    // save the source model, source/target differences,  
                    // and the beginning of the deployment plan.  
                    _sourceModel = context.Source;  
                    _diff = context.ComparisonResult;  
                    _planHead = context.PlanHandle.Head;  
                }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {// Assumes that we have a valid report file  
                    if (reportFile == null)  
                    {  
                        throw new ArgumentNullException("reportFile");  
                    }  
  
                    // set up the XML writer  
                    XmlWriterSettings xmlws = new XmlWriterSettings();  
                    // Indentation makes it a bit more readable  
                    xmlws.Indent = true;  
                    FileStream fs = new FileStream(reportFile.FullName, FileMode.Create, FileAccess.Write, FileShare.ReadWrite);  
                    XmlWriter xmlw = XmlWriter.Create(fs, xmlws);  
  
                    try  
                    {  
                        xmlw.WriteStartDocument(true);  
                        xmlw.WriteStartElement("DeploymentReport");  
  
                        // Summary report of the operations that  
                        // are contained in the plan.  
                        ReportPlanOperations(xmlw);  
  
                        // You could add a method call here  
                        // to produce a detailed listing of the   
                        // differences between the source and  
                        // target model.  
                        xmlw.WriteEndElement();  
                        xmlw.WriteEndDocument();  
                        xmlw.Flush();  
                        fs.Flush();  
                    }  
                    finally  
                    {  
                        xmlw.Close();  
                        fs.Dispose();  
                    }  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {// write the node to indicate the start  
                    // of the list of operations.  
                    xmlw.WriteStartElement("Operations");  
  
                    // Loop through the steps in the plan,  
                    // starting at the beginning.  
                    DeploymentStep currentStep = _planHead;  
                    while (currentStep != null)  
                    {  
                        // Report the type of step  
                        xmlw.WriteStartElement(currentStep.GetType().Name);  
  
                        // based on the type of step, report  
                        // the relevant information.  
                        // Note that this procedure only handles   
                        // a subset of all step types.  
                        if (currentStep is SqlRenameStep)  
                        {  
                            SqlRenameStep renameStep = (SqlRenameStep)currentStep;  
                            xmlw.WriteAttributeString("OriginalName", renameStep.OldName);  
                            xmlw.WriteAttributeString("NewName", renameStep.NewName);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(renameStep.RenamedElement));  
                        }  
                        else if (currentStep is SqlMoveSchemaStep)  
                        {  
                            SqlMoveSchemaStep moveStep = (SqlMoveSchemaStep)currentStep;  
                            xmlw.WriteAttributeString("OrignalName", moveStep.PreviousName);  
                            xmlw.WriteAttributeString("NewSchema", moveStep.NewSchema);  
                            xmlw.WriteAttributeString("Category", GetElementCategory(moveStep.MovedElement));  
                        }  
                        else if (currentStep is SqlTableMigrationStep)  
                        {  
                            SqlTableMigrationStep dmStep = (SqlTableMigrationStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dmStep.SourceTable));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dmStep.SourceElement));  
                        }  
                        else if (currentStep is CreateElementStep)  
                        {  
                            CreateElementStep createStep = (CreateElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(createStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(createStep.SourceElement));  
                        }  
                        else if (currentStep is AlterElementStep)  
                        {  
                            AlterElementStep alterStep = (AlterElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(alterStep.SourceElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(alterStep.SourceElement));  
                        }  
                        else if (currentStep is DropElementStep)  
                        {  
                            DropElementStep dropStep = (DropElementStep)currentStep;  
                            xmlw.WriteAttributeString("Name", GetElementName(dropStep.TargetElement));  
                            xmlw.WriteAttributeString("Category", GetElementCategory(dropStep.TargetElement));  
                        }  
  
                        // If the script bodies are to be included,  
                        // add them to the report.  
                        if (this.IncludeScripts)  
                        {  
                            using (StringWriter sw = new StringWriter())  
                            {  
                                currentStep.GenerateBatchScript(sw);  
                                string tsqlBody = sw.ToString();  
                                if (string.IsNullOrEmpty(tsqlBody) == false)  
                                {  
                                    xmlw.WriteCData(tsqlBody);  
                                }  
                            }  
                        }  
  
                        // close off the current step  
                        xmlw.WriteEndElement();  
                        currentStep = currentStep.Next;  
                    }  
                    xmlw.WriteEndElement();  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(TSqlObject element)  
                {  
                    return element.ObjectType.Name;  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
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
            }        /// <summary>  
            /// This class is used to generate a deployment  
            /// report.   
            /// </summary>  
            private class DeploymentReportWriter  
            {  
                /// <summary>  
                /// The constructor accepts the same context info  
                /// that was passed to the OnExecute method of the  
                /// deployment contributor.  
                /// </summary>  
                public DeploymentReportWriter(DeploymentPlanContributorContext context)  
                {  
               }  
                /// <summary>  
                /// Property indicating whether script bodies  
                /// should be included in the report.  
                /// </summary>  
                public bool IncludeScripts { get; set; }  
  
                /// <summary>  
                /// Drives the report generation, opening files,   
                /// writing the beginning and ending report elements,  
                /// and calling helper methods to report on the  
                /// plan operations.  
                /// </summary>  
                internal void WriteReport(FileInfo reportFile)  
                {  
                }  
  
                /// <summary>  
                /// Writes details for the various operation types  
                /// that could be contained in the deployment plan.  
                /// Optionally writes script bodies, depending on  
                /// the value of the IncludeScripts property.  
                /// </summary>  
                private void ReportPlanOperations(XmlWriter xmlw)  
                {  
                }  
  
                /// <summary>  
                /// Returns the category of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementCategory(IModelElement element)  
                {  
                }  
  
                /// <summary>  
                /// Returns the name of the specified element  
                /// in the source model  
                /// </summary>  
                private string GetElementName(IModelElement element)  
                {  
                }  
            }  
    ```  
  
-   Enregistrez les modifications dans le fichier de classe. Plusieurs types utiles sont référencés dans la classe d'assistance :  
  
    |**Zone de code**|**Types utiles**|  
    |-----------------|--------------------|  
    |Membres de classe|[TSqlModel](/dotnet/api/microsoft.sqlserver.dac.model.tsqlmodel), [ModelComparisonResult](/dotnet/api/microsoft.sqlserver.dac.deployment.modelcomparisonresult), [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep)|  
    |Méthode WriteReport|XmlWriter et XmlWriterSettings|  
    |Méthode ReportPlanOperations|Les types dignes d'intérêt sont les suivants : [DeploymentStep](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentstep), [SqlRenameStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlrenamestep), [SqlMoveSchemaStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqlmoveschemastep), [SqlTableMigrationStep](/dotnet/api/microsoft.sqlserver.dac.deployment.sqltablemigrationstep), [CreateElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.createelementstep), [AlterElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.alterelementstep), [DropElementStep](/dotnet/api/microsoft.sqlserver.dac.deployment.dropelementstep).<br /><br />Il existe de nombreuses autres étapes. Consultez la documentation API pour en obtenir la liste complète.|  
    |GetElementCategory|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
    |GetElementName|[TSqlObject](/dotnet/api/microsoft.sqlserver.dac.model.tsqlobject)|  
  
    Générez ensuite la bibliothèque de classes.  
  
#### <a name="to-sign-and-build-the-assembly"></a>Pour signer et générer l'assembly  
  
1.  Dans le menu **Projet**, cliquez sur **Propriétés de MyDeploymentContributor**.  
  
2.  Cliquez sur l'onglet **Signature** .  
  
3.  Cliquez sur **Signer l'assembly**.  
  
4.  Dans **Choisir un fichier de clé de nom fort**, cliquez sur **<New>** .  
  
5.  Dans la boîte de dialogue **Créer une clé de nom fort** , dans **Nom du fichier de clé**, tapez **MyRefKey**.  
  
6.  (facultatif) Vous pouvez spécifier un mot de passe pour votre fichier de clé de nom fort.  
  
7.  Cliquez sur **OK**.  
  
8.  Dans le menu **Fichier** , cliquez sur **Enregistrer tout**.  
  
9. Dans le menu **Générer**, cliquez sur **Générer la solution**.  
  
Ensuite, vous devez installer l'assembly afin qu'il soit chargé lorsque vous générez et déployez des projets SQL.  
  
## <a name="install-a-deployment-contributor"></a><a name="InstallDeploymentContributor"></a>Installer un contributeur de déploiement  
Pour installer un contributeur de déploiement, vous devez copier l'assembly et le fichier .pdb associé dans le dossier Extensions.  
  
#### <a name="to-install-the-mydeploymentcontributor-assembly"></a>Pour installer l'assembly MyDeploymentContributor  
  
-   Puis, vous allez copier les informations d'assembly dans le répertoire Extensions. Au démarrage de Visual Studio, les extensions sont identifiées dans le répertoire et les sous-répertoires %Program Files%\\Microsoft SQL Server\110\DAC\Bin\Extensions\Microsoft\SQLDB\TestConditions et mises à disposition :  
  
-   Copiez le fichier d'assembly de **MyDeploymentContributor.dll** du répertoire de sortie dans le répertoire %Program Files%\Microsoft SQL Server\110\DAC\Bin\Extensions. Par défaut, le chemin d'accès du fichier .dll compilé est le suivant : Chemin de votre solution\Chemin de votre projet\bin\Debug ou Chemin de votre solution\Chemin de votre projet\bin\Release.  
  
## <a name="test-your-deployment-contributor"></a><a name="TestDeploymentContributor"></a>Tester votre contributeur de déploiement  
Pour tester un contributeur de déploiement, vous devez effectuer les tâches suivantes :  
  
-   Ajouter des propriétés au fichier .sqlproj que vous envisagez de déployer.  
  
-   Déployer le projet à l'aide de MSBuild et en fournissant le paramètre approprié.  
  
### <a name="add-properties-to-the-sql-project-sqlproj-file"></a>Ajouter des propriétés au fichier de projet SQL (.sqlproj)  
Vous devez toujours mettre à jour le fichier de projet SQL pour spécifier l'ID des contributeurs à exécuter. En outre, ce contributeur attendant un argument « GenerateUpdateReport », celui-ci doit être spécifié comme argument de contributeur.  
  
Vous pouvez le faire de deux façons. Vous pouvez modifier manuellement le fichier .sqlproj pour ajouter les arguments requis. Vous pouvez décider de procéder de la sorte si un contributeur n'a pas d'arguments de contributeur requis pour la configuration ou si vous n'envisagez pas de réutiliser ce contributeur sur un grand nombre de projets. Si vous choisissez cette option, ajoutez les instructions suivantes dans le fichier .sqlproj après le premier nœud d'importation dans le fichier :  
  
```  
<PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors); MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
<ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments);DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
  </PropertyGroup>  
```  
  
La deuxième méthode consiste à créer un fichier de cibles qui contient les arguments de contributeur requis. Cela est utile si vous utilisez le même contributeur pour plusieurs projets et que vous possédez les arguments de contributeur requis, car il comprend les valeurs par défaut. Dans ce cas, créez un fichier de cibles dans le chemin d'accès des extensions Msbuild.  
  
1.  Accédez à %Program Files%\Msbuild.  
  
2.  Créez un dossier « MyContributors » où vos fichiers de cibles seront stockés.  
  
3.  Créez un fichier « MyContributors.targets » dans ce répertoire, ajoutez le texte suivant, puis enregistrez le fichier :  
  
    ```  
    <?xml version="1.0" encoding="utf-8"?>  
  
    <Project xmlns="https://schemas.microsoft.com/developer/msbuild/2003">  
      <PropertyGroup>  
    <DeploymentContributors>$(DeploymentContributors);MyDeploymentContributor.DeploymentUpdateReportContributor</DeploymentContributors>  
    <ContributorArguments Condition="'$(Configuration)' == 'Debug'">$(ContributorArguments); DeploymentUpdateReportContributor.GenerateUpdateReport=true;</ContributorArguments>  
      </PropertyGroup>  
    </Project>  
    ```  
  
4.  Dans le fichier .sqlproj d’un projet pour lequel vous souhaitez exécuter des contributeurs, importez le fichier de cibles en ajoutant l’instruction suivante au fichier .sqlproj après le nœud \<Import Project="$(MSBuildExtensionsPath)\Microsoft\VisualStudio\v$(VisualStudioVersion)\SSDT\Microsoft.Data.Tools.Schema.SqlTasks.targets" \/> dans le fichier :  
  
    ```  
    <Import Project="$(MSBuildExtensionsPath)\MyContributors\MyContributors.targets " />  
    ```  
  
Après avoir suivi une de ces approches, vous pouvez utiliser Msbuild pour transmettre les paramètres des générations par ligne de commande.  
  
> [!NOTE]  
> Vous devez toujours mettre à jour la propriété « DeploymentContributors » pour spécifier votre ID de contributeur Il s’agit du même ID que celui utilisé dans l’attribut « ExportDeploymentPlanExecutor », dans le fichier source du contributeur. Sans cet ID le contributeur ne s'exécute pas lors de la génération du projet. La propriété « ContributorArguments » doit être mise à jour uniquement si des arguments sont nécessaires à l’exécution de votre contributeur.  
  
### <a name="deploy-the-database-project"></a>Déployer le projet de base de données  
Votre projet peut être publié ou déployé normalement dans Visual Studio. Ouvrez une solution qui contient votre projet SQL et sélectionnez l’option « Publier... » dans le menu contextuel du projet, ou appuyez sur la touche F5 pour un déploiement de débogage dans LocalDB. Dans cet exemple nous utiliserons la boîte de dialogue « Publier… » pour générer un script de déploiement.  
  
##### <a name="to-deploy-your-sql-project-and-generate-a-deployment-report"></a>Pour déployer votre projet SQL et générer un rapport de déploiement  
  
1.  Ouvrez Visual Studio et ouvrez la solution qui contient votre projet SQL.  
  
2.  Sélectionnez votre projet et appuyez sur la touche « F5 » pour effectuer un déploiement de débogage. Remarque : Puisque l’élément ContributorArguments est configuré pour être inclus uniquement si la configuration est de type « Débogage », le rapport de déploiement est généré uniquement pour les déploiements de débogage. Pour changer cela, supprimez l’instruction Condition="'$(Configuration)' == 'Debug'" de la définition ContributorArguments.  
  
3.  Une sortie semblable à celle-ci doit être présente dans la fenêtre de sortie :  
  
    ```  
    ------ Deploy started: Project: Database1, Configuration: Debug Any CPU ------  
    Finished verifying cached model in 00:00:00  
    Deployment reports ->  
  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.summary.xml  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyTargetDatabase.details.xml  
  
      Deployment script generated to:  
      C:\Users\UserName\Documents\Visual Studio 2012\Projects\MyDatabaseProject\MyDatabaseProject\sql\debug\MyDatabaseProject.sql  
  
    ```  
  
4.  Ouvrez MyTargetDatabase.summary.xml et examinez son contenu. Le fichier s'apparente à l'exemple suivant qui illustre un nouveau déploiement de base de données :  
  
    ```  
    <?xml version="1.0" encoding="utf-8" standalone="yes"?>  
    <DeploymentReport>  
      <Operations>  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <BeginPreDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPreDeploymentScriptStep />  
        <SqlBeginPreservationStep />  
        <SqlEndPreservationStep />  
        <SqlBeginDropsStep />  
        <SqlEndDropsStep />  
        <SqlBeginAltersStep />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales" Category="Schema" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Customer" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Customer_CustID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Orders" Category="Table" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.PK_Orders_OrderID" Category="Primary Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDOrders" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Customer_YTDSales" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_OrderDate" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.Def_Orders_Status" Category="Default Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.FK_Orders_Customer_CustID" Category="Foreign Key" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_FilledDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.CK_Orders_OrderDate" Category="Check Constraint" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspCancelOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspFillOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspNewCustomer" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspPlaceNewOrder" Category="Procedure" />  
        <SqlPrintStep />  
        <CreateElementStep Name="Sales.uspShowOrderDetails" Category="Procedure" />  
        <SqlEndAltersStep />  
        <DeploymentScriptStep />  
        <BeginPostDeploymentScriptStep />  
        <DeploymentScriptStep />  
        <EndPostDeploymentScriptStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
        <DeploymentScriptDomStep />  
      </Operations>  
    </DeploymentReport>  
  
    ```  
  
    > [!NOTE]  
    > Si vous déployez un projet de base de données identique à la base de données cible, le rapport résultant ne sera pas très explicite. Pour obtenir des résultats significatifs, déployez les modifications sur une base de données ou déployez une nouvelle base de données.  
  
5.  Ouvrez MyTargetDatabase.details.xml et examinez son contenu. Une petite section du fichier de détails affiche les entrées et les scripts qui créent le schéma Sales, envoient un message sur la création d'une table et créent la table :  
  
    ```  
    <CreateElementStep Name="Sales" Category="Schema"><![CDATA[CREATE SCHEMA [Sales]  
        AUTHORIZATION [dbo];  
  
    ]]></CreateElementStep>  
        <SqlPrintStep><![CDATA[PRINT N'Creating [Sales].[Customer]...';  
  
    ]]></SqlPrintStep>  
        <CreateElementStep Name="Sales.Customer" Category="Table"><![CDATA[CREATE TABLE [Sales].[Customer] (  
        [CustomerID]   INT           IDENTITY (1, 1) NOT NULL,  
        [CustomerName] NVARCHAR (40) NOT NULL,  
        [YTDOrders]    INT           NOT NULL,  
        [YTDSales]     INT           NOT NULL  
    );  
  
    ]]></CreateElementStep>  
  
    ```  
  
    En analysant le plan de déploiement lorsqu'il est exécuté, vous pouvez rendre compte de toutes les informations contenues dans le déploiement et intervenir sur les étapes de ce plan.  
  
## <a name="next-steps"></a>Étapes suivantes  
Vous pouvez créer des outils supplémentaires pour effectuer le traitement des fichiers XML en sortie. Il s'agit d'un exemple de [DeploymentPlanExecutor](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanexecutor). Vous pouvez également créer un [DeploymentPlanModifier](/dotnet/api/microsoft.sqlserver.dac.deployment.deploymentplanmodifier) pour modifier un plan de déploiement avant son exécution.  
  
## <a name="see-also"></a>Voir aussi  
[Procédure pas à pas : étendre la génération du projet de base de données à la génération de statistiques de modèle](/previous-versions/visualstudio/visual-studio-2010/ee461508(v=vs.100))  
[Procédure pas à pas : étendre le déploiement du projet de base de données pour modifier le plan de déploiement](/previous-versions/visualstudio/visual-studio-2010/ee461507(v=vs.100))  
[Personnaliser la génération et le déploiement de bases de données à l'aide de contributeurs de génération et de déploiement](/previous-versions/visualstudio/visual-studio-2010/ee461505(v=vs.100))  
