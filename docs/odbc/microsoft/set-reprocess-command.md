---
description: SET REPROCESS, commande
title: SET Reprocess, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET REPROCESS command [ODBC]
ms.assetid: b0708757-b1d7-42f3-8988-787f2a806b8b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b45188db00eda553bb3443d791d889a4d8c31a77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208599"
---
# <a name="set-reprocess-command"></a>SET REPROCESS, commande
Spécifie le nombre de fois ou la durée de verrouillage d’un fichier ou d’un enregistrement après une tentative de verrouillage ayant échoué.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET REPROCESS TO nAttempts [SECONDS] | TO AUTOMATIC  
```  
  
## <a name="arguments"></a>Arguments  
 À *nAttempts*[secondes]  
 Spécifie le nombre de fois ou le nombre de secondes pendant lequel tenter de verrouiller un enregistrement ou un fichier après une tentative initiale ayant échoué. La valeur par défaut est 0 ; la valeur maximale est 32 000.  
  
 SECONDES spécifie que Visual FoxPro tente de verrouiller un fichier ou un enregistrement pendant *nAttempts* secondes. Elle est disponible uniquement lorsque *nAttempts* est supérieur à zéro.  
  
 Par exemple, si *nAttempts* est 30, Visual FoxPro tente de verrouiller un enregistrement ou un fichier jusqu’à 30 fois. Si vous incluez également secondes (définir le retraitement sur 30 secondes), Visual FoxPro tente continuellement de verrouiller un enregistrement ou un fichier pendant 30 secondes.  
  
 Si une routine en cas d’erreur est en vigueur et que des tentatives d’une commande de verrouillage de l’enregistrement ou du fichier échouent, la routine en cas d’erreur est exécutée. Toutefois, si une fonction tente le verrou, une routine en cas d’erreur n’est pas exécutée et la fonction retourne false (. F.).  
  
 Si une routine en cas d’erreur n’est pas en vigueur, une commande tente de verrouiller l’enregistrement ou le fichier, et le verrou ne peut pas être placé, une erreur est générée. Si une fonction tente de placer le verrou, l’alerte n’est pas affichée et la fonction retourne false (. F.).  
  
 Si *nAttempts* a la valeur 0 (valeur par défaut) et que vous émettez une commande ou une fonction qui tente de verrouiller un enregistrement ou un fichier, Visual FoxPro tente de verrouiller l’enregistrement ou le fichier indéfiniment. Si l’enregistrement ou le fichier devient disponible pour le verrouillage pendant l’attente, le verrou est placé et le message système est effacé. Si une fonction a tenté de placer le verrou, la fonction retourne true (. T.).  
  
 Si une routine en cas d’erreur est en vigueur et qu’une commande tente de verrouiller l’enregistrement ou le fichier, la routine en cas d’erreur a priorité sur les tentatives supplémentaires de verrouillage de l’enregistrement ou du fichier. La routine en cas d’erreur est immédiatement exécutée. Visual FoxPro ne tente pas d’effectuer d’autres verrous d’enregistrement ou de fichier et n’affiche pas le message système.  
  
 Si *nAttempts* a la valeur 1, Visual FoxPro tente de verrouiller l’enregistrement ou le fichier indéfiniment et une routine en cas d’erreur n’est pas exécutée.  
  
 Si un verrou a été placé par un autre utilisateur sur l’enregistrement ou le fichier que vous tentez de verrouiller, vous devez attendre que l’utilisateur libère le verrou.  
  
 SUR AUTOMATIQUE  
 Spécifie que Visual FoxPro tente de verrouiller l’enregistrement ou le fichier indéfiniment. (SET Reprocess TO-2 est une commande équivalente.)  
  
## <a name="remarks"></a>Notes  
 La première tentative de verrouillage d’un enregistrement ou d’un fichier n’est pas toujours réussie. Souvent, un enregistrement ou un fichier est verrouillé par un autre utilisateur sur le réseau. SET Reprocess détermine si Visual FoxPro effectue des tentatives supplémentaires pour verrouiller l’enregistrement ou le fichier en cas d’échec de la tentative initiale. Vous pouvez spécifier le nombre de tentatives supplémentaires effectuées ou la durée des tentatives. Une routine en cas d’erreur affecte la gestion des échecs de tentatives de verrouillage.
