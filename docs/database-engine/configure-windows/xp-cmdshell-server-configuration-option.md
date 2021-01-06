---
title: xp_cmdshell (option de configuration de serveur)
description: Découvrez l’option « xp_cmdshell ». Découvrez comment elle indique si SQL Server peut exécuter la procédure stockée étendue « xp_cmdshell ». Découvrez comment l’activer.
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- xp_cmdshell
ms.assetid: c147c9e1-b81d-49c8-b800-3019f4d86a13
author: markingmyname
ms.author: maghan
ms.custom: contperf-fy20q4
ms.date: 06/12/2020
ms.openlocfilehash: d12ea9c15f2a07fa6fdd4bc5dd5538fd1522d267
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878832"
---
# <a name="xp_cmdshell-server-configuration-option"></a>xp_cmdshell (option de configuration de serveur)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Cet article explique comment activer l’option de configuration **xp_cmdshell** SQL Server. Cette option permet aux administrateurs système de contrôler si la [procédure stockée étendue xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md) peut être exécutée sur un système. Par défaut, l’option **xp_cmdshell** est désactivée sur les nouvelles installations.

Avant d’activer cette option, il est important de prendre en compte les implications potentielles en matière de sécurité.

- Le code récemment développé ne doit pas utiliser la procédure stockée **xp_cmdshell** et, en général, il doit être désactivé.
- Certaines applications héritées nécessitent l’activation de **xp_cmdshell**. Si elles ne peuvent pas être modifiés pour éviter l’utilisation de cette procédure stockée, vous pouvez l’activer comme indiqué ci-dessous.

> [!NOTE]  
> Si **xp_cmdshell** doit être utilisé, il est recommandé, pour des raisons de sécurité, de l’activer uniquement pendant la durée d’exécution de la tâche qui l’exige.

Si vous devez activer **xp_cmdshell**, vous pouvez utiliser la [gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md) ou exécuter la procédure stockée système **sp_configure**, comme indiqué dans l’exemple de code suivant :  
  
``` sql
-- To allow advanced options to be changed.  
EXECUTE sp_configure 'show advanced options', 1;  
GO  
-- To update the currently configured value for advanced options.  
RECONFIGURE;  
GO  
-- To enable the feature.  
EXECUTE sp_configure 'xp_cmdshell', 1;  
GO  
-- To update the currently configured value for this feature.  
RECONFIGURE;  
GO  
```  
  
## <a name="next-steps"></a>Étapes suivantes

- [Procédure stockée étendue xp_cmdshell](../../relational-databases/system-stored-procedures/xp-cmdshell-transact-sql.md)
- [Options de configuration du serveur (SQL Server)](server-configuration-options-sql-server.md)
- [Administrer des serveurs à l'aide de la Gestion basée sur des stratégies](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)  
