---
title: Basculer un point d'arrêt
description: Découvrez comment basculer un point d’arrêt pour mettre en surbrillance l’instruction Transact-SQL associée et effectuer diverses actions sur l’instruction (telle que la modification).
titleSuffix: T-SQL debugger
ms.prod: sql
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c477ab89-a1cd-4f2c-aa7c-40525041100f
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0de50770b4be70a79ebddffd3cd1a983dc2524ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339352"
---
# <a name="toggle-a-breakpoint"></a>Basculer un point d'arrêt

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Le fait de définir un point d'arrêt sur une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] s'appelle le basculement d'un point d'arrêt.  

[!INCLUDE[ssms-old-versions](../../includes/ssms-old-versions.md)]

## <a name="breakpoints"></a>Points d’arrêt

Une fois le point d'arrêt défini, il est représenté par une icône dans la barre grise à gauche de l'instruction. Cette icône s'appelle un glyphe de point d'arrêt. [!INCLUDE[tsql](../../includes/tsql-md.md)] Les points d’arrêt sont appliqués à une instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] complète. Lorsqu'un point d'arrêt est activé, le débogueur met en surbrillance l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] associée.  
  
 S'il existe plusieurs instructions [!INCLUDE[tsql](../../includes/tsql-md.md)] sur une ligne, vous pouvez basculer un point d'arrêt pour chaque instruction. Cliquer dans la barre grise à gauche de la fenêtre bascule un point d'arrêt sur la première instruction de la ligne. Vous pouvez basculer un point d’arrêt dans une instruction suivante en mettant en surbrillance une partie de l’instruction ou en plaçant le curseur dans l’instruction, puis en appuyant sur F9 ou en cliquant sur **Basculer le point d’arrêt** dans le menu **Déboguer** . Si plusieurs points d'arrêt se trouvent sur une ligne, un seul glyphe de point d'arrêt se trouve dans la barre grise à gauche.  
  
 Une fois qu'un point d'arrêt a été basculé, vous pouvez effectuer plusieurs actions sur celui-ci, par exemple modifier ses propriétés ou les désactiver temporairement. Pour plus d’informations, consultez [Points d’arrêt Transact-SQL](./transact-sql-breakpoints.md).  
  
## <a name="toggle-a-breakpoint"></a>Basculer un point d'arrêt  
 **Pour basculer un point d'arrêt sur une instruction Transact-SQL**  
  
1.  Cliquez sur la barre grise à gauche de l'instruction [!INCLUDE[tsql](../../includes/tsql-md.md)] .  
  
2.  Vous pouvez également mettre en surbrillance une partie quelconque de l'instruction ou placer le curseur sur l'instruction, puis effectuer l'une des actions suivantes :  
  
    -   Appuyez sur F9.  
  
    -   Dans le menu **Déboguer** , cliquez sur **Basculer le point d’arrêt**.  
  
