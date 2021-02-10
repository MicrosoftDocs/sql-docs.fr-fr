---
description: Utilisation d’ADO avec Microsoft Visual Basic et Visual Basic pour Applications
title: Utilisation d’ADO avec Microsoft Visual Basic | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, Visual Basic
- Visual Basic [ADO]
ms.assetid: 9dfb6784-037d-4f9d-bb7f-b506b4498573
author: rothja
ms.author: jroth
ms.openlocfilehash: 229662bd5db68039202f1f39d7309c12ff6354a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028858"
---
# <a name="using-ado-with-microsoft-visual-basic-and-visual-basic-for-applications"></a>Utilisation d’ADO avec Microsoft Visual Basic et Visual Basic pour Applications
La configuration d’un projet ADO et l’écriture de code ADO sont similaires, que vous utilisiez Visual Basic ou Visual Basic pour Applications. Cette rubrique traite de l’utilisation d’ADO avec Visual Basic et Visual Basic pour Applications et note les différences éventuelles.

## <a name="referencing-the-ado-library"></a>Référencement de la bibliothèque ADO
 La bibliothèque ADO doit être référencée par votre projet.

#### <a name="to-reference-ado-from-microsoft-visual-basic"></a>Pour référencer ADO à partir de Microsoft Visual Basic

1.  Dans Visual Basic, dans le menu **projet** , sélectionnez **références...**.

2.  Sélectionnez la **bibliothèque Microsoft ActiveX Data Objects x. x** dans la liste. Vérifiez qu’au moins les bibliothèques suivantes sont également sélectionnées :

    -   Visual Basic pour Applications

    -   Visual Basic les objets et procédures d’exécution

    -   Objets et procédures Visual Basic

    -   OLE Automation

3.  Cliquez sur **OK**.

 Vous pouvez utiliser ADO tout aussi facilement avec Visual Basic pour Applications, à l’aide de Microsoft Access, par exemple.

#### <a name="to-reference-ado-from-microsoft-access"></a>Pour référencer ADO à partir de Microsoft Access

1.  Dans Microsoft Access, sélectionnez ou créez un module à partir de l’onglet **modules** de la fenêtre **base de données** .

2.  Dans le menu **Outils** , sélectionnez **références...**.

3.  Sélectionnez la **bibliothèque Microsoft ActiveX Data Objects x. x** dans la liste. Vérifiez qu’au moins les bibliothèques suivantes sont également sélectionnées :

    -   Visual Basic pour Applications

    -   Bibliothèque d’objets Microsoft Access 8,0 (ou version ultérieure)

    -   Bibliothèque d’objets Microsoft DAO 3,5 (ou version ultérieure)

4.  Cliquez sur **OK**.

## <a name="creating-ado-objects-in-visual-basic"></a>Création d’objets ADO dans Visual Basic
 Pour créer une variable Automation et une instance d’un objet pour cette variable, vous pouvez utiliser deux méthodes : **Dim** ou **CreateObject**.

### <a name="dim"></a>Dim
 Vous pouvez utiliser le mot clé **New** avec **Dim** pour déclarer et créer des instances d’objets ADO en une seule étape :

```
Dim conn As New ADODB.Connection
```

 La déclaration de l’instruction **Dim** et l’instanciation de l’objet peuvent également être deux étapes :

```
Dim conn As ADODB.Connection
Set conn = New ADODB.Connection
```

> [!NOTE]
>  Il n’est pas nécessaire d’utiliser explicitement le `ADODB` ProgID avec l’instruction **Dim** , en supposant que vous avez correctement référencé la bibliothèque ADO dans votre projet. Toutefois, son utilisation garantit que vous n’aurez pas de conflits de noms avec d’autres bibliothèques.

> [!NOTE]
>  Par exemple, si vous incluez des références à ADO et à DAO dans le même projet, vous devez inclure un qualificateur pour spécifier le modèle objet à utiliser lors de l’instanciation d’objets **Recordset** , comme dans le code suivant :

```
Dim adoRS As ADODB.Recordset
Dim daoRS As DAO.Recordset
```

### <a name="createobject"></a>CreateObject
 Avec la méthode **CreateObject** , la déclaration et l’instanciation d’objet doivent être deux étapes discrètes :

```
Dim conn1
Set conn1 = CreateObject("ADODB.Connection") As Object
```

 Les objets instanciés avec **CreateObject** sont à liaison tardive, ce qui signifie qu’ils ne sont pas fortement typés et que la saisie semi-automatique de la ligne de commande est désactivée. Toutefois, elle vous permet d’ignorer la référence à la bibliothèque ADO à partir de votre projet et vous permet d’instancier des versions spécifiques d’objets. Par exemple :

```
Set conn1 = CreateObject("ADODB.Connection.2.0") As Object
```

 Vous pouvez également le faire en créant spécifiquement une référence à la bibliothèque de types ADO version 2,0 et en créant l’objet.

 L’instanciation d’objets à l’aide de la méthode **CreateObject** est généralement plus lente que l’utilisation de l’instruction **Dim** .

## <a name="handling-events"></a>Gestion des événements
 Pour gérer les événements ADO dans Microsoft Visual Basic, vous devez déclarer une variable au niveau du module à l’aide du mot clé **WithEvents** . La variable ne peut être déclarée que dans le cadre d’un module de classe et doit être déclarée au niveau du module. Pour une discussion plus approfondie sur la gestion des événements ADO, consultez [gestion des événements ADO](../data/handling-ado-events.md).

## <a name="visual-basic-examples"></a>Exemples de Visual Basic
 De nombreux exemples de Visual Basic sont inclus dans la documentation ADO. Pour plus d’informations, consultez [exemples de code ADO dans Microsoft Visual Basic](../../reference/ado-api/ado-code-examples-in-visual-basic.md).

## <a name="see-also"></a>Voir aussi
 [Microsoft ActiveX Data Objects (ADO)](../../microsoft-activex-data-objects-ado.md) [à l’aide d’ADO avec Microsoft Visual C++](./using-ado-with-microsoft-visual-c.md) [à l’aide d’ADO avec des langages de script](./using-ado-with-scripting-languages.md)