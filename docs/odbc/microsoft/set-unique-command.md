---
description: SET UNIQUE, commande
title: DÉFINIR la commande UNIQUE | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET UNIQUE command [ODBC]
ms.assetid: 1f69e31e-4599-47cc-ac89-b86fba8703c5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f30e589349ab1f0388e43af0aec132465b1fcdce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208589"
---
# <a name="set-unique-command"></a>SET UNIQUE, commande
Spécifie si les enregistrements avec des valeurs de clé d’index dupliquées sont conservés dans un fichier d’index.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET UNIQUE ON | OFF  
```  
  
## <a name="arguments"></a>Arguments  
 ACTIVÉ  
 Spécifie que tout enregistrement avec une valeur de clé d’index dupliquée ne doit pas être inclus dans le fichier d’index. Seul le premier enregistrement avec la valeur de clé d’index d’origine est inclus dans le fichier d’index.  
  
 OFF  
 (Par défaut.) Spécifie que les enregistrements avec des valeurs de clé d’index dupliquées doivent être inclus dans le fichier d’index.  
  
## <a name="remarks"></a>Notes  
 Un fichier d’index conserve son paramètre SET UNIQUE lorsque vous émettez la réindexation. Pour plus d’informations, consultez [index](../../odbc/microsoft/index-command.md).
