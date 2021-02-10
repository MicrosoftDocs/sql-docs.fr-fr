---
description: Programmation ADO Visual C++
title: Visual C++ la programmation ADO | Microsoft Docs
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
- ADO, Visual C++
- Visual C++ [ADO]
ms.assetid: 11233b96-e05c-4221-9aed-5f20944b0f1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 420bfbba2eaa3946ce9a373c4ac31a88b540f7d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028704"
---
# <a name="visual-c-ado-programming"></a>Programmation ADO Visual C++
Les informations de référence sur l’API ADO décrivent les fonctionnalités de l’interface de programmation d’applications (API) ADO à l’aide d’une syntaxe similaire à celle de Microsoft Visual Basic. Bien que le public visé soit l’un de tous les utilisateurs, les programmeurs ADO utilisent différents langages, tels que Visual Basic, Visual C++ (avec et sans la directive **#import** ) et Visual J++ (avec le package de classe ADO/WFC).  

> [!NOTE]
> Microsoft a terminé la prise en charge de Visual J++ dans 2004.

 Pour tenir compte de cette diversité, les [index de syntaxe ADO for Visual C++](./using-ado-with-microsoft-visual-c.md) fournissent Visual C++ syntaxe spécifique au langage avec des liens vers des descriptions courantes de fonctionnalités, de paramètres, de comportements exceptionnels, etc., dans la référence de l’API.  
  
 ADO est implémenté avec les interfaces COM (Component Object Model). Toutefois, il est plus facile pour les programmeurs de travailler avec COM dans certains langages de programmation que d’autres. Par exemple, presque tous les détails de l’utilisation de COM sont gérés implicitement pour les programmeurs de Visual Basic, tandis que Visual C++ les programmeurs doivent assister à ces détails eux-mêmes.  
  
 Les sections suivantes résument les détails des programmeurs C et C++ à l’aide d’ADO et de la directive **#import** . Elle se concentre sur les types de données spécifiques à COM (**Variant**, **BSTR** et **SAFEARRAY**) et la gestion des erreurs (_com_error).  
  
## <a name="using-the-import-compiler-directive"></a>Utilisation de la directive de compilateur #import  
 La directive du compilateur **#import** Visual C++ simplifie l’utilisation des méthodes et des propriétés ADO. La directive prend le nom d’un fichier qui contient une bibliothèque de types, comme ADO. dll (Msado15.dll), et génère des fichiers d’en-tête contenant des déclarations typedef, des pointeurs intelligents pour les interfaces et des constantes énumérées. Chaque interface est encapsulée, ou encapsulée, dans une classe.  
  
 Pour chaque opération dans une classe (autrement dit, un appel de méthode ou de propriété), il existe une déclaration permettant d’appeler l’opération directement (c’est-à-dire la forme « brute » de l’opération), ainsi qu’une déclaration d’appel de l’opération brute et de lever une erreur COM si l’exécution de l’opération a échoué. Si l’opération est une propriété, il existe généralement une directive de compilateur qui crée une syntaxe alternative pour l’opération dont la syntaxe est comme Visual Basic.  
  
 Les opérations qui récupèrent la valeur d’une propriété ont des noms sous la forme, **obtenir** la _propriété_. Les opérations qui définissent la valeur d’une propriété ont des noms de la forme, **put**_Property_. Les opérations qui définissent la valeur d’une propriété avec un pointeur vers un objet ADO ont des noms au format de la _propriété_ **PutRef**.  
  
 Vous pouvez obtenir ou définir une propriété avec les appels de ces formes :  
  
```cpp
variable = objectPtr->GetProperty(); // get property value   
objectPtr->PutProperty(value);       // set property value  
objectPtr->PutRefProperty(&value);   // set property with object pointer  
```
  
## <a name="using-property-directives"></a>Utilisation des directives de propriété  
 La directive de compilateur **__declspec (Property...)** est une extension de langage C spécifique à Microsoft qui déclare une fonction utilisée comme propriété pour avoir une syntaxe alternative. Par conséquent, vous pouvez définir ou obtenir les valeurs d’une propriété d’une manière similaire à Visual Basic. Par exemple, vous pouvez définir et obtenir une propriété de cette façon :  
  
```cpp
objectPtr->property = value;        // set property value  
variable = objectPtr->property;     // get property value  
```
  
 Notez que vous n’avez pas besoin de coder :  
  
```cpp
objectPtr->PutProperty(value);      // set property value  
variable = objectPtr->GetProperty;  // get property value  
```
  
 Le compilateur génère l’appel de  _-_ _propriété_ obtenir, **put** ou **PutRef** approprié en fonction de la syntaxe de remplacement déclarée et de la lecture ou de l’écriture de la propriété.  
  
 La directive de compilateur **__declspec (Property...)** peut uniquement déclarer une syntaxe d' **extraction** **, d'** extraction ou d' **extraction** et de **placement** pour une fonction. Les opérations en lecture seule ont uniquement une déclaration d' **extraction** ; les opérations en écriture seule ont uniquement une déclaration **put** . les opérations qui sont à la fois en lecture et en écriture ont **des déclarations d’extraction et d'** **extraction** .  
  
 Seules deux déclarations sont possibles avec cette directive. Toutefois, chaque propriété peut avoir trois fonctions de propriété : **obtenir** la _propriété_, **Placer**_la propriété et__propriété_ **PutRef**. Dans ce cas, seules deux formes de la propriété ont la syntaxe alternative.  
  
 Par exemple, la propriété **ActiveConnection** de l’objet de **commande** est déclarée avec une syntaxe alternative pour **obtenir**_ActiveConnection_ et **PutRef**_ActiveConnection_. La syntaxe **PutRef** est un bon choix, car dans la pratique, vous souhaiterez généralement placer un objet de **connexion** ouvert (autrement dit, un pointeur d’objet de **connexion** ) dans cette propriété. D’un autre côté, l’objet **Recordset** contient des opérations d' **extraction**, **put** et **PutRef**_ActiveConnection_ , mais aucune autre syntaxe.  
  
## <a name="collections-the-getitem-method-and-the-item-property"></a>Collections, méthode GetItem et propriété Item  

 ADO définit plusieurs collections, y compris les **champs**, les **paramètres**, les **Propriétés** et les **Erreurs**. Dans Visual C++, la méthode **GetItem (_index_)** retourne un membre de la collection. *Index* est un **Variant**, dont la valeur est un index numérique du membre de la collection ou une chaîne contenant le nom du membre.  
  
 La directive du compilateur **__declspec (Property...)** déclare la propriété **Item** comme une autre syntaxe pour la méthode fondamentale **GetItem ()** de chaque collection. La syntaxe alternative utilise des crochets et ressemble à une référence de tableau. En général, les deux formulaires ressemblent à ce qui suit :  
  
```cpp
  
      collectionPtr->GetItem(index);  
collectionPtr->Item[index];  
```
  
 Par exemple, affectez une valeur à un champ d’un objet **Recordset** , nommé **_RS_**, dérivé de la table **Authors** de la base de données **pubs** . Utilisez la propriété **Item ()** pour accéder au troisième **champ** de la collection de **champs** de l’objet **Recordset** (les collections sont indexées à partir de zéro ; Supposons que le troisième champ est nommé au- **_\_ fname_**). Appelez ensuite la méthode **value ()** sur l’objet de **champ** pour assigner une valeur de chaîne.  
  
 Cela peut être exprimé en Visual Basic des quatre façons suivantes (les deux dernières formes sont uniques à Visual Basic ; les autres langages n’ont pas d’équivalents) :  
  
```cpp
rs.Fields.Item(2).Value = "value"  
rs.Fields.Item("au_fname").Value = "value"  
rs(2) = "value"  
rs!au_fname = "value"  
```
  
 L’équivalent dans Visual C++ aux deux premières formes ci-dessus est :  
  
```cpp
rs->Fields->GetItem(long(2))->PutValue("value");   
rs->Fields->GetItem("au_fname")->PutValue("value");  
```
  
 -ou-(la syntaxe alternative pour la propriété **value** est également indiquée)  
  
```cpp
rs->Fields->Item[long(2)]->Value = "value";  
rs->Fields->Item["au_fname"]->Value = "value";  
```
  
 Pour obtenir des exemples d’itération au sein d’une collection, consultez la section « collections ADO » de « référence ADO ».  
  
## <a name="com-specific-data-types"></a>Types de données COM-Specific  
 En général, tout Visual Basic type de données que vous trouvez dans la référence de l’API ADO a un Visual C++ équivalent. Celles-ci incluent des types de données standard tels que **unsigned char** pour un **octet** Visual Basic, **short** pour **Integer** et **long** pour **long**. Examinez la syntaxe Indexesto pour voir exactement ce qui est requis pour les opérandes d’une méthode ou d’une propriété donnée.  
  
 Les exceptions à cette règle sont les types de données spécifiques à COM : **Variant**, **BSTR** et **SAFEARRAY**.  
  
### <a name="variant"></a>Variante  
 Un **Variant** est un type de données structurées qui contient un membre de valeur et un membre de type de données. Une **variante** peut contenir un large éventail d’autres types de données, notamment un autre variant, BSTR, Boolean, IDispatch ou IUnknown, Currency, date, etc. COM fournit également des méthodes qui facilitent la conversion d’un type de données en un autre.  
  
 La classe **_variant_t** encapsule et gère le type de données **Variant** .  
  
 Lorsque la référence de l’API ADO indique qu’une méthode ou un opérande de propriété prend une valeur, cela signifie généralement que la valeur est passée dans un **_variant_t**.  
  
 Cette règle est explicitement vraie lorsque la section des **paramètres** dans les rubriques de la référence de l’API ADO indique qu’un opérande est un **Variant**. Une exception est quand la documentation indique explicitement que l’opérande accepte un type de données standard, tel que **long** ou **Byte**, ou une énumération. Une autre exception est lorsque l’opérande prend une **chaîne**.  
  
### <a name="bstr"></a>BSTR  
 Un **BSTR** (**B** ASIC **Str**) est un type de données structurées qui contient une chaîne de caractères et la longueur de la chaîne. COM fournit des méthodes pour allouer, manipuler et libérer un **BSTR**.  
  
 La classe **_bstr_t** encapsule et gère le type de données **BSTR** .  
  
 Lorsque la référence de l’API ADO indique qu’une méthode ou une propriété prend une valeur de **chaîne** , cela signifie que la valeur est sous la forme d’un **_bstr_t**.  
  
### <a name="casting-_variant_t-and-_bstr_t-classes"></a>Cast des classes _variant_t et _bstr_t  
 Il n’est souvent pas nécessaire de coder explicitement un **_variant_t** ou **_bstr_t** dans un argument d’une opération. Si la classe **_variant_t** ou **_bstr_t** a un constructeur qui correspond au type de données de l’argument, le compilateur génère le **_variant_t** ou **_bstr_t** approprié.  
  
 Toutefois, si l’argument est ambigu, autrement dit, si le type de données de l’argument correspond à plusieurs constructeurs, vous devez effectuer un cast de l’argument avec le type de données approprié pour appeler le constructeur correct.  
  
 Par exemple, la déclaration de la méthode **Recordset :: Open** est la suivante :  
  
```cpp
    HRESULT Open (  
        const _variant_t & Source,  
        const _variant_t & ActiveConnection,  
        enum CursorTypeEnum CursorType,  
        enum LockTypeEnum LockType,  
        long Options );  
```
  
 L' `ActiveConnection` argument prend une référence à un **_variant_t**, que vous pouvez coder comme une chaîne de connexion ou un pointeur vers un objet de **connexion** ouvert.  
  
 La **_variant_t** correcte est construite implicitement si vous transmettez une chaîne telle que « `DSN=pubs;uid=MyUserName;pwd=MyPassword;` » ou un pointeur tel que « `(IDispatch *) pConn` ».  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.  
  
 Vous pouvez également coder explicitement un **_variant_t** contenant un pointeur tel que « `_variant_t((IDispatch *) pConn, true)` ». Le cast, `(IDispatch *)` , résout l’ambiguïté avec un autre constructeur qui accepte un pointeur vers une interface IUnknown.  
  
 Il s’agit d’un facteur crucial, bien que rarement mentionné, qu’ADO soit une interface IDispatch. Chaque fois qu’un pointeur vers un objet ADO doit être passé en tant que **Variant**, ce pointeur doit être casté en tant que pointeur vers une interface IDispatch.  
  
 Le dernier cas code explicitement le deuxième argument booléen du constructeur avec sa valeur par défaut facultative `true` . Cet argument fait en sorte que le constructeur **Variant** appelle sa méthode **AddRef**(), qui compense l’appel automatique de la méthode **_variant_t :: Release**() par ADO lorsque l’appel de la méthode ou de la propriété ADO est terminé.  
  
### <a name="safearray"></a>SafeArray  
 Un **SAFEARRAY** est un type de données structurées qui contient un tableau d’autres types de données. Un **SAFEARRAY** est appelé *Safe* , car il contient des informations sur les limites de chaque dimension de tableau et limite l’accès aux éléments de tableau dans ces limites.  
  
 Lorsque la référence de l’API ADO indique qu’une méthode ou qu’une propriété prend ou retourne un tableau, cela signifie que la méthode ou la propriété prend ou retourne un **SAFEARRAY**, et non un tableau C/C++ natif.  
  
 Par exemple, le deuxième paramètre de la méthode **OpenSchema** de l’objet **Connection** requiert un tableau de valeurs de **type Variant** . Ces valeurs de **type Variant** doivent être passées en tant qu’éléments d’un **SAFEARRAY**, et ce **SAFEARRAY** doit être défini en tant que valeur d’une autre **variante**. Il s’agit de l’autre **Variant** qui est passé comme deuxième argument de **OpenSchema**.  
  
 À titre d’exemple, le premier argument de la méthode **Find** est un **Variant** dont la valeur est un **SAFEARRAY** unidimensionnel ; chacun des premier et deuxième arguments facultatifs de **AddNew** est un **SAFEARRAY** unidimensionnel ; et la valeur de retour de la méthode **GetRows** est une **variante** dont la valeur est un **SAFEARRAY** à deux dimensions.  
  
## <a name="missing-and-default-parameters"></a>Paramètres manquants et par défaut  
 Visual Basic autorise les paramètres manquants dans les méthodes. Par exemple, la méthode d' **ouverture** d’un objet **Recordset** a cinq paramètres, mais vous pouvez ignorer les paramètres intermédiaires et conserver les paramètres de fin. Un **BSTR** ou une **variante** par défaut est substitué selon le type de données de l’opérande manquant.  
  
 Dans C/C++, tous les opérandes doivent être spécifiés. Si vous souhaitez spécifier un paramètre manquant dont le type de données est une chaîne, spécifiez un **_bstr_t** contenant une chaîne NULL. Si vous souhaitez spécifier un paramètre manquant dont le type de données est **Variant**, spécifiez un **_variant_t** avec une valeur de DISP_E_PARAMNOTFOUND et un type de VT_ERROR. Vous pouvez également spécifier l’équivalent **_variant_t** constante, **vtMissing**, qui est fournie par la directive **#import** .  
  
 Trois méthodes sont des exceptions à l’utilisation classique de **vtMissing**. Il s’agit des méthodes **Execute** des objets **Connection** et **Command** , et de la méthode **NextRecordset** de l’objet **Recordset** . Les signatures sont les suivantes :  
  
```cpp
_RecordsetPtr <A HREF="mdmthcnnexecute.htm">Execute</A>( _bstr_t CommandText, VARIANT * RecordsAffected,   
        long Options );  // Connection  
_RecordsetPtr <A HREF="mdmthcmdexecute.htm">Execute</A>( VARIANT * RecordsAffected, VARIANT * Parameters,   
        long Options );  // Command  
_RecordsetPtr <A HREF="mdmthnextrec.htm">NextRecordset</A>( VARIANT * RecordsAffected );  // Recordset  
```
  
 Les paramètres, *RecordsAffected* et *Parameters*, sont des pointeurs vers un **Variant**. *Parameters* est un paramètre d’entrée qui spécifie l’adresse d’un **Variant** contenant un seul paramètre, ou un tableau de paramètres, qui modifiera la commande en cours d’exécution. *RecordsAffected* est un paramètre de sortie qui spécifie l’adresse d’un **Variant**, où le nombre de lignes affectées par la méthode est retourné.  
  
 Dans la méthode **Execute** de l’objet **Command** , indiquez qu’aucun paramètre n’est spécifié en définissant les *paramètres* sur `&vtMissing` (ce qui est recommandé) ou sur le pointeur null (autrement dit, **null** ou zéro (0)). Si *Parameters* a la valeur du pointeur null, la méthode remplace en interne l’équivalent de **vtMissing**, puis termine l’opération.  
  
 Dans toutes les méthodes, indiquez que le nombre d’enregistrements affectés ne doit pas être retourné en affectant à *RecordsAffected* le pointeur null. Dans ce cas, le pointeur NULL n’est pas tellement un paramètre manquant pour indiquer que la méthode doit ignorer le nombre d’enregistrements affectés.  
  
 Par conséquent, pour ces trois méthodes, il est valide de coder un code tel que :  
  
```cpp
pConnection->Execute("commandText", NULL, adCmdText);   
pCommand->Execute(NULL, NULL, adCmdText);  
pRecordset->NextRecordset(NULL);  
```
  
## <a name="error-handling"></a>Gestion des erreurs  
 Dans COM, la plupart des opérations retournent un code de retour HRESULT qui indique si une fonction s’est terminée avec succès. La directive **#import** génère du code wrapper autour de chaque méthode ou propriété « brute » et vérifie le HRESULT retourné. Si le HRESULT indique un échec, le code wrapper lève une erreur COM en appelant _com_issue_errorex () avec le code de retour HRESULT comme argument. Les objets d’erreur com peuvent être interceptés dans un bloc **try** - **catch** . (Pour des raisons d’efficacité, interceptez une référence à un objet **_com_error** .)  
  
 N’oubliez pas qu’il s’agit d’erreurs ADO : elles résultent de l’échec de l’opération ADO. Les erreurs retournées par le fournisseur sous-jacent apparaissent en tant qu’objets d' **erreur** dans la collection d' **Erreurs** de l’objet de **connexion** .  
  
 La directive **#import** crée uniquement des routines de gestion des erreurs pour les méthodes et les propriétés déclarées dans le fichier ADO. dll. Toutefois, vous pouvez tirer parti de ce même mécanisme de gestion des erreurs en écrivant votre propre macro de vérification des erreurs ou fonction Inline. Pour obtenir des exemples, consultez la rubrique, [Visual C++ extensions](./visual-c-extensions-for-ado.md)ou le code des sections suivantes.  
  
## <a name="visual-c-equivalents-of-visual-basic-conventions"></a>Visual C++ équivalents des conventions de Visual Basic  
 Voici un résumé de plusieurs conventions dans la documentation ADO, codées dans Visual Basic, ainsi que leurs équivalents dans Visual C++.  
  
### <a name="declaring-an-ado-object"></a>Déclaration d’un objet ADO  
 Dans Visual Basic, une variable objet ADO (dans ce cas pour un objet **Recordset** ) est déclarée comme suit :  
  
```vb
Dim rst As ADODB.Recordset  
```
  
 La clause, " `ADODB.Recordset` ", est le ProgID de l’objet **Recordset** tel que défini dans le registre. Une nouvelle instance d’un objet **Record** est déclarée comme suit :  
  
```vb
Dim rst As New ADODB.Recordset  
```
  
 -ou-  
  
```vb
Dim rst As ADODB.Recordset  
Set rst = New ADODB.Recordset  
```
  
 Dans Visual C++, la directive **#import** génère des déclarations de type pointeur intelligent pour tous les objets ADO. Par exemple, une variable qui pointe vers un objet **_Recordset** est de type **_RecordsetPtr**, et est déclarée comme suit :  
  
```cpp
_RecordsetPtr  rs;  
```
  
 Une variable qui pointe vers une nouvelle instance d’un objet **_Recordset** est déclarée comme suit :  
  
```cpp
_RecordsetPtr  rs("ADODB.Recordset");  
```
  
 - ou -  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance("ADODB.Recordset");  
```
  
 - ou -  
  
```cpp
_RecordsetPtr  rs;  
rs.CreateInstance(__uuidof(_Recordset));  
```
  
 Une fois la méthode **CreateInstance** appelée, la variable peut être utilisée comme suit :  
  
```cpp
rs->Open(...);  
```
  
 Notez que dans un cas, l' `.` opérateur « » est utilisé comme si la variable était une instance d’une classe ( `rs.CreateInstance` ) et dans un autre cas, l' `->` opérateur « » est utilisé comme si la variable était un pointeur vers une interface ( `rs->Open` ).  
  
 Une variable peut être utilisée de deux façons, car l' `->` opérateur «» est surchargé pour permettre à une instance d’une classe de se comporter comme un pointeur vers une interface. Un membre de classe privé de la variable d’instance contient un pointeur vers l’interface **_Recordset** ; l' `->` opérateur «» retourne ce pointeur ; et le pointeur retourné accède aux membres de l’objet **_Recordset** .  
  
### <a name="coding-a-missing-parameter---string"></a>Codage d’une chaîne de paramètre manquante  
 Lorsque vous devez coder un opérande de **chaîne** manquant dans Visual Basic, il vous suffit d’omettre l’opérande. Vous devez spécifier l’opérande dans Visual C++. Code un **_bstr_t** qui a une chaîne vide en tant que valeur.  
  
```cpp
_bstr_t strMissing(L"");  
```
  
### <a name="coding-a-missing-parameter---variant"></a>Codage d’une variante de paramètre manquante  
 Lorsque vous devez coder un opérande **Variant** manquant dans Visual Basic, il vous suffit d’omettre l’opérande. Vous devez spécifier tous les opérandes dans Visual C++. Codez un paramètre **Variant** manquant avec un **_variant_t** défini sur la valeur spéciale, DISP_E_PARAMNOTFOUND et type, VT_ERROR. Vous pouvez également spécifier **vtMissing**, qui est une constante prédéfinie équivalente fournie par la directive **#import** .  
  
```cpp
_variant_t  vtMissingYours(DISP_E_PARAMNOTFOUND, VT_ERROR);   
```
  
 -ou utilisez-  
  
```cpp
...vtMissing...;  
```
  
### <a name="declaring-a-variant"></a>Déclaration d’une variante  
 Dans Visual Basic, un **Variant** est déclaré avec l’instruction **Dim** comme suit :  
  
```vb
Dim VariableName As Variant  
```
  
 Dans Visual C++, déclarez une variable en tant que type **_variant_t**. Quelques déclarations de **_variant_t** schématique sont présentées ci-dessous.  
  
> [!NOTE]
>  Ces déclarations donnent simplement une idée approximative de ce que vous pourriez coder dans votre propre programme. Pour plus d’informations, consultez les exemples ci-dessous et la documentation Visual C++.  
  
```cpp
_variant_t  VariableName(value);  
_variant_t  VariableName((data type cast) value);  
_variant_t  VariableName(value, VT_DATATYPE);  
_variant_t  VariableName(interface * value, bool fAddRef = true);  
```
  
### <a name="using-arrays-of-variants"></a>Utilisation de tableaux de variants  
 Dans Visual Basic, les tableaux de **variants** peuvent être codés avec l’instruction **Dim** , ou vous pouvez utiliser la fonction **Array** , comme illustré dans l’exemple de code suivant :  
  
```vb
Public Sub ArrayOfVariants  
Dim cn As ADODB.Connection  
Dim rs As ADODB.Recordset  
Dim fld As ADODB.Field  
  
    cn.Open "DSN=pubs"  
    rs = cn.OpenSchema(adSchemaColumns, _  
        Array(Empty, Empty, "authors", Empty))  
    For Each fld in rs.Fields  
        Debug.Print "Name = "; fld.Name  
    Next fld  
    rs.Close  
    cn.Close  
End Sub  
```
  
 L’exemple de Visual C++ suivant illustre l’utilisation d’un **SAFEARRAY** utilisé avec une **_variant_t**.  
  
#### <a name="notes"></a>Notes  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Une fois encore, la fonction inline TESTHR () est définie pour tirer parti du mécanisme de gestion des erreurs existant.  
  
2.  Vous n’avez besoin que d’un tableau unidimensionnel. vous pouvez donc utiliser **SafeArrayCreateVector** au lieu de la Déclaration **SAFEARRAYBOUND** à usage général et de la fonction **SafeArrayCreate** . Voici à quoi ressemble le code à l’aide de **SafeArrayCreate**:  
  
    ```cpp
       SAFEARRAYBOUND   sabound[1];  
       sabound[0].lLbound = 0;  
       sabound[0].cElements = 4;  
       pSa = SafeArrayCreate(VT_VARIANT, 1, sabound);  
    ```
  
3.  Le schéma identifié par la constante énumérée, **adSchemaColumns**, est associé à quatre colonnes de contrainte : TABLE_CATALOG, TABLE_SCHEMA, TABLE_NAME et column_name. Par conséquent, un tableau de valeurs de **type Variant** avec quatre éléments est créé. Ensuite, une valeur de contrainte qui correspond à la troisième colonne, TABLE_NAME, est spécifiée.  
  
     Le **jeu d’enregistrements** retourné est constitué de plusieurs colonnes, dont un sous-ensemble est constitué des colonnes de contrainte. Les valeurs des colonnes de contrainte pour chaque ligne retournée doivent être les mêmes que les valeurs de contrainte correspondantes.  
  
4.  Les utilisateurs habitués à **SAFEARRAY** peuvent être surpris de savoir que **SafeArrayDestroy**() n’est pas appelé avant la sortie. En fait, l’appel de **SafeArrayDestroy**() dans ce cas entraîne une exception Runtime. La raison est que le destructeur de `vtCriteria` appelle **VariantClear**() lorsque le **_variant_t** est hors de portée, ce qui libère le **SAFEARRAY**. L’appel de **SafeArrayDestroy**, sans effacer manuellement le **_variant_t**, amène le destructeur à tenter de supprimer un pointeur **SAFEARRAY** non valide.  
  
     Si **SafeArrayDestroy** est appelé, le code ressemble à ceci :  
  
    ```cpp
          TESTHR(SafeArrayDestroy(pSa));  
       vtCriteria.vt = VT_EMPTY;  
          vtCriteria.parray = NULL;  
    ```
  
     Toutefois, il est bien plus simple de laisser le **_variant_t** gérer le **SAFEARRAY**.  
  
```cpp
// Visual_CPP_ADO_Prog_1.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Note 1  
inline void TESTHR( HRESULT _hr ) {   
   if FAILED(_hr)   
      _com_issue_error(_hr);   
}  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _RecordsetPtr pRs("ADODB.Recordset");  
      _ConnectionPtr pCn("ADODB.Connection");  
      _variant_t vtTableName("authors"), vtCriteria;  
      long ix[1];  
      SAFEARRAY *pSa = NULL;  
  
      pCn->Provider = "sqloledb";  
      pCn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
      // Note 2, Note 3  
      pSa = SafeArrayCreateVector(VT_VARIANT, 1, 4);  
      if (!pSa)   
         _com_issue_error(E_OUTOFMEMORY);  
  
      // Specify TABLE_NAME in the third array element (index of 2).   
      ix[0] = 2;        
      TESTHR(SafeArrayPutElement(pSa, ix, &vtTableName));  
  
      // There is no Variant constructor for a SafeArray, so manually set the   
      // type (SafeArray of Variant) and value (pointer to a SafeArray).  
  
      vtCriteria.vt = VT_ARRAY | VT_VARIANT;  
      vtCriteria.parray = pSa;  
  
      pRs = pCn->OpenSchema(adSchemaColumns, vtCriteria, vtMissing);  
  
      long limit = pRs->GetFields()->Count;  
      for ( long x = 0 ; x < limit ; x++ )  
         printf( "%d: %s\n", x + 1, ((char*) pRs->GetFields()->Item[x]->Name) );  
      // Note 4  
      pRs->Close();  
      pCn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Error:\n");  
      printf("Code = %08lx\n", e.Error());  
      printf("Code meaning = %s\n", (char*) e.ErrorMessage());  
      printf("Source = %s\n", (char*) e.Source());  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   CoUninitialize();  
}  
```
  
### <a name="using-property-getputputref"></a>Utilisation de la propriété obtient/put/PutRef  
 Dans Visual Basic, le nom d’une propriété n’est pas qualifié par le fait qu’elle soit récupérée, affectée ou assignée à une référence.  
  
```vb
Public Sub GetPutPutRef  
Dim rs As New ADODB.Recordset  
Dim cn As New ADODB.Connection  
Dim sz as Integer  
cn.Open "Provider=sqloledb;Data Source=yourserver;" & _  
         "Initial Catalog=pubs;Integrated Security=SSPI;"  
