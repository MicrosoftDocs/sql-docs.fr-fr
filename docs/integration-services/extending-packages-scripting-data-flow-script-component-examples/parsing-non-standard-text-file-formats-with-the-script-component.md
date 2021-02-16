---
description: Analyse de formats de fichiers texte non standard à l'aide du composant Script
title: Analyse de formats de fichiers texte non standard à l’aide du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- text file reading [Integration Services]
- Script component [Integration Services], non-standard text file formats
- transformations [Integration Services], components
- Script component [Integration Services], examples
ms.assetid: 1fda034d-09e4-4647-9a9f-e8d508c2cc8f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c48454ceec4f05f6b2386e4abb96092bbbbcb140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344860"
---
# <a name="parsing-non-standard-text-file-formats-with-the-script-component"></a>Analyse de formats de fichiers texte non standard à l'aide du composant Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Lorsque vos données sources sont organisées dans un format non standard, il peut être plus pratique de consolider l'ensemble de la logique d'analyse en un seul script au lieu de chaîner plusieurs transformations [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour parvenir au même résultat.  
  
 [Exemple 1 : Analyse d’enregistrements délimités par des lignes](#example1)  
  
 [Exemple 2 : fractionnement d’enregistrements parents et enfants](#example2)  
  
> [!NOTE]  
>  Si vous souhaitez créer un composant que vous pouvez réutiliser plus facilement dans plusieurs tâches de flux de données et plusieurs packages, utilisez le code présenté dans cet exemple de composant Script comme point de départ pour un composant de flux de données personnalisé. Pour plus d’informations, consultez [Développement d’un composant de flux de données personnalisé](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
##  <a name="example-1-parsing-row-delimited-records"></a><a name="example1"></a> Exemple 1 : analyse d’enregistrements délimités par des lignes  
 Cet exemple montre comment analyser un fichier texte, dans lequel chaque colonne de données figure sur une ligne séparée, dans une table de destination, à l'aide du composant Script.  
  
 Pour plus d’informations sur la configuration du composant Script en vue de son utilisation comme transformation dans le flux de données, consultez [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) et [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Pour configurer cet exemple de composant Script  
  
1.  Créez et enregistrez un fichier texte nommé **rowdelimiteddata.txt** contenant les données sources suivantes :  
  
    ```  
    FirstName: Nancy  
    LastName: Davolio  
    Title: Sales Representative  
    City: Seattle  
    StateProvince: WA  
  
    FirstName: Andrew  
    LastName: Fuller  
    Title: Vice President, Sales  
    City: Tacoma  
    StateProvince: WA  
  
    FirstName: Steven  
    LastName: Buchanan  
    Title: Sales Manager  
    City: London  
    StateProvince:  
  
    ```  
  
2.  Ouvrez [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] et connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Sélectionnez une base de données de destination et ouvrez une nouvelle fenêtre de requête. Dans la fenêtre de requête, exécutez le script suivant pour créer la table de destination :  
  
    ```sql
    create table RowDelimitedData  
    (  
    FirstName varchar(32),  
    LastName varchar(32),  
    Title varchar(32),  
    City varchar(32),  
    StateProvince varchar(32)  
    )  
  
    ```  
  
4.  Ouvrez [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] et créez un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nommé ParseRowDelim.dtsx.  
  
5.  Ajoutez un gestionnaire de connexions de fichiers plats au package, nommez-le RowDelimitedData et configurez-le pour qu'il se connecte au fichier rowdelimiteddata.txt créé à l'étape précédente.  
  
6.  Ajoutez un gestionnaire de connexions OLE DB au package et configurez-le pour qu'il se connecte à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à la base de données dans laquelle vous avez créé la table de destination.  
  
7.  Ajoutez une tâche de flux de données au package, puis cliquez sur l’onglet **Flux de données** du concepteur SSIS.  
  
8.  Ajoutez une source de fichier plat au flux de données et configurez-la pour qu'elle utilise le gestionnaire de connexions RowDelimitedData. Dans la page **Colonnes** de l’**Éditeur de source de fichier plat**, sélectionnez la seule colonne externe disponible.  
  
9. Ajoutez un composant Script au flux de données et configurez-le en tant que transformation. Connectez la sortie de la source de fichier plat au composant Script.  
  
10. Double-cliquez sur le composant Script pour afficher l’**Éditeur de transformation de script**.  
  
11. Dans la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, sélectionnez la seule colonne d’entrée disponible.  
  
12. Dans la page **Entrées et sorties** de l’**Éditeur de transformation de script**, sélectionnez Sortie 0 et attribuez la valeur Aucun à **SynchronousInputID**. Créez 5 colonnes de sortie, de type String [DT_STR] et de longueur 32 :  
  
    -   FirstName  
  
    -   LastName  
  
    -   Title  
  
    -   City  
  
    -   StateProvince  
  
13. Dans la page **Script** de l’**Éditeur de transformation de script**, cliquez sur **Modifier le script**, puis entrez le code indiqué dans la classe **ScriptMain** de l’exemple. Fermez l’environnement de développement de script et l’**Éditeur de transformation de script**.  
  
14. Ajoutez une destination pour SQL Server au flux de données. Configurez-la pour qu'elle utilise le gestionnaire de connexions OLE DB et la table RowDelimitedData. Connectez la sortie du composant Script à cette destination.  
  
15. Exécutez le package. Après l'exécution du package, examinez les enregistrements dans la table de destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Dim columnName As String  
    Dim columnValue As String  
  
    ' Check for an empty row.  
    If Row.Column0.Trim.Length > 0 Then  
        columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"))  
        ' Check for an empty value after the colon.  
        If Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd.Length > 1 Then  
            ' Extract the column value from after the colon and space.  
            columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2)  
            Select Case columnName  
                Case "FirstName"  
                    ' The FirstName value indicates a new record.  
                    Me.Output0Buffer.AddRow()  
                    Me.Output0Buffer.FirstName = columnValue  
                Case "LastName"  
                    Me.Output0Buffer.LastName = columnValue  
                Case "Title"  
                    Me.Output0Buffer.Title = columnValue  
                Case "City"  
                    Me.Output0Buffer.City = columnValue  
                Case "StateProvince"  
                    Me.Output0Buffer.StateProvince = columnValue  
            End Select  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
        string columnName;  
        string columnValue;  
  
        // Check for an empty row.  
        if (Row.Column0.Trim().Length > 0)  
        {  
            columnName = Row.Column0.Substring(0, Row.Column0.IndexOf(":"));  
            // Check for an empty value after the colon.  
            if (Row.Column0.Substring(Row.Column0.IndexOf(":")).TrimEnd().Length > 1)  
            // Extract the column value from after the colon and space.  
            {  
                columnValue = Row.Column0.Substring(Row.Column0.IndexOf(":") + 2);  
                switch (columnName)  
                {  
                    case "FirstName":  
                        // The FirstName value indicates a new record.  
                        this.Output0Buffer.AddRow();  
                        this.Output0Buffer.FirstName = columnValue;  
                        break;  
                    case "LastName":  
                        this.Output0Buffer.LastName = columnValue;  
                        break;  
                    case "Title":  
                        this.Output0Buffer.Title = columnValue;  
                        break;  
                    case "City":  
                        this.Output0Buffer.City = columnValue;  
                        break;  
                    case "StateProvince":  
                        this.Output0Buffer.StateProvince = columnValue;  
                        break;  
                }  
            }  
        }  
  
    }  
```  
  
##  <a name="example-2-splitting-parent-and-child-records"></a><a name="example2"></a> Exemple 2 : Fractionnement d’enregistrements parents et enfants  
 Cet exemple montre comment analyser un fichier texte, dans lequel une ligne de séparateur précède une ligne d'enregistrement parente suivie d'un nombre indéfini de lignes d'enregistrement enfants, dans des tables de destination parentes et enfants correctement normalisées, à l'aide du composant Script. Cet exemple simple pourrait être facilement adapté aux fichiers sources qui utilisent plusieurs lignes ou colonnes pour chaque enregistrement parent et enfant, tant qu'il est possible d'identifier le début et la fin de chaque enregistrement.  
  
> [!CAUTION]  
>  Cet exemple est fourni à titre de démonstration uniquement. Si vous exécutez l'exemple plusieurs fois, il insère des valeurs de clé dupliquées dans la table de destination.  
  
 Pour plus d’informations sur la configuration du composant Script en vue de son utilisation comme transformation dans le flux de données, consultez [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) et [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Pour configurer cet exemple de composant Script  
  
1.  Créez et enregistrez un fichier texte nommé **parentchilddata.txt** contenant les données sources suivantes :  
  
    ```  
    **********  
    PARENT 1 DATA  
    child 1 data  
    child 2 data  
    child 3 data  
    child 4 data  
    **********  
    PARENT 2 DATA  
    child 5 data  
    child 6 data  
    child 7 data  
    child 8 data  
    **********  
  
    ```  
  
2.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] et connectez-vous à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
3.  Sélectionnez une base de données de destination et ouvrez une nouvelle fenêtre de requête. Dans la fenêtre de requête, exécutez le script suivant pour créer les tables de destination :  
  
    ```sql
    CREATE TABLE [dbo].[Parents]([ParentID] [int] NOT NULL,  
    [ParentRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Parents] PRIMARY KEY CLUSTERED   
    ([ParentID] ASC))  
    GO  
    CREATE TABLE [dbo].[Children]([ChildID] [int] NOT NULL,  
    [ParentID] [int] NOT NULL,  
    [ChildRecord] [varchar](32) NOT NULL,  
     CONSTRAINT [PK_Children] PRIMARY KEY CLUSTERED   
    ([ChildID] ASC))  
    GO  
    ALTER TABLE [dbo].[Children] ADD CONSTRAINT [FK_Children_Parents] FOREIGN KEY([ParentID])  
    REFERENCES [dbo].[Parents] ([ParentID])  
  
    ```  
  
4.  Ouvrez [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] et créez un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] nommé SplitParentChild.dtsx.  
  
5.  Ajoutez un gestionnaire de connexions de fichiers plats au package, nommez-le ParentChildData et configurez-le pour qu'il se connecte au fichier parentchilddata.txt créé à l'étape précédente.  
  
6.  Ajoutez un gestionnaire de connexions OLE DB au package et configurez-le pour qu'il se connecte à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et à la base de données dans laquelle vous avez créé les tables de destination.  
  
7.  Ajoutez une tâche de flux de données au package, puis cliquez sur l’onglet **Flux de données** du concepteur SSIS.  
  
8.  Ajoutez une source de fichier plat au flux de données et configurez-la pour qu'elle utilise le gestionnaire de connexions ParentChildData. Dans la page **Colonnes** de l’**Éditeur de source de fichier plat**, sélectionnez la seule colonne externe disponible.  
  
9. Ajoutez un composant Script au flux de données et configurez-le en tant que transformation. Connectez la sortie de la source de fichier plat au composant Script.  
  
10. Double-cliquez sur le composant Script pour afficher l’**Éditeur de transformation de script**.  
  
11. Dans la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, sélectionnez la seule colonne d’entrée disponible.  
  
12. Dans la page **Entrées et sorties** de l’**Éditeur de transformation de script**, sélectionnez le paramètre Sortie 0, renommez-le ParentRecords et attribuez la valeur Aucune à **SynchronousInputID**. Créez 2 colonnes de sortie :  
  
    -   ParentID (la clé primaire), de type entier signé (4 bits) [DT_I4]  
  
    -   ParentRecord, de type String [DT_STR] et de longueur 32  
  
13. Créez une deuxième sortie et nommez-la ChildRecords. La propriété **SynchronousInputID** de la nouvelle sortie a déjà la valeur Aucun. Créez 3 colonnes de sortie :  
  
    -   ChildID (la clé primaire), de type entier signé (4 bits) [DT_I4]  
  
    -   ParentID (la clé étrangère), également de type entier signé (4 bits) [DT_I4]  
  
    -   ChildRecord, de type String [DT_STR] et de longueur 50  
  
14. Dans la page **Script** de l’**Éditeur de transformation de script**, cliquez sur **Modifier le script**. Dans la classe **ScriptMain**, entrez le code présenté dans l’exemple. Fermez l’environnement de développement de script et l’**Éditeur de transformation de script**.  
  
15. Ajoutez une destination pour SQL Server au flux de données. Connectez la sortie ParentRecords du composant Script à cette destination. Configurez-la pour qu'elle utilise le gestionnaire de connexions OLE DB et la table Parents.  
  
16. Ajoutez une autre destination pour SQL Server au flux de données. Connectez la sortie ChildRecords du composant Script à cette destination. Configurez-la pour qu'elle utilise le gestionnaire de connexions OLE DB et la table Children.  
  
17. Exécutez le package. Après l'exécution du package, examinez les enregistrements parents et enfants dans les deux tables de destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```vb  
Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)  
  
    Static nextRowIsParent As Boolean = False  
    Static parentCounter As Integer = 0  
    Static childCounter As Integer = 0  
  
    ' If current row starts with separator characters,  
    '  then following row contains new parent record.  
    If Row.Column0.StartsWith("***") Then  
        nextRowIsParent = True  
    Else  
        If nextRowIsParent Then  
            ' Current row contains parent record.  
            parentCounter += 1  
            Me.ParentRecordsBuffer.AddRow()  
            Me.ParentRecordsBuffer.ParentID = parentCounter  
            Me.ParentRecordsBuffer.ParentRecord = Row.Column0  
            nextRowIsParent = False  
        Else  
            ' Current row contains child record.  
            childCounter += 1  
            Me.ChildRecordsBuffer.AddRow()  
            Me.ChildRecordsBuffer.ChildID = childCounter  
            Me.ChildRecordsBuffer.ParentID = parentCounter  
            Me.ChildRecordsBuffer.ChildRecord = Row.Column0  
        End If  
    End If  
  
End Sub  
```  
  
```csharp  
public override void Input0_ProcessInputRow(Input0Buffer Row)  
    {  
  
    int static_Input0_ProcessInputRow_childCounter = 0;  
    int static_Input0_ProcessInputRow_parentCounter = 0;  
    bool static_Input0_ProcessInputRow_nextRowIsParent = false;  
  
        // If current row starts with separator characters,   
        // then following row contains new parent record.   
        if (Row.Column0.StartsWith("***"))  
        {  
            static_Input0_ProcessInputRow_nextRowIsParent = true;  
        }  
        else  
        {  
            if (static_Input0_ProcessInputRow_nextRowIsParent)  
            {  
                // Current row contains parent record.   
                static_Input0_ProcessInputRow_parentCounter += 1;  
                this.ParentRecordsBuffer.AddRow();  
                this.ParentRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ParentRecordsBuffer.ParentRecord = Row.Column0;  
                static_Input0_ProcessInputRow_nextRowIsParent = false;  
            }  
            else  
            {  
                // Current row contains child record.   
                static_Input0_ProcessInputRow_childCounter += 1;  
                this.ChildRecordsBuffer.AddRow();  
                this.ChildRecordsBuffer.ChildID = static_Input0_ProcessInputRow_childCounter;  
                this.ChildRecordsBuffer.ParentID = static_Input0_ProcessInputRow_parentCounter;  
                this.ChildRecordsBuffer.ChildRecord = Row.Column0;  
            }  
        }  
  
    }  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
 [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
