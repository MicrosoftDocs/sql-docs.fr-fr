---
description: SQLFetchScroll (bibliothèque de curseurs)
title: SQLFetchScroll (bibliothèque de curseurs) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLFetchScroll function [ODBC], Cursor Library
ms.assetid: 4417e57c-31dd-475e-8fe9-eab00a459c80
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 719504a6e1e412439314ee6971cf41e05805f592
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202848"
---
# <a name="sqlfetchscroll-cursor-library"></a>SQLFetchScroll (bibliothèque de curseurs)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.  
  
 Cette rubrique traite de l’utilisation de la fonction **SQLFetchScroll** dans la bibliothèque de curseurs. Pour obtenir des informations générales sur **SQLFetchScroll**, consultez [fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md).  
  
 La bibliothèque de curseurs implémente **SQLFetchScroll** en appelant de façon répétée **SQLFetch** dans le pilote. Il transfère les données qu’il récupère du pilote aux mémoires tampons d’ensembles de lignes fournies par l’application. Il met également en cache les données dans les fichiers de mémoire et de disque. Quand une application demande un nouvel ensemble de lignes, la bibliothèque de curseurs la récupère en fonction des besoins du pilote (si elle n’a pas été précédemment Récupérée) ou du cache (s’il a été précédemment extrait). Enfin, la bibliothèque de curseurs conserve l’état des données mises en cache et retourne ces informations à l’application dans le tableau d’état de ligne.  
  
 Lorsque la bibliothèque de curseurs est utilisée, les appels à **SQLFetchScroll** ne peuvent pas être mélangés avec des appels à **SQLFetch** ou **SQLExtendedFetch**.  
  
 Lorsque la bibliothèque de curseurs est utilisée, les appels à **SQLFetchScroll** sont pris en charge à la fois pour ODBC 2. *x* et pour ODBC 3. *x* pilotes.  
  
## <a name="rowset-buffers"></a>Mémoires tampons d’ensemble de lignes  
 La bibliothèque de curseurs optimise le transfert de données du pilote vers le tampon d’ensemble de lignes fourni par l’application si :  
  
-   L’application utilise une liaison selon les lignes.  
  
-   Il n’existe aucun octet inutilisé entre les champs de la structure que l’application déclare pour contenir une ligne de données.  
  
-   Les champs dans lesquels **SQLFetch** ou **SQLFetchScroll** retourne la longueur/l’indicateur d’une colonne suivent la mémoire tampon pour cette colonne et précèdent la mémoire tampon pour la colonne suivante. Ces champs sont facultatifs.  
  
 Lorsque l’application demande un nouvel ensemble de lignes, la bibliothèque de curseurs récupère les données à partir de son cache et du pilote, si nécessaire. Si les jeux de lignes nouveaux et anciens se chevauchent, la bibliothèque de curseurs peut optimiser ses performances en réutilisant les données des sections qui se chevauchent des mémoires tampons de l’ensemble de lignes. Par conséquent, les modifications non enregistrées dans les tampons d’ensembles de lignes sont perdues, sauf si les ensembles de lignes New et Old se chevauchent et que les modifications se trouvent dans les sections qui se chevauchent des mémoires tampons d’ensemble de lignes. Pour enregistrer les modifications, une application envoie une instruction Update positionnée.  
  
 Notez que la bibliothèque de curseurs actualise toujours les tampons de l’ensemble de lignes avec les données du cache lorsqu’une application appelle **SQLFetchScroll** avec l’argument *FetchOrientation* défini sur SQL_FETCH_RELATIVE et que l’argument *FetchOffset* a la valeur 0.  
  
 La bibliothèque de curseurs prend en charge l’appel à **SQLSetStmtAttr** avec un *attribut* de SQL_ATTR_ROW_ARRAY_SIZE pour modifier la taille de l’ensemble de lignes lorsqu’un curseur est ouvert. La nouvelle taille de l’ensemble de lignes prendra effet lors du prochain appel de **SQLFetchScroll** .  
  
## <a name="result-set-membership"></a>Appartenance au jeu de résultats  
 La bibliothèque de curseurs récupère les données du pilote uniquement lorsque l’application le demande. En fonction de la source de données et du paramètre de l’attribut d’instruction SQL_CONCURRENCY, cela a les conséquences suivantes :  
  
-   Les données récupérées par la bibliothèque de curseurs peuvent différer des données qui étaient disponibles au moment de l’exécution de l’instruction. Par exemple, une fois le curseur ouvert, les lignes insérées à un point au-delà de la position actuelle du curseur peuvent être récupérées par certains pilotes.  
  