rs.PageSize = 10  
sz = rs.PageSize  
rs.ActiveConnection = cn  
rs.Open "authors",,adOpenStatic  
' ...  
rs.Close  
cn.Close  
End Sub  
```
  
 Cet exemple de Visual C++ illustre la propriété obten / **put** / **PutRef**.  
  
#### <a name="notes"></a>Notes  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Cet exemple utilise deux formes d’un argument de chaîne manquant : une constante explicite, **strMissing** et une chaîne que le compilateur utilisera pour créer un **_bstr_t** temporaire qui existera pour la portée de la méthode **ouverte** .  
  
2.  Il n’est pas nécessaire d’effectuer un cast de l’opérande de `rs->PutRefActiveConnection(cn)` en `(IDispatch *)` , car le type de l’opérande est déjà `(IDispatch *)` .  
  
```cpp
// Visual_CPP_ado_prog_2.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _bstr_t strMissing(L"");  
      long oldPgSz = 0, newPgSz = 5;  
  
      // Note 1  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", strMissing, "", adConnectUnspecified);  
  
      oldPgSz = rs->GetPageSize();  
      // -or-  
      // oldPgSz = rs->PageSize;  
  
      rs->PutPageSize(newPgSz);  
      // -or-  
      // rs->PageSize = newPgSz;  
  
      // Note 2  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockReadOnly, adCmdTable);  
      printf("Original pagesize = %d, new pagesize = %d\n", oldPgSz, rs->GetPageSize());  
      rs->Close();  
      cn->Close();  
  
   }  
   catch (_com_error &e) {  
      printf("Description = %s\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="using-getitemx-and-itemx"></a>Utilisation de GetItem (x) et Item [x]  
 Cet exemple de Visual Basic illustre la syntaxe standard et alternative pour **Item**().  
  
```vb
Public Sub GetItemItem  
Dim rs As New ADODB.Recordset  
Dim name as String  
rs = rs.Open "authors", "DSN=pubs;", adOpenDynamic, _  
         adLockBatchOptimistic, adTable  
name = rs(0)  
' -or-  
name = rs.Fields.Item(0)  
rs(0) = "Test"  
rs.UpdateBatch  
' Restore name  
rs(0) = name  
rs.UpdateBatch  
rs.Close  
End Sub  
```
  
 Cet exemple de Visual C++ illustre **Item**.  
  
> [!NOTE]
>  La remarque suivante correspond aux sections commentées dans l’exemple de code : quand la collection est accessible avec **Item**, l’index, **2**, doit être converti en **long** afin qu’un constructeur approprié soit appelé.  
  
```cpp
// Visual_CPP_ado_prog_3.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
void main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr cn("ADODB.Connection");  
      _RecordsetPtr rs("ADODB.Recordset");  
      _variant_t vtFirstName;  
  
      cn->Provider = "sqloledb";  
      cn->Open("Data Source='(local)';Initial Catalog=pubs;Integrated Security=SSPI;", "", "", adConnectUnspecified);  
  
      rs->PutRefActiveConnection( cn );  
      rs->Open("authors", vtMissing, adOpenStatic, adLockOptimistic, adCmdTable);  
      rs->MoveFirst();  
  
      // Note 1. Get a field.  
      vtFirstName = rs->Fields->GetItem((long)2)->GetValue();  
      // -or-  
      vtFirstName = rs->Fields->Item[(long)2]->Value;  
  
      printf( "First name = '%s'\n", (char*)( (_bstr_t)vtFirstName) );  
  
      rs->Fields->GetItem((long)2)->Value = L"TEST";  
      rs->Update(vtMissing, vtMissing);  
  
      // Restore name  
      rs->Fields->GetItem((long)2)->PutValue(vtFirstName);  
      // -or-  
      rs->Fields->GetItem((long)2)->Value = vtFirstName;  
      rs->Update(vtMissing, vtMissing);  
      rs->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }  
   ::CoUninitialize();  
}  
```
  
### <a name="casting-ado-object-pointers-with-idispatch-"></a>Conversion de pointeurs d’objets ADO avec (IDispatch *)  
 L’exemple de Visual C++ suivant illustre l’utilisation de (IDispatch *) pour caster des pointeurs d’objet ADO.  
  
#### <a name="notes"></a>Notes  
 Les remarques suivantes correspondent aux sections commentées dans l’exemple de code.  
  
1.  Spécifiez un objet de **connexion** ouvert dans une **variante** codée explicitement. Effectuez un cast de celui-ci avec (IDispatch \* ) pour que le constructeur correct soit appelé. De même, définissez explicitement le deuxième paramètre **_variant_t** sur la valeur par défaut **true**, afin que le nombre de références d’objet soit correct lorsque l’opération **Recordset :: Open** se termine.  
  
2.  L’expression, `(_bstr_t)` , n’est pas un cast, mais un opérateur **_variant_t** qui extrait une chaîne **_bstr_t** à partir de la **variante** retournée par **value**.  
  
 L’expression, `(char*)` , n’est pas un cast, mais un opérateur **_bstr_t** qui extrait un pointeur vers la chaîne encapsulée dans un objet **_bstr_t** .  
  
 Cette section de code illustre certains des comportements utiles des opérateurs **_variant_t** et **_bstr_t** .  
  
```cpp
// Visual_CPP_ado_prog_4.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
int main() {  
   CoInitialize(NULL);  
   try {  
      _ConnectionPtr pConn("ADODB.Connection");  
      _RecordsetPtr pRst("ADODB.Recordset");  
  
      pConn->Provider = "sqloledb";  
      pConn->Open("Data Source='(local)';Initial Catalog='pubs';Integrated Security=SSPI", "", "", adConnectUnspecified);  
  
      // Note 1.  
      pRst->Open("authors", _variant_t((IDispatch *) pConn, true), adOpenStatic, adLockReadOnly, adCmdTable);  
      pRst->MoveLast();  
  
      // Note 2.  
      printf("Last name is '%s %s'\n",   
         (char*) ((_bstr_t) pRst->GetFields()->GetItem("au_fname")->GetValue()),  
         (char*) ((_bstr_t) pRst->Fields->Item["au_lname"]->Value));  
  
      pRst->Close();  
      pConn->Close();  
   }  
   catch (_com_error &e) {  
      printf("Description = '%s'\n", (char*) e.Description());  
   }     
   ::CoUninitialize();  
}  
```