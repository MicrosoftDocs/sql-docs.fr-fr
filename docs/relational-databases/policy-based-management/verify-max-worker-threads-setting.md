---
description: Vérifier le paramètre de l'option Max Worker Threads
title: Vérifier le paramètre de l’option Max Worker Threads | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: 2d94adfd-3ba1-493a-b29a-b436f9d583df
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8c82c2359a8dd4828b8d7da465752eeec44c70f1
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88420473"
---
# <a name="verify-max-worker-threads-setting"></a>Vérifier le paramètre de l'option Max Worker Threads
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette règle vérifie que le paramètre de l'option de serveur max worker threads (nombre maximum de threads de travail) est correct. L'affectation d'une valeur faible à l'option max worker threads peut empêcher qu'un nombre suffisant de threads traite les demandes entrantes des clients en temps voulu et peut entraîner une pénurie de threads. Toutefois, dans la mesure où chaque thread actif utilise jusqu’à 4 Mo sur des serveurs 64 bits, l’affectation d’une valeur élevée à cette option peut entraîner un gaspillage de l’espace d’adressage.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 Affectez à l’option Nombre maximum de threads de travail la valeur 0. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ainsi déterminer automatiquement le nombre correct de threads de travail actifs en fonction des demandes des utilisateurs.  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 [Configurer l'option de configuration du serveur max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Contrôler et appliquer les bonnes pratiques à l’aide de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
  
