---
title: SQL Server avec Azure Arc
titleSuffix: ''
description: Gérer des instances SQL Server à l’aide de SQL Server avec Azure Arc
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 12/08/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: references_regions
ms.openlocfilehash: 4acc04883d4e4fcc0933078b81dcb8be2a45be89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023263"
---
# <a name="azure-arc-enabled-sql-server-preview"></a>SQL Server avec Azure Arc (préversion)

SQL Server avec Azure Arc fait partie d’Azure Arc pour serveurs. Il étend les services Azure aux instances SQL Server hébergées en dehors d’Azure dans le centre de données du client, à la périphérie ou dans un environnement à plusieurs clouds.

Pour activer les services Azure, une instance SQL Server en cours d’exécution doit être inscrite auprès d’Azure Arc à l’aide du Portail Azure et d’un script d’inscription. Après l’inscription, l’instance est représentée sur Azure sous la forme d’une ressource __SQL Server – Azure Arc__. Les propriétés de cette ressource reflètent un sous-ensemble des paramètres de configuration de SQL Server.

Le SQL Server peut être installé sur une machine virtuelle ou physique exécutant Windows ou Linux connectée à Azure Arc via l’agent Connected Machine. L’agent est installé et la machine est inscrite automatiquement dans le cadre de l’inscription de l’instance SQL Server. L’agent Connected Machine communique de manière sécurisée vers Azure Arc sur le port TCP 443. Si la machine se connecte via un pare-feu ou un serveur proxy HTTP pour communiquer sur Internet, consultez la [configuration réseau requise pour l’agent Connected Machine](/azure/azure-arc/servers/agent-overview#prerequisites).

La préversion publique de SQL Server avec Azure Arc prend en charge un ensemble de solutions qui nécessitent l’installation de l’extension de serveur Microsoft Monitoring Agent (MMA) et la connexion à un espace de travail Azure Log Analytics pour la collecte et la création de rapports de données. Ces solutions incluent la sécurité avancée des données à l’aide d’Azure Security Center et d’Azure Sentinel, et les contrôles d’intégrité de l’environnement SQL à l’aide de la fonctionnalité SQL Assessment à la demande.

Le diagramme suivant illustre l’architecture d’Azure Arc avec SQL Server.

![Architecture de la préversion publique](media/overview/pubic-preview-architecture.png)

## <a name="prerequisites"></a>Prérequis

### <a name="supported-sql-versions-and-operating-systems"></a>Versions de SQL et systèmes d’exploitation pris en charge

SQL Server avec Azure Arc prend en charge SQL Server 2012 ou une version ultérieure exécutée sur l’une des versions suivantes du système d’exploitation Windows ou Linux :

- Windows Server 2012 R2 et versions ultérieures
- Ubuntu 16.04 et 18.04 (x64)
- Red Hat Enterprise Linux (RHEL) 7 (x64) 
- SUSE Linux Enterprise Server (SLES) 15 (x64)

### <a name="required-permissions"></a>Autorisations requises

Pour connecter les instances SQL Server et l’ordinateur d’hébergement à Azure Arc, vous devez avoir un compte avec des privilèges permettant d’effectuer les actions suivantes :
   * Microsoft.AzureArcData/sqlServerInstances/read
   * Microsoft.AzureArcData/sqlServerInstances/write
   * Microsoft.HybridCompute/machines/read
   * Microsoft.HybridCompute/machines/write
   * Microsoft.GuestConfiguration/guestConfigurationAssignments/read

Pour une sécurité optimale, nous vous recommandons de créer un rôle personnalisé dans Azure qui dispose des autorisations minimales indiquées. Pour plus d’informations sur la création d’un rôle personnalisé dans Azure avec ces autorisations, consultez [Vue d’ensemble des rôles personnalisés](/azure/active-directory/users-groups-roles/roles-custom-overview). Pour ajouter une attribution de rôle, consultez [Ajouter ou supprimer des attributions de rôles à l’aide du Portail Azure](/azure/role-based-access-control/role-assignments-portal) ou [Ajouter ou supprimer des attributions de rôles à l’aide du RBAC Azure et d’Azure CLI](/azure/role-based-access-control/role-assignments-cli).

### <a name="azure-subscription-and-service-limits"></a>Limites du service et de l’abonnement Azure

Avant de configurer vos instances SQL Server et machines à l’aide d’Azure Arc, passez en revue les [limites de l’abonnement](/azure/azure-resource-manager/management/azure-subscription-service-limits#subscription-limits) et les [limites du groupe de ressources](/azure/azure-resource-manager/management/azure-subscription-service-limits#resource-group-limits) d’Azure Resource Manager pour planifier le nombre de machines à connecter.

### <a name="networking-configuration-and-resource-providers"></a>Configuration de la mise en réseau et fournisseurs de ressources

Passez en revue la [configuration de la mise en réseau, Transport Layer Security et les fournisseurs de ressources](/azure/azure-arc/servers/agent-overview#prerequisites) requis pour l’agent Connected Machine.

Le fournisseur de ressources `Microsoft.AzureArcData` est nécessaire pour connecter les instances SQL Server à Azure Arc. Consultez les instructions d’inscription du fournisseur de ressources dans la section [Prérequis](connect.md#prerequisites).

Si vous avez déjà des instances SQL Server connectées à Azure Arc, effectuez ces étapes pour migrer les ressources **SQL Server – Azure Arc** existantes vers le nouvel espace de noms.

### <a name="supported-azure-regions"></a>Régions Azure prises en charge

La préversion publique est disponible dans les régions suivantes :
- USA Est
- USA Est 2
- USA Ouest 2
- Australie Est
- Asie Sud-Est
- Europe Nord
- Europe Ouest
- Sud du Royaume-Uni

## <a name="next-steps"></a>Étapes suivantes

- [Connecter votre SQL Server à Azure Arc](connect.md)
- [Configurer votre instance SQL Server pour le contrôle d’intégrité de l’environnement périodique à l’aide de SQL Assessment à la demande](assess.md)
- [Configurer Advanced Data Security pour votre instance SQL Server](configure-advanced-data-security.md)