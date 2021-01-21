---
title: Ajouter des extraits de code Transact-SQL
description: Découvrez comment ajouter vos propres extraits de code Transact-SQL au jeu d’extraits de code prédéfinis inclus dans SQL Server.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: 901c7995-8eb5-4d12-8bb0-de0a922b48f8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 51eaabf3196c2b7142228908dcef3587bc819b94
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98595101"
---
# <a name="add-transact-sql-snippets"></a>Ajouter des extraits de code Transact-SQL

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Vous pouvez ajouter vos propres extraits de code Transact-SQL au jeu d'extraits de code prédéfinis inclus dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="creating-a-transact-sql-snippet-file"></a>Création d'un fichier d'extrait de code Transact-SQL  
 La première partie de la création d'un extrait de code [!INCLUDE[tsql](../../includes/tsql-md.md)] consiste à créer un fichier XML contenant le texte de votre extrait de code. Le fichier doit avoir une extension de fichier .snippet et satisfaire à la configuration requise du [schéma d'extrait de code](/previous-versions/visualstudio/visual-studio-2015/ide/code-snippets-schema-reference). Définissez le langage de l'extrait de code sur SQL.  
  
 Vous pouvez utiliser les extraits de code prédéfinis fournis avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme exemples. Pour rechercher les extraits de code prédéfinis, ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], sélectionnez le menu **Outils** et cliquez sur **Gestionnaire des extraits de code**. Sélectionnez **SQL** dans la zone de liste **Langage** ; le chemin d'accès aux extraits de code [!INCLUDE[tsql](../../includes/tsql-md.md)] s'affiche dans la zone **Emplacement** .  
  
## <a name="registering-the-code-snippet"></a>Enregistrement de l'extrait de code  
 Après avoir créé le fichier d'extrait de code, utilisez le Gestionnaire des extraits de code pour enregistrer l'extrait de code dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez ajouter un dossier qui contient plusieurs extraits de code ou importer des extraits de code individuels vers le dossier **Mes extraits de code** .  
  
## <a name="procedures"></a>Procédures  
  
#### <a name="adding-a-snippet-folder"></a>Ajout d'un dossier d'extrait de code  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Sélectionnez le menu **Outils** et cliquez sur **Gestionnaire des extraits de code**.  
  
3.  Cliquez sur le bouton **Add** .  
  
4.  Accédez au dossier qui contient vos extraits de code et cliquez sur le bouton **Sélectionner un dossier** .  
  
#### <a name="importing-a-snippet"></a>Importation d'un extrait de code  
  
1.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
2.  Sélectionnez le menu **Outils** et cliquez sur **Gestionnaire des extraits de code**.  
  
3.  Cliquez sur le bouton **Import**.  
  
4.  Accédez au dossier qui contient votre extrait de code, cliquez sur le fichier .snippet, puis cliquez sur le bouton **Ouvrir** .  
  
## <a name="examples"></a>Exemples  
 L’exemple suivant crée un extrait de code d’entourage **TRY-CATCH** et l’importe vers [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
1.  Collez le code suivant dans le bloc-notes, puis enregistrez-le sous le fichier TryCatch.snippet.  
  
    ```  
    <?xml version="1.0" encoding="utf-8" ?>  
    <CodeSnippets  xmlns="https://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">  
    <_locDefinition xmlns="urn:locstudio">  
        <_locDefault _loc="locNone" />  
        <_locTag _loc="locData">Title</_locTag>  
        <_locTag _loc="locData">Description</_locTag>  
        <_locTag _loc="locData">Author</_locTag>  
        <_locTag _loc="locData">ToolTip</_locTag>  
       <_locTag _loc="locData">Default</_locTag>  
    </_locDefinition>  
    <CodeSnippet Format="1.0.0">  
    <Header>  
    <Title>TryCatch</Title>  
                            <Shortcut></Shortcut>  
    <Description>Example Snippet for Try-Catch.</Description>  
    <Author>SQL Server Books Online Example</Author>  
    <SnippetTypes>  
                                    <SnippetType>SurroundsWith</SnippetType>  
    </SnippetTypes>  
    </Header>  
    <Snippet>  
    <Declarations>  
                                    <Literal>  
                                    <ID>CatchCode</ID>  
                                    <ToolTip>Code to handle the caught error</ToolTip>  
                                    <Default>CatchCode</Default>  
                                    </Literal>  
    </Declarations>  
    <Code Language="SQL"><![CDATA[  
    BEGIN TRY  
  
    $selected$ $end$  
  
    END TRY  
    BEGIN CATCH  
  
    $CatchCode$  
  
    END CATCH;  
    ]]>  
    </Code>  
    </Snippet>  
    </CodeSnippet>  
    </CodeSnippets>  
    ```  
  
2.  Ouvrez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
3.  Sélectionnez le menu **Outils** et cliquez sur **Gestionnaire des extraits de code**.  
  
4.  Cliquez sur le bouton **Import**.  
  
5.  Accédez au dossier qui contient TryCatch.snippet, cliquez sur le fichier TryCatch.snippet, puis cliquez sur le bouton **Ouvrir** . Vous devez normalement voir un extrait de code TryCatch dans votre dossier **Mes extraits de code**.  
  
## <a name="see-also"></a>Voir aussi  
 [Insérer des extraits de code d’entourage (surround-with) Transact-SQL](./insert-surround-with-transact-sql-snippets.md)  
