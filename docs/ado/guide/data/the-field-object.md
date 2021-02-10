---
description: Field, objet
title: Objet Field | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Field object [ADO]
ms.assetid: 7d1c4ad5-4be3-42ab-b516-e7133ca300bc
author: rothja
ms.author: jroth
ms.openlocfilehash: bf3a704b26e35c77d9ba7ba059978091002cc424
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036938"
---
# <a name="the-field-object"></a>Field, objet
Chaque objet de **champ** correspond généralement à une colonne dans une table de base de données. Toutefois, un **champ** peut également représenter un pointeur vers un autre **Recordset**, appelé un chapitre. Les exceptions, telles que les colonnes de chapitre, seront abordées plus loin dans ce guide.  
  
 Utilisez la propriété **value** des objets **Field** pour définir ou renvoyer des données pour l’enregistrement actif. Selon la fonctionnalité que le fournisseur expose, certaines collections, méthodes ou propriétés d’un objet de **champ** peuvent ne pas être disponibles.  
  
 Avec les collections, les méthodes et les propriétés d’un objet **Field** , vous pouvez effectuer les opérations suivantes :  
  
-   Retourne le nom d’un champ à l’aide de la propriété **Name** .  
  
-   Affichez ou modifiez les données du champ à l’aide de la propriété **value** . La **valeur** est la propriété par défaut de l’objet de **champ** .  
  
-   Retourne les caractéristiques de base d’un champ à l’aide des propriétés **type**, **PRECISION** et **NumericScale** .  
  
-   Retourne la taille déclarée d’un champ à l’aide de la propriété **DefinedSize** .  
  
-   Retourne la taille réelle des données dans un champ donné à l’aide de la propriété **ActualSize** .  
  
-   Déterminez les types de fonctionnalités pris en charge pour un champ donné à l’aide de la propriété **attributes** et de la collection **Properties** .  
  
-   Manipuler les valeurs des champs contenant des données de type long Binary ou long Character à l’aide des méthodes **AppendChunk** et **GetChunk** .  
  
 Résolvez les incohérences dans les valeurs de champ lors de la mise à jour par lots à l’aide des propriétés **OriginalValue** et **UnderlyingValue** , si le fournisseur prend en charge les mises à jour par lots.  
  
## <a name="describing-a-field"></a>Description d’un champ  
 Les rubriques qui suivent décrivent les propriétés de l’objet de [champ](../../../ado/reference/ado-api/field-object.md) qui représentent des informations qui décrivent l’objet de **champ** lui-même, autrement dit, les métadonnées relatives au champ. Ces informations peuvent être utilisées pour déterminer une grande partie du schéma du **Recordset**. Ces propriétés sont les suivantes : **type**, **DefinedSize** , **ActualSize**, **Name** et **NumericScale** et **PRECISION**.  
  
### <a name="discovering-the-data-type"></a>Découverte du type de données  
 La propriété de **type** indique le type de données du champ. Les constantes énumérées de type de données prises en charge par ADO sont décrites dans [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) dans le *Guide de référence du programmeur ADO*.  
  
 Pour les types numériques à virgule flottante tels que **adNumeric**, vous pouvez obtenir plus d’informations. La propriété **NumericScale** indique le nombre de chiffres à droite de la virgule décimale qui sera utilisé pour représenter les valeurs du **champ**. La propriété **précision** spécifie le nombre maximal de chiffres utilisés pour représenter les valeurs du **champ**.  
  
### <a name="determining-field-size"></a>Détermination de la taille des champs  
 Utilisez la propriété **DefinedSize** pour déterminer la capacité de données d’un objet de **champ** .  
  
 Utilisez la propriété **ActualSize** pour retourner la longueur réelle de la valeur d’un objet **Field** . Pour tous les champs, la propriété **ActualSize** est en lecture seule. Si ADO ne peut pas déterminer la longueur de la valeur de l’objet **Field** , la propriété **ActualSize** retourne **adUnknown**.  
  
 Les propriétés **DefinedSize** et **ActualSize** ont des objectifs différents. Par exemple, considérez un objet de **champ** avec un type déclaré de **adVarChar** et une valeur de propriété **DefinedSize** de 50, contenant un caractère unique. La valeur de la propriété **ActualSize** renvoyée correspond à la longueur en octets du caractère unique.  
  
