---
description: Conserver la valeur par défaut de l'option de configuration locks
title: Conserver la valeur par défaut de l’option de configuration locks | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Best Practices [Database Engine]
ms.assetid: f214f05b-5f0b-4786-b2ad-b8b4b6e58d72
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: ab27552c473419efd942cdd1c37f7dca4b64c5b6
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98597292"
---
# <a name="keep-the-locks-configuration-option-default-value"></a>Conserver la valeur par défaut de l'option de configuration locks
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette règle vérifie la valeur de l'option de configuration locks (verrous). Cette option détermine le nombre maximal de verrous disponibles et limite ainsi la quantité de mémoire utilisée par le [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] pour les verrous. La valeur par défaut 0 permet à [!INCLUDE[ssDE](../../includes/ssde-md.md)] d’allouer et de libérer des structures de verrous de manière dynamique en fonction des modifications de la configuration requise.  
  
 Si la valeur de l'option locks est différente de zéro, les programmes de traitement par lots s'arrêtent et un message d'erreur indiquant un nombre de verrous insuffisant est généré lorsque la valeur spécifiée est dépassée.  
  
## <a name="best-practices-recommendations"></a>Meilleures pratiques recommandées  
 Utilisez la procédure stockée système sp_configure pour rétablir la valeur par défaut de l'option locks à l'aide de l'instruction suivante :  
  
```  
EXEC sp_configure 'locks', 0;  
```  
  
## <a name="for-more-information"></a>Pour plus d'informations  
 [Configurer l'option de configuration de serveur locks](../../database-engine/configure-windows/configure-the-locks-server-configuration-option.md)  
  
 [sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)  
  
 [sys.dm_os_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md)  
  
 [Article 271509 de la Base de connaissances Microsoft](/troubleshoot/sql/performance/understand-resolve-blocking)  
  
## <a name="see-also"></a>Voir aussi  
 [Contrôler et appliquer les bonnes pratiques à l’aide de la gestion basée sur des stratégies](../../relational-databases/policy-based-management/monitor-and-enforce-best-practices-by-using-policy-based-management.md)  
  
