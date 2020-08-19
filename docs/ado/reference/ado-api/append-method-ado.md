---
description: Append, méthode (ADO)
title: Append, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: dd3ee907aa2a7081ca7204dcc1b0b3b069581832
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88451261"
---
# <a name="append-method-ado"></a>Append, méthode (ADO)
Ajoute un objet à une collection. Si la collection est de [champs](../../../ado/reference/ado-api/fields-collection-ado.md), un nouvel objet de [champ](../../../ado/reference/ado-api/field-object.md) peut être créé avant d’être ajouté à la collection.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
collection.Append object  
fields.Append Name, Type, DefinedSize, Attrib, FieldValue  
```  
  
#### <a name="parameters"></a>Paramètres  
 *collecte*  
 Objet de collection.  
  
 *fields*  
 Collection de **champs** .  
  
 *object*  
 Variable objet qui représente l’objet à ajouter.  
  
 *Nom*  
 Valeur de **chaîne** qui contient le nom du nouvel objet de **champ** et qui ne doit pas être le même nom que tout autre objet dans les *champs*.  
  
 *Type*  
 Valeur [DataTypeEnum](../../../ado/reference/ado-api/datatypeenum.md) , dont la valeur par défaut est **adEmpty**, qui spécifie le type de données du nouveau champ. Les types de données suivants ne sont pas pris en charge par ADO et ne doivent pas être utilisés lors de l’ajout de nouveaux champs à un [objet Recordset (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md): **adIDispatch**, **adIUnknown**, **adVariant**.  
  
 *DefinedSize*  
 facultatif. Valeur de **type long** qui représente la taille définie, en caractères ou en octets, du nouveau champ. La valeur par défaut de ce paramètre est dérivée du *type*. Les champs qui ont une valeur *DefinedSize* supérieure à 255 octets sont traités comme des colonnes de longueur variable. La valeur par défaut de *DefinedSize* n’est pas spécifiée.  
  
 *Attrib*  
 facultatif. Valeur [FieldAttributeEnum](../../../ado/reference/ado-api/fieldattributeenum.md) , dont la valeur par défaut est **adFldDefault**, qui spécifie les attributs du nouveau champ. Si vous ne spécifiez pas cette valeur, le champ contient des attributs dérivés du *type*.  
  
 *FieldValue*  
 facultatif. **Variant** qui représente la valeur du nouveau champ. S’il n’est pas spécifié, le champ est ajouté avec une valeur null.  
  
## <a name="remarks"></a>Notes  
  
## <a name="parameters-collection"></a>Collection Parameters  
 Vous devez définir la propriété [type](../../../ado/reference/ado-api/type-property-ado.md) d’un objet [Parameter](../../../ado/reference/ado-api/parameter-object.md) avant de l’ajouter à la collection [Parameters](../../../ado/reference/ado-api/parameters-collection-ado.md) . Si vous sélectionnez un type de données de longueur variable, vous devez également affecter à la propriété [Size](../../../ado/reference/ado-api/size-property-ado-parameter.md) une valeur supérieure à zéro.  
  
 La description des paramètres vous permet de réduire les appels au fournisseur et, par conséquent, d’améliorer les performances lorsque vous utilisez des procédures stockées ou des requêtes paramétrables. Toutefois, vous devez connaître les propriétés des paramètres associés à la procédure stockée ou à la requête paramétrable que vous souhaitez appeler.  
  
 Utilisez la méthode [CreateParameter](../../../ado/reference/ado-api/createparameter-method-ado.md) pour créer des objets **Parameter** avec les paramètres de propriété appropriés et utilisez la méthode **Append** pour les ajouter à la collection [Parameters](../../../ado/reference/ado-api/parameters-collection-ado.md) . Cela vous permet de définir et de retourner des valeurs de paramètres sans avoir à appeler le fournisseur pour obtenir les informations sur les paramètres. Si vous écrivez dans un fournisseur qui ne fournit pas d’informations de paramètre, vous devez utiliser cette méthode pour remplir manuellement la collection de **paramètres** afin d’utiliser des paramètres.  
  
## <a name="fields-collection"></a>Collection Fields  
 Le paramètre *FieldValue* est valide uniquement lors de l’ajout d’un objet **champ** à un objet [Record](../../../ado/reference/ado-api/record-object-ado.md) et non à un objet **Recordset** . Avec un objet **Record** , vous pouvez ajouter des champs et fournir des valeurs en même temps. Avec un objet **Recordset** , vous devez créer des champs lors de la fermeture du **Recordset** , puis ouvrir le **jeu d’enregistrements** et affecter des valeurs aux champs.  
  
> [!NOTE]
>  Pour les nouveaux objets **Field** qui ont été ajoutés à la collection **Fields** d’un objet **Record** , la propriété [value](../../../ado/reference/ado-api/value-property-ado.md) doit être définie avant que toutes les autres propriétés de **champ** puissent être spécifiées. Tout d’abord, une valeur spécifique pour la propriété **value** doit avoir été assignée et [mise à jour](../../../ado/reference/ado-api/update-method.md) sur la collection **Fields** appelée. Ensuite, d’autres propriétés telles que le [type](../../../ado/reference/ado-api/type-property-ado.md) ou les [attributs](../../../ado/reference/ado-api/attributes-property-ado.md) sont accessibles. Les objets de **champ** des types de données suivants (**DataTypeEnum**) ne peuvent pas être ajoutés à la collection de **champs** et provoquent une erreur : **adArray**, **adChapter**, **adEmpty**, **adPropVariant**et **adUserDefined**. En outre, les types de données suivants ne sont pas pris en charge par ADO : **adIDispatch**, **adIUnknown**et **adIVariant**. Pour ces types, aucune erreur ne se produit lorsqu’il est ajouté, mais l’utilisation peut produire des résultats imprévisibles, notamment des fuites de mémoire.  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Si vous ne définissez pas la [propriété CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) avant d’appeler la méthode **Append** , **CursorLocation** prend automatiquement la **valeur adUseClient** (une valeur [CursorLocationEnum](../../../ado/reference/ado-api/cursorlocationenum.md) ) lorsque la méthode [Open](../../../ado/reference/ado-api/open-method-ado-recordset.md) de l’objet [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md) est appelée.  
  
 Une erreur d’exécution se produit si la méthode **Append** est appelée sur la collection **Fields** d’un **Recordset**ouvert ou sur un **Recordset** dans lequel la propriété [ActiveConnection](../../../ado/reference/ado-api/activeconnection-property-ado.md) a été définie. Vous pouvez uniquement ajouter des champs à un **jeu d’enregistrements** qui n’est pas ouvert et qui n’a pas encore été connecté à une source de données. C’est généralement le cas lorsqu’un objet **Recordset** est fabriqué avec la méthode [CreateRecordset](../../../ado/reference/rds-api/createrecordset-method-rds.md) ou assigné à une variable objet.  
  
## <a name="record"></a>Enregistrement  
 Une erreur au moment de l’exécution ne se produit pas si la méthode **Append** est appelée sur la collection **Fields** d’un **enregistrement**ouvert. Le nouveau champ est ajouté à la collection **Fields** de l’objet **Record** . Si l' **enregistrement** est dérivé d’un **Recordset**, le nouveau champ n’apparaît pas dans la collection de **champs** de l’objet **Recordset** .  
  
 Un champ inexistant peut être créé et ajouté à la collection de **champs** en affectant une valeur à l’objet de champ comme s’il existait déjà dans la collection. L’assignation déclenchera la création et l’ajout automatiques de l’objet **Field** , puis l’affectation sera terminée.  
  
 Après avoir ajouté un **champ** à la collection **Fields** d’un objet **Record** , appelez la méthode **Update** de la collection **Fields** pour enregistrer la modification.  
  
## <a name="applies-to"></a>S'applique à  
  
- [Fields, collection (ADO)](../../../ado/reference/ado-api/fields-collection-ado.md)  
- [Parameters, collection (ADO)](../../../ado/reference/ado-api/parameters-collection-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append et CreateParameter, exemple de méthodes (VB)](../../../ado/reference/ado-api/append-and-createparameter-methods-example-vb.md)   
 [Append et CreateParameter, exemples de méthodes (VC + +)](../../../ado/reference/ado-api/append-and-createparameter-methods-example-vc.md)   
 [CreateParameter, méthode (ADO)](../../../ado/reference/ado-api/createparameter-method-ado.md)   
 [Delete, méthode (collection Fields ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Delete, méthode (collection Parameters ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [Delete, méthode (objet Recordset ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [Update, méthode](../../../ado/reference/ado-api/update-method.md)