### <a name="determining-field-contents"></a>Détermination du contenu des champs  
 L’identificateur de la colonne de la source de données est représenté par la propriété **Name** du **champ**. La propriété **value** de l’objet **Field** retourne ou définit le contenu de données réel du champ. Il s’agit de la propriété par défaut.  
  
 Pour modifier les données d’un champ, affectez à la propriété **valeur** une nouvelle valeur du type correct. Votre type de curseur doit prendre en charge les mises à jour pour modifier le contenu d’un champ. La validation de la base de données n’est pas effectuée ici en mode batch. vous devrez donc vérifier les erreurs quand vous appelez **UpdateBatch** dans ce cas. Certains fournisseurs prennent également en charge les propriétés **UnderlyingValue** et **OriginalValue** de l’objet **Field** ADO pour vous aider à résoudre les conflits lorsque vous tentez d’effectuer des mises à jour par lots. Pour plus d’informations sur la résolution de ces conflits, consultez [modification des données](../../../ado/guide/data/editing-data.md).  
  
> [!NOTE]
>  Impossible de définir des valeurs de **champ de jeu d’enregistrements** lors de l’ajout de nouveaux **champs** à un **Recordset**. Au lieu de cela, de nouveaux **champs** peuvent être ajoutés à un **jeu d’enregistrements** fermé. Le **jeu d’enregistrements** doit être ouvert, et seules les valeurs peuvent être assignées à ces **champs**.  
  
### <a name="getting-more-field-information"></a>Obtenir plus d’informations sur les champs  
 Les objets ADO ont deux types de propriétés : intégré et dynamique. À ce stade, seules les propriétés intégrées de l’objet **Field** ont été discutées.  
  
 Les propriétés intégrées sont les propriétés implémentées dans ADO et immédiatement disponibles pour tout nouvel objet, à l’aide de la `MyObject.Property` syntaxe. Ils n’apparaissent pas en tant qu’objets de **propriété** dans la collection de **Propriétés** d’un objet.  
  
 Les propriétés dynamiques sont définies par le fournisseur de données sous-jacent et s’affichent dans la collection **Properties** pour l’objet ADO approprié. Par exemple, une propriété spécifique au fournisseur peut indiquer si un objet **Recordset** prend en charge les transactions ou la mise à jour. Ces propriétés supplémentaires s’affichent sous la forme d’objets de **propriété** dans la collection de **Propriétés** de cet objet **Recordset** . Les propriétés dynamiques peuvent être référencées uniquement par l’intermédiaire de la collection, à l’aide de la syntaxe `MyObject.Properties(0)` ou `MyObject.Properties("Name")` .  
  
 Vous ne pouvez pas supprimer un type de propriété.  
  
 Un objet de **propriété** dynamique a quatre propriétés intégrées :  
  
-   La propriété **Name** est une chaîne qui identifie la propriété.  
  
-   La propriété de **type** est un entier qui spécifie le type de données de la propriété.  
  
-   La propriété **valeur** est un variant qui contient le paramètre de propriété. La **valeur** est la propriété par défaut d’un objet de **propriété** .  
  
-   La propriété **attributes** est une valeur de **type long** qui indique les caractéristiques de la propriété spécifique au fournisseur.  
  
 La collection **Properties** de l’objet **Field** contient des métadonnées supplémentaires sur le champ. Le contenu de cette collection varie en fonction du fournisseur. L’exemple de code suivant examine la collection **Properties** de l’exemple **d’objet Recordset** présenté au début de cette section. Il examine d’abord le contenu de la collection. Ce code utilise le [fournisseur de OLE DB pour SQL Server](../../../ado/guide/appendixes/microsoft-ole-db-provider-for-sql-server.md), de sorte que la collection **Properties** contient des informations relatives à ce fournisseur.  
  
