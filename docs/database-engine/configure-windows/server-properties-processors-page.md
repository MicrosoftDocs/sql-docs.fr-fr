---
title: Propriétés du serveur (page Processeurs)
description: Se familiariser avec les paramètres du processeur dans SQL Server. Découvrez les options qui contrôlent le nombre de threads de travail, l’affectation de processeur et d’autres propriétés.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.serverproperties.processor.f1
ms.assetid: cc1581a2-492b-41f0-bda5-17909b65c4f7
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, matteot
ms.custom: ''
ms.date: 12/17/2020
ms.openlocfilehash: 874cbbae2b418e9b9e06c7a95d62d34a99af38f5
ms.sourcegitcommit: a81823f20262227454c0b5ce9c8ac607aaf537e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/18/2020
ms.locfileid: "97684213"
---
# <a name="server-properties-processors-page"></a>Propriétés du serveur (page Processeurs)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Utilisez cette page pour afficher ou modifier les options de votre processeur. Les paramètres d'affinité du processeur ne sont activés que si plusieurs processeurs sont installés.  

## <a name="options"></a>Options

### <a name="processor-affinity"></a>Affinité du processeur
Affecte des processeurs à des threads spécifiques afin d'éliminer les recharges de processeurs et de réduire la migration des threads entre les processeurs. Pour plus d’informations, consultez [affinity mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-mask-server-configuration-option.md).

### <a name="io-affinity"></a>Affinité d'E/S
Lie les E/S de disque [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server à un sous-ensemble spécifié de processeurs. Pour plus d’informations, consultez [affinity Input-Output mask (option de configuration de serveur)](../../database-engine/configure-windows/affinity-input-output-mask-server-configuration-option.md).

### <a name="automatically-set-processor-affinity-mask-for-all-processors"></a>Définir automatiquement le masque d'affinité du processeur pour tous les processeurs
Autorise SQL Server à définir l’affinité du processeur.

### <a name="automatically-set-io-affinity-mask-for-all-processors"></a>Définir automatiquement le masque d'affinité d'E/S pour tous les processeurs
Autorise SQL Server à définir l’affinité d’E/S.

### <a name="maximum-worker-threads"></a>Nombre maximal de threads de travail
0 autorise SQL Server à définir dynamiquement le nombre de threads de travail. Ce paramètre convient à la plupart des systèmes. Toutefois, selon votre configuration système, l'attribution d'une valeur spécifique à cette option peut permettre d'accroître les performances. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur max worker threads](../../database-engine/configure-windows/configure-the-max-worker-threads-server-configuration-option.md).  

### <a name="boost-sql-server-priority"></a>Renforcer la priorité SQL Server
Indique si l’exécution de SQL Server doit avoir une priorité de planification Microsoft Windows supérieure à celle d’autres processus du même ordinateur. Pour plus d’informations, consultez [Configurer l’option de configuration du serveur priority boost](../../database-engine/configure-windows/configure-the-priority-boost-server-configuration-option.md).  

> [!Note]
> Cette option n’est pas disponible avec SSMS 18.x et versions ultérieures.

### <a name="use-windows-fibers-lightweight-pooling"></a>Utiliser les fibres Windows (regroupement léger)
Utilise les fibres Windows plutôt que des threads pour le service SQL Server. Cette option n’est disponible que dans Windows 2003 Server Edition. Pour plus d’informations, consultez [lightweight pooling Server Configuration Option](../../database-engine/configure-windows/lightweight-pooling-server-configuration-option.md).

> [!Note]
> Cette option n’est pas disponible avec SSMS 18.x et versions ultérieures.

### <a name="configured-values"></a>Valeurs configurées
Affiche les valeurs configurées pour les options de ce volet. Si vous modifiez ces valeurs, sélectionnez **Valeurs en cours d’exécution** pour vérifier si les modifications ont été prises en compte. Si ce n’est pas le cas, l’instance de SQL Server doit être d’abord redémarrée.

### <a name="running-values"></a>Valeurs en cours d’exécution
Affiche les valeurs en cours d'exécution pour les options de ce volet. Ces valeurs sont en lecture seule.

## <a name="see-also"></a>Voir aussi
[Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)  


