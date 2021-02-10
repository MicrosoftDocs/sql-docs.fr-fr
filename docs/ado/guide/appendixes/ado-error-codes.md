---
description: Capturer les codes d’erreur ADO
title: Codes d’erreur ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], error codes
ms.assetid: 3aee61c7-a9b7-4596-b78e-5828a00d0281
author: rothja
ms.author: jroth
ms.openlocfilehash: 2d3b0fd0c3a673cc841d7a5318872151bc20b311
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029503"
---
# <a name="capture-ado-error-codes"></a>Capturer les codes d’erreur ADO
Outre les erreurs de fournisseur retournées dans les objets [Error](../../reference/ado-api/error-object.md) de la collection [Errors](../../reference/ado-api/errors-collection-ado.md) , ADO lui-même peut retourner des erreurs au mécanisme de gestion des exceptions de votre environnement d’exécution. Utilisez le mécanisme d’interception des erreurs de votre langage de programmation, tel que l’instruction **On Error** dans Microsoft® Visual Basic ou le bloc **try-catch** dans Microsoft Visual C++®, pour capturer les erreurs ADO.

 Pour obtenir la liste des codes d’erreur ADO, consultez [ErrorValueEnum](../../reference/ado-api/errorvalueenum.md).

## <a name="see-also"></a>Voir aussi
 [Error Object](../../reference/ado-api/error-object.md) [, collection d’erreurs (ADO)](../../reference/ado-api/errors-collection-ado.md)