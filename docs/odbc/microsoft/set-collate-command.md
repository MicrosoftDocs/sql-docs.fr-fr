---
description: SET COLLATE, commande
title: SET COLLATE, commande | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set collate command [ODBC]
ms.assetid: 00efbcd4-fea8-4061-86a5-82de413cb753
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6158f79c589e446c2b3c106a1d14fd58715714f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208644"
---
# <a name="set-collate-command"></a>SET COLLATE, commande
Spécifie une séquence de classement pour les champs caractère dans les opérations de tri et d’indexation ultérieures.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SET COLLATE TO cSequenceName  
```  
  
## <a name="arguments"></a>Arguments  
 *cSequenceName*  
 Spécifie une séquence de classement. Les options de séquence de classement disponibles sont décrites dans le tableau suivant.  
  
|Options|Langage|  
|-------------|--------------|  
|Néerlandais|Néerlandais|  
|GENERAL|Anglais, français, allemand, espagnol moderne, portugais et autres langues d’Europe occidentale|  
|Allemand|Ordre de l’annuaire allemand (DIN)|  
|Islande|Islandais|  
|USINAGE|Machine (séquence de classement par défaut pour les versions antérieures de FoxPro)|  
|NORDAN|Norvégien, danois|  
|Espagnol|Espagnol traditionnel|  
|SWEFIN|Suédois, finnois|  
|UNIQWT|Poids unique|  
  
> [!NOTE]  
>  Lorsque vous spécifiez l’option espagnol, *ch* est une lettre unique qui effectue un tri entre *c* et *d*, et *ll* effectue un tri entre *l* et *m*.  
  
 Si vous spécifiez une option de séquence de classement en tant que chaîne de caractères littéraux, veillez à placer l’option entre guillemets :  
  
```  
SET COLLATE TO "SWEFIN"  
```  
  
 MACHINE est l’option de séquence de classement par défaut et est celle qui est familière pour les utilisateurs de Sequence xbase. Les caractères sont ordonnés tels qu’ils apparaissent dans la page de codes actuelle.  
  
 GÉNÉRAL peut être préférable pour les États-Unis et les utilisateurs européens de l’Ouest. Les caractères sont ordonnés tels qu’ils apparaissent dans la page de codes actuelle. Dans les versions FoxPro antérieures à 2,5, les index peuvent avoir été créés à l’aide des fonctions **Upper**() ou **Lower**() pour convertir les champs de caractères en un cas cohérent. Dans les versions FoxPro ultérieures à 2,5, vous pouvez spécifier à la place l’option de séquence de classement général et omettre la conversion **supérieure**().  
  
 Si vous spécifiez une option de séquence de classement autre que MACHINE et que vous créez un fichier. idx, un compact. idx est toujours créé.  
  
 Utilisez SET (« COLLATE ») pour retourner la séquence de classement actuelle.  
  
 Vous pouvez spécifier une séquence de classement pour une source de données à l’aide de la [boîte de dialogue installation de Visual FoxPro ODBC](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) ou du mot clé COLLATE dans votre chaîne de connexion avec [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md). Cela est identique à l’émission de la commande suivante :  
  
```  
SET COLLATE TO cSequenceName  
```  
  
## <a name="remarks"></a>Notes  
 SET COLLATE vous permet de classer les tables contenant des caractères accentués pour n’importe quelle langue prise en charge. La modification de la valeur de SET COLLATE n’affecte pas la séquence de classement des index précédemment ouverts. Visual FoxPro gère automatiquement les index existants, ce qui vous permet de créer de nombreux types d’index différents, même pour le même champ.  
  
 Par exemple, si un index est créé avec SET COLLATE défini sur général et que le paramètre SET COLLATE est ensuite remplacé par l’espagnol, l’index conserve la séquence de classement générale.  
  
## <a name="see-also"></a>Voir aussi  
 [Configuration d’ODBC pour Visual FoxPro, boîte de dialogue](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
