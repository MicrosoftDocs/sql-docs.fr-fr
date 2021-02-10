---
description: Ajout d’enregistrements à un jeu d’enregistrements
title: Ajout d’enregistrements | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- AddNew method [ADO]
- ADO, editing data
- editing data [ADO], AddNew method
- editing data [ADO], adding data
ms.assetid: dd34669e-6f06-403b-9241-1c85c82aecc2
author: rothja
ms.author: jroth
ms.openlocfilehash: b39a0920d9816837b6d737df00e6bb4560358356
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033399"
---
# <a name="adding-records-to-a-recordset"></a>Ajout d’enregistrements à un jeu d’enregistrements
Utilisez la méthode **AddNew** pour créer et initialiser un nouvel enregistrement dans un **jeu d’enregistrements** existant. Vous pouvez utiliser la méthode **supports** avec une valeur **CursorOptionEnum** de **adAddNew** pour vérifier si vous pouvez ajouter des enregistrements à l’objet **Recordset** actuel.

 Une fois que vous avez appelé la méthode **AddNew** , le nouvel enregistrement devient l’enregistrement actif et reste actif après l’appel de la méthode **Update** . Si l’objet **Recordset** ne prend pas en charge les signets, vous risquez de ne pas pouvoir accéder au nouvel enregistrement après l’avoir déplacé vers un autre enregistrement. Par conséquent, en fonction de votre type de curseur, vous devrez peut-être appeler la méthode **Requery** pour rendre le nouvel enregistrement accessible.

 Si vous appelez **AddNew** pendant la modification de l’enregistrement en cours ou lors de l’ajout d’un nouvel enregistrement, ADO appelle la méthode **Update** pour enregistrer les modifications apportées, puis crée le nouvel enregistrement.

 Cette section contient les rubriques suivantes :

-   [Ajout d’enregistrements avec AddNew](./adding-records-using-addnew.md)

-   [Ajout de plusieurs champs](./adding-multiple-fields.md)

-   [Détermination du mode d’édition](./determining-edit-mode.md)

-   [Utilisation d’AddNew dans les modes immédiat et batch](./using-addnew-in-immediate-and-batch-modes.md)