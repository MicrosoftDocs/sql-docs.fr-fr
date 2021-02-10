---
description: Utilisation des extensions Visual C++
title: Utilisation des extensions de Visual C++ | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- Visual C++ [ADO], using VC++ extensions
- ADO, Visual C++
ms.assetid: ff759185-df41-4507-8d12-0921894ffbd9
author: rothja
ms.author: jroth
ms.openlocfilehash: b5acd1b06bbaad8476ac48261ba49a6ca90cb0b5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028654"
---
# <a name="visual-c-extensions"></a>Extensions de Visual C++
## <a name="the-iadorecordbinding-interface"></a>Interface IADORecordBinding
 Les extensions Microsoft Visual C++ pour ADO associent, ou lient, des champs d’un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à des variables C/C++. Chaque fois que la ligne actuelle de l’ensemble d' **enregistrements** lié change, tous les champs liés dans le **Recordset** sont copiés dans les variables C/C++. Si nécessaire, les données copiées sont converties dans le type de données déclaré de la variable C/C++.

 La méthode **BindToRecordset** de l’interface **IADORecordBinding** lie les champs aux variables C/C++. La méthode **AddNew** ajoute une nouvelle ligne au **Recordset** lié. La méthode **Update** remplit les champs dans les nouvelles lignes de l’ensemble d' **enregistrements** ou met à jour les champs des lignes existantes, avec la valeur des variables C/C++.

 L’interface **IADORecordBinding** est implémentée par l’objet **Recordset** . Vous ne codez pas l’implémentation vous-même.

## <a name="binding-entries"></a>Entrées de liaison
 Les extensions Visual C++ pour ADO mappent les champs d’un objet [Recordset](../../reference/ado-api/recordset-object-ado.md) à des variables C/C++. La définition d’un mappage entre un champ et une variable est appelée une *entrée de liaison*. Les macros fournissent des entrées de liaison pour des données numériques, de longueur fixe et de longueur variable. Les entrées de liaison et les variables C/C++ sont déclarées dans une classe dérivée de la classe d’extensions Visual C++, **CADORecordBinding**. La classe **CADORecordBinding** est définie en interne par les macros d’entrée de liaison.

 ADO mappe en interne les paramètres de ces macros à une OLE DB structure **DBBINDING** et crée un objet **accesseur** OLE DB pour gérer le déplacement et la conversion des données entre les champs et les variables. OLE DB définit des données telles qu’elles sont constituées de trois parties : une *mémoire tampon* dans laquelle les données sont stockées ; *État* qui indique si un champ a été correctement stocké dans la mémoire tampon ou comment la variable doit être restaurée dans le champ ; et la *longueur* des données. (Pour plus d’informations, consultez [obtention et définition des données (OLE DB)](/previous-versions/windows/desktop/ms713700(v=vs.85))dans le Guide de référence du programmeur OLE DB.)

## <a name="header-file"></a>Fichier d'en-tête
 Incluez le fichier suivant dans votre application afin d’utiliser les extensions de Visual C++ pour ADO :

```cpp
#include <icrsint.h>
```

## <a name="binding-recordset-fields"></a>Liaison de champs de Recordset

#### <a name="to-bind-recordset-fields-to-cc-variables"></a>Pour lier des champs Recordset à des variables C/C++

1.  Créez une classe dérivée de la classe **CADORecordBinding** .

2.  Spécifiez les entrées de liaison et les variables C/C++ correspondantes dans la classe dérivée. Mettre entre crochets les entrées de liaison entre les macros de **BEGIN_ADO_BINDING** et **END_ADO_BINDING** . Ne mettez pas fin aux macros avec des virgules ou des points-virgules. Les délimiteurs appropriés sont spécifiés automatiquement par chaque macro.

     Spécifiez une entrée de liaison pour chaque champ à mapper à une variable C/C++. Utilisez un membre approprié à partir de la famille de macros **ADO_FIXED_LENGTH_ENTRY**, **ADO_NUMERIC_ENTRY** ou **ADO_VARIABLE_LENGTH_ENTRY** .

