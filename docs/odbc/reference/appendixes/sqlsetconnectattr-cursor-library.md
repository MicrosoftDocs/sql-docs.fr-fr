---
description: SQLSetConnectAttr (bibliothèque de curseurs)
title: SQLSetConnectAttr (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC], Cursor Library
ms.assetid: 6f70bbd0-a057-49ef-8b05-4c80b58fc6e6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02f702991a4e03816f3e4a30a233d098198bfefe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202668"
---
# <a name="sqlsetconnectattr-cursor-library"></a>SQLSetConnectAttr (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLSetConnectAttr** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLSetConnectAttr**, consultez [fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 Une application appelle **SQLSetConnectAttr** avec l’attribut SQL_ATTR_ODBC_CURSORS pour spécifier si la bibliothèque de curseurs est toujours utilisée, utilisée si le pilote ne prend pas en charge les curseurs de défilement ou n’est jamais utilisée. La bibliothèque de curseurs suppose qu’un pilote prend en charge les curseurs de défilement s’il retourne SQL_CA1_RELATIVE pour le type d’informations SQL_STATIC_CURSOR_ATTRIBUTES1 dans **SQLGetInfo**.  
  
 L’application doit appeler **SQLSetConnectAttr** pour spécifier l’utilisation de la bibliothèque de curseurs après avoir appelé **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC pour allouer la connexion et avant qu’elle se connecte à la source de données. Si une application appelle **SQLSetConnectAttr** avec l’attribut SQL_ATTR_ODBC_CURSORS alors que la connexion est toujours active, la bibliothèque de curseurs retourne une erreur.  
  
 Pour définir un attribut d’instruction pris en charge par la bibliothèque de curseurs pour toutes les instructions associées à une connexion, une application doit appeler **SQLSetConnectAttr** pour cet attribut d’instruction après la connexion à la source de données et avant l’ouverture du curseur. Si une application appelle **SQLSetConnectAttr** avec un attribut d’instruction et qu’un curseur est ouvert sur une instruction associée à la connexion, l’attribut d’instruction n’est pas appliqué à cette instruction tant que le curseur n’est pas fermé et rouvert.
