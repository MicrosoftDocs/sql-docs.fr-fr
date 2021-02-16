---
description: Amélioration d'une sortie d'erreur à l'aide du composant Script
title: Amélioration d’une sortie d’erreur à l’aide du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- transformations [Integration Services], components
- Script component [Integration Services], examples
- error outputs [Integration Services], enhancing
- Script component [Integration Services], transformation components
ms.assetid: f7c02709-f1fa-4ebd-b255-dc8b81feeaa5
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 0e3fee4ea309a5cc48b938abd50f846f16358412
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344898"
---
# <a name="enhancing-an-error-output-with-the-script-component"></a>Amélioration d'une sortie d'erreur à l'aide du composant Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Par défaut, les deux colonnes supplémentaires d’une sortie d’erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], ErrorCode et ErrorColumn, contiennent uniquement des codes numériques identifiant un numéro d’erreur et l’ID de la colonne dans laquelle l’erreur s’est produite. Ces valeurs numériques peuvent se révéler d’une utilité limitée en l’absence de la description d’erreur et du nom de colonne correspondants.  
  
 Cette rubrique décrit comment ajouter la description d’erreur et le nom de colonne à des données de sortie d’erreur existantes dans le flux de données en utilisant le composant Script. L'exemple ajoute la description d'erreur qui correspond à un code d'erreur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] prédéfini spécifique à l'aide de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.GetErrorDescription%2A> de l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, disponible via la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ComponentMetaData%2A> du composant Script. Le nom de colonne qui correspond à l’ID de lignage capturé est ensuite ajouté à l’aide de la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData130.GetIdentificationStringByID%2A> de la même interface.  
  
> [!NOTE]  
>  Si vous souhaitez créer un composant que vous pouvez réutiliser plus facilement dans plusieurs tâches de flux de données et plusieurs packages, utilisez le code présenté dans cet exemple de composant Script comme point de départ pour un composant de flux de données personnalisé. Pour plus d’informations, consultez [Développement d’un composant de flux de données personnalisé](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md).  
  
## <a name="example"></a> Exemple  
 L’exemple indiqué ici utilise un composant Script configuré en tant que transformation pour ajouter la description d’erreur et le nom de colonne à des données de sortie d’erreur existantes dans le flux de données.  
  
 Pour plus d’informations sur la configuration du composant Script en vue de son utilisation comme transformation dans le flux de données, consultez [Création d’une transformation synchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md) et [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md).  
  
#### <a name="to-configure-this-script-component-example"></a>Pour configurer cet exemple de composant Script  
  
1.  Avant de créer le composant Script, configurez un composant en amont dans le flux de données pour rediriger des lignes vers sa sortie d'erreur lorsqu'une erreur ou troncation se produit. À des fins de test, vous pouvez configurer un composant de manière à garantir que des erreurs se produiront, par exemple en configurant une transformation de recherche entre deux tables où la recherche échouera.  
  
2.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que transformation.  
  
3.  Connectez la sortie d'erreur du composant en amont au nouveau composant Script.  
  
4.  Ouvrez l’**Éditeur de transformation de script**, puis dans la page **Script**, au niveau de la propriété **ScriptLanguage**, sélectionnez le langage de script.  
  
5.  Cliquez sur **Modifier le script** pour ouvrir l’environnement de développement intégré [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) et ajouter l’exemple de code indiqué ci-dessous.  
  
6.  Fermez VSTA.  
  
7.  Dans l’Éditeur de transformation de script, dans la page **Colonnes d’entrée**, sélectionnez les colonnes ErrorCode et ErrorColumn.  
  
8.  Dans la page **Entrées et sorties**, ajoutez deux nouvelles colonnes.  
  
    -   Ajoutez une nouvelle colonne de sortie de type **String** nommée **ErrorDescription**. Augmentez la longueur par défaut de la nouvelle colonne à 255 pour prendre en charge les longs messages.  
  
    -   Ajoutez une autre nouvelle colonne de sortie de type **String** nommée **ColumnName**. Augmentez la longueur par défaut de la nouvelle colonne à 255 pour prendre en charge les longues valeurs.  
  
9. Fermez l’**Éditeur de transformation de script**.  
  
10. Attachez la sortie du composant Script à une destination appropriée. Les destinations de fichiers plats sont les plus faciles à configurer à des fins de test ad hoc.  
  
11. Exécutez le package.  

```vb
Public Class ScriptMain      ' VB
    Inherits UserComponent
    Public Overrides Sub Input0_ProcessInputRow(ByVal Row As Input0Buffer)

        Row.ErrorDescription = _
            Me.ComponentMetaData.GetErrorDescription(Row.ErrorCode)

        Dim componentMetaData130 = TryCast(Me.ComponentMetaData, IDTSComponentMetaData130)

        If componentMetaData130 IsNot Nothing Then

            If Row.ErrorColumn = 0 Then
                ' 0 means no specific column is identified by ErrorColumn, this time.
                Row.ColumnName = "Check the row for a violation of a foreign key constraint."
            ELSE If Row.ErrorColumn = -1 Then
                ' -1 means you are using Table Lock for a Memory Optimised destination table which is not supported.
                Row.ColumnName = "Table lock is not compatible with Memory Optimised tables."
            Else
                Row.ColumnName = componentMetaData130.GetIdentificationStringByID(Row.ErrorColumn)
            End If
        End If
    End Sub
End Class
```

```csharp
public class ScriptMain:      // C#
    UserComponent
{
    public override void Input0_ProcessInputRow(Input0Buffer Row)
    {
        Row.ErrorDescription = this.ComponentMetaData.GetErrorDescription(Row.ErrorCode);

        var componentMetaData130 = this.ComponentMetaData as IDTSComponentMetaData130;
        if (componentMetaData130 != null)
        {
            // 0 means no specific column is identified by ErrorColumn, this time.
            if (Row.ErrorColumn == 0)
            {
                Row.ColumnName = "Check the row for a violation of a foreign key constraint.";
            }
            // -1 means you are using Table Lock for a Memory Optimised destination table which is not supported.
            else if (Row.ErrorColumn == -1)
            {
                Row.ColumnName = "Table lock is not compatible with Memory Optimised tables.";
            }
            else
            {
                Row.ColumnName = componentMetaData130.GetIdentificationStringByID(Row.ErrorColumn);
            }
        }
    }
}
```

## <a name="see-also"></a>Voir aussi  
 [Gestion des erreurs dans les données](../../integration-services/data-flow/error-handling-in-data.md)   
 [Utilisation de sorties d’erreur dans un composant de flux de données](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md)   
 [Création d'une transformation synchrone à l'aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)   
  
  
