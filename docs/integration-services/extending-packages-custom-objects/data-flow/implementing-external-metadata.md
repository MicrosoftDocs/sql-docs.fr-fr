---
description: Implémentation des métadonnées externes
title: Implémentation des métadonnées externes | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- disconnected validation [Integration Services]
- connected validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- metadata [Integration Services]
- data flow components [Integration Services], validating
- data flow components [Integration Services], external metadata
- custom data flow components [Integration Services], external metadata
- external metadata [Integration Services]
ms.assetid: 8f5bd3ed-3e79-43a4-b6c1-435e4c2cc8cc
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9b24d33156162028fdef7e0806a5ac2f8d9f0ef5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343669"
---
# <a name="implementing-external-metadata"></a>Implémentation des métadonnées externes

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Lorsqu'un composant est déconnecté de sa source de données, vous pouvez valider les colonnes comprises dans les collections de colonnes d'entrée et de sortie par rapport aux colonnes de sa source de données externe en utilisant l'interface <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100>. Cette interface vous permet de conserver un instantané des colonnes à la source de données externe et de les mapper aux colonnes de la collection de colonnes d'entrée et de sortie du composant.  
  
 L'implémentation de colonnes de métadonnées externes ajoute une couche de charge et de complexité au développement du composant, parce que vous devez effectuer une gestion et une validation par rapport à une collection de colonnes supplémentaire, mais la capacité d'éviter des allers-retours onéreux vers le serveur pour la validation peut rendre ce travail de développement valable.  
  
## <a name="populating-external-metadata-columns"></a>Remplissage de colonnes de métadonnées externes  
 Les colonnes de métadonnées externes sont ajoutées en général à la collection lorsque la colonne d'entrée ou de sortie correspondante est créée. Les nouvelles colonnes sont créées en appelant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.New%2A>. Les propriétés de la colonne sont alors configurées pour correspondre à la source de données externe.  
  
 La colonne de métadonnées externes est mappée à la colonne d'entrée ou de sortie correspondante en assignant l'ID de la colonne de métadonnées externes à la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A> de la colonne d'entrée ou de sortie. Cela vous permet de localiser facilement la colonne de métadonnées externes pour une colonne d'entrée ou de sortie spécifique en utilisant la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.GetObjectByID%2A> de la collection.  
  
 L'exemple suivant indique comment créer une colonne de métadonnées externes, puis comment la mapper à une colonne de sortie en définissant la propriété<xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.ExternalMetadataColumnID%2A>.  
  
```csharp  
public void CreateExternalMetaDataColumn(IDTSOutput100 output, int outputColumnID )  
{  
    IDTSOutputColumn100 oColumn = output.OutputColumnCollection.GetObjectByID(outputColumnID);  
    IDTSExternalMetadataColumn100 eColumn = output.ExternalMetadataColumnCollection.New();  
  
    eColumn.DataType = oColumn.DataType;  
    eColumn.Precision = oColumn.Precision;  
    eColumn.Scale = oColumn.Scale;  
    eColumn.Length = oColumn.Length;  
    eColumn.CodePage = oColumn.CodePage;  
  
    oColumn.ExternalMetadataColumnID = eColumn.ID;  
}  
```  
  
```vb  
Public Sub CreateExternalMetaDataColumn(ByVal output As IDTSOutput100, ByVal outputColumnID As Integer)   
 Dim oColumn As IDTSOutputColumn100 = output.OutputColumnCollection.GetObjectByID(outputColumnID)   
 Dim eColumn As IDTSExternalMetadataColumn100 = output.ExternalMetadataColumnCollection.New   
 eColumn.DataType = oColumn.DataType   
 eColumn.Precision = oColumn.Precision   
 eColumn.Scale = oColumn.Scale   
 eColumn.Length = oColumn.Length   
 eColumn.CodePage = oColumn.CodePage   
 oColumn.ExternalMetadataColumnID = eColumn.ID   
End Sub  
```  
  
## <a name="validating-with-external-metadata-columns"></a>Validation avec les colonnes de métadonnées externes  
 La validation requiert des étapes supplémentaires pour les composants qui gèrent une collection de colonnes de métadonnées externes, parce que vous devez effectuer une validation par rapport à une collection supplémentaire de colonnes. La validation peut être divisée en validation connectée ou validation déconnectée.  
  
### <a name="connected-validation"></a>Validation connectée  
 Lorsqu'un composant est connecté à une source de données externe, les colonnes comprises dans les collections d'entrée ou de sortie sont vérifiées directement par rapport à la source de données externe. En outre, les colonnes comprises dans la collection de métadonnées externes doivent être validées. Cette validation est requise car la collection de métadonnées externe peut être modifiée à l’aide de l’**Éditeur avancé** dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], et les modifications apportées à la collection ne sont pas détectables. Par conséquent, en cas de connexion, les composants doivent s'assurer que les colonnes comprises dans la collection de colonnes de métadonnées externes continuent à refléter les colonnes à la source de données externe.  
  
 Vous pouvez choisir de masquer la collection de métadonnées externes dans l’**Éditeur avancé** en définissant la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSExternalMetadataColumnCollection100.IsUsed%2A> de la collection sur **false**. Cependant, cela masque également l’onglet **Mappage de colonnes** de l’éditeur, lequel permet aux utilisateurs de mapper des colonnes de la collection d’entrée ou de sortie aux colonnes de la collection de colonnes de métadonnées externes. La définition de cette propriété sur **false** n’empêche pas les développeurs de modifier la collection par programmation, mais elle fournit un niveau de protection pour la collection de colonnes de métadonnées externes d’un composant qui est utilisé exclusivement dans [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
### <a name="disconnected-validation"></a>Validation déconnectée  
 Lorsqu'un composant est déconnecté d'une source de données externe, la validation est simplifiée parce que les colonnes comprises dans la collection d'entrée ou de sortie sont vérifiées directement par rapport aux colonnes de la collection de métadonnées externes et non pas par rapport à la source externe. Un composant doit effectuer une validation déconnectée lorsque la connexion à sa source de données externe n’a pas été établie ou lorsque la propriété <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ValidateExternalMetadata%2A> a la valeur **false**.  
  
 L'exemple de code suivant montre l'implémentation d'un composant qui effectue la validation par rapport à sa collection de colonnes de métadonnées externes.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    if( this.isConnected && ComponentMetaData.ValidateExternalMetaData )  
    {  
        // TODO: Perform connected validation.  
    }  
    else  
    {  
        // TODO: Perform disconnected validation.  
    }  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
 If Me.isConnected AndAlso ComponentMetaData.ValidateExternalMetaData Then   
  ' TODO: Perform connected validation.  
 Else   
  ' TODO: Perform disconnected validation.  
 End If   
End Function  
```  

## <a name="see-also"></a>Voir aussi  
 [Flux de données](../../../integration-services/data-flow/data-flow.md)  
  
  
