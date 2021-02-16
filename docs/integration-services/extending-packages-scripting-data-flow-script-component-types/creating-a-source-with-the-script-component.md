---
description: Création d'une source à l'aide du composant Script
title: Création d’une source à l’aide du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- Script component [Integration Services], source components
- output columns [Integration Services]
- sources [Integration Services], components
ms.assetid: 547c4179-ea82-4265-8c6f-04a2aa77a3c0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1aa4ed0d717e8b2d6dfe7f2b6b7d331936e00100
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344874"
---
# <a name="creating-a-source-with-the-script-component"></a>Création d'une source à l'aide du composant Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Les composants sources dans le flux d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] permettent de charger des données à partir d'une source de données et de les transférer à des transformations et des destinations en aval. En principe, vous vous connectez à la source de données via un gestionnaire de connexions existant.  
  
 Pour une vue d’ensemble du composant Script, consultez [Extension du flux de données avec le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Le composant Script et le code d'infrastructure qu'il génère simplifient considérablement le processus qui consiste à développer un composant de flux de données personnalisé. Toutefois, pour comprendre le fonctionnement du composant Script, il peut être utile de consulter les étapes de développement d'un composant de flux de données personnalisé. Consultez la section [Développement d’un composant de flux de données personnalisé](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) et plus particulièrement la rubrique [Développement d’un composant source personnalisé](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="getting-started-with-a-source-component"></a>Mise en route d'un composant source  
 Lorsque vous ajoutez un composant Script au volet Flux de données du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)], la boîte de dialogue **Sélectionner le type de composant de script** s’ouvre et vous invite à sélectionner un script de type Source, Destination ou Transformation. Dans cette boîte de dialogue, sélectionnez **Source**.  
  
## <a name="configuring-a-source-component-in-metadata-design-mode"></a>Configuration d'un composant source en mode Création de métadonnées  
 Après avoir sélectionné l'option pour créer un composant source, vous pouvez configurer le composant à l'aide de l' **Éditeur de transformation de script**. Pour plus d’informations, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Un composant source de flux de données ne possède pas d'entrée et prend en charge une ou plusieurs sorties. La configuration des sorties du composant est l'une des étapes à exécuter en mode Création de métadonnées, à l'aide de l' **Éditeur de transformation de script**, avant d'écrire le script personnalisé.  
  
 Vous pouvez également spécifier le langage de script en définissant la propriété **ScriptLanguage** dans la page **Script** de l' **Éditeur de transformation de script**.  
  
> [!NOTE]  
>  Pour définir le langage de script par défaut des composants Script et des tâches de script, utilisez l'option **Langage de script** dans la page **Général** de la boîte de dialogue **Options** . Pour plus d'informations, consultez [General Page](~/integration-services/control-flow/script-task-editor-general-page.md).  
  
### <a name="adding-connection-managers"></a>Ajout de gestionnaires de connexions  
 En principe, une composant source utilise un gestionnaire de connexions existant pour se connecter à la source de données à partir de laquelle il charge des données dans le flux de données. Dans la page **Gestionnaires de connexions** de l' **Éditeur de transformation de script**, cliquez sur **Ajouter** pour ajouter le gestionnaire de connexions approprié.  
  
 Toutefois, un gestionnaire de connexions n'est qu'une unité pratique qui permet d'encapsuler et de stocker les informations dont il doit disposer pour se connecter à une source de données d'un type particulier. Vous devez écrire votre propre code personnalisé pour charger ou enregistrer vos données et éventuellement ouvrir et fermer la connexion à la source de données.  
  
 Pour obtenir des informations générales sur l’utilisation des gestionnaires de connexions avec le composant Script, consultez [Connexion aux sources de données dans le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/connecting-to-data-sources-in-the-script-component.md).  
  
 Pour plus d’informations sur la page **Gestionnaires de connexions** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Gestionnaires de connexions&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-outputs-and-output-columns"></a>Configuration des sorties et des colonnes de sortie  
 Un composant source ne possède pas d'entrée et prend en charge une ou plusieurs sorties. La page **Entrées et sorties** de l' **Éditeur de transformation de script**, montre qu'une seule sortie a été créée par défaut, mais qu'aucune colonne de sortie n'a été créée. Dans cette page de l'éditeur, vous pouvez avoir besoin ou envie de configurer les éléments suivants.  
  