-   Les données du jeu de résultats peuvent être verrouillées par la source de données pour la bibliothèque de curseurs et donc être indisponibles pour d’autres utilisateurs.  
  
 Une fois que la bibliothèque de curseurs a mis en cache une ligne de données, elle ne peut pas détecter les modifications apportées à cette ligne dans la source de données sous-jacente (à l’exception des mises à jour positionnées et des suppressions opérant sur le même cache de curseur). Cela se produit parce que, pour les appels à **SQLFetchScroll**, la bibliothèque de curseurs ne réextrait jamais les données de la source de données. Au lieu de cela, il récupère les données à partir de son cache.  
  
## <a name="scrolling"></a>Défilement  
 La bibliothèque de curseurs prend en charge les types d’extraction suivants dans **SQLFetchScroll**.  
  
|Type de curseur|Types d’extraction|  
|-----------------|-----------------|  
|Curseur avant uniquement|SQL_FETCH_NEXT|  
|statique|SQL_FETCH_NEXT<br /><br /> SQL_FETCH_PRIOR<br /><br /> SQL_FETCH_FIRST<br /><br /> SQL_FETCH_LAST<br /><br /> SQL_FETCH_RELATIVE<br /><br /> SQL_FETCH_ABSOLUTE<br /><br /> SQL_FETCH_BOOKMARK|  
  
## <a name="errors"></a>Erreurs  
 Lorsque **SQLFetchScroll** est appelé et que l’un des appels à **SQLFetch** retourne SQL_ERROR, la bibliothèque de curseurs se poursuit comme suit. Une fois ces étapes terminées, la bibliothèque de curseurs continue le traitement.  
  
1.  Appelle **SQLGetDiagRec** pour obtenir des informations d’erreur à partir du pilote et le publie en tant qu’enregistrement de diagnostic dans le gestionnaire de pilotes.  
  
2.  Définit le champ SQL_DIAG_ROW_NUMBER de l’enregistrement de diagnostic sur la valeur appropriée.  
  
3.  Définit le champ SQL_DIAG_COLUMN_NUMBER dans l’enregistrement de diagnostic sur la valeur appropriée, le cas échéant ; dans le cas contraire, elle définit la valeur 0.  
  
4.  Définit la valeur de la ligne erronée dans le tableau d’état des lignes sur SQL_ROW_ERROR.  
  
 Une fois que la bibliothèque de curseurs a appelé **SQLFetch** plusieurs fois dans son implémentation de **SQLFetchScroll**, toute erreur ou avertissement renvoyé par l’un des appels à **SQLFetch** se trouve dans un enregistrement de diagnostic et peut être récupéré par un appel à **SQLGetDiagRec**. Si les données ont été tronquées lors de leur extraction, les données tronquées se trouvent désormais dans le cache de la bibliothèque de curseurs. Les appels suivants à **SQLFetchScroll** pour faire défiler jusqu’à une ligne avec des données tronquées retournent les données tronquées, et aucun avertissement n’est déclenché, car les données sont extraites du cache de la bibliothèque de curseurs. Pour effectuer le suivi de la longueur des données retournées afin qu’elles puissent déterminer si les données retournées dans une mémoire tampon ont été tronquées, une application doit lier la mémoire tampon de longueur/d’indicateur.  
  
## <a name="bookmark-operations"></a>Opérations de signet  
 La bibliothèque de curseurs prend en charge l’appel de **SQLFetchScroll** avec un *FetchOrientation* de SQL_FETCH_BOOKMARK. Il prend également en charge la spécification d’un décalage dans l’argument *FetchOffset* qui peut être utilisé dans l’opération de signet. Il s’agit de la seule opération de signet prise en charge par la bibliothèque de curseurs. La bibliothèque de curseurs ne prend pas en charge l’appel de **SQLBulkOperations**.  
  
 Si l’application a défini l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS et qu’elle est liée à la colonne de signets, la bibliothèque de curseurs génère un signet de longueur fixe et le retourne à l’application. La bibliothèque de curseurs crée et gère les signets qu’elle utilise ; elle n’utilise pas les signets maintenus au niveau de la source de données. Lorsque **SQLFetchScroll** est appelé pour récupérer un bloc de données qui a déjà été extrait de la source de données, il récupère les données du cache de la bibliothèque de curseurs. Par conséquent, le signet utilisé dans un appel à **SQLFetchScroll** avec un *FetchOrientation* de SQL_FETCH_BOOKMARK doit être créé et géré par la bibliothèque de curseurs.  
  
## <a name="interaction-with-other-functions"></a>Interaction avec d’autres fonctions  
 Une application doit appeler **SQLFetch** ou **SQLFetchScroll** avant de préparer ou d’exécuter une instruction UPDATE ou DELETE positionnée.
