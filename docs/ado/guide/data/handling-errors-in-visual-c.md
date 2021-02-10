---
description: Gestion des erreurs dans Visual C++
title: Gestion des erreurs dans Visual C++ | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- errors [ADO], Visual C++
- Visual C++ error handling [ADO]
ms.assetid: b7576f07-020a-45f7-9e79-b5756f33f7ab
author: rothja
ms.author: jroth
ms.openlocfilehash: 61435c9d66800704e962cd4a399c87e72f6558c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037369"
---
# <a name="handling-errors-in-visual-c"></a>Gestion des erreurs dans Visual C++
Dans COM, la plupart des opérations retournent un code de retour HRESULT qui indique si une fonction s’est terminée avec succès. La directive #import génère du code wrapper autour de chaque méthode ou propriété « brute » et vérifie le HRESULT retourné. Si le HRESULT indique un échec, le code wrapper lève une erreur COM en appelant _com_issue_errorex () avec le code de retour HRESULT comme argument. Les objets d’erreur COM peuvent être interceptés dans un bloc **try-catch** . (Pour des raisons d’efficacité, interceptez une référence à un objet _com_error.)  
  
 N’oubliez pas qu’il s’agit d’erreurs ADO : elles résultent de l’échec de l’opération ADO. Les erreurs retournées par le fournisseur sous-jacent apparaissent en tant qu’objets d' **erreur** dans la collection d' **Erreurs** de l’objet de **connexion** .  
  
 La directive #import crée uniquement des routines de gestion des erreurs pour les méthodes et les propriétés déclarées dans le fichier ADO. dll. Toutefois, vous pouvez tirer parti de ce même mécanisme de gestion des erreurs en écrivant votre propre macro de vérification des erreurs ou fonction Inline. Pour obtenir des exemples, consultez la rubrique Visual C++ des extensions®.
