---
title: Modifier un emplacement de point d'arrêt
description: Découvrez comment déplacer un point d’arrêt dans un fichier de script Transact-SQL vers un autre emplacement du script ou vers un script différent.
titleSuffix: T-SQL debugger
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL debugger, breakpoint location
ms.assetid: 5c28e411-0377-4210-a7ce-2a5c13dcdf74
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 12/04/2019
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a5cc11057258c1a85f6b185187055a04e05b7689
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354655"
---
# <a name="edit-a-breakpoint-location"></a>Modifier un emplacement de point d'arrêt

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

L'emplacement du point d'arrêt spécifie la ligne et le caractère où réside ce point d'arrêt dans un fichier de script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Vous pouvez modifier cet emplacement pour déplacer le point d'arrêt vers un autre emplacement du script ou vers un script différent.

## <a name="editing-a-location"></a>Modification d'un emplacement

Lorsque vous modifiez l'emplacement d'un point d'arrêt, ce point d'arrêt se déplace jusqu'à son nouvel emplacement et conserve toutes ses propriétés existantes, telles que le nombre d'accès ou la condition.  

#### <a name="to-edit-a-breakpoint-location"></a>Pour modifier l'emplacement d'un point d'arrêt

1. Dans la fenêtre de l’éditeur, cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Emplacement** dans le menu contextuel.  
  
     -ou-  
  
     Dans la fenêtre **Points d’arrêt** , cliquez avec le bouton droit sur le glyphe du point d’arrêt, puis cliquez sur **Emplacement** dans le menu contextuel.  
  
2. Dans la boîte de dialogue **Point d’arrêt sur fichier** , modifiez **Fichier** pour spécifier un nouveau fichier, **Ligne** pour spécifier une nouvelle ligne ou **Caractère** pour spécifier un nouvel emplacement dans la ligne. Si le nouveau fichier que vous spécifiez est déjà ouvert dans une fenêtre de l'éditeur de requête, le point d'arrêt est déplacé vers cette fenêtre de l'éditeur. Si le fichier n'est pas ouvert, une nouvelle fenêtre d'éditeur s'ouvre, le fichier y est chargé et le point d'arrêt est déplacé vers le nouvel emplacement.  
  
     L’option **Permettre que le code source soit différent de la version d’origine** n’a aucun effet lors du débogage de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="see-also"></a>Voir aussi

- [Spécifier un nombre d’accès](./specify-a-hit-count.md)
- [Spécifier une action de point d’arrêt](./specify-a-breakpoint-action.md)
- [Spécifier une condition de point d’arrêt](./specify-a-breakpoint-condition.md)
- [Pour spécifier un filtre de point d’arrêt](./specify-a-breakpoint-filter.md)