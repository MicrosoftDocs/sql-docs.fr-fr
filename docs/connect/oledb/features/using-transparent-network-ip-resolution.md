---
description: Utilisation de la résolution d’adresses IP réseau transparente
title: Utiliser la résolution d’adresses IP réseau transparente | Microsoft Docs
ms.custom: ''
ms.date: 01/02/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: reference
author: chris-rossi
ms.author: v-chross
ms.openlocfilehash: dbbafac3c6b895d76ec4c931602194d767f9aabd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837457"
---
# <a name="using-transparent-network-ip-resolution"></a>Utilisation de la résolution d’adresses IP réseau transparente
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

## <a name="purpose"></a>Objectif
La résolution d’adresses IP réseau transparente (TNIR) est une révision de la fonctionnalité MultiSubnetFailover existante. Elle affecte la séquence de connexion du pilote dans le cas où la première adresse IP résolue du nom d’hôte ne répond pas et qu’il existe plusieurs adresses IP associées au nom d’hôte. Elle interagit avec MultiSubnetFailover pour fournir les trois séquences de connexion suivantes :<br />
* 0 : Une adresse IP est tentée, suivie de toutes les adresses IP en parallèle.
* 1 : Toutes les adresses IP sont tentées en parallèle.
* 2 : Toutes les adresses IP sont tentées l’une après l’autre.

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportement|
|--------|--------|--------|
|True|True|1|
|Vrai|False|0|
|False|True|1|
|False|False|2|

## <a name="setting-transparent-network-ip-resolution"></a>Définition de la résolution d’adresses IP réseau transparente
TransparentNetworkIPResolution est activé par défaut. MultiSubnetFailover est désactivé par défaut. Pour plus d’informations sur la définition de ces propriétés, consultez les pages suivantes : 
- [Utilisation de mots clés de chaîne de connexion avec OLE DB Driver pour SQL Server](..\applications\using-connection-string-keywords-with-oledb-driver-for-sql-server.md)
- [Propriétés d’initialisation et d’autorisation](..\ole-db-data-source-objects\initialization-and-authorization-properties.md)

## <a name="see-also"></a>Voir aussi 
[Prise en charge de la récupération d’urgence et de la haute disponibilité par OLE DB Driver pour SQL Server](./oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)