3.  Dans votre application, créez une instance de la classe dérivée de **CADORecordBinding**. Obtient l’interface **IADORecordBinding** à partir du **Recordset**. Appelez ensuite la méthode **BindToRecordset** pour lier les champs du **Recordset** aux variables C/C++.

 Pour plus d’informations, consultez l' [exemple Visual C++ extensions](./visual-c-extensions-example.md).

## <a name="interface-methods"></a>Méthodes d'interface
 L’interface **IADORecordBinding** a trois méthodes : **BindToRecordset**, **AddNew** et **Update**. Le seul argument de chaque méthode est un pointeur vers une instance de la classe dérivée de **CADORecordBinding**. Par conséquent, les méthodes **AddNew** et **Update** ne peuvent pas spécifier l’un des paramètres de leur méthode ADO namesakes.

## <a name="syntax"></a>Syntaxe
 La méthode **BindToRecordset** associe les champs du **Recordset** à des variables C/C++.

```cpp
BindToRecordset(CADORecordBinding *binding)
```

 La méthode **AddNew** appelle son Namesake, la méthode ADO [AddNew](../../reference/ado-api/addnew-method-ado.md) , pour ajouter une nouvelle ligne au **Recordset**.

```cpp
AddNew(CADORecordBinding *binding)
```

 La méthode **Update** appelle son Namesake, la méthode ADO [Update](../../reference/ado-api/update-method.md) , pour mettre à jour le **Recordset**.

```cpp
Update(CADORecordBinding *binding)
```

## <a name="binding-entry-macros"></a>Macros d’entrée de liaison
 Les macros d’entrée de liaison définissent l’Association d’un champ de **Recordset** et d’une variable. Une macro de début et de fin délimite le jeu d’entrées de liaison.

 Des familles de macros sont fournies pour les données de longueur fixe, telles que **adDate** ou **adBoolean**; données numériques, telles que **adTinyInt**, **adInteger** ou **adDouble**; et les données de longueur variable, telles que **adChar**, **adVarChar** ou **adVarBinary**. Tous les types numériques, à l’exception de **adVarNumeric**, sont également des types de longueur fixe. Chaque famille a des jeux de paramètres différents, ce qui vous permet d’exclure les informations de liaison qui ne sont pas intéressantes.

 Pour plus d’informations, consultez [annexe A : types de données](/previous-versions/windows/desktop/ms723969(v=vs.85))de la OLE DB Guide de référence du programmeur.

### <a name="begin-binding-entries"></a>Début des entrées de liaison
 **BEGIN_ADO_BINDING**(*classe*)

### <a name="fixed-length-data"></a>Données Fixed-Length
 **ADO_FIXED_LENGTH_ENTRY**(*ordinal, type de données, mémoire tampon, État, modifier*)

 **ADO_FIXED_LENGTH_ENTRY2**(*ordinal, type de données, mémoire tampon, modifier*)

### <a name="numeric-data"></a>Données numériques
 **ADO_NUMERIC_ENTRY**(*ordinal, type de données, mémoire tampon, précision, échelle, État, modifier*)

 **ADO_NUMERIC_ENTRY2**(*ordinal, type de données, mémoire tampon, précision, échelle, modifier*)

### <a name="variable-length-data"></a>Données Variable-Length
 **ADO_VARIABLE_LENGTH_ENTRY**(*ordinal, type de données, mémoire tampon, taille, État, longueur, modifier*)

 **ADO_VARIABLE_LENGTH_ENTRY2**(*ordinal, type de données, mémoire tampon, taille, État, modifier*)

 **ADO_VARIABLE_LENGTH_ENTRY3**(*ordinal, type de données, mémoire tampon, taille, longueur, modifier*)

 **ADO_VARIABLE_LENGTH_ENTRY4**(*ordinal, type de données, mémoire tampon, taille, modifier*)

### <a name="end-binding-entries"></a>Terminer les entrées de liaison
 **END_ADO_BINDING**()

