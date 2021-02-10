---
description: Append, méthode (ADO)
title: Append, méthode (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _DynaCollection::Append
helpviewer_keywords:
- Append method [ADO]
ms.assetid: f8a9bbed-ba9c-4698-945d-317ad22d2e92
author: rothja
ms.author: jroth
ms.openlocfilehash: 099cce7da61fbfa9f3a70904a6c8593b9be4d304
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027855"
---
# <a name="append-method-ado"></a>Append, méthode (ADO)
Ajoute un objet à une collection. Si la collection est de [champs](./fields-collection-ado.md), un nouvel objet de [champ](./field-object.md) peut être créé avant d’être ajouté à la collection.  
  
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
 Valeur [DataTypeEnum](./datatypeenum.md) , dont la valeur par défaut est **adEmpty**, qui spécifie le type de données du nouveau champ. Les types de données suivants ne sont pas pris en charge par ADO et ne doivent pas être utilisés lors de l’ajout de nouveaux champs à un [objet Recordset (ADO)](./recordset-object-ado.md): **adIDispatch**, **adIUnknown**, **adVariant**.  
  
 *DefinedSize*  
 facultatif. Valeur de **type long** qui représente la taille définie, en caractères ou en octets, du nouveau champ. La valeur par défaut de ce paramètre est dérivée du *type*. Les champs qui ont une valeur *DefinedSize* supérieure à 255 octets sont traités comme des colonnes de longueur variable. La valeur par défaut de *DefinedSize* n’est pas spécifiée.  
  
 *Attrib*  
 facultatif. Valeur [FieldAttributeEnum](./fieldattributeenum.md) , dont la valeur par défaut est **adFldDefault**, qui spécifie les attributs du nouveau champ. Si vous ne spécifiez pas cette valeur, le champ contient des attributs dérivés du *type*.  
  
 *FieldValue*  
 facultatif. **Variant** qui représente la valeur du nouveau champ. S’il n’est pas spécifié, le champ est ajouté avec une valeur null.  
  
## <a name="remarks"></a>Notes  
  
## <a name="parameters-collection"></a>Collection Parameters  
 Vous devez définir la propriété [type](./type-property-ado.md) d’un objet [Parameter](./parameter-object.md) avant de l’ajouter à la collection [Parameters](./parameters-collection-ado.md) . Si vous sélectionnez un type de données de longueur variable, vous devez également affecter à la propriété [Size](./size-property-ado-parameter.md) une valeur supérieure à zéro.  
  
 La description des paramètres vous permet de réduire les appels au fournisseur et, par conséquent, d’améliorer les performances lorsque vous utilisez des procédures stockées ou des requêtes paramétrables. Toutefois, vous devez connaître les propriétés des paramètres associés à la procédure stockée ou à la requête paramétrable que vous souhaitez appeler.  
  
 Utilisez la méthode [CreateParameter](./createparameter-method-ado.md) pour créer des objets **Parameter** avec les paramètres de propriété appropriés et utilisez la méthode **Append** pour les ajouter à la collection [Parameters](./parameters-collection-ado.md) . Cela vous permet de définir et de retourner des valeurs de paramètres sans avoir à appeler le fournisseur pour obtenir les informations sur les paramètres. Si vous écrivez dans un fournisseur qui ne fournit pas d’informations de paramètre, vous devez utiliser cette méthode pour remplir manuellement la collection de **paramètres** afin d’utiliser des paramètres.  
  
## <a name="fields-collection"></a>Collection Fields  
 Le paramètre *FieldValue* est valide uniquement lors de l’ajout d’un objet **champ** à un objet [Record](./record-object-ado.md) et non à un objet **Recordset** . Avec un objet **Record** , vous pouvez ajouter des champs et fournir des valeurs en même temps. Avec un objet **Recordset** , vous devez créer des champs lors de la fermeture du **Recordset** , puis ouvrir le **jeu d’enregistrements** et affecter des valeurs aux champs.  
  
> [!NOTE]
>  Pour les nouveaux objets **Field** qui ont été ajoutés à la collection **Fields** d’un objet **Record** , la propriété [value](./value-property-ado.md) doit être définie avant que toutes les autres propriétés de **champ** puissent être spécifiées. Tout d’abord, une valeur spécifique pour la propriété **value** doit avoir été assignée et [mise à jour](./update-method.md) sur la collection **Fields** appelée. Ensuite, d’autres propriétés telles que le [type](./type-property-ado.md) ou les [attributs](./attributes-property-ado.md) sont accessibles. Les objets de **champ** des types de données suivants (**DataTypeEnum**) ne peuvent pas être ajoutés à la collection de **champs** et provoquent une erreur : **adArray**, **adChapter**, **adEmpty**, **adPropVariant** et **adUserDefined**. En outre, les types de données suivants ne sont pas pris en charge par ADO : **adIDispatch**, **adIUnknown** et **adIVariant**. Pour ces types, aucune erreur ne se produit lorsqu’il est ajouté, mais l’utilisation peut produire des résultats imprévisibles, notamment des fuites de mémoire.  
  
## <a name="recordset"></a>Ensemble d'enregistrements  
 Si vous ne définissez pas la [propriété CursorLocation](./cursorlocation-property-ado.md) avant d’appeler la méthode **Append** , **CursorLocation** prend automatiquement la **valeur adUseClient** (une valeur [CursorLocationEnum](./cursorlocationenum.md) ) lorsque la méthode [Open](./open-method-ado-recordset.md) de l’objet [Recordset](./recordset-object-ado.md) est appelée.  
  
 Une erreur d’exécution se produit si la méthode **Append** est appelée sur la collection **Fields** d’un **Recordset** ouvert ou sur un **Recordset** dans lequel la propriété [ActiveConnection](./activeconnection-property-ado.md) a été définie. Vous pouvez uniquement ajouter des champs à un **jeu d’enregistrements** qui n’est pas ouvert et qui n’a pas encore été connecté à une source de données. C’est généralement le cas lorsqu’un objet **Recordset** est fabriqué avec la méthode [CreateRecordset](../rds-api/createrecordset-method-rds.md) ou assigné à une variable objet.  
  
## <a name="record"></a>Enregistrer  
 Une erreur au moment de l’exécution ne se produit pas si la méthode **Append** est appelée sur la collection **Fields** d’un **enregistrement** ouvert. Le nouveau champ est ajouté à la collection **Fields** de l’objet **Record** . Si l' **enregistrement** est dérivé d’un **Recordset**, le nouveau champ n’apparaît pas dans la collection de **champs** de l’objet **Recordset** .  
  
 Un champ inexistant peut être créé et ajouté à la collection de **champs** en affectant une valeur à l’objet de champ comme s’il existait déjà dans la collection. L’assignation déclenchera la création et l’ajout automatiques de l’objet **Field** , puis l’affectation sera terminée.  
  
 Après avoir ajouté un **champ** à la collection **Fields** d’un objet **Record** , appelez la méthode **Update** de la collection **Fields** pour enregistrer la modification.  
  
## <a name="applies-to"></a>S'applique à  
  
- [Fields, collection (ADO)](./fields-collection-ado.md)  
- [Parameters, collection (ADO)](./parameters-collection-ado.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Append et CreateParameter, exemple de méthodes (VB)](./append-and-createparameter-methods-example-vb.md)   
 [Append et CreateParameter, exemples de méthodes (VC + +)](./append-and-createparameter-methods-example-vc.md)   
 [CreateParameter, méthode (ADO)](./createparameter-method-ado.md)   
 [Delete, méthode (collection Fields ADO)](./delete-method-ado-fields-collection.md)   
 [Delete, méthode (collection Parameters ADO)](./delete-method-ado-parameters-collection.md)   
 [Delete, méthode (objet Recordset ADO)](./delete-method-ado-recordset.md)   
 [Update, méthode](./update-method.md)