```  
'BeginFieldProps  
    Dim objProp As ADODB.Property  
  
    For intLoop = 0 To (objFields.Count - 1)  
        Debug.Print objFields.Item(intLoop).Name  
  
        For Each objProp In objFields(intLoop).Properties  
            Debug.Print vbTab & objProp.Name & " = " & objProp.Value  
        Next objProp  
    Next intLoop  
'EndFieldProps  
```  
  
### <a name="dealing-with-binary-data"></a>Traitement des données binaires  
 Utilisez la méthode [AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) sur un objet **Field** pour la remplir avec des données de type binaire ou caractère. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la méthode **AppendChunk** pour manipuler des valeurs longues dans des parties plutôt que dans leur intégralité.  
  
 Si le **bit adFldLong** dans la propriété **attributes** d’un objet de **champ** a la valeur **true**, vous pouvez utiliser la méthode **AppendChunk** pour ce champ.  
  
 Le premier appel de **AppendChunk** sur un objet de **champ** écrit des données dans le champ, en remplaçant toutes les données existantes. Les appels **AppendChunk** suivants sont ajoutés aux données existantes. Si vous ajoutez des données à un champ, puis que vous définissez ou lisez la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé d’ajouter des données au premier champ. Si vous rappelez la méthode **AppendChunk** sur le premier champ, ADO interprète l’appel comme une nouvelle opération **AppendChunk** et remplace les données existantes. L’accès aux champs d’autres objets **Recordset** qui ne sont pas des clones du premier objet **Recordset** n’interrompt pas les opérations **AppendChunk** .  
  
 Utilisez la méthode **GetChunk** sur un objet **Field** pour récupérer tout ou partie de ses données binaires ou caractères longues. Dans les situations où la mémoire système est limitée, vous pouvez utiliser la méthode **GetChunk** pour manipuler des valeurs longues en plusieurs parties, plutôt que dans leur intégralité.  
  
 Les données retournées par un appel à **GetChunk** sont affectées à une *variable*. Si la *taille* est supérieure aux données restantes, la méthode **GetChunk** retourne uniquement les données restantes sans *variable* de remplissage avec des espaces vides. Si le champ est vide, la méthode **GetChunk** retourne une valeur null.  
  
 Chaque appel **GetChunk** suivant récupère les données à partir de l’endroit où l’appel **GetChunk** précédent s’est arrêté. Toutefois, si vous extrayez des données d’un champ, puis que vous définissez ou lisez la valeur d’un autre champ dans l’enregistrement actif, ADO suppose que vous avez terminé la récupération des données du premier champ. Si vous renommez la méthode **GetChunk** sur le premier champ, ADO interprète l’appel comme une nouvelle opération **GetChunk** et commence la lecture à partir du début des données. L’accès aux champs d’autres objets **Recordset** qui ne sont pas des clones du premier objet **Recordset** n’interrompt pas les opérations **GetChunk** .  
  
 Si le **bit adFldLong** dans la propriété **attributes** d’un objet de **champ** a la valeur **true**, vous pouvez utiliser la méthode **GetChunk** pour ce champ.  
  
 S’il n’y a pas d’enregistrement actif lorsque vous utilisez la méthode **GetChunk** ou **AppendChunk** sur un objet de **champ** , l’erreur 3021 (aucun enregistrement en cours) se produit.  
  
 Pour obtenir un exemple d’utilisation de ces méthodes pour manipuler des données binaires, consultez la [méthode AppendChunk](../../../ado/reference/ado-api/appendchunk-method-ado.md) et les exemples de [méthodes GetChunk](../../../ado/reference/ado-api/getchunk-method-ado.md) dans le *Guide de référence du programmeur ADO*.