-   Vous devez ajouter et configurer manuellement des colonnes de sortie pour chaque sortie. Sélectionnez le dossier Colonnes de sortie pour chaque sortie, puis utilisez les boutons **Ajouter une colonne** et **Supprimer une colonne** pour gérer les colonnes de sortie de chaque sortie du composant source. Vous ferez ultérieurement référence aux colonnes de sortie dans le script par le nom que vous leur assignez dans cette page, à l'aide des propriétés d'accesseur typées, créées dans le code généré automatiquement.  
  
-   Vous pouvez créer une ou plusieurs sorties supplémentaires, telles qu'une sortie d'erreur simulée pour les lignes qui contiennent des valeurs inattendues. Utilisez les boutons **Ajouter une sortie** et **Supprimer une sortie** pour gérer les sorties du composant source. Toutes les lignes d'entrée sont dirigées vers toutes les sorties disponibles, sauf si vous attribuez une valeur identique non nulle à la propriété **ExclusionGroup** de ces sorties, auquel cas vous pourrez diriger chaque ligne vers une seule de ces sorties partageant la même valeur **ExclusionGroup** . La valeur entière particulière sélectionnée pour identifier **ExclusionGroup** n'a pas d'importance.  
  
    > [!NOTE]  
    >  Vous pouvez également utiliser une valeur de propriété **ExclusionGroup** non nulle avec une sortie unique lorsque vous ne souhaitez pas générer des sorties pour toutes les lignes. Dans ce cas, toutefois, vous devez appeler explicitement la méthode **DirectRowTo\<outputbuffer>** pour chaque ligne que vous souhaitez envoyer à la sortie.  
  
-   Vous pouvez assigner un nom convivial aux sorties. Vous ferez ultérieurement référence aux sorties par leur nom dans le script, à l'aide des propriétés d'accesseur typées, créées dans le code généré automatiquement.  
  
