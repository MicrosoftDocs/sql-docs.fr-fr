---
description: Bibliothèque de curseurs ODBC
title: Bibliothèque de curseurs ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC cursor library [ODBC], about cursor library
- ODBC cursor library [ODBC]
- cursor library [ODBC], about cursor library
- scrollable cursors [ODBC]
- cursors [ODBC], cursor library
- block cursors [ODBC]
ms.assetid: 32fb7df0-953a-4f68-b041-7d2852e45d0f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4e5e3a01633a19c4ae82596ba1e180c64bcce06c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88487512"
---
# <a name="the-odbc-cursor-library"></a>Bibliothèque de curseurs ODBC
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Les curseurs de bloc et de défilement sont des ajouts très utiles à de nombreuses applications. Toutefois, tous les pilotes ne prennent pas en charge les curseurs de blocage et de défilement. Il en va de même pour les instructions Update et DELETE positionnées et **SQLSetPos**, qui sont décrites dans mise à jour des données. Par conséquent, le composant ODBC de la SDK Windows, anciennement inclus dans le kit de développement logiciel (SDK) Microsoft Data Access Components (MDAC), inclut une bibliothèque de curseurs. La bibliothèque de curseurs implémente des curseurs de bloc, statiques, de mise à jour et de suppression positionnés, et **SQLSetPos** pour tous les pilotes qui remplissent le niveau de conformité de l’interface CLI standard Open Group. La bibliothèque de curseurs peut être redistribuée avec des applications ODBC ; Pour plus d’informations, consultez le contrat de licence dans le kit de développement logiciel (SDK).  
  
 Pour utiliser la bibliothèque de curseurs, une application définit l’attribut de connexion SQL_ATTR_ODBC_CURSORS avant de se connecter à la source de données. Pour plus d’informations sur la bibliothèque de curseurs, consultez [annexe F : bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/appendix-f-odbc-cursor-library.md).
