---
title: Mettre en cluster le service DTC pour un groupe de disponibilité
description: 'Décrit les exigences et les étapes liées au clustering du service Microsoft Distributed Transaction Coordinator (DTC) pour un groupe de disponibilité Always On. '
ms.custom: seo-lt-2019
ms.date: 08/30/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: a47c5005-20e3-4880-945c-9f78d311af7a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 981ce2dcfadf234383382103a33faf4f0ab9302e
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97643047"
---
# <a name="how-to-cluster-the-dtc-service-for-an-always-on-availability-group"></a>Mettre en cluster le service DTC pour un groupe de disponibilité Always On

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

Cette rubrique décrit les exigences et les étapes liées au clustering du service Microsoft Distributed Transaction Coordinator (DTC) pour [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Pour plus d’informations sur les transactions distribuées et [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], consultez [Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).

 ## <a name="checklist-preliminary-requirements"></a>Liste de vérification : spécifications préliminaires

|Tâche|Informations de référence|  
|-----------------|----------|  
|Assurez-vous que tous les nœuds, les services et le groupe de disponibilité ont été configurés correctement.|[Prérequis, restrictions et recommandations pour les groupes de disponibilité Always On (SQL Server)](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)|
|Vérifiez que les exigences relatives au groupe de disponibilité DTC sont remplies.|[Transactions entre bases de données et transactions distribuées pour des groupes de disponibilité Always On et la mise en miroir de bases de données (SQL Server)](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)

## <a name="checklist-clustered-dtc-resource-dependencies"></a>Liste de vérification : Dépendances de la ressource DTC en cluster

|Tâche|Informations de référence|  
|-----------------|----------|  
|Un lecteur de stockage partagé.|[Configuration du lecteur de stockage partagé](https://msdn.microsoft.com/library/cc982358(v=bts.10).aspx). Envisagez d’utiliser la lettre de lecteur **M**.|
|Une ressource de nom réseau DTC unique.  Le nom sera enregistré comme un objet Ordinateur en cluster dans Active Directory.<br /><br />Assurez-vous qu’une des conditions suivantes est remplie :<br /><br />• L’utilisateur qui crée la ressource de nom de réseau DTC est autorisé à créer des objets Ordinateur sur l’unité d’organisation ou le conteneur dans lequel se trouve la ressource de nom de réseau DTC.<br /><br />• Si l’utilisateur n’est pas autorisé à créer d’objets Ordinateur, demandez à un administrateur de domaine de préconfigurer un objet Ordinateur en cluster pour la ressource de nom de réseau DTC.|[Préconfigurer des objets Ordinateur en Cluster dans les services de domaine Active Directory](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn466519(v=ws.11))|
|Une adresse IP statique disponible valide et le masque de sous-réseau approprié pour cette adresse IP.||

## <a name="cluster-the-dtc-resource"></a>Mettre en cluster la ressource DTC
Une fois que vous avez créé votre ressource de groupe de disponibilité, créez une ressource DTC en cluster et ajoutez-la au groupe de disponibilité.  Pour voir un exemple de script, consultez [Créer un DTC en cluster pour un groupe de disponibilité Always On](../../../database-engine/availability-groups/windows/create-clustered-dtc-for-an-always-on-availability-group.md).


## <a name="checklist-post-clustered-dtc-resource-configurations"></a>Liste de vérification : Configurations de la ressource DTC après sa mise en cluster

|Tâche|Informations de référence|  
|-----------------|----------|  
|Activer l’accès réseau sécurisé pour la ressource DTC mise en cluster.|[Activer l’accès réseau sécurisé pour MS DTC](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753620(v=ws.10))|
|Arrêter et désactiver le service DTC local.|[Configurer le démarrage d’un service](https://technet.microsoft.com/library/cc755249(v=ws.11).aspx)|
|Parcourez tour à tour le service SQL Server pour chaque instance du groupe de disponibilité.  Basculer le groupe de disponibilité en fonction des besoins.|[Effectuer un basculement manuel planifié d'un groupe de disponibilité (SQL Server)](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)<br /><br />[Démarrer, arrêter, suspendre, reprendre, redémarrer le moteur de base de données, SQL Server Agent ou le service SQL Server Browser](../../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)|

- Si le serveur est Windows Server 2012 R2, l’article [3030373 de la Base de connaissances](https://support.microsoft.com/kb/3090973) doit être appliqué au système d’exploitation.

- Préparez les serveurs des groupes de disponibilité selon les listes de vérification de la page [Conditions préalables requises, restrictions et recommandations pour les groupes de disponibilité Always On](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).

- Configurez les instances de serveur pour les [**groupes de disponibilité Always On**](../../../database-engine/availability-groups/windows/configuration-of-a-server-instance-for-always-on-availability-groups-sql-server.md).

### <a name="resources"></a>RESSOURCES


[Autres informations sur le test DTC sur les groupes de disponibilité :](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups)

[Surveillance des vues système des groupes de disponibilité Always On](monitor-availability-groups-transact-sql.md)

[Créer un groupe de disponibilité étape par étape](create-an-availability-group-transact-sql.md)


[Prise en charge de SQL Server 2016 DTC dans les groupes de disponibilité](/archive/blogs/dataplatform/sql-server-2016-dtc-support-in-availability-groups) 

[Lien externe : Configure DTC for a clustered instance of SQL Server with Windows Server 2008 R2](https://sqlha.com/2013/03/12/how-to-properly-configure-dtc-for-clustered-instances-of-sql-server-with-windows-server-2008-r2/) (Configurer DTC pour une instance en cluster de SQL Server avec Windows Server 2008 R2)