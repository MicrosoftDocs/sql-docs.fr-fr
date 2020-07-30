---
title: srv_willconvert (API de procédure stockée étendue) | Microsoft Docs
description: Découvrez comment srv_willconvert détermine si une conversion de type de données spécifique est disponible dans la bibliothèque ODS.
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
apiname:
- srv_willconvert
apilocation:
- opends60.dll
apitype: DLLExport
dev_langs:
- C++
helpviewer_keywords:
- srv_willconvert
ms.assetid: 6f4db5fd-215a-461c-95e4-17697852733e
author: rothja
ms.author: jroth
ms.openlocfilehash: ef515b200221a6bb439a65a02e546017046dd0e4
ms.sourcegitcommit: 216f377451e53874718ae1645a2611cdb198808a
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87248213"
---
# <a name="srv_willconvert-extended-stored-procedure-api"></a>srv_willconvert (API de procédure stockée étendue)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
    
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Utilisez l’intégration CLR à la place.  
  
 Détermine si une conversion de type de données spécifique est disponible au sein de la bibliothèque ODS.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
BOOL srv_willconvert (  
int  
srctype  
,  
int  
desttype   
);  
```  
  
## <a name="arguments"></a>Arguments  
 *srctype*  
 Indique le type de données des données à convertir. Ce paramètre peut être n'importe lequel des types de données des API de procédure stockée étendue.  
  
 *desttype*  
 Indique le type de données vers lequel les données sources sont converties. Ce paramètre peut être n'importe lequel des types de données des API de procédure stockée étendue.  
  
## <a name="returns"></a>Retourne  
 TRUE si la conversion de type de données est prise en charge ; FALSE, dans le cas contraire.  
  
## <a name="remarks"></a>Notes  
 Pour obtenir une description de chaque type de données, consultez [Types de données &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/data-types-extended-stored-procedure-api.md).  
  
> [!IMPORTANT]  
>  Il est préférable d'examiner avec soin le code source des procédures stockées étendues et de tester les DLL compilées avant de les installer sur un serveur de production. Pour plus d'informations sur l'examen et les tests de sécurité, consultez ce [site Web de Microsoft](https://www.microsoft.com/msrc?rtc=1).  
  
## <a name="see-also"></a>Voir aussi  
 [srv_convert &#40;API de procédure stockée étendue&#41;](../../relational-databases/extended-stored-procedures-reference/srv-convert-extended-stored-procedure-api.md)  
  
  