-   Normalement, plusieurs sorties dans le même **ExclusionGroup** possèdent les mêmes colonnes de sortie. Toutefois, si vous créez une sortie d'erreur simulée, vous pouvez ajouter des colonnes supplémentaires pour stocker les informations d'erreur. Pour plus d’informations sur la manière dont le moteur de flux de données traite les lignes d’erreur, consultez [Utilisation de sorties d’erreur dans un composant de flux de données](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). Dans le composant Script, vous devez cependant écrire votre propre code pour remplir les colonnes supplémentaires avec les informations d'erreur appropriées. Pour plus d’informations, consultez [Simulation d’une sortie d’erreur pour le composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Pour plus d’informations sur la page **Entrées et sorties** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Entrées et sorties&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Ajout de variables  
 S'il existe des variables dont vous souhaitez utiliser les valeurs dans votre script, vous pouvez les ajouter dans les champs de propriété **ReadOnlyVariables** et **ReadWriteVariables** dans la page **Script** de l' **Éditeur de transformation de script**.  
  
 Lorsque vous entrez plusieurs variables dans les champs de propriété, séparez les noms de variables par des virgules. Vous pouvez également entrer plusieurs variables en cliquant sur le bouton de sélection ( **...** ) en regard des champs de propriété **ReadOnlyVariables** et **ReadWriteVariables**, puis en sélectionnant les variables dans la boîte de dialogue **Sélectionner des variables**.  
  
 Pour obtenir des informations générales sur l’utilisation de variables avec le composant Script, consultez [Utilisation de variables dans le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Pour plus d’informations sur la page **Script** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Script&#41;](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-source-component-in-code-design-mode"></a>Script d'un composant source en mode Création de code  
 Après avoir configuré les métadonnées du composant, ouvrez l’environnement de développement intégré [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) pour coder votre script personnalisé. Pour ouvrir VSTA, cliquez sur **Modifier le script** dans la page **Script** de l' **Éditeur de transformation de script**. Vous pouvez écrire votre script à l’aide de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C#, selon le langage de script sélectionné pour la propriété **ScriptLanguage**.  
  
 Pour obtenir des informations importantes concernant tous les types de composants créés à l’aide du composant Script, consultez [Codage et débogage du composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Fonctionnement du code généré automatiquement  
 Lorsque vous ouvrez l'environnement de développement intégré VSTA après avoir créé et configuré un composant source, la classe **ScriptMain** modifiable apparaît dans l'éditeur de code. Vous pouvez écrire votre code personnalisé dans la classe **ScriptMain** .  
  
 La classe **ScriptMain** inclut un stub pour la méthode **CreateNewOutputRows** . **CreateNewOutputRows** est la méthode la plus importante d'un composant source.  
  
 Si vous ouvrez la fenêtre **Explorateur de projets** dans VSTA, vous constatez que le composant Script a également généré des éléments de projet **BufferWrapper** et **ComponentWrapper** en lecture seule. La classe **ScriptMain** hérite de la classe **UserComponent** dans l'élément de projet **ComponentWrapper** .  
  
 Au moment de l’exécution, le moteur de flux de données appelle la méthode **PrimeOutput** dans la classe **UserComponent**, qui remplace la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.PrimeOutput%2A> de la classe parente <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La méthode **PrimeOutput** appelle à son tour les méthodes suivantes :  
  
1.  La méthode **CreateNewOutputRows** , que vous substituez dans **ScriptMain** pour ajouter des lignes de la source de données aux mémoires tampons de sortie, initialement vides.  
  
2.  La méthode **FinishOutputs** , vide par défaut. Substituez cette méthode dans **ScriptMain** pour effectuer les traitements requis pour terminer l'opération de sortie.  
  
3.  La méthode **MarkOutputsAsFinished** privée, qui appelle la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer.SetEndOfRowset%2A> de la classe parente <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptBuffer> pour indiquer au moteur de flux que l’opération de sortie est terminée. Vous n'avez pas besoin d'appeler **SetEndOfRowset** de manière explicite dans votre propre code.  
  
### <a name="writing-your-custom-code"></a>Écriture du code personnalisé  
 Pour achever la création d'un composant source personnalisé, vous pouvez écrire le script dans les méthodes suivantes disponibles dans la classe **ScriptMain** .  
  
1.  Substituez la méthode **AcquireConnections** pour vous connecter à la source de données externe. Extrayez l'objet de connexion, ou les informations de connexion requises, du gestionnaire de connexions.  
  
2.  Substituez la méthode **PreExecute** pour charger des données, si vous pouvez charger toutes les donnée sources en même temps. Par exemple, vous pouvez exécuter un objet **SqlCommand** sur une connexion [!INCLUDE[vstecado](../../includes/vstecado-md.md)] à une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et charger en même temps toutes les données sources dans **SqlDataReader**. Si vous devez charger les données sources une ligne à la fois, (par exemple, lors de la lecture d'un fichier texte) vous pouvez charger les données pendant que vous parcourez les lignes dans **CreateNewOutputRows**.  
  
3.  Utilisez la méthode **CreateNewOutputRows** substituée pour ajouter de nouvelles lignes aux mémoires tampons de sortie vides et remplir les valeurs de chaque colonne dans les nouvelles lignes de sortie. Utilisez la méthode **AddRow** de chaque mémoire tampon de sortie pour ajouter une nouvelle ligne vide, puis définissez les valeurs de chaque colonne. En général, vous pouvez copier les valeurs des colonnes chargées à partir de la source externe.  
  
4.  Substituez la méthode **PostExecute** pour terminer le traitement des données. Par exemple, vous pouvez fermer **SqlDataReader** qui vous a permis de charger des données.  
  
5.  Substituez la méthode **ReleaseConnections** pour vous déconnecter de la source de données externe, le cas échéant.  
  
## <a name="examples"></a>Exemples  
 Les exemples suivants montrent le code personnalisé requis dans la classe **ScriptMain** pour créer un composant source.  
  
> [!NOTE]  
>  Ces exemples utilisent la table **Person.Address** de l’exemple de base de données **AdventureWorks** et passent ses première et quatrième colonnes (à savoir les colonnes **intAddressID** et **nvarchar(30)City**) dans le flux de données. Les mêmes données sont utilisées dans les exemples de source, transformation et destination de cette section. Des conditions préalables et des hypothèses supplémentaires sont documentées pour chaque exemple.  
  
### <a name="adonet-source-example"></a>Exemple de source ADO.NET  
 Cet exemple montre un composant source qui utilise un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] existant pour charger les données d’une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans le flux de données.  
  
 Si vous souhaitez exécuter cet exemple de code, vous devez configurer le package et le composant comme suit :  
  
1.  Créez un gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] qui utilise le fournisseur **SqlClient** pour se connecter à la base de données **AdventureWorks**.  
  
2.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que source.  
  
3.  Ouvrez l' **Éditeur de transformation de script**. Dans la page **Entrées et sorties** , renommez la sortie par défaut avec un nom plus descriptif, tel que **MyAddressOutput**, puis ajoutez et configurez les deux colonnes de sortie, **AddressID** et **City**.  
  
    > [!NOTE]  
    >  N'oubliez pas de remplacer le type de données de la colonne de sortie de ville **City** par DT_WSTR.  
  
4.  Dans la page **Gestionnaires de connexions**, ajoutez ou créez le gestionnaire de connexions [!INCLUDE[vstecado](../../includes/vstecado-md.md)] et attribuez-lui un nom, par exemple **MyADONETConnection**.  
  
5.  Dans la page **Script** , cliquez sur **Modifier le script** , puis entrez le script suivant. Ensuite, fermez l'environnement de développement de script et l' **Éditeur de transformation de script**.  
  
6.  Créez et configurez un composant de destination, comme une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l’exemple de composant de destination présenté dans [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md), qui attend les colonnes **AddressID** et **City**. Puis, connectez le composant source à la destination. (Vous pouvez connecter directement une source à une destination sans transformation.) Vous pouvez créer une table de destination en exécutant la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans la base de données **AdventureWorks** :  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Exécutez l’exemple.  
  
    ```vb  
    Imports System.Data.SqlClient  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Dim connMgr As IDTSConnectionManager100  
        Dim sqlConn As SqlConnection  
        Dim sqlReader As SqlDataReader  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            connMgr = Me.Connections.MyADONETConnection  
            sqlConn = CType(connMgr.AcquireConnection(Nothing), SqlConnection)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim cmd As New SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn)  
            sqlReader = cmd.ExecuteReader  
  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Do While sqlReader.Read  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = sqlReader.GetInt32(0)  
                    .City = sqlReader.GetString(1)  
                End With  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
  
            sqlReader.Close()  
  
        End Sub  
  
        Public Overrides Sub ReleaseConnections()  
  
            connMgr.ReleaseConnection(sqlConn)  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.Data.SqlClient;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        IDTSConnectionManager100 connMgr;  
        SqlConnection sqlConn;  
        SqlDataReader sqlReader;  
  
        public override void AcquireConnections(object Transaction)  
        {  
            connMgr = this.Connections.MyADONETConnection;  
            sqlConn = (SqlConnection)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
  
            SqlCommand cmd = new SqlCommand("SELECT AddressID, City, StateProvinceID FROM Person.Address", sqlConn);  
            sqlReader = cmd.ExecuteReader();  
  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            while (sqlReader.Read())  
            {  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = sqlReader.GetInt32(0);  
                    MyAddressOutputBuffer.City = sqlReader.GetString(1);  
                }  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            sqlReader.Close();  
  
        }  
  
        public override void ReleaseConnections()  
        {  
  
            connMgr.ReleaseConnection(sqlConn);  
  
        }  
  
    }  
    ```  
  
### <a name="flat-file-source-example"></a>Exemple de source de fichier plat  
 Cet exemple montre un composant source qui utilise un gestionnaire de connexions de fichiers plats existant pour charger les données d'un fichier plat dans le flux de données. Vous pouvez créer des données sources de fichier plat en les exportant de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si vous souhaitez exécuter cet exemple de code, vous devez configurer le package et le composant comme suit :  
  
1.  Utilisez l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour exporter la table **Person.Address** de l’exemple de base de données **AdventureWorks** vers un fichier plat délimité par des virgules. Cet exemple utilise le nom de fichier ExportedAddresses.txt.  
  
2.  Créez un gestionnaire de connexions de fichiers plats qui se connecte au fichier de données exporté.  
  
3.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que source.  
  
4.  Ouvrez l' **Éditeur de transformation de script**. Dans la page **Entrées et sorties** , renommez la sortie par défaut avec un nom plus descriptif, tel que **MyAddressOutput**. Ajoutez et configurez les deux colonnes de sortie, **AddressID** et **City**.  
  
5.  Dans la page **Gestionnaires de connexions** , ajoutez ou créez le gestionnaire de connexions de fichiers plats, en lui attribuant un nom descriptif, tel que **MyFlatFileSrcConnectionManager**.  
  
6.  Dans la page **Script** , cliquez sur **Modifier le script** , puis entrez le script suivant. Ensuite, fermez l'environnement de développement de script et l' **Éditeur de transformation de script**.  
  
7.  Créez et configurez un composant de destination, comme une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l’exemple de composant de destination présenté dans [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Puis, connectez le composant source à la destination. (Vous pouvez connecter directement une source à une destination sans transformation.) Vous pouvez créer une table de destination en exécutant la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans la base de données **AdventureWorks** :  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
8.  Exécutez l’exemple.  
  
    ```vb  
    Imports System.IO  
    ...  
    Public Class ScriptMain  
        Inherits UserComponent  
  
        Private textReader As StreamReader  
        Private exportedAddressFile As String  
  
        Public Overrides Sub AcquireConnections(ByVal Transaction As Object)  
  
            Dim connMgr As IDTSConnectionManager100 = _  
                Me.Connections.MyFlatFileSrcConnectionManager  
            exportedAddressFile = _  
                CType(connMgr.AcquireConnection(Nothing), String)  
  
        End Sub  
  
        Public Overrides Sub PreExecute()  
            MyBase.PreExecute()  
            textReader = New StreamReader(exportedAddressFile)  
        End Sub  
  
        Public Overrides Sub CreateNewOutputRows()  
  
            Dim nextLine As String  
            Dim columns As String()  
  
            Dim delimiters As Char()  
            delimiters = ",".ToCharArray  
  
            nextLine = textReader.ReadLine  
            Do While nextLine IsNot Nothing  
                columns = nextLine.Split(delimiters)  
                With MyAddressOutputBuffer  
                    .AddRow()  
                    .AddressID = columns(0)  
                    .City = columns(3)  
                End With  
                nextLine = textReader.ReadLine  
            Loop  
  
        End Sub  
  
        Public Overrides Sub PostExecute()  
            MyBase.PostExecute()  
            textReader.Close()  
  
        End Sub  
  
    End Class  
    ```  
  
    ```csharp  
    using System.IO;  
    public class ScriptMain:  
        UserComponent  
  
    {  
        private StreamReader textReader;  
        private string exportedAddressFile;  
  
        public override void AcquireConnections(object Transaction)  
        {  
  
            IDTSConnectionManager100 connMgr = this.Connections.MyFlatFileSrcConnectionManager;  
            exportedAddressFile = (string)connMgr.AcquireConnection(null);  
  
        }  
  
        public override void PreExecute()  
        {  
            base.PreExecute();  
            textReader = new StreamReader(exportedAddressFile);  
        }  
  
        public override void CreateNewOutputRows()  
        {  
  
            string nextLine;  
            string[] columns;  
  
            char[] delimiters;  
            delimiters = ",".ToCharArray();  
  
            nextLine = textReader.ReadLine();  
            while (nextLine != null)  
            {  
                columns = nextLine.Split(delimiters);  
                {  
                    MyAddressOutputBuffer.AddRow();  
                    MyAddressOutputBuffer.AddressID = columns[0];  
                    MyAddressOutputBuffer.City = columns[3];  
                }  
                nextLine = textReader.ReadLine();  
            }  
  
        }  
  
        public override void PostExecute()  
        {  
  
            base.PostExecute();  
            textReader.Close();  
  
        }  
  
    }  
    ```  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md)   
 [Développement d’un composant source personnalisé](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md)  
  