|Paramètre|Description|
|---------------|-----------------|
|*Classe*|Classe dans laquelle les entrées de liaison et les variables C/C++ sont définies.|
|*Ordinal*|Nombre ordinal, en comptant de un, du champ **Recordset** correspondant à votre variable C/C++.|
|*DataType*|Type de données ADO équivalent de la variable C/C++ (consultez [DataTypeEnum](../../reference/ado-api/datatypeenum.md) pour obtenir la liste des types de données valides). La valeur du champ **Recordset** est convertie dans ce type de données si nécessaire.|
|*Buffer*|Nom de la variable C/C++ dans laquelle le champ du **Recordset** sera stocké.|
|*Taille*|Taille maximale, en octets, de la *mémoire tampon*. Si *buffer* contient une chaîne de longueur variable, autorisez l’espace pour un zéro de fin.|
|*État*|Nom d’une variable qui indique si le contenu de la *mémoire tampon* est valide et si la conversion du champ en *type de données* a réussi.<br /><br /> Les deux valeurs les plus importantes pour cette variable sont **adFldOK**, ce qui signifie que la conversion a réussi ; et **adFldNull**, ce qui signifie que la valeur du champ serait un variant de type VT_NULL et non simplement vide.<br /><br /> Les valeurs possibles de l' *État* sont répertoriées dans le tableau suivant, « valeurs d’État ».|
|*Modify*|Indicateur booléen ; Si la valeur est TRUE, le champ ADO est autorisé à mettre à jour le champ **Recordset** correspondant avec la valeur contenue dans *buffer*.<br /><br /> Définissez le paramètre de *modification* BOOLÉENNE sur true pour permettre à ADO de mettre à jour le champ lié, et false si vous souhaitez examiner le champ, mais pas le modifier.|
|*Précision*|Nombre de chiffres qui peuvent être représentés dans une variable numérique.|
|*Mettre à l'échelle*|Nombre de décimales dans une variable numérique.|
|*Durée*|Nom d’une variable de 4 octets qui doit contenir la longueur réelle des données dans la *mémoire tampon*.|

## <a name="status-values"></a>Valeurs d'état
 La valeur de la variable d' *État* indique si un champ a été correctement copié dans une variable.

 Lors de la définition des données, l' *État* peut être défini sur **adFldNull** pour indiquer que le champ **Recordset** doit avoir la valeur null.

|Constante|Valeur|Description|
|--------------|-----------|-----------------|
|**adFldOK**|0|Une valeur de champ non null a été retournée.|
|**adFldBadAccessor**|1|La liaison n’est pas valide.|
|**adFldCantConvertValue**|2|La valeur n’a pas pu être convertie pour des raisons autres que l’incompatibilité de signe ou le dépassement de données.|
|**adFldNull**|3|Lors de l’obtention d’un champ, indique qu’une valeur null a été retournée.<br /><br /> Lors de la définition d’un champ, indique que le champ doit être défini sur **null** lorsque le champ ne peut pas encoder **null** lui-même (par exemple, un tableau de caractères ou un entier).|
|**adFldTruncated**|4|Les données de longueur variable ou les chiffres numériques ont été tronqués.|
|**adFldSignMismatch**|5|La valeur est signée et le type de données de la variable n’est pas signé.|
|**adFldDataOverFlow**|6|La valeur est supérieure à celle qui peut être stockée dans le type de données de la variable.|
|**adFldCantCreate**|7|Type de colonne inconnu et champ déjà ouvert.|
|**adFldUnavailable**|8|Impossible de déterminer la valeur du champ, par exemple, sur un nouveau champ non assigné sans valeur par défaut.|
|**adFldPermissionDenied**|9|Lors de la mise à jour, aucune autorisation d’écriture de données.|
|**adFldIntegrityViolation**|10|Lors de la mise à jour, la valeur du champ viole l’intégrité de la colonne.|
|**adFldSchemaViolation**|11|Lors de la mise à jour, la valeur de champ viole le schéma de colonne.|
|**adFldBadStatus**|12|Lors de la mise à jour, paramètre d’État non valide.|
|**adFldDefault**|13|Lors de la mise à jour, une valeur par défaut a été utilisée.|

## <a name="see-also"></a>Voir aussi
 [Exemple](./visual-c-extensions-example.md) d’extensions de Visual C++ [Visual C++ en-tête d’extensions](./visual-c-extensions-header.md)