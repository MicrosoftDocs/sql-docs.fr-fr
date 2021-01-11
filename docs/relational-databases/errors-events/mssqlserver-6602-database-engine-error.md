---
description: MSSQLSERVER_6602
title: MSSQLSERVER_6602
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 6602 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: dc40432af6994b184678414efba4dcd098dc400b
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797751"
---
# <a name="mssqlserver_6602"></a>MSSQLSERVER_6602
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|6602|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|XMLERR_PARSEERR2|
|Texte du message|La description de l'erreur est '%.*ls'.|
||

## <a name="explanation"></a>Explication

Cette erreur se produit lorsque vous essayez d’exécuter une procédure stockée `sp_xml_preparedocument` dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle le contenu du paramètre `xmltext` est un document XML complexe. In message d’erreur semblable au suivant est signalé à l’utilisateur

> L’erreur d’analyse XML 0x80004005 s’est produite au numéro de ligne 1, à proximité du texte XML "\<XML document sample>"  
MSG 6602, niveau 16, état 2, procédure sp_xml_preparedocument, ligne 1  
La description de l’erreur est 'Erreur non spécifiée'.

## <a name="cause"></a>Cause

Ce problème se produit en raison d’une limitation de conception de l’analyseur MSXML (fichier msxmlsql.dll) que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise.

Le problème n’est pas strictement lié à la taille du document XML, mais à sa structure complexe. Une combinaison de la profondeur de la structure de l’élément XML, le nombre et la taille des attributs, ainsi que le nombre d’entités dans les attributs peut entraîner ce problème. Toutefois, le niveau de complexité requis pour atteindre cette limite se trouve dans des documents XML de plusieurs mégaoctets.

## <a name="user-action"></a>Action requise

Pour contourner ce problème, essayez de réduire la complexité du document XML.

> [!NOTE]
> Méfiez-vous des très grands attributs de chaîne unique qui contiennent de nombreuses entités XML.
